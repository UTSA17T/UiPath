---
layout: layout
title: "UiExplorer &mdash; the UI and Selectors"
---

## What is UiExplorer?

UiExplorer is a tool for (guess what!) inspecting applications' UI hierarchy, for obtaining or improving [selectors](https://github.com/Deskover/UiPath/wiki/Selector) that [our library](https://github.com/Deskover/UiPath/wiki/UI-Automation-API-reference) identifies [UiNodes](https://github.com/Deskover/UiPath/wiki/Uinode) with.

Most often, the selectors you are working with will have been composed by the [GetSelector](https://github.com/Deskover/UiPath/wiki/Uinode#wiki-GetSelector) method of UiNode. 
Just as often, they should be sufficient for your needs, as is the case in apps with static user interface. 
However, some programs have volatile layout &ndash;because of nodes with unsteady attribute values; such is the case of web-apps, but not only&ndash; that make automatic generation of *reliable* selectors impossible &mdash; GetSelector can't predict how an attribute's value will change along the runtime, or between runs of an app.

We know editing selectors in these cases can be bothersome, if not difficult and time consuming &mdash; we built UiExplorer to make quick work of this task: it conveniently exposes node values, position, and can sometimes even repair selectors automatically, with minimal input from the user.

## Layout and States

UiExplorer operates in two modes: *Top-Level* and *Selector-Editing*. 

The Top-Level view is the state that UiExplorer normally starts up in. 
In it, you can explore the Ui-Tree of apps, and see node values in the Runtime Attributes view. One of its uses is locating nodes that are hard to [select interactively](https://github.com/Deskover/UiPath/wiki/Uinode#wiki-SelectInteractive). 
You can go from Selector-Editing mode into Top-Levels using *View &#8594; Top Levels*.

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

The second state is Selector-Editing mode; UiExplorer opens in it when you chose to edit a selector activity property, in [UiPath Studio](https://github.com/Deskover/UiPath/wiki/UiPath-Studio). This state allows you to choose which nodes &ndash;in the parent hierarchy of the UiNode of interest&ndash; you want to include in the selector, or to alter some tags' attributes. You can enter Selector-Editing mode in a number of ways:

  * From the Ui-Tree view, for example by double-clicking
  * Using [interactive selection](https://github.com/Deskover/UiPath/wiki/Uinode#wiki-SelectInteractive) (arrow button in the toolbar)
  * Entering a selector in the *Edit Manually* dialog (opened by pressing the pencil button in the toolbar)

In Selector-Editing mode, the Ui-Tree will initially be hidden. You can make it visible using *View &#8594; Show Children*. The Ui-Tree's top-level will be that of the UiNode's *who's selector you are editing*. You can hide the Ui-Tree the same way you opened it (View &#8594; Hide Children).

As you can see in the images, UiExplorer has five views (not all of which are visible at all times):

  1. **The Selector View** (top-left) displays selectors, their efficiency (how fast the node is obtained using the [FindFirst](https://github.com/Deskover/UiPath/wiki/Uinode#wiki-FindFirst) method), and sometimes error messages.
  1. **The Hierarchy View** (middle-left) is only visible when in *Selector-Editing* mode, and shows the parent hierarchy of the UiNode matched by the currently edited selector.
  1. **The Ui-Tree View** (bottom-left) is used for navigating the UI hierarchy. You can easily see where the currently selected UiNode is located by turning on *highlight* from the applications toolbar. Double-clicking an element gets you into Selector-Editing mode.
  1. **The Selector Properties View** (top-right) is used for adding, removing or changing the attribute in a selector tag, when you have a selector to work with.
  1. **The Runtime Attributes View** (bottom-right) shows the current attribute values of the selected UiNode.

I'd like to mention once again, that if you have a selector available, you can start editing by entering it into the Edit Manually dialog.

## Selector editing capabilities

### Adding intermediate tags to a selector

Adding tags is useful when the element you are interested in is hard to identify within the whole app, but easy within the descendants of one of its parents. 
The example I'm using is taken from UiPath Studio; I'm interested in the name of the start-point of workflows (green highlight in first figure). 

<table style="table-layout: fixed; width: 100%; margin: 0.7em 0">
<thead>
<tr><td colspan=3><span style="display: block; text-align: center"><strong>Adding an ancestor to the selector</strong></td></tr>
</thead>

<tbody>
<tr>
<td style="width: 100%; padding: 1%"><img src="/img/blog/UiExplorer-Blog Intermediate-Tag text-view.gif" /></td>
<td style="width: 100%; padding: 1%"><img src="/img/blog/UiExplorer-Blog Intermediate-Tag weak.gif" /></td>
<td style="width: 100%; padding: 1%"><img src="/img/blog/UiExplorer-Blog Intermediate-Tag strong.gif" /></td>
</tr>

<tr>
<td><span style="display: block; text-align: center">Workflow-Startpoint text</span></td>
<td><span style="display: block; text-align: center">Generated Selector</span></td>
<td><span style="display: block; text-align: center">Improved Selector</span></td>
</tr>
</tbody>
</table>

The second image shows the selector generated by GetSelector. 
Notice it makes use of the *automationid* property, who's value is "TextBlock\_1". 
The value is likely to be dynamically generated, and is not a good fit for a resilient selector; so I replaced it with the *role* property.
Because nodes with a text role appear a lot, I've also added the node's parent to the selector, who's automationid property seems to have a value that will apear in all flowcharts.

### Modifying attributes

Elements will sometimes have attributes whose value changes; however, values will often have invariant parts. 
Notepad's title, for example, depends on the name of the file being edited, but always ends in " &mdash; Notepad". 
You can use "&ast;" to replace the transient part; this way, you could match it with: *title="&ast; &mdash; Notepad"*. 
Attribute values can be changed from the *Selector Properties* view (top-right); the current runtime attributes of the inspected node can be seen in the *Runtime Attributes* view (bottom-right).
UiExplorer makes sure the attribute value you provide matches that of the element.

### Automatically repairing selectors

The main use we see for UiExplorer is helping produce selectors for elements that change often. When an element changes state, UiExplorer can sometimes automatically repair its selector (if it no longer matches the intended node). For that, you can use *Selector &#8594; Repair Selector* &mdash; UiExplorer will try to improve the selector, without requiring any other input from the user. Unfortunatelly, there are cases where the visual aspect of the app remains the same, but the underlying node is programmatically changed, and the new node must be indicated on screen; if that is the case, you will be asked to interactively indicate it. If Improve Selector succeeded, a selector matching the two states of the element is created.

The following example is taken from [Yahoo Finance](http://finance.yahoo.com/q?s=AAPL); the shown price is updated in real time, to reflect the current unit value. 
The first image shows the selector obtained by interactivelly selecting the price field; it will become invalid after the first market change.
After chosing repair selector, UiExplorer is able to automatically find common patterns in the different states.

<table style="table-layout: fixed; width: 100%; margin: 0.7em 0">
<thead>
<tr><td colspan=2><span style="display: block; text-align: center"><strong>Repairing selectors in volatile apps</strong></td></tr>
</thead>

<tbody>
<tr>
<td style="width: 100%; padding: 1%"><img src="/img/blog/UiExplorer-Blog Improve-Selector fresh.gif" /></td>
<td style="width: 100%; padding: 1%"><img src="/img/blog/UiExplorer-Blog Improve-Selector break.gif" /></td>
</tr>
<tr>
<td><span style="display: block; text-align: center">Live price</span></td>
<td><span style="display: block; text-align: center">Selector became invalid</span></td>
</tr>

<tr>
<td style="width: 100%; padding: 1%"><img src="/img/blog/UiExplorer-Blog Improve-Selector menu.gif" /></td>
<td style="width: 100%; padding: 1%"><img src="/img/blog/UiExplorer-Blog Improve-Selector done.gif" /></td>
</tr>
<tr>
<td><span style="display: block; text-align: center">Repair Selector Option</span></td>
<td><span style="display: block; text-align: center">Selector after Repair</span></td>
</tr>
</tbody>
</table>



## UI exploration capabilities

### Usage of the Ui-Tree

UiExplorer enables you to easily explore the UI of an app by exposing its tree. 
You can tell where each UiNode is positioned on screen using the *highlight* option &mdash; when toggled on, it will draw a rectangle around the selected UiNode (if the node is visible). 
Most selectors you will ever edit will be ones returned by the GetSelector method, from nodes obtained using interactive selection. 
However, strangely positioned elements can throw off interactive selection, and you have to reach the node by navigating the tree.

### Filtering descendants

The nodes in the UI can also be easily filtered, by selecting one of the options in the drop-down of the view's toolbar (not the apps toolbar!). 
Filtering can be done using an attribute, or you can come up with your own selector. 
This allows you to see which nodes are similar (in terms of attributes) to the node you are interested in. 
If you wanted to see which were the nodes that matched the last tag's attributes, you could do it in the following way:

1. Select the top-level node of the application, and go into Selector-Editing mode
2. Open the Ui-Tree, and chose to filter elements using a selector
3. Choose to see all descending UiNodes &mdash; as already mentioned, these will be descendants of the UiNode's whose selector you are currently editing
4. Insert the last tag of the selector, after removing the idx attribute
5. Press the Find button

<table style="table-layout: fixed; width: 100%; margin: 0.7em 0">
<thead>
<tr><td colspan=3><span style="display: block; text-align: center"><strong>Listing all matching descendants</strong></td></tr>
</thead>

<tbody>
<tr>
<td style="width: 100%; padding: 1%"><img src="/img/blog/UiExplorer-Blog List-Children start.gif" /></td>
<td style="width: 100%; padding: 1%"><img src="/img/blog/UiExplorer-Blog List-Children parent.gif" /></td>
<td style="width: 100%; padding: 1%"><img src="/img/blog/UiExplorer-Blog List-Children result.gif" /></td>
</tr>

<tr>
<td><span style="display: block; text-align: center">The start node</span></td>
<td><span style="display: block; text-align: center">After removing the last tag</span></td>
<td><span style="display: block; text-align: center">Find all matching last tag</span></td>
</tr>
</tbody>
</table>

## That was it

Hopefully you now know what UiExplorer can do, and how you can use it.

Till next time,<br/>
Liviu


