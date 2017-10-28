---
layout: post
title:  "A call to arms"
date:   2017-10-27 15:02:42 -0400
categories: github tutorial announcement
author: aclay
excerpt_separator: <!--end_excerpt-->
---

{% assign img_root = "/images/posts/a-call-to-arms" %}
![Euromnis 2017]({{ img_root }}/euromnis2017.png){:class="centered"}

Last week I was privileged to attend my first [EurOmnis](http://www.omnisworld.co.uk) conference, and even moreso humbled to be invited to speak. Thursday evening featured an annual general meeting for OmnisWorld, the not-for-profit organization that organizes EurOmnis. One of the topics raised was using the new JSON export feature in Studio 8.1 to share Omnis code on GitHub. Specifically, the community would like to see Omnis as an officially-recognized language on GitHub to boost awareness, add syntax highlighting, and generally establish our development platform as legitimate within the largest hosted service for source code management.

<!--end_excerpt-->

![Euromnis 2017]({{ img_root }}/omnis_plus_github.png){:class="centered"}

Adding a new language to GitHub is accomplished through GitHub's [Linguist](https://github.com/github/linguist) project. Linguist identifies the languages used in a repository primarily through file extensions, and secondarily by matching patterns within the code. For Omnis, we would register the `.omh` extension for the code files in a JSON export as well as `.lbs` extension for a complete library.

There are a number of requirements that must be met when [adding a new language](https://github.com/github/linguist/blob/master/CONTRIBUTING.md#adding-a-language). Some of these are easy enough to meet, such as the defining language settings in `languages.yml` or creating code samples. We also need a grammar for Omnis Studio, but fortuantely, Henk Noppe has already [built one](https://github.com/Frogli/OmnisStudioHighlighter).

There are already repositories with both Omnis code and non-Omnis code using the `.omh` [(search results)](https://github.com/search?utf8=✓&q=extension%3Aomh+NOT+nothack&type=Code) and `.lbs` [(search results)](https://github.com/search?utf8=✓&q=extension%3Albs+NOT+nothack&type=Code) extensions, but we can train Linguist how to detect Omnis code specifically as long as we have the JSON export.

The real challenge will be to get a critical mass of repositories sharing Omnis code. Per the Linguist project:

> We try only to add languages once they have some usage on GitHub. In most cases we prefer that each new file extension be in use in hundreds of repositories before supporting them in Linguist.

Omnis software has been around for decades, but we're only now able to sensibly share it on GitHub thanks to the JSON export. The JSON export allow GitHub to read the code, recognize it as Omnis, and format it properly. To that end, we need to ensure repositories include both `.lbs` files _and_ JSON exports. We also need a large number of repositories, and at EurOmnis, the developers present agreed there's plenty of sharable Omnis code out there and we just need to get it on GitHub.

Because Omnis developers may not have worked with GitHub much and because the JSON export is new, a request was raised for a step-by-step guide explaing how to share an Omnis library on GitHub. I volunteered to write that guide, and I'm happy to announce it is available on this site under [Sharing Omnis libraries on GitHub](/guides/sharing-omnis-libraries-on-github.html). Henk Noppe contributed a second guide explaining how to [Contribute to Omnis libraries](/guides/contribute-to-omnis-libraries.html) once they're on GitHub, and I predict more guides and resources will be written as this project moves forward.

And so, I'm issuing a general call to arms. Find your snippets, utilities, examples, and doodles. Rip out that useful window in your enterprise app that you want to share, or just make yet another *Hello World* app. Whether it's in 8.1, 4.3, or even Classic, convert it up to Studio 8.1, and put it on GitHub. Let's put Omnis on the map and start building a library (pardon the pun) of Omnis utilities, frameworks, examples and tools on the most popular code sharing site on the Internet.

![We can do it]({{ img_root }}/we_can_do_it.png){:class="centered"}