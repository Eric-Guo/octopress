---
layout: post
title: "Pow used .dev will be redirect to https by Google Chrome"
date: 2017-12-07T12:14:06+08:00
comments: true
external-url: 
categories:  [Rails]
---

After Chrome 63 released, [Google makes the decision to redirect .dev to https direct as they buy the domain TLD](https://ma.ttias.be/chrome-force-dev-domains-https-via-preloaded-hsts/).

So if you using [pow as well like me](/2017/04/20/using-pow-and-byebug-together/), you need to add below line to ~/.profile:

```
export POW_DOMAINS=localhost
```

And running below command and restart pow.

```bash
cd /etc/resolver/
sudo mv dev localhost
launchctl unload -w ~/Library/LaunchAgents/cx.pow.powd.plist
sudo launchctl unload -w /Library/LaunchDaemons/cx.pow.firewall.plist
sudo pow --install-system
pow --install-local
sudo launchctl load -w /Library/LaunchDaemons/cx.pow.firewall.plist
launchctl load -w ~/Library/LaunchAgents/cx.pow.powd.plist
```

So you can continue using http://faria.oa.localhost/ to access your local rails projects.

