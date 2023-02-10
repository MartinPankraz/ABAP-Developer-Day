# Quest 1 - Novice's path

## Introduction
Throughout the next excercises we will use an Office 365 user, an Azure subscription and some Office 365 tools. In order to make sure that everything is fine, it is best to quickly try and verify all used resources. 

* First open up your [Office 365 Outlook Inbox](https://outlook.office.com/mail/inbox/). Make sure to authenticate with your Developer Office 365 user, not with your corporate / personal user. One of the last emails in the Inbox should be the invitation email for the Contoso YAAC Azure subscription. 
![Check Outlook](Quest1/CheckOutlook.jpg)


* Next open [Microsoft Teams](https://teams.microsoft.com/). When doing that it is best to use the Browser experience (in order not to interfere with any other local Teams and Accounts that you might have installed). 

* Now check if you have Excel on-prem installed. Here you do not need to authenticate and it is fine to use the rich-client. In Quest 4 we will connect to an OData service and read data from our Online Store. 

* For the last exercise we are going to integrate in [Microsoft Word](https://www.office.com/launch/word?auth=2). You can open and create a Blank document from there. 

* For every participant a resource group in Azure has been created in a central Azure subscription. In order to not interfere with other users, please always make sure that you only operate in the resource group assigned to you. 

To log-on to the [Azure portal](https://portal.azure.com/#home), please use your Microsoft 365 users which has been assisnged as a contribute to the central Azure subscription. Make sure that the Directory shown in the upper right corner of your user says "Contoso YAAC". 
![Contoso YaaC](Quest1/ContosoYaac.jpg)

 and if not switch to this Directory, by clicking on your user, selecting Switch directory and then clicking on Switch for the Contoso YAAC Directory. 
![Switch Directory](Quest1/SwitchDirectory.jpg)

* And we have of course our Online Shop. Open xxx and create a first order! Screenshot
TODO-TODO-TODO-TODO-TODO-TODO-TODO-TODO-TODO-TODO-TODO-TODO-TODO-TODO-



### Create Logic Apps
1) In the Search bar at the top search for Logic Apps and click on the Logic Apps under Services
![Search Logic Apps](Quest1/SearchLogicApps.jpg)

2) Click on "+ Add" 
![Add Logic Apps](Quest1/AddLogicApps.jpg)

3) Enter the required Logic Apps information

|Property|Value|
|---|---|
|Resource Group|Select the resource group available to you, e.g. Developer002|
|Logic App Name|Use a unique name, e.g. Developer002-OrderItem|
|Region|Select North Europe|
|Plan Type|Select Consumption|

![Create Logic App](Quest1/CreateLogicApp.jpg)
 
4) then Click on Create + Review / Create 

5) Once the creation is done, click on "Go to resource"
![Create Logic App](Quest1/GoToResource.jpg)

### Logic Apps Designer
#### Setting up the Flow
1) On the welcome screen, select "When a HTTP request is received". In the first step we will trigger the Logic Apps flow manually, but later on the SAP system can trigger the request automatically when a new order has been created in the Online Shop. 
![Start with HTTP Trigger](Quest1/WhenHTTPRequest.jpg)

2) Click on "+ New Step" 
![New Step](Quest1/NewStep.jpg)

3) Search for and select "SAP" in the "Choose an operations" box. 
![Select SAP](Quest1/SelectSAP.jpg)
 
4) Select "[RFC] Call function in SAP (preview)" and select the on-prem data Gateway system. Also provide the SAP System detail information. 
![Call RFC](Quest1/CallFunctions.jpg)

5) Create the Connection using the following value. Then click on Create

|Property|Value|
|---|---|
|Connection name|Use a unique name, e.g. Dev002-SAPConnection|
|Connection Gateway|Select AKDevelopmentWithJohnWin4|
|Client|100|
|SAP Username|S4H_EXT|
|SAP Password|Welcome1|
|AS Host|10.0.0.34|
|AS Service|3200|
|AS System Number|00|

