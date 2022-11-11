---
layout: post
title:  "Getting Onspring App Data in Power BI"
date:   2022-11-11 8:00:00 -0500
tag: data-integration
published: true
---

 | [![thumbnail](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/thumbnail.png)](https://youtu.be/A1XKJhS47rQ) |
 |:--:|
 |*Click above to watch a video demonstration*|

### Introduction

[Onspring](https://onspring.com/) has a really powerful feature set when it comes to data visualization and reporting. The usage of and access to these features within [Onspring](https://onspring.com/) though may be limited to only a small group of users within your company. Of course you could always fix this by purchasing more licenses for users in your company, but I know that isn't something that is always possible. Plus there is always the chance that other teams just prefer using different sets of tools to facilitate their workflows and reporting needs other than [Onspring](https://onspring.com/).

However given how much work these days is done cross-functionally it stands to reason there will arise situations where a user or team that doesn't have access to [Onspring](https://onspring.com/) needs to incorporate your dataset in [Onspring](https://onspring.com/) into their toolset to enhance their workflow or reporting. And a common tool that I've seen come up in this regards is [Power BI](https://powerbi.microsoft.com/en-us/). Specifically how to go about getting datasets from [Onspring](https://onspring.com/) reports or apps into [Power BI](https://powerbi.microsoft.com/en-us/) without the need to continually export the data from [Onspring](https://onspring.com/) and import it into [Power BI](https://powerbi.microsoft.com/en-us/).

I'd like to help show you how this can be done pretty easily using Onspring's API and the existing capabilities within [Power BI's](https://powerbi.microsoft.com/en-us/) power query editor. I'll specifically demonstrate here how you can do this with a report in [Onspring](https://onspring.com/), but if you really need to get the entire dataset from an app into [Onspring](https://onspring.com/) then you can keep an eye out for a future walk through of how you can do just that in more detail.

### Setting Up Onspring

#### Get Report Id and App Name

The first thing you want to do is identify the report in [Onspring](https://onspring.com/) that you actually want to load into [Power BI](https://powerbi.microsoft.com/en-us/). The reason for this is you will need to identify the app from which this report draws it's information so that you can create an [Onspring API](https://api.onspring.com/swagger/index.html) key with the proper permissions to read records from that app as well as locate the report's unique id. You will need this unique id to identify which report's data you want to request from [Power BI](https://powerbi.microsoft.com/en-us/) using the [Onspring API](https://api.onspring.com/swagger/index.html).

The easiest way to get this information is to navigate to the report within [Onspring](https://onspring.com/) and look at the breadcrump trail in the top left corner to find the name of the app from which the report's data originates and then look at the report's url in your browser to locate the report's unique id.

![app-name-report-id-example](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/app-name-report-id-example.png)

#### Create A Role

Now that we have this information we will want to create a role that has access to atleast read access to the content records in this app. Note if your report contains reference fields to other apps in [Onspring](https://onspring.com/) such as the Users app as my report does in this example then you'll also want the role you create to have read access to referenced records in that app.

![role-example-1](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/role-example-1.png)

![role-example-2](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/role-example-2.png)

#### Create An Api Key

We can now assign this role to the api key we create. See below for an example.

![api-key-example](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/api-key-example.png)

#### Look Up Onspring API Endpoint

And the final piece of information we need to collect before setting up our query in [Power BI](https://powerbi.microsoft.com/en-us/) is exactly how to request a specific report's data through the [Onspring API](https://api.onspring.com/swagger/index.html). The best resource for this is [Onspring's swagger page](https://api.onspring.com/swagger/index.html) which offers documentation on the available endpoints and how to use them. The endpoint you will want to look at specifically is the `/Reports/id/{reportId}` endpoint.

![report-endpoint-example](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/report-endpoint-example.png)

This endpoint has a couple of options you can pass to it to change the format of the data returned as well as what data from the report is returned such as the report's chart data instead of the report data if the report is configured as a chart also. The big take away here though is that we will need to setup our [Power BI](https://powerbi.microsoft.com/en-us/) query to request our reports data using the following URL.

```text
GET https://api.onspring.com/Reports/id/{reportId}
```

Okay so now we have the following information:

+ The id of the report who's data we want to query
+ An api key with the proper permissions to view the report's data
+ The correct endpoint we need to request the reports data from Power BI

We can now move to [Power BI](https://powerbi.microsoft.com/en-us/) and get our query setup.

### Setting Up Power BI

#### Create Initial Web Query

Start by opening a new [Power BI](https://powerbi.microsoft.com/en-us/) file, click `Get data`, and choose the web option. This should present you with a modal asking you to enter where you want to get the data from on the web.

![get-data-power-bi-1](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/get-data-power-bi-1.png)

![get-data-power-bi-2](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/get-data-power-bi-2.png)

On this modal you want to choose the advanced option and then complete the form by entering the proper URL for your report using the [Onspring API](https://api.onspring.com/swagger/index.html) as well as enter your api key as the value for the x-apikey header so that the [Onspring API](https://api.onspring.com/swagger/index.html) can properly authorize the request from [Power BI](https://powerbi.microsoft.com/en-us/).

![get-data-power-bi-3](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/get-data-power-bi-3.png)

Once you've completed this form you should then click on the `OK` button. This will ask [Power BI](https://powerbi.microsoft.com/en-us/) to make a request to the URL you entered along with the headers you entered. If all the information you collected from [Onspring](https://onspring.com/) is correct and the role you assigned the api key has the correct permissions the request should be successful and PowerBI should display the response from the request in a records view.

![get-data-power-bi-4](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/get-data-power-bi-4.gif)

At this point now the data for your report is loaded into [Power BI's](https://powerbi.microsoft.com/en-us/) query editor as a record and you can transform that record and the response data using any of the normal [Power BI](https://powerbi.microsoft.com/en-us/) features you are use to. For reference the report data is initially structured like the following:

```json
{
  "columns": [
    "string"
  ],
  "rows": [
    {
      "recordId": 0,
      "cells": [
        "string"
      ]
    }
  ]
}
```

#### Parameterize Query

Before though I get to walking through the way I think is easiest to transform the data into a typical table style [Power BI](https://powerbi.microsoft.com/en-us/) data set I want to walk you through modifying the [Power BI](https://powerbi.microsoft.com/en-us/) query to make it re-usable and more modular so that in the future you don't necessarily need to walk through this query setup process each time you want to get data for a different report or perhaps add a query for data from another report.

The first thing I will do is rick click on the query and rename the query to something a bit more descriptive like `Get Onspring report data` such as the following:

![rename-query-example](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/rename-query-example.png)

Next I will add a parameter to the query for each aspect of the request to the [Onspring API](https://api.onspring.com/swagger/index.html) endpoint that you need or may want to send in the request. For the `/Reports/id/{reportId}` endpoint you will want to add a query parameter for each of the following:

+ Report Id
+ ApiKey
+ ApiDataFormat
+ DataType

Below is an example of how I add each of these parameters to the query:

##### Report Id

![report-id-parameter-example](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/report-id-parameter-example.png)

##### ApiKey

![api-key-parameter-example](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/api-key-parameter-example.png)

##### ApiDataFormat

![api-data-format-parameter-example](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/api-data-format-parameter-example.png)

##### DataType

![data-type-parameter-example](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/data-type-parameter-example.png)

Okay so now that I have all the parameters added that I will use in the query's request to [Onspring](https://onspring.com/) that I might want to be able to change as needed I will modify the [Power BI](https://powerbi.microsoft.com/en-us/) query to reference these parameter value. I've found the easiest way to do that is to open up the `Advanced Editor` in [Power BI](https://powerbi.microsoft.com/en-us/). The existing query should look like the following within the `Advanced Editor`.

![existing-query-advanced-editor-example](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/existing-query-advanced-editor-example.png)

Now I will modify the query to reference the parameters I created. I'll also modify the information we are passing to the `Web.Contents()` method so that if or when the dataset is published to the [Power BI](https://powerbi.microsoft.com/en-us/) service the dataset can have an automatic refresh scheduled.

Below is the modified query that references the parameters and will allow for an automatic refresh when published to the [Power BI](https://powerbi.microsoft.com/en-us/) service.

{% raw %}

```text
let
    Source = Json.Document(Web.Contents("https://api.onspring.com", 
    [
        RelativePath="Reports/id/" & ReportId,
        Query=[apiDataFormat= ApiDataFormat, dataType=DataType],
        Headers=[#"x-apikey"=ApiKey, #"x-api-version"="2"]
    ])),
    #"Converted to Table" = Table.FromRecords({Source}),
    #"Changed Type" = Table.TransformColumnTypes(#"Converted to Table",{{"columns", type any}, {"rows", type any}})
in
    #"Changed Type"
```

{% endraw %}

Note the reason again for modifying how we are passing the request information to the `Web.Content()` method is so that we can just use the parameters in our query to enter the proper report id, api key, etc. and also so that the data set can be scheduled for an automatic refresh if and when the data set is published to the [Power BI](https://powerbi.microsoft.com/en-us/) service.

After making these changes to the query the query will initially be broken, but once you enter values for each of the parameters we created the query should once again return the report's data.

Now that we've parameterized the query we are going to create a custom function out of the query so that we can reuse it as we'd like in other queries. You'll do this by right clicking on the existing `Get Onspring report data` query and clicking on `Create Function` and then giving the function a name.

![create-function-example](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/create-function-example.png)

You can then use that function with whatever parameters values you need to and invoke it to create a query that gets the report of your choosing.

![function-invoke-example](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/function-invoke-example.png)

#### Transforming Query Response Data

Alright now back to transforming the response data into a more standard [Power BI](https://powerbi.microsoft.com/en-us/) table data set. What I've found the easiest to do is to navigate through the response data to the `rows` list.

![navigate-to-rows-list-example](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/navigate-to-rows-list-example.png)

Next I'll covert this list of records to a table.

![rows-list-to-table-1](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/rows-list-to-table-1.png)

![rows-list-to-table-2](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/rows-list-to-table-2.png)

I'll then expand the lone column into a `recordId` and `cells` columns.

![expand-column-1-example](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/expand-column-1-example.png)

I'll extract the `cells` column values delimiting based on a comma.

![extract-values-1](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/extract-values-1.png)

![extract-values-2](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/extract-values-2.png)

And finally I'll split the `cells` column by a comma delimiter to give me each field in it's own column.

![split-column-1](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/split-column-1.png)

![split-column-2](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/split-column-2.png)

From here I'll typically rename each column to the proper corresponding field name and change the type of the column to align with the data in the column. This step could likely be made dynamic by using a duplicate query and collecting the column names from the response, but reports in [Onspring](https://onspring.com/) are usually pretty static so it seems feasible to just update the query whenever the report in [Onspring](https://onspring.com/) is updated.

![rename-change-type-example](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/rename-change-type-example.png)

From here you should be able to close and apply the query and see a standard [Power BI](https://powerbi.microsoft.com/en-us/) table data set that you can use to build visualizations from.

![final-data-set-example](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/final-data-set-example.png)

### Publishing Dataset To Power BI Service

The real benefit though a lot of times in setting up a [Power BI](https://powerbi.microsoft.com/en-us/) query like this is to make the dataset that the query produces available for others to explore and create visualizations with and have that dataset refreshed regularly by publishing the query to the [Power BI](https://powerbi.microsoft.com/en-us/) service.

#### Publishing

To publish the query I just created I'll save my [Power BI](https://powerbi.microsoft.com/en-us/) file locally first. I'll then click `File`, `Publish`, and `Publish to Power BI`. I'll select the proper workspace and click `Select`. The query should then be published successfully.

![publish-1](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/publish-1.png)

![publish-2](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/publish-2.png)

#### Configure Credentials

I'll next navigate to published dataset on the [Power BI](https://powerbi.microsoft.com/en-us/) service and go to it's settings.

![data-set-settings](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/data-set-settings.png)

Here I will first configure the credentials for the dataset. By clicking on `Data source credentials` and then on `Edit credentials`. On the resulting modal I'll set the Authentication method to `Anonymous` and select the appropriate privacy level for the data source. Also make sure to select the option to `Skip test connection`.

![data-set-credentials](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/data-set-credentials.png)

#### Setup Scheduled Refresh

Now you can setup the scheduled refreshes of the data set by clicking on `Scheduled refresh` and configuring the appropriate frequency for your needs.

![data-set-scheduled-refresh](/thehelpfuladmin/assets/2022-11-11-onspring-report-data-in-power-bi/data-set-scheduled-refresh.png)

This will now allow the [Power BI](https://powerbi.microsoft.com/en-us/) service to make a request to the [Onspring API](https://api.onspring.com/swagger/index.html) on the frequency you've configured to get the last data for the given Onspring report. And that data will be available in [Power BI](https://powerbi.microsoft.com/en-us/) for reporting and visualization purposes.

### Summary

With the information above you should now be all set to regularly and rather easily automatically import data from [Onspring](https://onspring.com/) reports into [Power BI](https://powerbi.microsoft.com/en-us/). And while that will likely satisfy most needs there may be a time when loading your [Onspring](https://onspring.com/) record data into [Power BI](https://powerbi.microsoft.com/en-us/) via report just isn't feasible such as when you are dealing with an extremely large dataset. In these cases it will be necessary to make use of some other [Onspring API](https://api.onspring.com/swagger/index.html) endpoints and a little bit more complex [Power BI](https://powerbi.microsoft.com/en-us/) query to get at the data you want. I plan to cover this in a future walkthrough in detail so keep an eye out for that.

I hope this is helpful and makes your job as an admin just a little easier.
