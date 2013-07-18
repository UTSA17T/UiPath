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
The straightforward way to automate the GUI is to call operating systems API to generate hardware mouse / keyboard events. *Win32 API* provides useful functions like: 
[keybd_event](http://msdn.microsoft.com/en-us/library/windows/desktop/ms646304\(v=vs.85\).aspx), [mouse_event](http://msdn.microsoft.com/en-us/library/windows/desktop/ms646260\(v=vs.85\).aspx) and [SendInput](http://msdn.microsoft.com/en-us/library/windows/desktop/ms646310\(v=vs.85\).aspx).
The problem with this approach is that it interferes with user actions. The mouse and keyboard control is taken away from the user, the window to be automated must be placed in foreground and the focused control has to be changed.

For non-intrusive autoamtion, *UiPath* makes use of Win32 windows messages, [Active Accessibility](http://en.wikipedia.org/wiki/Microsoft_Active_Accessibility), [Java Accessibility API](http://docs.oracle.com/javase/7/docs/technotes/guides/access/jab/index.html), [HTML DOM](http://en.wikipedia.org/wiki/Document_Object_Model) and [UI Automation API](http://msdn.microsoft.com/en-us/library/windows/desktop/ee684009\(v=vs.85\).aspx).

[UiInputMethod](https://github.com/Deskover/UiPath/wiki/Uinode#wiki-UiInputMethod) enumeration specifies the automation technology used by [Click](https://github.com/Deskover/UiPath/wiki/Uinode#wiki-Click), [Hover](https://github.com/Deskover/UiPath/wiki/Uinode#wiki-Hover) and [WriteText](https://github.com/Deskover/UiPath/wiki/Uinode#wiki-WriteText).
 - UI_HARDWARE_EVENTS - keystrokes and mouse events are synthesized by [SendInput](http://msdn.microsoft.com/en-us/library/windows/desktop/ms646310\(v=vs.85\).aspx) Win32 API function.
 - UI_WIN32_MSG - WM_LBUTTONDOWN, WM_LBUTTONUP and WM_CHAR windows messages are posted to target UI control.
 - UI_CONTROL_API - the underlying technology of the UI object is used to generate events (like Active Accessibility, Java Accessibility Bridge, HTML events)

There is also support in *UiPath library* for launching a hidden *Internet Explorer* browser \(see [UiBrowserType](https://github.com/Deskover/UiPath/wiki/UiBrowser#wiki-UiBrowserType)\) which may prove very useful when performing background web automation.

Which method to use really depends on the target application you want to automate. Sometimes *Active Accessibility* is poorly implemented by target apps, for some scenarios *UI_WIN32_MSG* works best, for automation HTML *UI_CONTROL_API* is the way to go.
You have to play a little bit with all the methods to see which works best in your particular case (you can do that with [UiStudio](https://github.com/Deskover/UiPath/wiki/Studio) and [UiWorkflow](https://github.com/Deskover/UiPath/wiki/Workflow-designer) tools). 

Another case when you would want non intrusive automation is when using UiPath monitoring events \(see: [UiNodeMonitor](https://github.com/Deskover/UiPath/wiki/UiNodeMonitor) and [UiEventInfo](https://github.com/Deskover/UiPath/wiki/UiEventInfo)\).
This is one cool feature that lets you easilly intercept user actions on UI elements. When an event that you are interested in is caught you almost always want to perform some non intrusive automation.
It is a really interesting scenario and we'll dedicate it a full blog post very soon.

##Conclusions
You would want non intrusive GUI automation when you
 - deal with UiPath events
 - want to run multiple test scripts without interference
 - integrate apps without disturbing the user
 - perform background automation
 - automate apps logging in other windows account + disconnected remote desktops
 - automate hidden windows/browsers (cannot click hidden controls)

See you later,

Adrian.
