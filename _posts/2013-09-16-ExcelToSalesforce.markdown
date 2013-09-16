---
layout: layout
title: "Application Integration activities - scenario"
---
##Overview
UiPath Studio comes with built in application integration activities that allow data sharing between multiple applications. We’ll expand this concept by showing how to export data stored in an excel file to an online CRM.



##Problem
Working in a multi-application world we’re often faced with having to transfer data from one to another. You might have it locally stored into a file, or in an email but, more often than not, a time comes when you’ll want it integrated into a platform. Even if you’re in a happy case of working with applications that can import and export files, they usually require knowledge about how the data is actually packed and it can lead to information loss or mix-up. 

Let’s say that once a month you receive an updated excel file of contacts information. It is your task to store them to your Salesforce account. By designing a workflow to model your actions, you’ll be able to run it anytime you’re required to update the online list.


##How to
Our scenario for today will walk us through the steps of getting that excel data into your Salesforce account. 

We’ve created Excel activities because Excel is the first thought that comes to mind when you need to store structured or tabular data. All of these activities work with the Excel application by opening or creating a file or by using a workbook already opened. We’ve kept the basic concepts needed to read and store data from and to a file (sheet, range, cell) so you’ll be able to work with Excel from your workflow in no time. If you plan on using them on your computer you must have an edition of Microsoft Excel software installed.

### Log-in to Salesforce

Firstly we need to get to the contacts view, where we’ll add the new data.  The clearest way of doing this is by opening a web page with the Salesforce address, logging into the account by typing your username and password and then clicking the contacts tab. These steps can be recorded using *Record* feature which will add the corresponding activities to the workflow. Pretty easy, right? In fact, you can wrap the login actions in a separate workflow and use it anytime you are working with Salesforce.

### Read data from Excel

Now, we are able to input the required contact information.  We’ll use *Read Cells Range* activity to get the data from the excel file. Along with the workbook file path and sheet name, the next important property is the *Range* property. It specifies the upper left cell and the lower right cell of the spreadsheet. All the cell values between these start and end points are included in the returned data. This is mapped into a *DataTable* output, preserving the columns and rows order.  We can also check the *IncludeColumnNames* property, which instructs the activity that the first row in the range represents the column names. In this case, the *DataTable* result will have columns with names set from the values in the first row.  The output can be used by other activities such as *Write CSV* File or *Insert DataTable* into a Database.

### Write data to Salesforce

In our example, a contact is known by its First Name, Last Name, Account, Email and Phone information. These are all columns in the spreadsheet and will be columns in the result data table. Each contact will then be a row. We access a contact’s information by taking the values for every column (either by index or by name) and we type it into its corresponding web input. 


With this in mind, we add Type activities for each column passing the cell’s value to the correct input control. Having the form filled in, we click ‘Save & New’, waiting for the page to reload so we can go to the next contact. 

These are the steps for one contact. To have all of the contacts created and saved, we’ll put them into a *Foreach* activity, which passes each row to our steps. And since every UI operation is done inside the *New Contact page*, we’ll add a *Within Parent* activity, using the returned *UiElement* as the parent for all the activities that work with the Contact form. 

##Conclusions

We now have a workflow that can be used every time you need to update your contact list. It can be modified to work with other type of entities within different platforms. You can use CSV files, or you can retrieve the data from your account, add it to a DataTable and write it in a Database. 

All you have to do is run the workflow and enjoy your coffee.

Lavinia

