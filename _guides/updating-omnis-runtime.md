---
title: Updating the Omnis runtime
summary: How to update your Omnis runtime app to a new version
rank: 7
author: aclay
date: 2019-04-28
---

{% assign img_root = "/images/guides/updating-omnis-runtime" %}

Because Omnis uses a [split installation](/guides/omnis-installation-locations.html) model, updating your application can be more complicated than simply installing a new version.

When installing a new version, several requirements must be met:
 1. Files in [program directory](/guides/omnis-installation-locations.html#program_directory) must be replaced with new ones
 1. Files in the [application data directory](/guides/omnis-installation-locations.html#application_data_directory) must be updated **for each user** using the application
 1. On Windows, the version registered in Windows Installer should be updated

## Topics

* [Updating the program directory](#program_directory)
* [Updating the application data directory](#application_data)
* [Updating Windows Installer](#windows_installer)

## <a id="program_directory"></a>Updating the program directory
The simplest way to update files in the program directory is to [build an installer](/guides/installing-omnis-runtime.html) with new files, then run it. However, you may want to create a more optimized system that only changes the files that change frequently, such as your libraries and string tables. It's possible to build this solution directly in Omnis, but such a solution is outside the scope of this guide.

## <a id="application_data"></a>Updating the application data directory
Once the files in `firstruninstall/` under the program directory are changed, you need to replace those files in the application data directory with current ones for each user.

If using one of the installer templates listed in the [Installing the Omnis runtime](/guides/installing-omnis-runtime.html) guide, the application data directory for the user running the installer will be cleared and replaced with fresh files from `firstruninstall/` by the installer.

However, you may be operating in a multi-user environment when multiple users share the same application. This may be as simple as a laptop with two macOS or Windows user accounts, or as massive as an enterprise-level Microsoft Terminal Services environment hosting dozens or hundred of users.

<div class="callout">Consider a laptop with two users, Alice and Bob. Both run the Omnis app "Coffee Picker":
<ol>
 <li>Alice installs Coffee Picker 1.0 and does some work</li>
 <li>Later, Bob logs on a fires up Coffee Picker
 	<ol>
 		<li>Omnis copies version 1.0 to Bob's application data directory</li>
 		<li>Bob gets version 1.0 with the app comes up</li>
 		<li>Bob and Alice both work in Coffee Picker 1.0 without a problem</li></ol></li>
</ol>

<p>Two weeks later, Alice receives a notice that Coffee Picker 2.0 is available.</p>
<ol start="3">
 <li>Alice downloads and runs the installer for Coffee Picker 2.0:
 	<ol>
	 <li>The installer updates her program directory, then clears and pre-populates her application data directory with the version 2.0 files</li>
	 <li>Alice opens Coffee Picker and works in version 2.0</li>
	</ol></li>
<li>Later that day, Bob logs on to the laptop and fires up Coffee Picker
 	<ol>
	 <li>Even though the program data is version 2.0, version 1.0 is still installed in his application data directory</li>
	 <li>Bob fires up Coffee Picker and gets version 1.0 instead of 2.0</li>
	</ol></li>
</ol>
</div>
![Application data update example]({{ img_root }}/app-data-update-chart.png)

If you are working in a multi-user environment, using an in-app updater, or using an installer solution that doesn't reset the application data directory, you might consider the [app-loader](https://github.com/suransys/app-loader) project from Suran Systems.

`app_loader.lbs` will compare a simple build number between the program directory and the application data directory. If you update this build number each time you release a new version of your app, `app_loader.lbs` will ensure the current version is loaded each time a user launches your app.

You can also manually trigger an update from within Omnis. This works extremely well with a built-in updater to provide a "live" update to your application, updating your libraries to the current version without ever exiting Omnis.

If you don't want to use the `app_loader.lbs` tool, you will need to find another mechanism to ensure **ALL** copies of your application data directory are updated when installing a new version of the app.

## <a id="windows_installer"></a>Updating Windows Installer
When a program is installed on Windows using Window Installer, the registry is updated to track that program. This tracking allows Windows to provide a list of installed programs under the **Apps & Features** control panel. Many installers, including Advanced Installer, can register an uninstaller with the program, thereby allowing the user to trigger an uninstall from this control panel.

Windows Installer tracks two key identifiers for each installed product—the `ProductCode` and the `UpgradeCode`.

### UpgradeCode
The `UpgradeCode` is a unique identifier for your application regardless of version. This allows Windows to know that version 3.1 and 3.4.5 are the same product. The `UpgradeCode` should not change during the lifespan of an application unless you are releasing an independent version of the app.

### ProductCode
The `ProductCode` is a unique identifier for your application at a specific version. Windows Installer can use the `UpgradeCode` and `ProductCode` to detect if an update to your application is being installed and ask how the user wants to proceed.

When updating your Omnis application, ensure a new `ProductCode` is generated, especially if the version number changes. An in-app update that does not run an installer might not change the `ProductCode`, but this can create a disconnect between the version installed and the version registered. In practice, this disconnect does not adversely affect the user, but can cause confusion and support tickets, hence it should be mentioned.