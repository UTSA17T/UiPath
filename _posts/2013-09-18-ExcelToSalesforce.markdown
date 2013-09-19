---
layout: layout
title: "Application Integration &mdash; Import Excel scenario"
---
##Overview

UiPath Studio provides a solution for [integrating applicaitons](https://github.com/Deskover/UiPath/wiki/Workflow-activities#wiki-App_Integration) that were not specifically designed to collaborate. 
We’ll expand this concept by showing how you can export data stored in an excel file to an online CRM.


##Problem

Working in a multi-application world, we’re often faced with having to transfer data between them. 
You might have it locally stored into a file, or in an email but, more often than not, a time comes when you’ll want it integrated into a platform. 
Even if you’re in the fortunate situation of working with applications that can import and export files this can lead to information loss or mix-up and 
you'll have to repeat the integration steps every time you need to sync the data. 

Let’s say that once a month you receive an updated excel file of contacts information. It is your task to store them to your Salesforce account.
By designing a workflow to model your actions, you’ll be able to run it anytime you’re required to update the online list.


##How to

We’ve created [Excel activities](https://github.com/Deskover/UiPath/wiki/Workflow-activities#wiki-Excel) because Excel is the first thought that comes to mind when you need to store structured or tabular data. 
All of these activities work with the Excel application by opening or creating a file or by using a workbook already opened. 
We’ve kept the basic concepts needed to read and store data from and to a file (sheet, range, cell) so you’ll be able to work with Excel from your workflow in no time.
If you plan on using them on your computer you must have an edition of Microsoft Excel software installed.

Our scenario for today will walk us through the steps of getting that excel data into your Salesforce account. 

### Log-in to Salesforce

Firstly we need to get to the contacts view, where we’ll add the new data.
The clearest way of doing this is by opening a web page with the Salesforce address, 
logging into the account by typing your username and password and then clicking the contacts tab. 
These steps can be recorded using *Record* feature which will add the corresponding activities to the workflow. 
Pretty easy, right? In fact, you can wrap the login actions in a separate workflow and use it anytime you are 
working with Salesforce<sup>[\[1\]](#salesforce-workflow)</sup>.

### Read data from Excel

Now, we are able to input the required contact information.  We’ll use [*Read Cells Range*](https://github.com/Deskover/UiPath/wiki/Workflow-activities#readcellsrange) activity to get the data from the excel file. 
Along with the **WorkbookPath** and **SheetName**, the next important property is the **Range** property. It specifies the upper left cell and the lower right cell of the spreadsheet. 
All the cell values between these start and end points are included in the returned data. 
This is mapped into a *DataTable* output, preserving the columns and rows order. 
We can also check the **IncludeColumnNames** property, which instructs the activity that the first row in the range represents the column names.
In this case, the *DataTable* result will have columns with names set from the values in the first row.
The output can be used by other activities such as [*Write CSV*](https://github.com/Deskover/UiPath/wiki/Workflow-activities#wiki-WriteCsvFile) file or [*Insert DataTable*](https://github.com/Deskover/UiPath/wiki/Workflow-activities#dbinsertdatatable) into a Database.

!["Read cells range"](/img/blog/ExcelRead.png)

### Write data to Salesforce

In our example, a contact is known by its First Name, Last Name, Account, Email and Phone information. 
These are all columns in the spreadsheet and will be columns in the result data table, that can be accessed either by index or by name.
Each contact will then be a row. With every contact, we need to get the required columns and fill in the New Contact form.


!["Type email"](/img/blog/Type Email.jpg)

With this in mind, we add [*Type*](https://github.com/Deskover/UiPath/wiki/Workflow-activities#wiki-TypeInto) activities
for each column passing the cell’s value to the correct input control. 
Having the form filled in, we click ‘Save & New’, waiting for the page to reload so we can go to the next contact. 

These are the steps for one contact. To have all of the contacts created and saved, we’ll put them into a [*Foreach*](http://msdn.microsoft.com/en-us/library/dd647676%28v=vs.100%29.aspx)<sup>[\[2\]](#salesforce-workflow)</sup>
activity, which passes each row to our steps. And since every UI operation is done inside the *New Contact page*, we’ll add a [*Within Parent*] (https://github.com/Deskover/UiPath/wiki/Workflow-activities#wiki-WithinUiElement),
using the returned *UiElement* as the parent for all the activities that work with the contact form. 

<table style="table-layout: fixed; width: 100%; margin: 0.7em 0" id="salesforce-workflow">
<tbody>
<tr>
<td style="width: 100%; padding: 1%; vertical-align: top"><img src="/img/blog/LoginToSalesforce.jpg" style="margin: auto; display: block"/></td>
<td style="width: 92%; padding: 1%; vertical-align: top"><img src="/img/blog/InputAllContacts.jpg" style="margin: auto; display: block"/></td>
</tr>
</tbody>
</table>

##Conclusions

We now have a workflow that can be used every time you need to update your contact list.
It can be modified to work with other type of entities within different platforms. 
You can use CSV files, or you can retrieve the data from your email or from another online account.  

You can find the entire workflow along with its additional resources in the [ImportExcelData](https://github.com/Deskover/UiPath/tree/master/Samples/Workflow/Workflow%20samples) workspace.

All you have to do is run the workflow and enjoy your coffee.

Lavinia

