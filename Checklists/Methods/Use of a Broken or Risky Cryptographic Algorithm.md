1. Search in the if it has `SHA1, MD5` or any weak cryptographic algorithm 

Agr bakchodi ho then:
```
Hi buddy Thanks for reaching out. Use of SHA1 itself a security issue and it is vulnerable to hash collisions and other attacks.

Check the below link: https://www.computerworld.com/article/3173616/the-sha1-hash-function-is-now-completely-unsafe.html

Upgrading from SHA1 to SHA256 wont be a big challenge.
```

2. Search in source code if it has use of `!=,==` operators like this when they are using MD5 algorithm as it is vulnerable to `Type Juggling`. [[Type Juggling]]
3. 