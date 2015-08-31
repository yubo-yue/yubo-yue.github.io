---
layout: post
title:  "RESTFul API - Follow me"
date: 2015-08-31 17:50:13
categories: REST RESTFul Webservice
---
Orinial Author
<strong> 
brian@apigee.com
</strong>

## 目的
提供一个简明的tutorial来快速创建RESTFul的API，假设这里我们要管理的资源是dog。

## How

### Keep the simple things simple, each resource only has two URLs

集合和单体

- /dogs - 集合
    * POST /dogs create a new dog
    * GET /dogs list dogs
    * PUT /dogs bulk update dogs
    * DELETE /dogs delete all dogs
- /dogs/:dogid - 单体
    * POST /dogs/1234 ERROR
    * GET /dogs/1234 show dog 1234
    * PUT /dogs/1234 if 1234 exists, update it, if not, ERROR
    * DELETE /dogs/1234 delete dog 1234
    
<strong>Takeaway - Verbs are bad, nouns are good, plurals is beeter.</strong>

### Abstract or Concrete naming?

/things or /animals or /dogs? <strong> winner is /dogs</strong>

<strong>Takeaway - Concrete is better</strong>

### What about associations?

- GET /owners/1234/dogs
- POST /owners/1234/dogs

### What about complex variations?

- GET /dogs?color=red&state=ca&location=park

<strong> Takeaway: Sweep the variation with "?"</strong>

### What about errors?

- Code for code
```
200 - HTTP OK
401 - HTTP Unauthorized
```

- Message for people

```
{“message” : “Verbose, plain language description of the problem with hints about how to fix it.”
“more_info” : “http://dev.teachdogrest.com/ errors/12345”}
```

### What about versioning?

- /v1/dogs

### give me more exactly what i need?

- /dogs?fields=name,color,location

### What about pagination?

- /dogs?limit=25&offset=50

### What about the format?

- /dogs.json
- /dogs.xml

### what about the default

- format : json
- pagination: limit=10&offset=0

### What about the attribute name

<strong>target user: Javascript</strong>

```
Javascript convention - camel case
"createdAt": 1320296464
```

### What about non-resource-y stuffs
- Calculate
- Convert
- Translate

Results:
- Use verb not nouns
```
/convert?from=EUR&to=CNY&amount=100
```
### What about searching

- Global
```
/search?q=fluffy+fur
```
- Scope
```
/owners/5678/dogs/search?q=fluffy+fur
```
- Formatted
```
/search.xml?q=fluffy+fur
```

### what about exceptional case?

- Always return OK
```
/dogs?suppress_response_codes=true
```
- Code for ignoring
```
200 - OK
```
- Message for people & code
```
{"response_code" : "401", "message" : "Verbose, plain language description of the problem with hins about how to fix it.","more_info" : "http://dev.teachdogrest.com/ errors/12345", "code" : 12345}
```

### What about the authentication?

<strong>Use latest and greatest OAuth 2.0</strong>

### How do application developer use your API

- First be completely and RESTful
- provides shortcut
- Complement your API with libraries and SDK


## 总结

- Be RESTful
- Only 2 URLs 
- No verbs 
- Use nouns as plurals
- Concrete over abstract
- For JSON follow JavaScript conventions 
- Sweep complexity behind the ‘?’ 
- Borrow from leading APIs
- Account for exceptional clients 
- Add virtualization layer
