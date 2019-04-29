---
title: Omnis installation locations
summary: A guide to the locations used by the Omnis runtime
rank: 3
author: aclay
date: 2019-04-28
---

{% assign img_root = "/images/guides/omnis-installation-locations" %}

Omnis is installed in two locations—the read-only **program directory** and the writeable **application data directory**. Each directory serves a specific purpose, and understanding how each directory is created and maintained is critical to managing an Omnis installation.

## Topics

* [Program Directory](#program_directory)
* [Application Data Directory](#application_data_directory)
* [Single Location Alternative](#single_location_alternative)
 
## <a id="program_directory"></a>Program Directory

| OS | Standard Location |
|---|---|
| macOS | `/Applications/[your app name].app/` |
| Windows | `%PROGRAMFILES%\[your company]\your app name\` |

This location is available programmatically in Omnis:
```omnis
Calculate lcProgramDirectoryPath as sys(215)
```

The program directory contains the Omnis executable, supporting resources like html controls and iconsets, and, by default, xcomps. These files are installed once per computer and are not modified when Omnis is run.

The program directory is usually created and updated when the user runs your application installer.

To access the program directory on macOS, right-click the app bundle and choose **Show Package Contents**, then open the `Contents` folder. Most files you'll need to work with are located under `MacOS`, although some are under `Frameworks` or `Resources`.

## <a id="application_data_directory"></a>Application Data Directory

| OS | Standard Location |
|---|---|
| macOS | `${HOME}/Library/Application Support/[your company id]/[your app name]/` |
| Windows | `%LOCALAPPDATA%\[your company]\your app name\` |

This location is available programmatically in Omnis:
```omnis
Calculate lcApplicationDataDirectoryPath as sys(115)
```

Omnis requires read-write access to a number of files. Modern versions of macOS and Windows do not grant applications read-write access to files in `/Applications` or `%PROGRAMFILES%` for security reasons. As such, Omnis consolidates all files requiring read-write access into a directory placed within the user's home folder. This includes your application's library files.

Application data is stored a minimum of two times on the disk where Omnis installed. A master copy of this data is located in the **Program Directory** under `firstruninstall`. A copy is also placed in the location listed above __for each user__ on the computer who runs the application.

The first time a user launches your app, Omnis will automatically copy the `firstruninstall` contents to the standard **Application Data Directory** location. This can cause a small delay and one needs to be run once. Once this data is copied, Omnis will use the existing data in the **Application Data Directory** on subsequent launches.

This replication of application data makes managing the application data directory a challenge especially in multi-user environment like Windows Terminal Services or a shared desktop or laptop. After installing a new version of your application with updated library files in `firstruninstall`, Omnis will continue to use the old library file(s) from the **Application Data Directory**, and your user won't be running the updated application. 

You need to clean out the **Application Data Directory** folder when updating your application to ensure your users have the new version. If you are in a multi-user installation, you must clean this folder __for each user__ who uses your application.

Therefore, it is recommend to have your installer clean and replace the application data directory with the contents from `firstruninstall` to "seed" a new installation with current application data. This will ensure any old data is removed and reduce the delay when launching Omnis.

Beyond that, it is also recommend to add a system by which old copies of the application data will be replaced with current copies when a user launches the app. See the guide on [Updating the Omnis runtime](/guides/updating-omnis-runtime.html) for a further discussion and solution to this problem.

## <a id="single_location_alternative"></a>Single Location Alternative
It is highly recommended you use the split program/application data installation model as it complies with operating system standards and allows applications to be code signed. However, you may wish to forego the split installation and run your application from a single, non-standard location to simplify deployment. This isn't recommended, but may be a good option for a controlled environment.

To bypass the split installation, install the contents of `firstruninstall` one level higher, into the **program directory**. On macOS, this means into `/Applications/[your app name].app/Contents/MacOS`.

Install and run this app from a location that does not restrict read/write access, such as `${HOME}/Applications` on macOS or `%USERPROFILE%\Programs` on Windows.