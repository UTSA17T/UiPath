---
layout: layout
title: "UiExplorer &ndash; the UI and selectors"
---

# What is UiExplorer?

UiExplorer is a tool for (guess what!) inspecting the UI hierarchy of applications, for obtaining or improving [selectors]() used by our library, to identify [UiNodes]().

Most often, the selectors you are working with will have been obtained using the [GetSelector]() method of UiNode. In most cases, GetSelector should be sufficient for your needs, as is the case in apps with static layout; however, there are apps out there with volatile layout &ndash;because of nodes with varying attribute values; such is the case with webapps, but not only&ndash;, that make automatic generation of *reliable* selectors impossible. GetSelector has no way of knowing how an attribute's value is going to change along the runtime, or between runs of an app.

We know editing selectors in these cases can be bothersome, if not difficult and time consuming &mdash; we built UiExplorer in order to make quick work of this task: it conveniently exposes node values, position, and can sometimes even repair selectors automatically, with minimal input from the user.

# Layout and States

UiExplorer has two operating modes: *Top-Level* view and *Selector-Editing*. 

The Top-Level view is the state UiExplorer normally starts up in. In this state, you can only explore the Ui-Tree of apps, and see nodes in the Runtime Attributes view. One of it's uses is locating nodes for which interactive selection is difficult. You can go from Selector-Editing mode into Top-Levels using *View &#8594; Top Levels*.

<table style="table-layout: fixed; width: 100%; margin: 0.7em 0">
<thead>
<tr><td colspan=3><span style="display: block; text-align: center"><strong>UiExplorer's States</strong></td></tr>
</thead>

<tbody>
<tr>
<td style="width: 100%; padding: 1%"><img src="/img/blog/UiExplorer-Blog Top-Level-View.png" /></td>
<td style="width: 100%; padding: 1%"><img src="/img/blog/UiExplorer-Blog Selector-Edit-Mode children.png" /></td>
<td style="width: 100%; padding: 1%"><img src="/img/blog/UiExplorer-Blog Edit-Selector-Mode no-children.png" /></td>
</tr>

<tr>
<td><span style="display: block; text-align: center">Top-Level View</span></td>
<td><span style="display: block; text-align: center">Editing w/ Ui-Tree</span></td>
<td><span style="display: block; text-align: center">Editing w/o Ui-Tree</span></td>
</tr>
</tbody>
</table>

The second state is the Selector-Editing mode; UiExplorer opens in this state when you chose to edit a selector activity property in [UiWorkflow](http://google.com). This state is used when you want to chose which nodes &ndash;in the parent hierarchy of the UiNode of interest&ndash; you want to include in the selector, or to alter some tag's attributes. You can enter this state in a number of ways:

  * From the Ui-Tree view, for example by double-clicking
  * Using the *Select Interactive* method (arrow button in the toolbar)
  * Entering a selector in the *Edit Manually* dialog (opened by pressing the "E" button in the toolbar)

In the Selector-Editing mode, the Ui-Tree will initially be hidden. You can make it visible using *View &#8594; Show Children*. The Ui-Tree's top level will be that of the UiNode's *who's selector you are editing*. You can hide the Ui-Tree the same way you opened it (View &#8594; Hide Children).

As you can see in the images, UiExplorer has five views (not all of which are visible at all times):

  1. **The Selector View** (top-left) displays selectors, their efficiency (how fast the node is obtained using the [FindFirst](https://github.com/Deskover/UiPath/wiki/Uinode#wiki-FindFirst) method), and sometimes error messages.
  1. **The Hierarchy View** (middle-left) is only visible when in the *Selector-Editing* mode, and shows the parent hierarchy of the UiNode matched by the currently edited selector.
  1. **The Ui-Tree View** (bottom-right) is used to navigate the UI hierarchy. You can easilly see where the currently selected UiNode is located by turning on *highlight* from the applications toolbar. By double-clicking an element, you go into the *Selector-Editing* mode.
  1. **The Selector Properties View** (top-right) is used for adding, removing or changing the attribute in a selector tag, when you have a selector to work with.
  1. **The Runtime Attributes View** (bottom-right) shows the current attribute values of the selected UiNode.

I'd like to mention once again, that if you have a selector available, you can start editing by entering it into the Edit Manually dialog.

# Selector editing capabilities

## Adding intermediate tags to a selector

Adding tags is useful when the element you are interested in is hard to identify whithin the whole app, but easy within the descendants of one of it's parents. The example I'm using is taken from UiWorkflow; I'm interested in the title of the startpoint of a workflow. Notice the selector generated automatically makes use of the [idx attribute](https://github.com/Deskover/UiPath/wiki/Selector#the-index-attribute) &mdash; this happens because there are multiple elements descending from &lt;Insert text here&gt;. However, as the second image shows, adding a tag identifying an ancestor makes the use of an idx unnecesary.

## Modifying attributes

Elements will sometimes have attributes whose value changes; however, their value will often have an invariant part. Notepad's title, for example, changes based on the name of the file currently being edited, but it always ends in " &ndash; Notepad". You can use "&ast;" to replace the variable part; this way, the best attribute value would be: *title="&ast; &ndash; Notepad"*. Attribute values can be changed from the *Selector Properties View* (top-right property-view). The current runtime attributes of the inspected node can be seen in the *Runtime Attributes View* (bottom-right view). UiExplorer will make sure the element can still be identified using the attribute value you provide.

## Automatically reparing selectors

The main use we see for UiExplorer, is helping produce selectors for elements that change often. When an element changes state, UiExplorer can sometimes automatically repair it's selector (if it no longer matches that node). For that, you can use *Selector &#8594; Improve Selector* &mdash; UiExplorer will try to improve the selector, without requireing any other input from the user. Unfortunatelly, there are cases where the visual aspect of the app remains the same, but the underlying node is programatically changed, and the new node must be indicated on screen; if that is the case, you will be asked to interactivelly indicate it. If Improve Selector succedes, a selector matching the two states of the element will be created.

# UI exploration capabilities

## Usage of the Ui-Tree

UiExplorer enables you to easily explore the UI of an app by exposing it's tree. You can tell where each UiNode is positioned on screen using the *hightlight* option &ndash; when toggled on, it will draw a rectangle around the currently selected UiNode (if the node is visible). Most selectors you will ever edit will be ones returned by the GetSelector method, from nodes obtained using [SelectInteractive](http://google.com). However, strangelly positioned elements can throw off interactive selection. Consider the following case, where I want to select the Edit button in UiWorkflow's ribbon; there is a weird element that stretches more than it seems it needs to, which is obturating the button I want to select. Navigating the tree, I am able to quickly locate the edit button and obtain it's selector.

## Filtering descendants

The nodes in the UI can also be easilly filtered, by selecting one of the options in the dropdown of the view's toolbar (not the apps toolbar!). Filtering can be done using an attribute, or you can come up with your own selector. This allows you to see which nodes are similar (in terms of attributes) to the node you are interested in. Take for instance the idx removal example. If you wanted to see which were the nodes that matched the last tag's attributes, you could do it in the following way:

  1. Select the top-level node of the application, and go into Selector-Editing mode
  2. Open the Ui-Tree, and chose to filter elements using a selector
  3. Choose to see all descending UiNodes &ndash; as already mentioned, these will be descendants of the UiNode's whose selector you are currently editing
  4. Insert the last tag of the selector, after removing the idx attribute
  5. Press the Find button

The node you are interested in should be the 11-th item being displayed.

# Afterword