![Connection Details](Quest1/ConnectionDetails.jpg)

 
5) Click on the "Add new paramater" drop down and select "RFC Filter Group"
>> Note: You need to click "somewhere" else so that the dialog box closes.  
![RFC Filter Group](Quest1/RFCFilterGroup.jpg)

6) Fetching all the filter groups from the SAP system can take some time. You can also click on "Enter Custom Value"
![Enter Custom Value](/student/Quest1/LA-EnterCustomValue.jpg)

7) Now enter the Filter group name "Z_ONLINESHOP_MSF"
![Select Filter Group](/student/Quest1/SelectFilterGroup.jpg)

 8) and from the RFC Name field select the "ZF_RFC_ONLINESHOP_GET_ORDER:order:Z_ONLINESHOP_MSF"
![Enter Custom Value](/student/Quest1/LA-EnterRFCName.jpg)

8) Since we want to lookup a specific Order, we need to provide the IM_ORDER number. For the first test, just enter which looks for Order ID 12 (feel free to select a number of an order that you have previously created!)
``` 
<ZF_RFC_ONLINESHOP_GET_ORDER xmlns="http://Microsoft.LobServices.Sap/2007/03/Rfc/">
<IM_ORDER>       12</IM_ORDER>
</ZF_RFC_ONLINESHOP_GET_ORDER>
```
![Call SAP Function](/student/Quest1/LA-CallSAPFunction.jpg)
>> Note: See [How to create the Logic App Content](LogicAppContent.md) for more details on how to construct the XML playload for the RFC request in Logic Apps

9) From the top of the flow click on "Save" and "Run Trigger" to execute the Flow. 
![Save and run](/student/Quest1/SaveAndRun.jpg)

10) As a result you will see the OrderID and some additional information coming back from the SAP System. 
![First results](/student/Quest1/FirstResults.jpg)
>>Note: You can Copy results from the Json Response to your Clipboard, since we will use it in the next steps. 

#### Working with the results from SAP
1) With the results from the RFC call, we can now take this data and use it to publish to Teams. The first step is that we need to parse the results from the RFC call. Click on the "+ New Step" 
![Open Action](/student/Quest1/NewStepParse.jpg)

2) Type "Parse JSON" and select the Parse JSON Action.
![Open Action](/student/Quest1/SelectParseJson.jpg)

3) Select the Content Field and from the Dynamic content pane on the right select "JsonResponse" Then click on "Use sample payload to generate schema". This allows us to leverage the output of the RFC call from the SAP system and generate a Schema directly out of it. 
![Open Action](/student/Quest1/ContentField.jpg)

2) Next we can paste the sample payload from the first run. 
```json
{
  "EM_ORDER": [
    {
      "CLIENT": "",
      "ORDERUUID": "44g8idhTHt2oxgLO5ZOh1w==",
      "ORDERID": "00000012",
      "ORDEREDITEM": "Laptop",
      "PURCHASEREQN": "0010001417",
      "PRSTATUS": "In Process",
      "CREATED_AT": 0,
      "CREATED_BY": "",
      "LAST_CHANGED_BY": "",
      "LAST_CHANGED_AT": 0,
      "LOCAL_LAST_CHANGED_AT": 0
    }
  ]
}
```
![Paste Sample](/student/Quest1/PasteSample.jpg)

3) The Parse JSON Action should now look like this:
![Paste Sample](/student/Quest1/ParseJsonResults.jpg)

#### Create Adaptive Card in Teams
1) In order to create an Adaptive Card to Teams, we now need to add another Action. As before, click on "+ New Step" and search for "Post adaptive card". Select the "Post adative card in a chat or channel"
![Open Action](/student/Quest1/PostAdaptiveCard.jpg)

