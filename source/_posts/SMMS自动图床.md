---
title: markdown自动图床配置
description: 实现自动markdown文档图片云备份，云分享的功能
categories: markdown
series: markdown
abbrlink: 5b3244a8
---
# 1.注册SM.MS账号

只需要邮件即可注册，甚至不需要验证码，每个账户有5GB的免费空间

![image-20231128223511415](https://s2.loli.net/2023/11/28/IDjUporvWlNwm72.png)

创建一个文件上传的请求**Token**,用于Typora图片上传

![image-20231128223738992](https://s2.loli.net/2023/11/28/diZK3ORkNaEb9LX.png)

# 2.配置Typora文件上传插件

**文件-->偏好设置**

![image-20231128224107342](https://s2.loli.net/2023/11/28/4c3SEkwbijFDzPp.png)

![image-20231128224600702](https://s2.loli.net/2023/11/28/S7Nv5QiGfhJk8ax.png)

**第五步配置文件**

```json
{
  "picBed": {
    "current": "smms",
    "smms": {
      "token": "[在这输入在sm.ms中生成的api token]"
    }
  },
  "picgoPlugins": {}
}
```

依次选择后，进行验证如果验证失败也没关系，可能是由于网络原因，直接打开一个markdown文档进行插如图片看能否自动转换为图床url即可。

![image-20231128224728166](https://s2.loli.net/2023/11/28/SfuktwcdBDVmZGv.png)
![image-20231128233054878.png](https://s2.loli.net/2023/11/28/4moP9uck3yw8s6S.png)

这样就实现了当在markdown文档中插入图片后会自动将插入的图片上传自图床，实现了可直接分享md文档而不需要再提供本地图片了。
