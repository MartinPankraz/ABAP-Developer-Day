# Quest 1 - Novice's path

## Introduction
For every participant a resource group has been created in a central Azure subscription. In order to not interfere with other users, please always make sure that you only operate in the resource group assigned to you. 

To log-on to the Azure portal, please use your Microsoft 365 users which has been assisnged as a contribute to the central Azure subscription. 

### Create Logic Apps
1) Click on the "*" Sign and search for Logic Apps

2) Click on Logic Apps

3) Enter the required Logic Apps information
- Resoruce Group Name
- 

### Logic Apps Designer
#### Setting up the Flow
1) On the welcome screen, select "When a HTTP request is received". In the first step we will trigger the Logic Apps flow manually, but later on the SAP system can trigger the request automatically when a new order has been created in the Online Shop. 

2) Click on "+ New Step" 

3) Search for and select "SAP" in the "Choose an operations" box. 
 
4) Select "[RFC] Call Functions" and select the on-prem data Gateway system. Also provide the SAP System detail information. 
 
5) Click on the "Add new paramater" drop down and select "RFC Filter Group"
>> Note: You need to click "somewhere" else so that the dialog box closes.  

6) Fetching all the filter groups from the SAP system can take some time. You can also click on "Enter Custom Value"
![Enter Custom Value](/student/Quest1/LA-EnterCustomValue.jpg)

7) Now enter the Filter group name "Z_ONLINESHOP_MSF" and from the RFC Name field select the "ZF_RFC_ONLINESHOP_GET_ORDER:order:Z_ONLINESHOP_MSF"
![Enter Custom Value](/student/Quest1/LA-EnterRFCName.jpg)

8) Since we want to lookup a specific Order, we need to provide the IM_ORDER number. For the first test, just enter: 
``` 
<ZF_RFC_ONLINESHOP_GET_ORDER xmlns="http://Microsoft.LobServices.Sap/2007/03/Rfc/">
<IM_ORDER>       12</IM_ORDER>
</ZF_RFC_ONLINESHOP_GET_ORDER>
```
![Call SAP Function](/student/Quest1/LA-CallSAPFunction.jpg)
>> Note: See [How to create the Logic App Content](LogicAppContent.md) for more details on how to construct the XML playload for the RFC request in Logic Apps

9) From the top of the flow click on "Save" and "Run Trigger" to exectue the Flow. 

#### Working with the results from SAP
1) Parse Json
![Open Action](/student/Quest1/PraseJson.jpg)

2) Paste Sample
![Paste Sample](/student/Quest1/PasteSample.jpg)

3) Add Content
--- Update Screenshot
![Paste Sample](/student/Quest1/SelectJsonResponse.jpg)

#### Create Adaptive Card in Teams
1) Add new Action
![Open Action](/student/Quest1/PostAdaptiveCard.jpg)

1) Sign-In Teams
![Open Action](/student/Quest1/TeamsSignIn.jpg)

1) Select User and Authentiate
![Open Action](/student/Quest1/SelectUser.jpg)

1) Select Channel
![Open Action](/student/Quest1/SelectChannel.jpg)

1) Select Teams and Channel
![Open Action](/student/Quest1/SelectTeamsAndChannel.jpg)

1) Copy and paste the Adaptive Card 
We have used the [Adaptive Card Designer](https://www.adaptivecards.io/designer/) to create an Adaptive Card. Copy and paste the content from below in the Adaptive Card field. 
![Open Action](/student/Quest1/xxx)


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

1) Replace the placeholders with the variables and results from the SAP RFC call.   
* Order ID
* Status Purchase Requisition
* Ordered Items
* Purchase Requisition
![Open Action](/student/Quest1/xxx)

1) Trigger and run the Logic Apps Flow.










**[🏠Home](./README.md)** - [ Quest 2 >](quest2.md)

