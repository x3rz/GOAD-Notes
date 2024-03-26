[Guide](https://blog.netwrix.com/2018/05/29/regular-expressions-for-beginners-how-to-get-started-discovering-sensitive-data/)

For AWS Access ID:
`AKIA[0-9A-Z]{16}`
	- starting with AKIA word
	- any char from 0-9 and A-Z included
	- is 16 letter long.

For twillo API keys:
`SK[a-z0-9]{32}`

For PII:
- password=,secret =, key =,api_key
- `-----BEGIN RSA PRIVATE KEY-----`
- `((password=)|(passwd=)|(pass=))[^&\/\?]+/gi`


Email adresses:
- `([a-zA-Z0-9_\.-]+)@([\da-zA-Z\.-]+)\.([a-zA-Z\.]{2,6})`
- ``[a-z0-9!#$%&'*+\/=?^_`{|.}~-]+@(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?)``
Social Security number:
- `^\d{3}-?\d{2}-?\d{4}$`

IP addresses:
- `^(?:[0-9]{1,3}\.){3}[0-9]{1,3}$`

For Passwords in URL:
- `[a-zA-Z]{3,15}:\/\/[^\/\\:@]+:[^\/\\\:@]+@.{1,100}`
	- finds url like `protocol://username:password@example.com`

[AWS PII regexes](https://github.com/datumbrain/aws-macie-pii-confidential-regexes/blob/master/regex_list.csv)
[Token hunter regex](https://github.com/codeEmitter/token-hunter/blob/master/regexes.json)
[Dalfox regex](https://github.com/hahwul/dalfox/blob/master/pkg/scanning/grep.go)
[Regex](https://github.com/cdk-team/CDK/blob/main/conf/exploit_conf.go)
[APKleaks regex](https://github.com/dwisiswant0/apkleaks/blob/master/config/regexes.json)

# ToDo 
Find more regex to get the sensitive information.
- Owasp code review guid has alot of regex patterns.
- 