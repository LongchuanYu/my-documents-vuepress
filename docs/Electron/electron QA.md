## node-pty报错：Cannot find module ...
#### [ 描述 ]
使用electron调试包含node-pty的项目，报错：
```
innerError Error: Cannot find module '../build/Debug/pty.node'
```

#### [ 解决方案 ]
`var pty = require('node-pty')`这句话不要写在渲染进程，写到主进程去！

