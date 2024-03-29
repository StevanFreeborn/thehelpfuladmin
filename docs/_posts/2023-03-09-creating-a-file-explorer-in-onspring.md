---
layout: post
title: 'Creating A File Explorer in Onspring'
date: 2023-03-09 00:00 -0500
tag: reports, documents, files
published: true
---

| [![thumbnail](/thehelpfuladmin/assets/2023-03-09-creating-a-file-explorer-in-onspring/play-creating-a-file-explorer-in-onspring.png)](https://youtu.be/01whmghT-yg) |
| :-----------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|                                                            _Click above to watch a video demonstration_                                                             |

## Introduction

Onspring is great for organizing all types of data and information. It can be particularly helpful for organizing documents which doesn't necessarily mean storing your documents in Onspring, but it can.

This is because Onspring lets you add all types of contextual metadata around each document and reference that document in all sorts of different places through your Onspring instance.

However as the number of documents you are maintaining in Onspring increases you might begin to wonder how you can best organize and visualize the documents.

And a lot of people I think will lean towards wanting to organize things in a traditional file system structure. Where you have folders and then inside those folders you have files and/or other folders.

Building this type of structure in Onspring though might not seem readily apparent so I want to offer my approach to configuring this type of solution in Onspring.

### Create a Folders app

The first thing you will need to do is create a new app. This app will be used to create records that will act as Folders in your solution. These folder records will be what you relate your document records to in order to simulate putting a file in a folder as you would in a traditional file system.

#### Steps

1. Navigate to the admin panel
2. Click on the **Apps** tile
3. Click the **Create App** button
4. Name the app **Folders** and click **Continue**
5. Modify the app layout to your liking and click **Save**
6. Add a **Text** field to the app and name it **Name**
7. Add a **Reference** field to the app and name it **Parent Folder**
   - Set the **Reference** field to reference the **Folders** app
   - Set the **Reference** field's Selection Type to **Single Select**
8. Rename the newly created **Parallel Reference** field called **Folders** to **Child Folders**
9. Add both the **Name**, **Parent Folder**, and **Child Folders** fields to the app's layout
10. Click **Save and Close**
11. Navigate to the **General** tab of the **Folders** app
12. Change the Display Link Field to **Name**

### Create a Report with Related Data

Next you will want to create a report that will display the folders records and their child folders records in a hierarchical structure. This report will be used to navigate through the folders and their child folders.

#### Steps

1. Navigate to the **Reports** tab
2. Click the **Create Report** button
3. Select the **Folders** app
4. Create the report as a saved report and name it **Folders with Documents**
5. Click on the **+** button next to the **Folders** field
6. Select the **Child Folders** field and click **Add**
7. Click **Save Changes and Run**

**Note:** You will likely want to add a filter to this report so that you can only see the top level folders. You can do this by adding a filter to the report and that states **Parent Folder Is Empty**.

### Connect Documents app to Folders app

Now that you have a report that displays the folders and their child folders you will want to connect the **Documents** app to the **Folders** app so that you can relate documents to folders. This will allow you to simulate putting documents in folders.

#### Steps

1. Navigate to the admin panel
2. Click on the **Apps** tile
3. Click on the **Documents** app
4. Add a **Reference** field to the app and name it **Folder**
   - Set the **Reference** field to reference the **Folders** app
   - Set the **Reference** field's Selection Type to **Single Select**
5. Add the **Folder** field to the app's layout
6. Click **Save and Close**
7. Navigate to the **Folders** app
8. Rename the **Parallel Reference** field from the **Documents** app to **Documents**
9. Add the **Documents** field to the app's layout

### Add Documents to Report with Related Data

Now that you have connected the **Documents** app to the **Folders** app you will want to add the **Documents** field to the **Folders with Documents** report so that you can see the documents that are related to each folder. You can do this for both the top level folders and the child folders.

#### Steps

1. Navigate to the **Reports** tab
2. Click on the **Folders with Documents** report
3. Click on the **+** button next to the **Folders** field
4. Select the **Documents** field and click **Add**
5. Click **Save Changes and Run**

**Note:** You can repeat this same action for the **Child Folders** field if you want to see the documents that are related to the child folders as well from the **Folders with Documents** report.

### A File Explorer in Onspring

When creating documents in Onspring you can now relate them to a folder. And you can use the **Folders** app to create folders and relate them to other folders to create a hierarchical structure.

Then you can use the **Folders with Documents** report to navigate through the folders and their child folders and see the documents that are related to each folder.

I hope this is helpful and makes your job as an admin just a little easier.
