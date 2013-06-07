---
layout: layout
title: "Installing the browser extensions in Chrome and Firefox"
---

#Overview
The UiPath browser extensions play a very important role in isolating and automating UI elements in Web pages. All the UiPath features, such as the visual selection, the UiNode object and all its live attributes, or the monitoring the user interaction through UiEvents, are only possible in browsers through these extensions.

When you install UiStudio, these extensions are automatically enabled in Chrome and Firefox. However, you might want to know how to enable them manually, because problems may occur and you must know how to deal with them.

In this article, we will learn how to enable these features for Chrome and Firefox by installing the browser extensions explicitly.

#Installing the Google Chrome extension

##Manual installation
Installing the UiPath extension for the Google Chrome browser is fairly easy. If you did it before, that is...

OK, so here are the steps to enable the UiPath browser extension:

1.  Click the *Settings* icon in the upper-right part of the Chrome main window, then choose *Tools* and *Extensions*, as shown in the figure below:
    !["Chrome Step 1"](/img/blog/Chrome_step_1.jpg)

2.  You should see the list of extensions in Google Chrome. Now you have to add the UiPath extensions to this list. To find our extension, go to the installation directory where you installed UiPath. Let's say it's the default location, like *"C:\Program Files\UiPath Studio\v6"*. You will notice a *"BrowserExtensions"* subdirectory, which you will navigate into and find a file named *"uiPathExt.crx"*. You have to drag this file in Windows Explorer using the mouse and drop it into the list of Chrome extensions, as shown below:
    !["Chrome Step 2"](/img/blog/Chrome_step_2.jpg)

3.  The Chrome extension is now successfully installed and you can automate Web pages in Chrome using UiPath.

##Automated deployment
If you want to deploy the Chrome extension with your product setup package, all you have to do is add the following registry content on the target machine:

```
Windows Registry Editor Version 5.00
 
[Software\Google\Chrome\Extensions\mfbmkebgbbbcpfeppkndaicpmlgijlke]
"path"="[INSTALLDIR]uiPathExt.crx"
"version"="5.0.0"
```

*"[INSTALLDIR]"* is the target installation directory, for example *"c:\program files\AnySoft\AnyProduct\"*. If you are working with MSI packages, then the *"[INSTALLDIR]"* itself will point to the actual target directory.

#Installing the Mozilla Firefox extension
The Firefox extension must be installed in a totally different manner: through Windows Registry files. For this operation, it is required that you have Administrator privileges enabled for your current Windows account.
First, you have to know where UiPath is installed. Let's say it's in *"C:\Program Files\UiPath Studio\v6"*. Then the browser extension directory will be *"C:\Program Files\UiPath Studio\v6\BrowserExtensions"*. Considering these, we have the following REG file content:

```
Windows Registry Editor Version 5.00
 
[HKEY_LOCAL_MACHINE\Software\Mozilla\Firefox\Extensions]
"info@uipath.com"="C:\Program Files\UiPath Studio\v6\BrowserExtension"
```

Copy this text content into the Clipboard and save it into a file named *"Install_UiPath_for_Firefox.REG"* (or any other suggestive name that you like) using a text editor, such as Notepad. Notice the blank line between *"Windows Registry Editor Version 5.00"* and the text that follows, it is important.
Then, open it in Windows Explorer via double click or the *<Enter>* key and restart Firefox (if opened). This will merge the registry values contained by the file into the Windows Registry, thus enabling the UiPath extension in Firefox.

This registry content can also be used in your setup package for deploying the UiPath Firefox extension on the target machine.


That's it, we hope that UiPath is working now in Chrome and Firefox and that you are enjoying all the features in our product!
