---
title: Installing the Omnis runtime
summary: How to install your Omnis runtime app
rank: 6
author: aclay
date: 2019-04-28
---

{% assign img_root = "/images/guides/installing-omnis-runtime" %}

Once you've [branded](/guides/brand-omnis-runtime.html) and [customized](/guides/customize-omnis-runtime.html) your Omnis app, the next step is to package it in an installer.

## Topics

* [Installing on macOS](#macos)
* [Installing on Windows](#windows)
* [Code Signing](#code_signing)

## <a id="mac"></a>MacOS
Omnis can be installed via two methods on macOS. You can bundle your entire app on a .dmg that users can drag to their **Applications** folder. You can also create an installer that installs your app and performs additional installation steps.

While the app bundle approach is simpler to install, the installer route allows for greater customization and helps alleviate upgrade issue by ensuring the [application data directory](/guides/omnis-installation-locations.html#application_data_directory) is cleaned on each install.

You can create an installer for macOS using [Packages](http://s.sudre.free.fr/Software/Packages/about.html). There is a [template](https://github.com/barkingfoodog/omnis-pkg) available on GitHub with instructions for how to build an installer for your Omnis app.

## <a id="windows"></a>Windows
Omnis is best installed using an installer on Windows. There are numerous free and paid products that can create a standard `.msi` installer. Consider your requirements before selecting an installer product:

 1. Do you need a single installer for both 32-bit (x86) and 64-bit (x64) platforms?
 1. Do you need to produce a plain `.msi` for enterprise deployment or can you provide a `.exe` installer that wraps the `.msi`?
 1. Does your installer need to install the [Visual C++ Redistributable for Visual Studio 2015](https://www.microsoft.com/en-us/download/details.aspx?id=48145) pre-requisite for Omnis, or will that be installed separately?
 1. Do you need to code-sign your installer to offer peace-of-mind and/or meet compliance requirements from your users?

If you want a user-friendly solution that installs both 32 and 64-bit versions from one file, installs the pre-requisites and offers code signing, consider this [template](https://github.com/barkingfoodog/omnis-aip) that uses Capyhon's [Advanced Installer](https://www.advancedinstaller.com/) to create a user-friendly installer for your app.

## <a id="code_signing"></a>Code Signing
Code Signing offers security and compliance with modern operarting systems. Code signing is not free, but the professional deployment and peace-of-mind it offers users can be well worth the cost.

There are notes on how to code sign your installer on the templates for a [macOS installer](https://github.com/barkingfoodog/omnis-pkg#code-signing) and a [Windows installer](https://github.com/barkingfoodog/omnis-aip#code-signing).

If you use any kind of in-place update system that replaces parts of the Omnis application it is recommended to sign the installer and not code sign the `.app` bundle on macOS.