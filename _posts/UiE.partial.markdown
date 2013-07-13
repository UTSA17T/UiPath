# What is UiExplorer?

UiExplorer is a tool meant to enable developers to inspect the UI hierarchy of an app, and to improve selectors in cases where the ones produces by the [GetSelector](https://github.com/Deskover/UiPath/wiki/Uinode#wiki-GetSelector) method of the [UiNode](https://github.com/Deskover/UiPath/wiki/Uinode) class are unsatisfactory. For applications with static layout, the GetSelector method returns reliable selectors, but often the element you are trying to identify will have changing attributes, which makes automatic generation more difficult.

## Layout and States

UiExplorer's layout consist of five views, not all of which are visible at all times:

  * **The Selector View** (top-left) is used by UiExplorer to display various selectors, the efficiency of the selector (how fast the node is obtained from the selector using the [FindFirst](https://github.com/Deskover/UiPath/wiki/Uinode#wiki-FindFirst) method), and sometimes error messages.
  * **The Hierarchy View** (middle-left) is only visible when in the *Selector-Editing* mode, and shows the parent hierarchy of the UiNode matched by the currently edited selector.
  * **The UI-Tree View** (bottom-right) is used to navigate the UI hierarchy. You can easilly see where the currently selected UiNode is located turning on *Highlight* from the applications toolbar. By double-clicking an element, you go into the *Selector-Editing* mode.
  * **The Selector Properties View** (top-right) is used for adding, removing or changing the attribute in a selector tag, when you have a selector to work with.
  * **The Runtime Attributes View** (bottom-right) shows the current attribute values of the selected UiNode.

It's two states are *Top-Level* view and *Selector-Editing* mode. 

In the Top-Level view, you can only explore the UI-Tree of apps, and see node attributes in the Runtime Attributes view. This is the state the application normally opens in. You can go from the Selector-Editing view into the Top-Level view by pressing *View &#8594; Top Levels*.

The second app state is the Selector-Editing mode. UiExplorer sometimes opens in this state, when you want to edit a selector activity property in [UiWorkflow](http://google.com). This state is used when you want to chose which nodes &mdash;on the UiNode's of interest parent hierarchy&mdash; you want to include in the selector, and to alter some tag's attributes. You can enter this state in a number of ways:

  * From the Ui-Tree view, for example by double-clicking
  * Using the *Select Interactive* method (arrow button in the toolbar)
  * Entering a selector in the *Edit Manually* dialog (opened by pressing the "E" button in the toolbar)

In the Selector-Editing mode, the UI-Tree will initially be hidden. You can make it visible using *View &#8594; Show Children*. The UI-Tree's top level will be that of the UiNode's *who's selector you are editing*. You can hide the UI-Tree the same way you opened it (View &#8594; Hide Children).

# UI exploration capabilities

UiExplorer enables you to easily explore the UI of an app by exposing it's tree. You can tell where each UiNode is positioned on screen using the Hightlight function. When toggled on, it will draw a rectangle around the currently selected UiNode (if the node is visible). Most selectors you will ever edit will be ones returned by the GetSelector method, from nodes obtained using [SelectInteractive](http://google.com). However, strangelly positioned elements can throw off interactive selection. Consider the following case, where I want to select the Edit button in UiWorkflows ribbon: there is a weird element that stretches more than it seems it needs to, which is obturating the button I want to select. Navigating the tree, I am able to quickly locate the edit button and obtain it's selector.

The nodes in the UI can also be easilly filtered, by selecting one of the options in the dropdown of the view's toolbar (not the apps toolbar!). Filtering can be done using an attribute, or you can come up with your own selector. This allows you to see which nodes are similar (in terms of attributes) to the node you are interested in. Take for instance the idx removal example. If you wanted to see which were the nodes that matched the last tag's attributes, you could do it in the following way:

  1. Select the top-level node of the application, and go into Selector-Editing mode
  1. Open the UI-Tree, and chose to filter elements using a selector
  1. Choose to see all descending UiNodes (as already mentioned, these will be descendants of the UiNode's whose selector you are currently editing)
  1. Use the last tag of the selector, but removing the idx attribute
  1. Press the Find button

The node you are interested in should be the 11-th item being displayed.

# Selector editing capabilities

## Adding intermediate tags to a selector

This is useful when the element you are interested in is hard to identify whithin the whole app, but easy within the descendants of one of it's parents. The example I'm using is taken from UiWorkflow. I'm interested in the title of the startpoint of a workflow. Notice the selector generated automatically makes use of the [idx attribute](https://github.com/Deskover/UiPath/wiki/Selector#the-index-attribute). This happens because there are multiple elements descending from &lt;Insert text here&gt;. However, as the second image shows, adding a tag identifying an ancestor makes the use of an idx unnecesary.

## Modifying attributes

Elements will sometimes have attributes whose value changes. However, their value will often have an invariant part. Take for instance Notepad: Nodepad's title changes based on the name of the file being edited, but it always ends in " &ndash; Notepad". You can use "&midast;" to replace the variable part; this way, a selector attribute value capturing this would be *title="&midast; &ndash; Notepad"*. Attribute values can be changed from the *Selector Properties View* (top-right property-view). The current runtime attributes of the inspected node can be seen in the *Runtime Attributes View* (bottom-right view). UiExplorer will make sure the element can still be identified using the attribute value you provide.

## Automatically improving a selector

The main use we see for UiExplorer is helping produce selectors for elements that change often. When an element changes state, UiExplorer can sometimes automatically generate an improved selector. For that, you can use *Selector &#8594; Improve Selector*. When you do that, UiExplorer will try to improve the selector, without requireing any other input from the user. Unfortunatelly, there are cases where the visual aspect of the app remains the same, but the underlying node is programatically changed, and the new node must be indicated on screen. If Improve Selector succedes, a selector matching the two states of the element will be produced.
