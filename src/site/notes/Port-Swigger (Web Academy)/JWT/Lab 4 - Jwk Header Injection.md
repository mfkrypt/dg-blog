---
{"dg-publish":true,"permalink":"/port-swigger-web-academy/jwt/lab-4-jwk-header-injection/"}
---


---

The JSON Web Signature (JWS) specification describes an optional `jwk` header parameter, which servers can use to embed their public key directly within the token itself in JWK format.

You can see an example of this in the following JWT header:

```json
{ 
	"kid": "ed2Nf8sb-sD6ng0-scs5390g-fFD8sfxG", 
	"typ": "JWT", 
	"alg": "RS256", 
	"jwk": { 
		"kty": "RSA", 
		"e": "AQAB", 
		"kid": "ed2Nf8sb-sD6ng0-scs5390g-fFD8sfxG", 
		"n": "yy1wpYmffgXBxhAUJzHHocCuJolwDqql75ZWuCQ_cb33K2vh9m" 
	} 
}
```

Ideally, servers should only use a limited whitelist of public keys to verify JWT signatures. However, misconfigured servers sometimes use any key that's embedded in the `jwk` parameter.

You can exploit this behavior by signing a modified JWT using your own RSA private key, then embedding the matching public key in the `jwk` header.

You can also perform this attack manually by adding the `jwk` header yourself. However, you may also need to update the JWT's key id,`kid` header parameter to match the `kid` of the embedded key. The extension's built-in attack takes care of this step for you.

Let's login with our given credentials

![Pasted image 20250419220917.png](/img/user/Images/Pasted%20image%2020250419220917.png)

Grab the JWT Token from the session key and use the 

![Pasted image 20250419221952.png](/img/user/Images/Pasted%20image%2020250419221952.png)

Now we use the `jwt_tool` with the `-X` to tamper with known vulnerabilities and `i` to inject inline JWKS. This basically means that it will inject its auto-generated RSA public key (in JWKS format) into the token header then It uses the  private key to sign the token

TLDR: Injcecting the Public Key in header (JWKS format), Private Key for signing

```shell
python3 jwt_tool.py [TOKEN] -X i -T 
```

One more thing we need to remember is the `kid` header needs to match the key's `kid` . By default, the tool names the key's `kid` :`jwt_tool` so we will tamper the header's kid and give it the same value.

![Pasted image 20250419223359.png](/img/user/Images/Pasted%20image%2020250419223359.png)

Now, we just change the `sub` parameter to `administrator` like usual and we should be good.

![Pasted image 20250419223506.png](/img/user/Images/Pasted%20image%2020250419223506.png)

Inspecting the token at jwt.io will reveal the same `kid` key

![Pasted image 20250419223650.png](/img/user/Images/Pasted%20image%2020250419223650.png)

Replace the session cookie and now we can access `/admin`

![Pasted image 20250419223803.png](/img/user/Images/Pasted%20image%2020250419223803.png)

