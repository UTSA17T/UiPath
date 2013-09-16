---
layout: layout
title: "UiPath UiAutomation API walk–through"
---

##Overview
If you are reading this then you are most likely interested in GUI automation and/or screens scraping. This article presents UiPath GUI automation library. You will learn about what it can do, its main features and core API components.

##What is?

UiPath is a complete solution for software automation with an accent on GUI automation. The [workflow designer](https://github.com/Deskover/UiPath/wiki/Workflow-designer) along with the GUI action recorder and GUI automation wizards make the user interface of other applications accessible with no programming effort.

For the diehard programmer that needs maximum flexibility, beneath workflow activities there is a powerful *COM* library that can be used from virtually any programming language / IDE dubbed UiPath UiAutomation API (blame my manager for the name) . This API makes the GUI of other applications programmable, exposing the attributes of GUI objects on the screen and also simulating user actions on them. For those in a hurry here is the [UiPath UiAutomation API reference](https://github.com/Deskover/UiPath/wiki/UI-Automation-API-reference).

##UiPath at a glance

A code sample is worth a thousand words so here is the “hello world” GUI automation code:

``` javascript
var UI_WIN32_MSG = 1;

// Instantiate UiSystem library object and launch notepad.exe
var s = WScript.CreateObject("UiPath.UiSystem");
s.RunApplication("notepad.exe");

// Instantiate UiNode library object.
var n = WScript.CreateObject("UiPath.UiNode");

// Search for the editable control inside notepad application.
n.FromSelector("<wnd app='notepad.exe' cls='Notepad' title='Untitled - Notepad'/><wnd cls='Edit'/><ctrl role='editable text'/>");

// Write some text into the edit control.
n.WriteText("Hello World", UI_WIN32_MSG);
```

This is [JScript](http://en.wikipedia.org/wiki/JScript) sample that you can run with wscript.exe or cscript.exe. It feels a little bit too long for a “hello world” sample because of the comments and constant definitions required by good programming practices which double the line count.
As you can see it starts with instantiating a library system object, launches notepad.exe application and fills out “hello world” text into the editable area.
UiPath can perform this magic because it understands the internals of various UI frameworks: *Win32*, *WPF*, *HTML*, *PDF*, *Java*. The UI objects are usually arranged in a hierarchically tree, the root being the top level window the app and the UI controls being the leaves.


##UiNode

The most important object of the library is [UiNode](https://github.com/Deskover/UiPath/wiki/Uinode) which represents an object in the UI hierarchy and it can do a lot of things:
 * find UI objects on the screen: FromSelector, FromScreenPoint, FromScreenRegion, FromDesktop, FromWindow
 * navigate through the UI tree: Child, FindFirst, FindAll, Parent, TopParent
 * simulate user actions: Click, Hover, WriteText, SetFocus
 * scrape the text / image of UI objects: Scrape, Screenshot
 * get/set attributes of the UI objects: Get, Set
 * inject scripts into web pages and extract HTMl data: InjectAndRunJS, ExtractData


##Selector

The most important method of UiNode is [FromSelector](https://github.com/Deskover/UiPath/wiki/Uinode#wiki-FromSelector) which searches for a GUI object on the screen based on a XML-like string called selector. The selector contains all the necessary information to identify an GUI object at runtime. Here is the selector that identifies the editable area in notepad.exe app.

``` xml
<wnd app='notepad.exe' cls='Notepad' title='Untitled - Notepad'/><wnd cls='Edit'/><ctrl role='editable text'/>
```

As you can see, the selector describes a path from the top level window to the GUI object we’re looking for. Each XML part defines an object by specifying its relevant attributes. The selector is important because it provides a way of identifying the GUI objects based on their attributes rather than their size and position on the screen.
Selectors may seem complicated but worry not, UiPath has tools that generate them automatically. [Here](https://github.com/Deskover/UiPath/wiki/Selector) you may find the full selector story.


##Image-based GUI automation

As we have seen, the selector is the best method of object recognition (when it works). There are applications like remote desktop clients which only paint the controls on the screen. For these apps the selector can only identify the top level window of the application but not the controls inside.
For these special scenarios UiPath provides image-based GUI automation and *OCR* screen scraping. The idea behind image-based GUI automation is to use screenshot patterns to identify controls and GUI components and then direct mouse and keyboard events to them. First you take a screenshot of the control you want to automate and them the library searches for the image to get the position of the control on the screen.

Here’s the pseudo-code of image-based GUI automation:

 * instantiate a UiImage object and call LoadFile against the file containing the image of the GUI object.
 * instantiate a UiNode and use FromSelector to find the top level window of the app to be automated.
 * call uiNode.FindImage against the image object at step 1). and get a UiRegion which represents the coordinates of the UI object on the screen.
 * having the coordinates we can Click on the GUI object.

##Other library objects

There are several other objects in the library that completes the feature list.
 * UiBrowser which provides browser-related functionality like: Start, Navigate, Refresh, Close, WaitPage
 * UiWindow exposes window-related methods like: Move, Maximize, Minimize, Restore, Close, Show, Hide
 * UiSystem provides system-related features like: GetClipboardText, SetClipboardText, RunApplication, CloseApp, Cleanup, GetActiveWindow

##Event triggers

Event trigger is UiPath’s unique feature. In a way, it is the opposite of GUI automation. While GUI automation simulates user actions by generating click and keyboard events on GUI controls, events triggers intercepts user actions on certain controls.

It works like this:
 * you first specify what event you are interested in: mouse or keyboard and which mouse button / key combination
 * you specify a selector to specify what GUI objects to monitor
 * specify a screen region to restricts the area inside the matching visual elements in which the clicks will be monitored

When the event occurs and all the conditions are satisfied your code will be notified (a *COM* callback function will be called). The client code has the option to cancel the event so it won’t get to the target GUI control.

Here are the event-related library objects and methods:
 * UiNodeMonitor: MonitorClick, MonitorClickOnImage, MonitorHotkey, StopMonitor.
 * UiSystem: MonitorClick, MonitorHover, MonitoryHotkey, StopMonitoring. UiSystem provides global monitoring (not restricted to any GUI object).

[Here](https://github.com/Deskover/UiPath/wiki/Api-documentation#wiki-UI_events) is the full story on UI event triggers.

##Conclusions

By now, you should have a fairly good idea about what UiPath GUI automation library is, what it can do and its core concepts.

Keep in mind that UiPath product is more than only a GUI automation library. It’s a software automation and application integration tool that automates business processes. With its powerful workflow designer one can easily:
 * automatically send emails and sms
 * process XML, CSV, Excel files
 * invoke Powershell scripts
 * make HTTP and SOAP calls
 * perform database transactions
 * and much more ...

Until next time,
Adrian.
