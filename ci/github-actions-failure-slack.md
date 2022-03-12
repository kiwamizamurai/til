# CI Slack Notification

Create `ci.yml`

```yaml
name: slack notidy CI

on:
    pull_request:

jobs:
    example_job:
        runs-on: ubuntu-latest
        steps:
        - uses: actions/checkout@v2
        - name: Set COMMIT_MESSAGE
          run: |
            echo ::set-env name=COMMIT_MESSAGE::$(echo "${{ github.event.head_commit.message }}" | tr '\n' ' ')
        - name: Slack Notification on FAILURE
          if: failure()
          uses: tokorom/action-slack-incoming-webhook@master
          env:
            INCOMING_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
          with:
            text: CI failed...
            attachments: |
              [
                {
                  "color": "bad",
                  "author_name": "${{ github.actor }}",
                  "author_icon": "${{ github.event.sender.avatar_url }}",
                  "fields": [
                    {
                      "title": "Commit Message",
                      "value": "${{ env.COMMIT_MESSAGE }}"
                    },
                    {
                      "title": "GitHub Actions URL",
                      "value": "https://github.com/YOUR_REPOSITORY_NAME/actions/runs/${{ github.run_id }}"
                    }
                  ]
                }
              ]
```

## References

- https://note.com/anatoo/n/n16e875c065f9
- https://qiita.com/progrhyme/items/badf31978612b22dbf47
- https://spinners.work/posts/github-actions-context/
- https://stackoverflow.com/questions/57968497/how-do-i-set-an-env-var-with-a-bash-expression-in-github-actions