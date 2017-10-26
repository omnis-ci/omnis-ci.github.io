---
title: Sharing Omnis libraries on GitHub
---
# Sharing Omnis libraries on GitHub
[GitHub.com](https://github.com) is the premier tool for sharing source code. Using the JSON export tool in [Omnis Studio 8.1](http://www.omnis.net/products/studio/index.jsp), Omnis developers can easily share their code on GitHub for other developers to discover, use, and improve.

This guide will walk you through the complete process to share an Omnis library on GitHub.

### Prerequisites
* An Omnis library to share
* Omnis Studio 8.1 or later
* macOS or Windows

## Overview
There are several phases to sharing an Omnis Library with GitHub. This guide will cover all of them, but several steps only need to be performed one per machine or project.

* **Phase 1**: Setup your computer for GitHub development (once per machine)
* **Phase 2**: Setup a repository for your project (once per project)
* **Phase 3**: Add your library to the repository and publish it (once per project)
* **Phase 4**: Publish changes to your repository (as needed)

### Public versus private repositories
<div class="callout">
This guide will help you create a public repository on GitHub. Anyone can view your source and download your library from a public repository. GitHub also provides private repositories that limit access to your code. Private repositories require a paid subscription and are outside the scope of this guide.
</div>

## Phase 1: Setup your computer for GitHub development

### Create a GitHub account
If you don't already have a GitHub account, head over to [https://github.com/join](https://github.com/join) to create a new account on GitHub.

<div class="callout">
Even if you plan to use GitHub for an organization or company, you still need a personal account. You can create your organization or company later.
</div>

Enter a username, email address, and password, then click __Create an account__.

![Join GitHub](/images/join_github.png)

GitHub will send you a verification email. Follow the steps in that email to complete verification.

GitHub will ask you to customize your plan and tailor your experience, but these steps are optional. Complete them if you'd like, or move to the step step.

### Install GitHub Desktop
GitHub Desktop is a user-friendly graphical app for working with GitHub repositories. Download and install it from [https://desktop.github.com](https://desktop.github.com).

Launch GitHub Desktop and choose **Sign into GitHub.com**.

![GitHub Desktop Splash](/images/github_desktop_splash.png)

Enter your GitHub user name and password, then click **Sign In**.

![Sign into GitHub Desktop](/images/sign_into_github_desktop.png)

Configure the name and email address that will appear with any changes you commit.

![Configure GitHub Desktop](/images/configure_github_desktop.png)

Click **Continue** and complete the setup.

## Phase 2: Setup a repository for your project

### Create a repository
GitHub organizes projects into repositories, similar to a project in the Omnis VCS. Open GitHub Desktop and go to **File** -> **New Repository**. Fill out the form as follows:

| Field | Value |
|---|---|
| Name | The name for your repository. Try using your library's name. GitHub's convention is use dashes for spaces |
| Description | A brief description of this library |
| Local Path | The parent folder that will contain your repository |
| Initialize this project with a README | Checked |
| Git Ignore | None |
| Licence | Choose an appropriate license if you'd like. See [choosealicense.com](https://choosealicense.com) for details on each license |

When finished the form should look something like this:

<img src="/images/create_repository_in_github_desktop.png" alt="Create repository in GitHub Desktop" class="reduced standard">

Click **Create Repository**.

### Configure your repository
In GitHub Desktop, choose **Repository** -> **Show in Finder** on macOS or **Repository** -> **Show in Explorer** on Windows.

Add these folders:
```
lib/
src/
```

Copy or move your Omnis library into `lib/`.

Your folder should look something like this:

![Repository folder](/images/repository_folder.png)

### Configure the README
The **README.md** file is a great place to provide instructions for using your library. When a visitor views your project on GitHub, the contents of this file are displayed on the main page for the project. 

**README.md** uses the Markdown formatting syntax, which can easily dress up text with heading, lists, and code snippets. Checkout the [Mastering Markdown](https://guides.github.com/features/mastering-markdown/) guide on GitHub for help with the Markdown syntax.

You might want to use an app that specializes in working with Markdown to edit this file. Try [MarkdownPad](http://markdownpad.com) on Windows and [MacDown](https://macdown.uranusjr.com) on macOS, both free.

You can also [copy a sample README.md](https://github.com/omnis-ci/omnis-lightsaber/raw/master/README.md) and customize it for your project.

## Phase 3: Add your library to the repository and publish it

### Export your library
Open your library from `lib/` folder using Omnis Studio. Select the library in the Studio Browser and click **Export Lib to JSON...** in the sidebar.

![Export to JSON](/images/export_to_json.png)

Omnis will prompt for a location for the export. Choose the `src/` folder in your repository.

<img src="/images/export_json_source.png" alt="Export to JSON Source" class="reduced slightly">

### Import your library
It's a good idea to ensure your library can be rebuilt from the JSON export. The Studio Browser will now present an option to **Rebuild from JSON...**.

![Rebuild from JSON](/images/rebuild_from_json.png)

Click **Rebuild from JSON...**, then click **Import** on the confirmation prompt. 

<img src="/images/confirm_rebuild_from_json.png" alt="Confirm Rebuild from JSON" class="reduced slightly">

Review your Omnis library to ensure it works properly. If you need to access the original library, click **Restore Library...** in the Studio Browser sidebar. You can also find your library on disk in these locations:

| Platform | Default Path |
|---|---
| macOS | `~/Library/Application Support/Omnis/Omnis Studio [Version] x64/archives` |
| Windows | `%LOCALAPPDATA%/Omnis/Omnis Studio [Version] x64/archives` |

### Commit your changes
Return to GitHub Desktop and you will see a number of files marked for change. Enter a summary for your commit, such as ___Initial commit___.

![Initial commit](/images/initial_commit.png)

Click **Commit to master** and your changes will be saved to the local repository on your machine.

### Publish your repository to GitHub
Click **Publish Repository** in the GitHub Desktop toolbar to share your repository with the world.

![Publish repository](/images/publish_repository.png)

Fill out a description and ***uncheck*** **Keep this code private**. Leave the organization as **None**.

<img src="/images/remote_repository_settings.png" alt="Remote repository settings" class="reduced standard">

Click **Publish Repository** and GitHub Desktop will push your code to GitHub.

Once the repository is posted, go to **Repository** -> **View on GitHub** to see it online. Note the URL for your repository---this is what you should share for other developers to access your library.

### Share your repository
Share the link to your repository on GitHub to any or all of these news channels:
1. Post under the [News form on the Omnis Developer portal](https://developer.omnis.net/forums/forum/omnis-forum/news)
1. Email [developer@omnis.net](mailto://developer@omnis.net) with a request to add your library to the Libraries section of the Omnis Developer portal
1. Post an announcement to the [Omnis Developer List](http://www.omnis-dev.com)
1. Email the link directly

Developers can download your library directly from the `lib/` folder in your repository, or they can clone the repository and build the `lbs` from `src/`.

## Phase 4: Publish changes to your repository
When you need to change the library, make them in the `lbs` file in the `lib/` folder of your repository.

Next, open the Studio Browser and select your library. Click **Update JSON tree...** in the sidebar.

![Update JSON tree](/images/update_json_tree.lbs)

Confirm you want to do this.

<img src="/images/confirm_update_json_tree.png" alt="Confirm update JSON tree" class="reduced slightly">

Open GitHub Desktop and review your changes. Check the changes you'd like to commit and enter a summary and any commit notes.

![Review changes to commit](/images/commit_changes.png)

Click **Commit to master** to save the changes locally, then click **Push Origin** in the toolbar to upload your changes to GitHub.

![Push origin](/images/push_origin.png)

Developers can now download an updated `lbs` with your changes, or pull the `src` and import it directly into Omnis.