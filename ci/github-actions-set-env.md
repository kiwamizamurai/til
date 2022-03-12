# CI pytest docker

Create `ci.yml`

```yaml
name: pytest CI

on:
pull_request:

env:
DB_DATABASE: example_db
DB_USER: example_user
DB_PASSWORD: root
DB_ROOT_PASSWORD: root

jobs:
test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - name: Create .env file
        run: |
        echo "${{ secrets.ENV }}" > path/to/.env
    - name: Set Up
        run: |
        docker-compose up -d db
        docker-compose up -d
    - name: Run pytest
        run: |
        docker-compose exec -T app pytest -c pytest.ini
```


my env file in github setting is as follows:

```yaml
SLACK_API_TOKEN=xxxxx
SLACK_SIGNING_SECRET=xxxxx
DB_DATABASE=example_db
DB_USER=example_user
DB_PASSWORD=root
DB_PORT=3306
DB_HOST=db
UVICORN_PORT=8080
UVICORN_HOST=0.0.0.0
LOG_LEVEL=DEBUG
LOG_GROUP=my_log_group
LOG_STREAM=my_log_stream
LOG_DEST=LOCAL
PROFILE=example_profile
```

The point is that we have to wait for the initialization of db container