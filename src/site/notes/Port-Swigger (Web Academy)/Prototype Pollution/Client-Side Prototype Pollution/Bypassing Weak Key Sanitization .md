---
{"dg-publish":true,"permalink":"/port-swigger-web-academy/prototype-pollution/client-side-prototype-pollution/bypassing-weak-key-sanitization/"}
---

Instead of the normal exploit:

```
/?__proto__.gadget=payload
```

If the sanitization process only strips the string `__proto__` only once and not recursively

xsxsxsxznxjbx