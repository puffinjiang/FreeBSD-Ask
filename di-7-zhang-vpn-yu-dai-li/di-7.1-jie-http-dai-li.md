# 第 7.1 节 HTTP 代理

示例：V2ray 或 clash 开启允许局域网连接。然后按照具体配置可能如下：

首先查看使用的 shell
```
$ echo $SHELL
```

- 如果使用的是 `csh`

设置：
```
# setenv http_proxy http://192.168.X.X:7890
```

取消：
```
# unsetenv http_proxy
```

- 如果使用的是 `sh`, `bash`, `zsh`
  
**环境变量 HTTP_PROXY 一定要是大写！小写不生效！**
  
设置：
```
# export HTTP_PROXY=http://192.168.X.X:7890
```

取消：
```
# unset HTTP_PROXY
```
