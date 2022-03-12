# Github Actions Docker Image Cache

`.github/workflows/cache.yaml`

```yaml
name: CI

on:
    pull_request:

    env:
    COMPOSE_FILE: docker-compose-ci.yml

jobs:
    job_pytest:
        runs-on: ubuntu-latest
        steps:
        - uses: actions/checkout@v2
        - name: Cache Build
            run: |
            echo ${{ secrets.GITHUB_TOKEN }} | docker login docker.pkg.github.com -u $GITHUB_ACTOR --password-stdin
            docker pull docker.pkg.github.com/$GITHUB_REPOSITORY/AnyPackageName || true
            docker build . -f ./docker/app/Dockerfile -t AnyPackageName --cache-from=docker.pkg.github.com/$GITHUB_REPOSITORY/AnyPackageName
            docker tag AnyPackageName docker.pkg.github.com/$GITHUB_REPOSITORY/AnyPackageName && docker push docker.pkg.github.com/$GITHUB_REPOSITORY/AnyPackageName || true
```

`docker-compose-ci.yml`

```yaml
    version: "3.8"
    services:
    app:
        container_name: something
        image: docker.pkg.github.com/$GITHUB_REPOSITORY/AnyPackageName
```

## References

- https://dev.to/dtinth/caching-docker-builds-in-github-actions-which-approach-is-the-fastest-a-research-18ei
