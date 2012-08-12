---
layout: post
title: "Install simple Node.js based TCP socket proxy server in Windows by Batch File"
date: 2012-04-22 17:37
comments: true
external-url:
categories: [Node.JS, Proxy]
---
In my working Semiconductor factory, some of the machine is heavily protected by network isolate (You can image those machine a very frangible PC which have no windows update turn on), the network is so isolate that only certain bridge PC can connect in both inside intranet and outside - still intranet. To make things even worse, the isolated network is nested; the real machine need cross two level bridge PC to reach the application server.

That's why we need many proxy server to relay the network between Client/Server<!--more-->, we need a TCP proxy server as the application protocol is based on TCP, if we didn't use proxy server, another option maybe port redirection, quite typical feature/usage in Linux via iptables, but the bridge server is running Windows, so finally, I still turn to the proxy server approach.

I research many different program in Windows, I have to say <a href="http://delog.wordpress.com/2011/04/08/a-simple-tcp-proxy-in-node-js/" target="_blank">using Node.js to write a tcp socket proxy</a> is coolest and here is my batch file (consider the number of installation is large and no ruby/python runtime, batch file is fittest) to install that service in bridge PC:

```bat map port 9999 to cvpcimap06:9999 windows batch file
cd
echo This program will install proxyCIM, please make sure the folder is SocketProxyServer
echo If not please run it in command line with administrator priviledge
pause
msiexec /i node-v0.6.15.msi /quiet /passive
copy nssm.exe %windir%\\par copy tcpproxy.js %windir%\\par net stop proxyCIM
%windir%\nssm remove proxyCIM confirm
%windir%\nssm install proxyCIM "%ProgramFiles%\nodejs\node.exe" %windir%\tcpproxy.js 9999 cvpcimap06 9999
net start proxyCIM
```

nssm.exe is the <a href="http://nssm.cc/" target="_blank">Non-Sucking Service Manager</a>, tcpproxy.js just the core JavaScript file which turn the <a href="http://nodejs.org/" target="_blank">Node.js</a> into a TCP socket proxy:

```javascript tcpproxy.js
var util = require('util');
var net = require("net");

process.on("uncaughtException", function(e) {
	console.log(e);
});

if (process.argv.length != 5) {
  console.log("Require the following command line arguments:" +
    " proxy_port service_host service_port");
    console.log(" e.g. 9001 www.google.com 80");
  process.exit();
}

var proxyPort = process.argv[2];
var serviceHost = process.argv[3];
var servicePort = process.argv[4];

net.createServer(function (proxySocket) {
  var connected = false;
  var buffers = new Array();
  var serviceSocket = new net.Socket();
  serviceSocket.connect(parseInt(servicePort), serviceHost, function() {
    connected = true;
    if (buffers.length > 0) {
      for (i = 0; i < buffers.length; i++) {
        console.log(buffers[i]);
        serviceSocket.write(buffers[i]);
      }
    }
  });
  proxySocket.on("error", function (e) {
    serviceSocket.end();
  });
  serviceSocket.on("error", function (e) {
    console.log("Could not connect to service at host "
      + serviceHost + ', port ' + servicePort);
    proxySocket.end();
  });
  proxySocket.on("data", function (data) {
    if (connected) {
      serviceSocket.write(data);
    } else {
      buffers[buffers.length] = data;
    }
  });
  serviceSocket.on("data", function(data) {
    proxySocket.write(data);
  });
  proxySocket.on("close", function(had_error) {
    serviceSocket.end();
  });
  serviceSocket.on("close", function(had_error) {
    proxySocket.end();
  });
}).listen(proxyPort);
```
