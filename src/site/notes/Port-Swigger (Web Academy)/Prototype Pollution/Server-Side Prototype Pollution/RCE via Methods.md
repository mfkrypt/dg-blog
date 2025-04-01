---
{"dg-publish":true,"permalink":"/port-swigger-web-academy/prototype-pollution/server-side-prototype-pollution/rce-via-methods/"}
---


---

Methods such as `child_process.spawn()` and `child_process.fork()` enable developers to create new Node subprocesses. The `fork()` method accepts an options object in which one of the potential options is the `execArgv` property.

As this gadget lets you directly control the command-line arguments, this gives you access to some attack vectors that wouldn't be possible using `NODE_OPTIONS`. Of particular interest is the `--eval` argument, which enables you to pass in arbitrary JavaScript that will be executed by the `child_process`.

```json
"execArgv": [ "--eval=require('<module>')" ]
```

In addition to `fork()`, the `child_process` module contains the `execSync()` method, which executes an arbitrary string as a system command. By chaining these JavaScript and command injection sinks, we can probably gain full RCE capability on the server.