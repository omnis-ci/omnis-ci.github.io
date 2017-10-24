---
title: Sharing Omnis libraries on GitHub
---
# Sharing Omnis libraries on GitHub
[GitHub.com](https://github.com) is the premier tool for sharing source code. Using the JSON export tool in [Omnis Studio 8.1](http://www.omnis.net/products/studio/index.jsp), Omnis developers can easily share their code on GitHub for other to discover, use, and improve.

This guide will walk you through the complete process for sharing an Omnis library on GitHub.

## Public versus private repositories
<div class="callout">
This guide will help you create a public repository on GitHub. Anyone can view your source and download your library from a public repository. GitHub also provides private repositories that limit access to your code. Private repositories require a paid subscription and are outside the scope of this guide.
</div>

## Prerequisites
* An Omnis library to share
* Omnis Studio 8.1 or later

## Create a GitHub account
If you don't already have a GitHub account, [create one](/guides/creating-a-github-account.html).

## Install GitHub Desktop
Follow [this guide](/guides/setting-up-github-desktop.html) to install and set up GitHub Desktop, a user-friendly graphical front-end to working with GitHub projects.

## Create a repository
GitHub and git organize projects into repositories, similar to a project in the Omnis VCS. Open GitHub Desktop and go to **File** -> **New Repository**. Fill out the form as follows:

| Field | Value |
|---|---|
| Name | The name for your repository. Try using your library's name. GitHub's convention is use dashes for spaces, but underscores work as well |
| Description | A brief description of this library |
| Local Path | A new folder dedicated to storing your local copy of this repository. You can enter a new folder here an GitHub Desktop will automatically create it |
| Initialize this project with a README | Checked. The README's contents will be displayed on the mail page for your project on GitHub |
| Git Ignore | None |
| Licence | Choose an appropriate license if you'd like. See (https://choosealicense.com)[https://choosealicense.com] for details on each license |

When you're finished the form should look something like this:

![Create repository in GitHub Desktop](/images/create_repository_in_github_desktop.png)