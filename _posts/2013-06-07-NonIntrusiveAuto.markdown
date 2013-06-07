---
layout: layout
title: "Non intrusive desktop automation"
---
##Overview

This blog post is about desktop automation in [UiPath]([http://www.uipath.com/) and more specifically about non intrusive GUI automation. 
We'll talk about methods to simulate keyboard and mouse events available in UiPath. We'll get into more details about their internals and use case scenarios.

##Problem

GUI automation can be the solution when dealing with legacy software systems that were designed to be used only by humans with no API support for data automation and interoperability.
Data is automatically extracted and input directly into the controls on the screen. Anything you see on the screen from SAP and Oracle to .Net and Adobe apps can be controlled and automated by UiPath.
Another hot area for GUI automation is automated GUI testing. 

Some GUI automation scenarios have special requirments regarding how the automation scripts interfere with the user that is currently logged on.
*Non intrusive GUI automation* requires that automation scripts should not remove or hide users' access to existing applications.

Automation code should not change the focus, the active window, take away the mouse or keyboard, logout from the current windows session or restrict the user's control in any way.
Non invasive GUI automation should remain unnoticed by the users while it performs automation tasks in background.

##How to
The easiest way to automate the GUI is to call operating systems API to generate hardware mouse and keyboard events.


##Conclusions


See you later,

Adrian.
