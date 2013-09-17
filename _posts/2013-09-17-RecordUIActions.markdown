---
layout: layout
title: "Record UI Actions"
---
##Overview
Recording UI Actions is the process of observing your mouse and keyboard actions on other apps and generate an automation script (we refer to it as automation workflow). [UiPath Studio](http://uipath.com/benefits) is then able to playback the generated workflow and simulate the recorded actions.

You can always [add activities](https://github.com/Deskover/UiPath/wiki/Activities-toolbox) one by one from toolbox to your workflow, but it can be a tedious and error prone task, while recording is faster and generally produces a better script.

For those of you who are not familiar with UiPath workflows, a workflow consists of a sequence of connected activities. Each activity encapsulates a user action. [UiPath Studio](http://uipath.com/benefits) offers many [activities](https://github.com/Deskover/UiPath/wiki/Workflow-activities) dedicated to UI automation, each one capable of simulating a certain action. E.g. : typing into an editable control can be done through a *TypeInto* or *SetText* activity, while closing an application through a *CloseWindow* or *CloseApplication* activity.

##How recording works
Recording doesn’t need a stiff learning curve. After clicking Record [button](https://github.com/Deskover/UiPath/wiki/Inserting-activities#wiki-Record_UI_actions_as_activities), a blue selection window will appear. You just need to move the cursor over the UI element you want to operate on, wait for the element to be completely recognised by the selection window and then click on it.

Depending on the element you selected, the Recording will insert the appropriate activity:
* click on  a button or link will generate a Click activity
* click on a radio or checkbox will generate a Check activity
* click on an editable field : a popup  window will appear requiring you to insert the text you want to set in the editable element; afterwards a TypeInto activity will be added
* click on a combobox, dropdown list, list or tree : a popup window will appear asking you to choose between the available elements that can be selected in the control; afterwards a SelectItem activity will be added 

The Recording feature from UiPath Studio generates logic actions on UI objects. It doesn’t recognise elements through their position on screen, it actually identifies the UI object through [its properties](https://github.com/Deskover/UiPath/wiki/Selector). This way, the playback of the workflow becomes more reliable and independent of screen resolution or window placement. 

Also, Recording is able to determine if you performed multiple actions on the same form and arranges the generated activities into logical groups. We created a special activity that represents a logical group called [Within](https://github.com/Deskover/UiPath/wiki/Workflow-activities#wiki-WithinUiElement). E.g. If you type something in an edit box and click on a submit button on the same form, the activities simulating this actions will be placed inside a *Within* activity.
This optimizes your workflow by narrowing down the search area for your controls. Instead of looking for the submit button in the entire application, [UiPath Studio](http://uipath.com/benefits) will search that button only in the area defined by *Within*.

After clicking Record  button, you are prompted with a recording configuration dialog where you can make certain settings before starting your recording. You can choose to open an application, a web site or take into account how much time elapses between your actions. The *delay* recorded will be automatically included in the final workflow. This is useful when you automate an application that doesn’t move very quickly or doesn’t have a specific time response between actions.

!["Recording configuration"](/img/blog/recordingConfiguration.png)

##Practical example
*Hint: Before starting your automation, lay down the steps necessary to perform your scenario.*

Let’s consider you have to save the Economic Loss Zone description of some addresses from this [Zone Locator Economic](http://69.166.140.73/zonelocatoreconomic/) application. To be able to extract that information, you must go through *the same click-save* steps for each address. If you have to extract the description for multiple addresses, this becomes a time-consuming repetitive task. 

To make your job easier you can create a workflow that automates this. Then, every time you need to extract the description of a new address, you just execute it and everything is done.

Here are the steps of our scenario: 
* Open [“http://69.166.140.73/zonelocatoreconomic/”](http://69.166.140.73/zonelocatoreconomic/) page in Internet Explorer
* Click “Begin” button
* Click “Enter An Address” button
* Type an address into the “Full address” editable field
* Click “Locate Address” button
* Click “Location Correct” button
* On the next screen, scrape the value of the “Economic Loss Zone” (e.g. “The address you provided is within Economic Loss Zone B”)
* Set the zone result value into a Notepad
* Close the IE browser

By default you can record only basic actions: 
* type text into an editable field
* click UI elements
* check radio buttons or checkboxes
* select items in dropdowns, lists, trees
* 
Knowing this, let’s take a look at the sample scenario and decide where the Recording would come in handy.

The first step should be done by checking the second option in the Recording Informative dialog. This will allow you to specify the website you want to open (“http://69.166.140.73/zonelocatoreconomic/”) and the browser in which you want it opened (Internet Explorer)

Steps 2 through 6 can be accomplished using the default Recorder. As you click on elements on screen, the Recorder will insert the appropriate activity for each action automatically. You shouldn’t worry about anything; just select each control step by step.

##Advanced recording
There are cases when the default Recording can’t figure out what action you want to perform on a specific UI object or you may need advanced actions, like hovering over an element that opens a menu.

For these cases we provide the option of manually inserting activities during recording. If you press the ESC key the Insert Activity toolbar will appear allowing you to choose your next action.

!["Insert Activity"](/img/blog/InsertActivityToolbar.png)

During your automation you may want to wait for a specific control or image to appear or disappear from screen before continuing. You can achieve this using one of the actions defined in the *Wait* category.

If you want to make sure that your application is in a specific state at a specific step you should add *Checkpoint* activities that verify if a control or image exists on screen.

In our practical example, steps 7, 8 and 9 define such advanced actions that are not supported by the default Recorder.

For complex scenarios, the recording feature is designed to help you get started. In case you need to perform an action that is not supported by the Recorder, we recommend you to manually add the specific activity from the toolbox. 

We advise you to review the generated workflow and make any modifications you consider necessary.

You can find [here](https://github.com/Deskover/UiPath/blob/master/Samples/Workflow/RecordingFlash.uiwf) the resulted workflow.

!["FlashScenario"](/img/blog/FlashScenario.png)



