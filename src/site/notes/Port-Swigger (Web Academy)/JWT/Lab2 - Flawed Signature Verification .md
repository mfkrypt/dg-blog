---
{"dg-publish":true,"permalink":"/port-swigger-web-academy/jwt/lab2-flawed-signature-verification/"}
---


---

Among other things, the JWT header contains an `alg` parameter. JWTs can be signed using a range of different algorithms, but can also be left unsigned. In this case, the `alg` parameter is set to `none`, which indicates a so-called "unsecured JWT".

Log in normally

![Pasted image 20250419170714.png](/img/user/Images/Pasted%20image%2020250419170714.png)

The JWT Token is in the session cookie

![Pasted image 20250419170837.png](/img/user/Images/Pasted%20image%2020250419170837.png)

We use the tool, `jwt_tool` to modify `wiener` to `administrator` and  change the `alg` parameter value from `RS256` to `none` 

```shell
python3 jwt_tool.py [TOKEN] -T
```

![Pasted image 20250419171039.png](/img/user/Images/Pasted%20image%2020250419171039.png)

![Pasted image 20250419172252.png](/img/user/Images/Pasted%20image%2020250419172252.png)

Use the tampered JWT Token and replace the cookie but we only take the `header` and `payload` part . The signature is removed (leaving the trailing period)

```
eyJraWQiOiJhMWNjODc1OS02NGEwLTRhZGItYTczMy1hZjdhOTM0ZTcyZDkiLCJhbGciOiJub25lIn0.eyJpc3MiOiJw
b3J0c3dpZ2dlciIsImV4cCI6MTc0NTA1Njk1OCwic3ViIjoiYWRtaW5pc3RyYXRvciJ9.
```

![Pasted image 20250419172701.png](/img/user/Images/Pasted%20image%2020250419172701.png)

