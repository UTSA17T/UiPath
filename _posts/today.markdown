# What is UiExplorer?

UiExplorer is a tool meant to enable developers to inspect the UI hierarchy of an app, and to improve selectors in cases where the ones produces by the [GetSelector](https://github.com/Deskover/UiPath/wiki/Uinode#wiki-GetSelector) method of the [UiNode](https://github.com/Deskover/UiPath/wiki/Uinode) class are unsatisfactory. For applications with static layout, the GetSelector method returns reliable selectors, but often the element you are trying to identify will have changing attributes, which makes automatic generation more difficult.

##Layout

UiExplorer's layout consist of five views, not all of which are visible at all times:

  * **The Selector View** (top-left) is used by UiExplorer to display various selectors, the efficiency of the selector (how fast the node is obtained from the selector using the [FindFirst](https://github.com/Deskover/UiPath/wiki/Uinode#wiki-FindFirst) method), and sometimes error messages.
  * **The Hierarchy View** (middle-left) is only visible when in the *Selector Editing* mode, and shows the hierarchy of the UiNode matched by the currently edited selector.
  * **The UI-Tree View** (bottom-right) is used to navigate the UI hierarchy. You can easilly see where the currently selected UiNode is located turning on *Highlight* from the applications toolbar. By double-clicking an element, you go into the *Selector Editing* mode.
  * **The Selector Properties View** (top-right) is used for adding, removing or changing the attribute in a selector tag, when you have a selector to work with.
  * **The Runtime Attributes View** (bottom-right) shows the current attribute values of the selected UiNode.

# UI exploration capabilities

UiExplorer enables you to easily explore the UI of an app by exposing it's tree. You can tell where each UiNode is positioned on screen using the Hightlight function. When toggled on, it will draw a rectangle around the currently selected UiNode (if the node is visible).

The nodes in the UI can also be easilly filtered, by selecting one of the options in the dropdown of the view's toolbar (not the apps toolbar!). Filtering 

# Selector editing capabilities

## Entering the Selector Editing mode 

You can enter the Selector Editing mode in a number of ways:

  * From the Ui-Tree view, for example by double-clicking
  * Using the *Select Interactive* method (arrow button in the toolbar)
  * Entering a selector in the *Edit Manually* dialog (opened by pressing the "E" button in the toolbar)
  * [UiWorkflow](http://google.com) can open UiExplorer in this state, when a user wants to edit a selector

## Adding intermediate tags to a selector

This is useful when the element you are interested in is hard to identify whithin the whole app, but easy within the descendants of one of it's parents. The example I'm using is taken from UiWorkflow. I'm interested in the title of the startpoint of a workflow. Notice the selector generated automatically makes use of the [idx attribute](https://github.com/Deskover/UiPath/wiki/Selector#the-index-attribute). This happens because there are multiple elements descending from &lt;Insert text here&gt;. However, as the second image shows, adding a tag identifying an ancestor makes the use of an idx unnecesary.

## Modifying attributes

Elements will sometimes have attributes whose value changes. However, their value will often have an invariant part. Take for instance Notepad: Nodepad's title changes based on the name of the file being edited, but it always ends in " &ndash; Notepad". You can use "&midast;" to replace the variable part; this way, a selector attribute value capturing this would be *title="&midast; &ndash; Notepad"*. Attribute values can be changed from the *Selector Properties View* (top-right property-view). The current runtime attributes of the inspected node can be seen in the *Runtime Attributes View* (bottom-right view). UiExplorer will make sure the element can still be identified using the attribute value you provide.

## Automatically improving a selector

Sometimes, UiExplorer can automatically generate an improved selector. For that, you can use *Selector &#8594; Improve Selector*. When you do that, UiExplorer will try to improve the selector, without requireing any other input from the user. Unfortunatelly, there are cases where the visual aspect of the app remains the same, but the underlying node is programatically changed, and the new node must be indicated on screen. If Improve Selector succedes, a selector matching the two states of the element will be produced.
