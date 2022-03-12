# Pytest Clean db for every test method

here is scope explanation

- function: the default scope, the fixture is destroyed at the end of the test.
- class: the fixture is destroyed during teardown of the last test in the class.
- module: the fixture is destroyed during teardown of the last test in the module.
- package: the fixture is destroyed during teardown of the last test in the package.
- session: the fixture is destroyed at the end of the test session.

`conftest.py`

```python
    @pytest.fixture(scope='session')
    def db_sessionlocal():
        TEST_SQLALCHEMY_DATABASE_URL = (f"mysql://{os.environ['TEST_DB_USER']}:"
                                        f"{os.environ['TEST_DB_PASSWORD']}@"
                                        f"{os.environ['TEST_DB_HOST']}:"
                                        f"{os.environ['TEST_DB_PORT']}/"
                                        f"{os.environ['TEST_DB_DATABASE']}"
                                        "?charset=utf8mb4")

        engine_ = create_engine(TEST_SQLALCHEMY_DATABASE_URL)
        assert not database_exists(
            engine_.url
        ), "Test database already exists. Aborting tests."

        create_database(engine_.url)
        Base.metadata.create_all(engine_)
        SessionLocal = sessionmaker(autocommit=False,
                                    autoflush=False,
                                    bind=engine_)

        print("\n--------------[  class   setup   ]--------------\n")
        # Run the tests
        yield SessionLocal
        print("\n--------------[  class teardown  ]--------------\n")

        drop_database(TEST_SQLALCHEMY_DATABASE_URL)


    @pytest.fixture(scope='function')
    def db_session(db_sessionlocal):

        def override_get_database():
            try:
                db = db_sessionlocal()
                yield db
            finally:
                db.close()

        app.dependency_overrides[get_database] = override_get_database

        print("\n--------------[  setup  ]--------------\n")
        test_db = db_sessionlocal()
        yield test_db
        print("\n--------------[ teardown ]--------------\n")

        test_db.close()
        clear_db(engine_=test_db.bind)

        app.dependency_overrides[get_database] = get_database


    # Each method of TestClass can use fresh/clean db
    def clear_db(engine_):
        Base.metadata.drop_all(bind=engine_)
        Base.metadata.create_all(bind=engine_)
```

test file is as follows:

```python
    from conftest import db_session

    class TestExample:

        def test_first(self, db_session):
            // do something
            session_ = db_session

        def test_second(self, db_session):
            // do something
            session_ = db_session
```

Inside each method, `db_session` is different object from others.


The output is something like this

```
    $ docker-compose exec app pytest -c pytest-in-app.ini -s tests/routers/test_example.py

    =============================================================================== test session starts ===============================================================================
    platform linux -- Python 3.7.9, pytest-6.0.1, py-1.9.0, pluggy-0.13.1
    rootdir: /app/tests/routers, configfile: ../../pytest-in-app.ini
    plugins: env-0.6.2
    collected 2 item

    tests/routers/test_example.py
    --------------[  class   setup   ]--------------


    --------------[  setup  ]--------------
    .
    --------------[ teardown ]--------------

    --------------[  setup  ]--------------
    .
    --------------[ teardown ]--------------

    --------------[  class teardown  ]--------------



    ================================================================================ 1 passed in 0.29s ================================================================================
```

BTW, `--setup-show` options is useful

```python
tests/routers/test_example.py
    SETUP    C db_sessionlocal
        SETUP    F db_session (fixtures used: db_sessionlocal)
        test_example.py::TestExample::test_normal (fixtures used: db_session, db_sessionlocal).
        TEARDOWN F db_session
        SETUP    F db_session (fixtures used: db_sessionlocal)
        test_example.py::TestExample::test_second (fixtures used: db_session, db_sessionlocal).
        TEARDOWN F db_session
    TEARDOWN C db_sessionlocal
```

Although we are going off the track, I use `pytest-in-app.ini` in order to overwrite `.env`

```yml
    [pytest]
    env =
        D:TEST_DB_DATABASE=bot_test
        D:TEST_DB_USER=root
        D:TEST_DB_PASSWORD=root
        D:TEST_DB_PORT=3306
        D:TEST_DB_HOST=db
```


## References

- https://fastapi.tiangolo.com/advanced/testing-database/
- https://gist.github.com/vkotovv/6281951
- https://stackoverflow.com/questions/4763472/sqlalchemy-clear-database-content-but-dont-drop-the-schema
- https://docs.pytest.org/en/stable/fixture.html