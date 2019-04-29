---
title: Customizing the Omnis runtime
summary: How to customize the Omnis runtime app for your app
rank: 5
author: aclay
date: 2019-04-28
---

{% assign img_root = "/images/guides/customize-omnis-runtime" %}

The Omnis Runtime includes every component available with Omnis. In order to save space and complexity, you may want to remove some components. This guide will explain which components are good candidates for removal if you don't need their functionality.

This guide also discusses where to add your library(s), iconset(s), htmlcontrols, and supporting files.

This guide will also address how to add support for SSL-encrypted PostgreSQL connections.

## Topics

* [Stock Components](#stock_components)
* [Files for your app](#your_app)
* [PostgreSQL library for SSL](#postgresql_library_for_ssl)

## <a id="stock_components"></a>Stock components

If you don't need these files consider removing them from your installation.

| File | Purpose | When to keep |
|--|--|
| _CodeSignature (macOS only) | The code signature for the stock Omnis Runtime | Remove since you are customizing Omnis. Replace with your own code signature  |
| firstruninstall/local/locomnis.pdf | Documentation on how to localize Omnis | Remove after customizing `omnisloc.df1` |
| firstruninstall/local/omnisloc.lbs | A library used to customization `omnisloc.df1` | Remove after customizing `omnisloc.df1` |
| firstruninstall/python | A python runtime that powers the OmnisPDF device | Remove if you don't need the OmnisPDF device |
| firstruninstall/help.lbs | The Omnis help library | Unclear |
| firstruninstall/omsqlconv.lbs | Converts OmnisSQL DML commands to SQL | If you use OmnisSQL DML |
| firstruninstall/remotedebug.lbs | Provides a **RemoteDebug** menu to configure a remote debugging client | Can be removed if you don't need remote debugging or want to build your own config. Removing this file does not disable the core remote debugging functionality |
| omdotnet.dll (Windows only) | Add an interface from Omnis to .NET components | Remove if you don't need this interface |
| uninstall and uninstall.exe (Windows only) | Uninstalls the Omnis runtime | Replace this with your own uninstaller |
| Resources/\*.lproj (macOS only) | Contains branding information for various languages | Remove languages you don't support, although `English.lproj` should always be present |
| xcomp/\* | Contains xcomps adding various functionality | Remove xcomps you don't need, such as those for DAMs you don't use or specialized visual controls like tile or marquee |

## <a id="your_app"></a>Files for your app

A minimum you should add your library or libraries to `firstruninstall/startup/`. This will launch your code when the app is launched. Be sure to read the guide on updating the [Application Data Directory](/guides/omnis-installation-locations.html#application_data_directory) to understand how the library file is accessed when you app is installed.

You can also add files to the [program](/guides/omnis-installation-locations.html#program_directory) and [application data](/guides/omnis-installation-locations.html#application_data_directory) directories, such as string tables, embedded binaries, and database templates. Put read-only files in the program directory and read/write files in application data.

If you have custom icons, add them to `iconsets/[your iconset name]` under the program directory if using iconsets or `firstruninstall/icons/userpic.df1` if using old-style embedded icons.

Custom xcomps can be placed in two different locations. Xcomps that do not change frequently can be installed under the program directory in the `xcomps/` directory. You can also place xcomps under the application data directory in an `xcomps/` folder. If your xcomps change frequently and you use a built-in automatic updater that doesn't always update the program data files, placing xcomps in the application data folder may be a better option.

## <a id="postgresql_library_for_ssl"></a>PostgreSQL library for SSL

PostgreSQL is a popular database backend choice for Omnis developers, and Omnis offers a Postgres DAM that makes working with PostgreSQL from Omnis easy.

Unfortunately, the stock Omnis installation does not support encrypted PostgreSQL connections using SSL. With a few changes to the Omnis installation, however, you can easily add SSL support to your PostgreSQL connections.

### SSL PostgreSQL connections on macOS
You will need these three libraries, compiled to support PostgreSQL connections over SSL:
* libcrypto.1.0.0.dylib
* libpq.dll
* libssl.1.0.0.dylib

Compiling these libraries from source is outside the scope of this guide. Instead, you can grab the latest [pre-compiled PostgreSQL binaries](https://www.enterprisedb.com/download-postgresql-binaries) from EnterpriseDB and pull these `.dylib` libraries the `pgsql/lib` directory.

![Libs from the psql binaries]({{ img_root }}/libs-from-psql.gif)

Copy these three libraries to `Contents/Frameworks`.

You need to change how these libraries are linked so they find each other in the Omnis runtime. Use these commands to make that change, updating the first command to the path to your specific application:
```bash
cd [path to your runtime].app/Contents/Frameworks
install_name_tool -change @loader_path/../lib/libcrypto.1.0.0.dylib @rpath/libcrypto.1.0.0.dylib libssl.1.0.0.dylib
install_name_tool -change @loader_path/../lib/libssl.1.0.0.dylib @rpath/libssl.1.0.0.dylib libpq.dylib
install_name_tool -change @loader_path/../lib/libcrypto.1.0.0.dylib @rpath/libcrypto.1.0.0.dylib libpq.dylib
```
<div class="callout">These commands require Xcode to be installed. You can get Xcode for free from the Mac App Store.</div>

### SSL PostgreSQL connections on Windows
Your will need these four libraries, compiled to support PostgreSQL connections over SSL:
* libeay32.dll
* libintl-9.dll (this may have a different number depending on which PostgreSQL binaries you download)
* libpq.dll
* ssleay32.dll

Compiling these libraries from source is outside the scope of this guide. Instead, you can grab the latest [pre-compiled PostgreSQL binaries](https://www.enterprisedb.com/download-postgresql-binaries) from EnterpriseDB and pull these `.dll` libraries from the `pgsql\bin` directory.

![DLLs from the psql binaries]({{ img_root }}/dlls-from-psql.gif)

Copy these `.dll` files to the program data directory of your Omnis application. You will also need to ensure the [Visual C++ Redistributable for Visual Studio 2015](https://www.microsoft.com/en-us/download/details.aspx?id=48145) is installed with your application.

### Making an SSL connection

After loading the libraries, you can use the `sslmode=require` option when logging on to your PostgreSQL database to enable SSL on the connection. Note that your server also needs to support SSL connections. Refer to [the PostgreSQL docs](https://www.postgresql.org/docs/current/ssl-tcp.html) for how to enable this configuration.