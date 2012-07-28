---
layout: post
title: "Install Android SDK Drivers on HTC Incredible S/G11"
date: 2012-07-28 13:58
comments: true
external-url:
categories: [Android, Mobile]
---
I use the Windows 7 and after you install Android SDK Tools, you can locate the USB driver at [C:\Program Files\Android\android-sdk\extras\google\usb_driver](file://C:\Program Files\Android\android-sdk\extras\google\usb_driver)

You need to modify the file android_winusb.inf (may need to save to Desktop and move to original folder due to Windows 7 protection), Add below line after section [Google.NTx86]

    ;HTC Incredible
    %SingleAdbInterface%        = USB_Install, USB\VID_0BB4&PID_0CAC
    %CompositeAdbInterface%     = USB_Install, USB\VID_0BB4&PID_0CAC&MI_01

If you use the x64 version, Also add below line after [Google.NTamd64]

    ;HTC Incredible
    %SingleAdbInterface%        = USB_Install, USB\VID_0BB4&PID_0CAC
    %CompositeAdbInterface%     = USB_Install, USB\VID_0BB4&PID_0CAC&MI_01

After you modify the file, you can in Device Manager and update the Andriod Device driver by manually click the button "Update Driver Software" and point to the folder of android_winusb.inf by install it.

