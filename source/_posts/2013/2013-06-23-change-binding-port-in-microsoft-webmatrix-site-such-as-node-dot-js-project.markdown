---
layout: post
title: "Change binding port in Microsoft WebMatrix Site such as node.js project"
date: 2013-06-23T16:34:52+08:00
comments: true
external-url:
categories: [Node.js, IDE, Windows7]
---

Microsoft now is become quit outdated technology after they [release so many outdated technology](http://www.drdobbs.com/windows/225701475), but WebMatrix is in my opinion, the best tools to right node.js tools in Windows.

But I'm used to using port 3000 as default debug port since I'm switch to Ruby & Rails, so really not want to change port time to time, so here is how to change binding port.

```xml D:\EricDocuments\IISExpress\config\applicationhost.config
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.applicationHost>
        <sites>
            <site name="firstNode" id="1">
                <application path="/" applicationPool="UnmanagedClassicAppPool">
                    <virtualDirectory path="/" physicalPath="D:\EricDocuments\My Web Sites\firstNode" />
                </application>
                <bindings>
                    <binding protocol="http" bindingInformation="*:3000:localhost" />
                </bindings>
            </site>
        </sites>
    </system.applicationHost>
</configuration>
```

Just find the your node.js project entry in applicationhost.config and change port.