1) Click on the "Sign-In" button, to Sign-in with your Office 365 user in Teams
![Open Action](/student/Quest1/TeamsSignIn.jpg)

1) Select your User and Authenticate
![Open Action](/student/Quest1/SelectUser.jpg)

1) In the "Post in" drop-down select "Channel"
![Open Action](/student/Quest1/SelectChannel.jpg)

1) Select a Team and Channel (just remember which combination you selected)
![Open Action](/student/Quest1/SelectTeamsAndChannel.jpg)

1) Copy and paste the Adaptive Card 
We have used the [Adaptive Card Designer](https://www.adaptivecards.io/designer/) to create an Adaptive Card. Copy and paste the content from below in the Adaptive Card field. 


```
{
    "type": "AdaptiveCard",
    "body": [
        {
            "type": "TextBlock",
            "size": "Medium",
            "weight": "Bolder",
            "text": "New Order has been placed in the Online Shop"
        },
        {
            "type": "ColumnSet",
            "columns": [
                {
                    "type": "Column",
                    "items": [
                        {
                            "type": "TextBlock",
                            "weight": "Bolder",
                            "text": "Hi USER",
                            "wrap": true
                        },
                        {
                            "type": "TextBlock",
                            "spacing": "None",
                            "text": "Created {{DATE(${createdUtc},SHORT)}}",
                            "isSubtle": true,
                            "wrap": true
                        }
                    ],
                    "width": "stretch"
                }
            ]
        },
        {
            "type": "TextBlock",
            "text": "A new order has been place in the Online Shop URL. Please review the provided information. ",
            "wrap": true
        },
        {
            "type": "FactSet",
            "facts": [
                {
                    "title": "Order ID",
                    "value": "${value}"
                },
                {
                    "title": "Status Purchase Requisition",
                    "value": "${value}"
                },
                {
                    "title": "Ordered Items",
                    "value": "${value}"
                },
                {
                    "title": "Quantity",
                    "value": "${value}"
                },
                {
                    "title": "Description Text: ",
                    "value": "${value}"
                },
                {
                    "title": "Purchase Requisition: ",
                    "value": "${value}"
                }
            ]
        }
    ],
    "actions": [
        {
            "type": "Action.OpenUrl",
            "title": "View",
            "url": "URL"
        }
    ],
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.3"
}
```
![Open Action](/student/Quest1/CopyPasteAdaptiveCard.jpg)

>> Note: Feel free to switch over to the Adaptive Card Designer and create your own Adaptive Card. Just make sure that you have several "placeholder" for Order ID, Status Purchase Requisition, Ordered Items and Purchase Requisition. 

1) The adaptive Card has some hard coded "placeholder" in it. Find the place for Order ID and replace the "${value}" with the Dynamic Content from the Parsed JSON response from the SAP System. 
Do the same for the other properties (we are not yet updating Description Text or Quantity since these values are not returned by the RFC). 
* Order ID
* Status Purchase Requisition
* Ordered Items
* Purchase Requisition
![Replace Placeholder](/student/Quest1/ReplacePlaceholders.jpg)

>>Note: You might have seen that when adding the first OrderID dynamic variable, the Adaptive Card was put in a "For each" loop. That is because the schema of the resulting Json returns an array and potentially we could get multiple Orders back when filtering for a specific Order ID. 
![For each section](/student/Quest1/ForEach.jpg)


1) Now we can Save and Run the Logic App flow again. As a result you should see an Adapitve Card in Teams showing the information from the Order specified. 
![For each section](/student/Quest1/ResultsInTeams.jpg)
>>Note: If not all properties are shown, go back to the Teams action in the Logic Apps and validate the configuration. 





>> Note: You can find a template for the Logic App flow (you still need to adjust to your specific SAP Connector and Teams connection) [here](Quest1/Quest1-LogicApp-CodeView.json)





[< The Journey](thejourney.md) - **[🏠Home](../README.md)** - [ Quest 2 >](quest2.md)



