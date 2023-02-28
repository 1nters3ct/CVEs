# CVE-2023-26255
## Overview
“**_Stagil navigation for jira – Menù & Themes_**" is a Jira GUI customization plugin that allows, among other things, to insert a custom image as a header and/or footer. This plugin was developed by Stagil, an independent company that is a Silver Solution Partner and focuses on designing efficient and durable plugin solutions for the Jira environment.

## Vulnerability Description
Prior to version 2.0.52 of the “_Stagil navigation for jira – Menù & Themes_", the _fileName_ parameter is vulnerable to a "_Directory Traversal_" that would allow an attacker to read files on the server knowing their path.

Directory Traversal is a vulnerability that allows an attacker to read arbitrary files on the server that is running an application. This might include application data, credentials for back-end systems, and sensitive operating system files.

## Impacts
This vulnerability allows files on the server to be read. It is also possible to retrieve configuration files containing plaintext passwords, as well as application logs to conduct analysis on users browsing the site.

## CVE-2023-26255

### Proof of concept (POC)
#### Reproducing Steps
First you need to have the “_Stagil navigation for jira – Menù & Themes v2.0.50”_ plugin installed, which can be downloaded from the atlassian marketplace.
Once you have customized the Jira GUI and added a new image as the navigation bar background, you can exploit the vulnerability in question.

![2023-02-28 11_04_31-pathTraversal2-2](https://user-images.githubusercontent.com/126457349/221821644-43ab4865-e8cc-4aa2-b1b5-d64e0faef684.jpg)

Once the image has been loaded whenever you navigate a project menu, an HTTP GET request is made that invokes that image.

This request use two paramenters: “_fileName_” and “_fileMime_”, the former being vulnerable to Path Traversal since no type of check is done on the content of this parameter.

In fact, it is possible to insert a payload, consisting of the path we want to retrieve, inside "_fileName_" to get the contents of the retrieved file as the following images show:

![2023-02-28 11_06_04-pathTraversal2-3](https://user-images.githubusercontent.com/126457349/221822008-16a5954e-1336-4901-9443-dadb54a3d718.jpg)

Moreover, this request can be made even without being authenticated, in fact in the next evidence the request is made without session cookies:

![pathTraversal2](https://user-images.githubusercontent.com/126457349/221822710-80af6ad2-5b5c-4c26-bc98-4929c7c53cab.jpg)

### Suggestions
To make the fix for this vulnerability, it is recommended to update the plugin to version 2.0.52 where this issue is no longer present.

### Discovered by
#### [Alessandro Fondacci](https://www.linkedin.com/in/alessandro-fondacci-326978a1/) of [Cybertech S.p.A.](https://www.cybertech.eu/)
