## Single Lambda Request

Invoke a function with args, save the response, and then read it into `jq`.

```bash
aws lambda invoke --cli-binary-format raw-in-base64-out --function-name function-name --payload '{ "request_body_args": { "endDate": "2022-04-01", "dryRun": true } }' response.txt && cat response.txt | jq .
```
