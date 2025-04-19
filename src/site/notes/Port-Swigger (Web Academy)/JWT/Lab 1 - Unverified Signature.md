---
{"dg-publish":true,"permalink":"/port-swigger-web-academy/jwt/lab-1-unverified-signature/"}
---


---

JWT libraries typically provide one method for verifying tokens and another that just decodes them. For example, the Node.js library `jsonwebtoken` has `verify()` and `decode()`.

Occasionally, developers confuse these two methods and only pass incoming tokens to the `decode()` method. This effectively means that the application doesn't verify the signature at all.

## Objective: Gain access to `/admin`

We login to our account with the given credentials:
`wiener:peter`

Inspecting the requests in Burp, we can see the JWT token here is within the our session cookie.

![Pasted image 20250403024441.png](/img/user/Images/Pasted%20image%2020250403024441.png)

Next, we willl use the `jwt_tool` from github to solve this. Copy the JWT Token ans use it in this command:

```bash
python3 jwt_tool.py [TOKEN] -T
```

Get to the payload part

![Pasted image 20250403070937.png](/img/user/Images/Pasted%20image%2020250403070937.png)

We will try to modify `wiener` to `administrator` 

![Pasted image 20250403072259.png](/img/user/Images/Pasted%20image%2020250403072259.png)

Navigate to `/admin` and replace the cookie session

![Pasted image 20250403071436.png](/img/user/Images/Pasted%20image%2020250403071436.png)

Nice, it worked

![Pasted image 20250403072418.png](/img/user/Images/Pasted%20image%2020250403072418.png)