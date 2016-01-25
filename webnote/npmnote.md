##npmnote
###配置淘宝镜像

1.通过config命令

```shell
npm config set registry https://registry.npm.taobao.org  
npm info underscore （如果上面配置正确这个命令会有字符串response）
```

2.编辑 ~/.npmrc 加入下面内容

registry = https://registry.npm.taobao.org