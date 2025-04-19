---
{"dg-publish":true,"permalink":"/port-swigger-web-academy/jwt/lab-3-weak-signing-key/"}
---


---

Some signing algorithms, such as HS256 (HMAC + SHA-256), use an arbitrary, standalone string as the secret key resulting in the key can be easily guess/brute-forced

Login with the given credentials

![Pasted image 20250419211748.png](/img/user/Images/Pasted%20image%2020250419211748.png)

Grab the JWT Token from the session key

![Pasted image 20250419211857.png](/img/user/Images/Pasted%20image%2020250419211857.png)

We will use the `jwt_tool`  to crack the secret key using the `-C` and `-d` for the dictionary

```shell
python3 jwt_tool.py [TOKEN] -C -d /usr/share/wordlists/rockyou.txt
```

![Pasted image 20250419212427.png](/img/user/Images/Pasted%20image%2020250419212427.png)

The secret key is: `secret1`

Now we change the `sub` parameter to `administrator` and sign the JWT Token with the secret key with  `-S` to specify the algorithm and `-p` to specify the key

```shell
python3 jwt_tool.py [TOKEN] -T -S hs256 -p secret1
```

![Pasted image 20250419213246.png](/img/user/Images/Pasted%20image%2020250419213246.png)

Replace the session with the new generated Token and we can access `/admin`



