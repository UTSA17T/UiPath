---
layout: layout
title: "Installing the browser extensions in Chrome and Firefox"
---

##Overview
The UiPath browser extensions play a very important role in isolating and automating UI elements in Web pages. All the UiPath features, such as the visual selection, the UiNode object and all its live attributes, or the monitoring the user interaction through UiEvents, are only possible in browsers through these extensions.

When you install UiStudio, these extensions are automatically enabled in Chrome and Firefox. However, you might want to know how to enable them manually, because problems may occur and you must know how to deal with them.

In this article, we will learn how to enable these features for Chrome and Firefox by installing the browser extensions explicitly.

##Installing the Google Chrome extension

###Manual installation
Installing the UiPath extension for the Google Chrome browser is fairly easy. If you did it before, that is...

OK, so here are the steps to enable the UiPath browser extension:

1.  Click the *Settings* icon in the upper-right part of the Chrome main window, then choose *Tools* and *Extensions*, as shown in the figure below:
    !["Chrome Step 1"](/img/blog/Chrome_step_1.jpg)

2.  You should see the list of extensions in Google Chrome. Now you have to add the UiPath extensions to this list. To find our extension, go to the installation directory where you installed UiPath. Let's say it's the default location, like *"C:\Program Files\UiPath Studio\v6"*. You will notice a *"BrowserExtensions"* subdirectory, which you will navigate into and find a file named *"uiPathExt.crx"*. You have to drag this file in Windows Explorer using the mouse and drop it into the list of Chrome extensions, as shown below:
    !["Chrome Step 2"](/img/blog/Chrome_step_2.jpg)

3.  The Chrome extension is now successfully installed and you can automate Web pages in Chrome using UiPath.

###Automated deployment
If you want to deploy the Chrome extension with your product setup package, all you have to do is add the following registry content on the target machine:

```
Windows Registry Editor Version 5.00
 
[Software\Google\Chrome\Extensions\mfbmkebgbbbcpfeppkndaicpmlgijlke]
"path"="[INSTALLDIR]uiPathExt.crx"
"version"="5.0.0"
```

*"[INSTALLDIR]"* is the target installation directory, for example *"c:\program files\AnySoft\AnyProduct\"*. If you are working with MSI packages, then the *"[INSTALLDIR]"* itself will point to the actual target directory.

###Rebranding
This section will help you deploy the UiPath Chrome extension under a different name. Just follow these steps to accomplish this:

1.	Go to the *"BrowserExtensions"* subdirectory in the UiPath installation directory and you will find the Chrome extension as the *"uiPathExt.crx"* file. Copy this file somewhere else and rename it to *"uiPathExt.zip"*.

2.	Expand the ZIP archive into a directory named as you like. Let's say for example that we extract into *"c:\temp\myChromeExt"*.

3.	Browse this directory and you will find the version information file under the name of *"manifest.json"*. Open it using a text editor and modify the **name**, **version** and **description** fields to suit your needs.

4.	You can modify the icons that will appear in the list of extensions by replacing the three PNG files: *"uiPath16.png"* for the icon with 16x16 pixel resolution, *"uiPath48.png"* for 48x48, and *"uiPath128.png"* for the big 128x128 icon.

5.	Now that the extension is rebranded, it will have to be re-packaged into a new CRX file. For that, you will have to go to the *Extensions* page in Chrome, as described in the **Manual installation** section. Then, you will press the *Pack extension...* button and you will see the following screen:
	!["Chrome Rebranding 1"](/img/blog/Chrome_rebranding_1.jpg)

6.	In the **Extension root directory** field, you must write the path to your rebrander extension files. In our example, it's the *"c:\temp\myChromeExt"* from step 2.

7.	You do not have a **Private key file** yet. Leave this field blank for now, this will tell Google Chrome that you need such a file and it will create one for you.

8.	Click the **Pack Extension** button. Google Chrome will respond with a dialog that looks like this:
	!["Chrome Rebranding 2"](/img/blog/Chrome_rebranding_2.jpg)

9. 	The CRX file is the your rebranded Chrome extension. The PEM file is the private key and you must keep it, it uniquely identifies your extension. Next time you pack your extension, fill in the **Private key file** field at step 7 with the path to the PEM file.

10.	Now that you have the private key (PEM) file, you can pack the Chrome extension automatically using this command line:

```
<Path_to_Chrome..exe> --pack-extension=<Path_to_extension_directory> --pack-extension-key=<Path_to_private_key_file>
```

*Example:*

```
"C:\programs\chrome.exe" --pack-extension="c:\temp\myChromeExt" --pack-extension-key="c:\temp\myChromeExt.pem"
```

##Installing the Mozilla Firefox extension

###Manual installation
The Firefox extension must be installed in a totally different manner: through Windows Registry files. For this operation, it is required that you have Administrator privileges enabled for your current Windows account.

First, you have to know where UiPath is installed. Let's say it's in *"C:\Program Files\UiPath Studio\v6"*. Then the browser extension directory will be *"C:\Program Files\UiPath Studio\v6\BrowserExtensions"*. Considering these, we have the following REG file content:

```
Windows Registry Editor Version 5.00
 
[HKEY_LOCAL_MACHINE\Software\Mozilla\Firefox\Extensions]
"info@uipath.com"="C:\Program Files\UiPath Studio\v6\BrowserExtension"
```

Copy this text content into the Clipboard and save it into a file named *"Install_UiPath_for_Firefox.REG"* (or any other suggestive name that you like) using a text editor, such as Notepad. Notice the blank line between *"Windows Registry Editor Version 5.00"* and the text that follows, it is important.

Then, open it in Windows Explorer via double click or the *<Enter>* key and restart Firefox (if opened). This will merge the registry values contained by the file into the Windows Registry, thus enabling the UiPath extension in Firefox.

###Automated deployment
This registry content can also be used in your setup package for deploying the UiPath Firefox extension on the target machine.

###Rebranding
This section will help you change the name and the company under which the UiPath extension will be listed in Mozilla Firefox.

1.	Go to the *"BrowserExtensions"* subdirectory in the UiPath installation directory and you will find the Firefox extension, consisting the following files and drectories: 

	* The *"chrome"* directory. 
	
	* The *"plugins"* directory. 
	
	* The *"chrome.manifest"* file. 
	
	* The *"install.rdf"* file. 
	
Take all those files and copy them somewhere else. Let's say for example that we copy them in *"c:\temp\myFirefoxExt"*.

2.	Open the *"install.rdf"* file in a text editor. You will notice it's written in the XML format. Here are the important sections:

	* **em:id** - it's the identifier of your extension. For UiPath, it's *info@uipath.com*. Let's say for example that your identifier is *steve@devs.com*.
	
	* **em:name** - the name of your extension.
	
	* **em:creator** - the name of your company.
	
	* **em:description** - just a brief description of your Firefox extension.
	
	* under the **em:targetApplication** section, you will find another XML entry named **em:id**. That string is a *globally unique identifier* (GUID) and it's a unique characteristic of your extension. You will have to generate another one for making sure that you have a one-of-a-kind extension.
	
3.	For deploying your newly rebranded extension, you will need to generate a registry file similar to the one in the **Installation** section. Considering our example where the identifier of your extension is *steve@devs.com*, the REG file used for deployment should look like this:

```
Windows Registry Editor Version 5.00
 
[HKEY_LOCAL_MACHINE\Software\Mozilla\Firefox\Extensions]
"steve@devs.com"="c:\programs\MyProduct\MyFirefoxExtension"
```


That's it, we hope that UiPath is working now in Chrome and Firefox and that you are enjoying all the features in our product!
