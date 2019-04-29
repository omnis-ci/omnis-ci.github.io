---
title: Branding the Omnis runtime
summary: How to brand the Omnis runtime app as your app
rank: 4
author: aclay
date: 2019-04-28
---

{% assign img_root = "/images/guides/brand-omnis-runtime" %}

This guide explains how to change the icon, name, version, copyright, company and other identifying information in the Omnis runtime to match your app.

These instructions are written for Omnis Studio 10.0 or later but may be adapted to previous versions.

## Topics

* [Tools](#tools)
* [Icon: Requirements](#icon_requirements)
* [Icon: macOS](#icon_macos)
* [Icon: Windows](#icon_windows)
* [App Name, Copyright, and Version: macOS](#app_name_macos)
* [App Name, Copyright, and Version: Windows](#app_name_windows)

## <a id="tools"></a>Tools
Where possible this guide will use tools built-in to the operating system or free tools. However, customizing the Windows runtime requires a resource editor, and a commercial tool may be required to produce the desired results.

This guide will use [Resource Tuner 2](http://www.restuner.com), but an alternate resource editor may work.

If you want to automate these tasks then consider licensing [Resource Tuner Console](http://www.restuner.com/tour-resource-tuner-console.htm). Many of these steps can be programmed into a script that Resource Tuner Console can use to brand new Omnis runtimes.

## <a id="icon_requirements"></a>Icon Requirements
For both macOS and Windows you will need your application icon in a number of resolutions. You can use a single large icon and convert it to smaller sizes, or create individual sizes with varying levels of detail.

In all cases it's best to use a PNG file.

__Using a single file__
* 1024x1024

__Using individual files__
* 16x16
* 32x32
* 48x48 (Windows only)
* 64x64
* 128x128
* 256x256
* 512x512
* 1024x1024

<div class="callout">This guide will use a single large image and resize it the various required sizes.</div>

## <a id="icon_macos"></a>Icon: macOS
The application icon is managed through a `.icns` file which contains the icon in numerous sizes. There is a built-in `iconutil` command that can make a `.icns` file 
from a folder containing individual files at different sizes. This folder is called an __iconset__.

Start by creating the iconset folder as `omnis.iconset` on your Desktop using either the Finder or the Terminal:
```bash
mkdir ~/Desktop/omnis.iconset
```
![omnis.iconset]({{ img_root }}/omnis.iconset.png)

Put your icons in the `omnis.iconset` folder using these names and sizes:

| Name | Size |
|---|---|
| `icon_16x16.png` | 16x16 |
| `icon_16x16@2x.png` | 32x32 |
| `icon_32x32.png` | 32x32 |
| `icon_32x32@2x.png` | 64x64 |
| `icon_128x128.png` | 128x128 |
| `icon_128x128@2x.png` | 256x256 |
| `icon_256x256.png` | 256x256 |
| `icon_256x256@2x.png` | 512x512 |
| `icon_512x512.png` | 512x512 |
| `icon_512x512@2x.png` | 1024x1024 |

If using a single image, copy it into `omnis.iconset` as `icon_512x512@2x.png`. You can then run these commands in the terminal to create the additional sizes:
```bash
sips -Z 512 ~/Desktop/omnis.iconset/icon_512x512@2x.png --out ~/Desktop/omnis.iconset/icon_512x512.png
cp ~/Desktop/omnis.iconset/icon_512x512.png ~/Desktop/omnis.iconset/icon_256x256@2x.png
sips -Z 256 ~/Desktop/omnis.iconset/icon_512x512@2x.png --out ~/Desktop/omnis.iconset/icon_256x256.png
cp ~/Desktop/omnis.iconset/icon_256x256.png ~/Desktop/omnis.iconset/icon_128x128@2x.png
sips -Z 128 ~/Desktop/omnis.iconset/icon_512x512@2x.png --out ~/Desktop/omnis.iconset/icon_128x128.png
cp ~/Desktop/omnis.iconset/icon_128x128.png ~/Desktop/omnis.iconset/icon_64x64@2x.png
sips -Z 64 ~/Desktop/omnis.iconset/icon_512x512@2x.png --out ~/Desktop/omnis.iconset/icon_64x64.png
cp ~/Desktop/omnis.iconset/icon_64x64.png ~/Desktop/omnis.iconset/icon_32x32@2x.png
sips -Z 32 ~/Desktop/omnis.iconset/icon_512x512@2x.png --out ~/Desktop/omnis.iconset/icon_32x32.png
cp ~/Desktop/omnis.iconset/icon_32x32.png ~/Desktop/omnis.iconset/icon_16x16@2x.png
sips -Z 16 ~/Desktop/omnis.iconset/icon_512x512@2x.png --out ~/Desktop/omnis.iconset/icon_16x16.png
``` 

You should end up with a folder that looks like this:

![omnis.iconset with icons]({{ img_root }}/omnis.iconset_with_icons.png)

You can now make your `omnis.icns` file using the `iconutil` command:
```bash
iconutil -c icns ~/Desktop/omnis.iconset
```

This will produce a `omnis.icns` file you can use to brand your runtime.

![omnis.icns]({{ img_root }}/omnis.icns.png)

This file should be placed in your runtime under `Contents/Resources/omnis.icns` and will change the icon on the runtime.

![Omnis macOS runtime with icon]({{ img_root }}/omnis_macos_runtime_with_icon.png)

## <a id="icon_windows"></a>Icon: Windows
There are three locations on Windows that need the icon updated:
* `omnis.exe`
* `omnisdat.dll`
* Icon 2033 in your iconset

`omnis.exe` and `omnisdat.dll` need to be modified with Resource Tuner or another resource editor. These require a `.ico` file containing various sizes of your icon.

The iconset icons can simply be replaced on disk using the appropriately-sized PNG files.

An easy way build the `.ico` file is a web-based tool that resizes and packages a `.png` into a `.ico`, such as [icoconvert.com](https://icoconvert.com). To use icoconvert.com upload a PNG of your icon that is at least 256x256.

![Upload image to icoconvert.com]({{ img_root }}/icoconvert_upload_image.png)

Under the icon format choose **Custom sizes**, then **Multi-size in one icon**. Check these sizes:
* 16x16
* 32x32
* 48x48
* 64x64
* 256x256

![Icon format choices on icoconvert.com]({{ img_root }}/icoconvert_select_format.png)

Click the **Convert ICO** button, then download your icon.

To apply the icons, open `omnis.exe` with Resource Tuner. Navigate to **Icon Group**, then **5: English (United Kingdom)**. Right-click on this entry and choose **Resource Tools** then **Add or Replace Icon within Icon Group**.

![Add or Replace Icon within Icon Group in Resource Tuner]({{ img_root }}/resource_tuner_add_or_replace_icon.png)

Click **Open** on the dialog that appears and select the `.ico` file you downloaded. Choose the **Swap all** option and click **OK**.

![Loading the ico in Resource Tuner]({{ img_root }}/resource_tuner_load_ico.png)

Save your changes in Resource Tuner.

<div class="callout">When making changes to `.exe` and `.dll` files with Resource Tuner, Windows Explorer doesn't always immediately refresh the file icon. You can force a refresh by dragging the file to another location, then back to its original location.</div>

![omnis.exe]({{ img_root }}/omnis.exe.png)

Repeat these steps in Resource Tuner to change the icons in `omnisdat.dll`. If you are deploying both 32-bit and 64-bit runtimes, you will need to apply these steps to these two files for each architecture.

The final icon to customize is the one appearing in the top left corner of the Omnis application window. This is managed with the Omnis iconsets introduced in Studio 8.0 through the icon with ID 2033.

To customize this icon you need to replace these PNG files in the studio iconset with files containing your icon:

| Name | Size |
|---|---|
| `omnis_2033_16x16.png` | 16x16 |
| `omnis_2033_16x16_2x.png` | 32x32 |
| `omnis_2033_32x32.png` | 32x32 |
| `omnis_2033_32x32_2x.png` | 64x64 |
| `omnis_2033_48x48.png` | 48x48 |
| `omnis_2033_48x48_2x.png` | 96x96 |

## <a id="app_name_macos"></a>App Name, Copyright, and Version: macOS

There are four places to rename the Omnis runtime to have your app's name, company, copyright, and version:

* The `.app` bundle
* The main `Info.plist` file
* The `InfoPlist.strings` file for each bundled language
* The `Localizable.strings` file for each bundled language

### App bundle
Rename the app bundle to have your application's name by as changing the application name in the Finder.

![Rename the app bundle]({{ img_root }}/rename-app-bundle.gif)

### Info.plist and InfoPlist.strings
The `Info.plist` file is found inside the app bundle at `Contents/Info.plist` and can be edited with a text editor or Xcode. There is an `InfoPlist.strings` file in `Contents/Resources/[language].lproj/` for each language you support in your application. For English, the path is `Contents/Resources/English.lproj/InfoPlist.strings`. For French, it would be `Contents/Resources/French.lproj/InfoPlist.strings`, and so on.

You must retain the primary `Info.plist` file and folders for each language you wish to support. If you do not plan to support all included languages, you can remove the corresponding folder from `Contents/Resources`. You should always retain `English.lproj` since that is the primary language used when developing Omnis Studio.

Each of these files contains one or more keys you need to adjust to contain your application's branding. The keys listed in this table link to Apple's documentation on that specific key.

| Key | Purpose |
|---|-------|
| [`CFBundleGetInfoString`](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-112854-TPXREF117) | A string with the copyright notice for the bundle. Apple replaces this key with `NSHumanReadableCopyright`, but Omnis still uses `CFBundleGetInfoString` |
| [`CFBundleIdentifier`](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/20001431-102070) | A unique identifier for your app used by the OS (\*see note below) |
| [`CFBundleName`](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-109585) | The short name of the bundle, less than 16 characters |
| [`CFBundleShortVersionString`](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/20001431-111349) | Your app's version followed by the copyright |
| [`CFBundleVersion`](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/20001431-102364) | Your app's version (\*\* see note below) |

\* `CFBundleIdentifier` should be formated as a reverse DNS string identifying your company and the bundle. For example, an app called **Coffee Picker** from **Acme, Inc.** might have a bundle identifer of:

```
com.acme.coffee-picker
```

Note the bundle identifier should only include alphanumeric (A-Z,a-z,0-9), hyphen (-), and period (.) characters per [Apple's specifications](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/20001431-102070).

\*\* `CFBundleVersion` uses three non-negative, period-separated integers with the first integer being greater than zero—for example, 3.1.2. See [Apple's documentation](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference) for details on the meaning of each number.

### Localizable.strings
This file is located at `Contents/Resources/English.lproj/Localizable.strings` and can be edited with a text editor or Xcode. Adjust these entries:

| Entry | Value |
|---|-------|
| `CORE_RES_1` | Application name |
| `CORE_RES_577` | Application name. This affects the application menu entries |
| `CORE_RES_25599` | The folder under `~/Library/Application Support` where your <a href="/guides/omnis-installation-locations.html#application_data_directory">application data</a> will be stored |

## <a id="app_name_windows"></a>App Name, Copyright, and Version: Windows

There are two locations to customize the Windows runtime with your application name:

* `omnis.exe`
* `omnisdat.dll`

### omnis.exe
Rename the `omnis.exe` file to use your app's name. This can be accomplished in Windows File Explorer.

![Rename omnis.exe]({{ img_root }}/rename-omnis.exe.gif)

Edit your exe file with a resource editor like **Resource Tuner**. There are several entries to edit with your application name under Version 1:

| String | Value |
|-----|-----|
| `FileVersion` | Your version formatted with 4 numbers. Use a 0 for extra numbers, such as 3.1.0.0 for version 3.1 |
| `ProductVersion` | Your version without extra formatting |
| `CompanyName` | Your company's name |
| `FileDescription` | Your application's name followed by the architecture and "core executable" |
| `LegalCopyright` | Your copyright |
| `ProductName` | Your application's name |

Also set the **File Version** and **Product Version** data. In **Resource Tuner** this is available after selecting *Version 1* in the lower left-hand corner under **Object Inspector**. These values should match the `FileVersion` and `ProductVersion` strings.

![Customizing omnis.exe]({{ img_root }}/customize-omnis.exe.png)

### omnisdat.dll

You need to rename the `omnisdat.dll` file using a specific convention based on your custom name for `omnis.exe`. Use the first 8 characters from your renamed `omnis.exe` file name, then add `dat.dll` to get the new name for `omnisdat.dll`.

For example, if you rename `omnis.exe` to `Coffee Picker.dll` change the `omnisdat.dll` file to `Coffee Pdat.dll`.

Edit your renamed `omnisdat.dll` file with a resource editor like **Resource Tuner**. There are two entries to change under String Table.

| String Table | ID | Value |
|----|--|----|
| 1 | 1 | Your application's name |
| 32 | 499 | The folder under `%LOCALAPPDATA%` where your <a href="/guides/omnis-installation-locations.html#application_data_directory">application data</a> will be stored |