# Description

Exposure of server side sensitive information due to unhandled exception in handling request method.

# Proof of Concept

1.  Go to this link http://v4.nexopos.com/api/nexopos/v4/crud/ns.payments-types/4
2.  See that the page returns with sensitive server side data. Here is a sample

```
    "message": "The GET method is not supported for this route. Supported methods: PUT, DELETE.",
    "exception": "Symfony\\Component\\HttpKernel\\Exception\\MethodNotAllowedHttpException",
    "file": "/var/www/html/v4.nexopos.com/vendor/laravel/framework/src/Illuminate/Routing/AbstractRouteCollection.php",
    "line": 117,
```

# Impact

This vulnerability is capable of exposure of server side information.