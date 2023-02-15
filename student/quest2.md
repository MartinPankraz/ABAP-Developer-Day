# Quest 2 - Apprentice's curious road

[< Quest 1](quest1.md) - **[🏠Home](../README.md)** - [ Quest 3 >](quest3.md)

Now that we have the Logic App in place to fetch data from the SAP system and create an Adaptive Card in Teams, we want to trigger this flow automatically from the SAP System once a new order is placed in the Online Shop.

For this we will enhance the SAP System to react on the custom event triggered by the Online Shop. This event will then be picked up by the ABAP SDK for Azure and sent to an Azure Service Bus. From there our Logic App from Quest 1 can be triggered.

## Create a Queue in Azure Service Bus

1. Search from the Azure Portal for "ServiceBusForSAPOnlineShop" and select the Service Bus

<p align="center" width="100%">
<img alt="Select Service Bus" src="../img/student/Quest2/SelectServiceBus.jpg"  width="600">
</p>

2. Create a new Queue by clicking on "+ Queue" and enter a Queue name, like Developer\<XXX>

<p align="center" width="100%">
<img alt="Create Queue" src="../img/student/Quest2/CreateQueue.jpg"  width="800">
</p>

3. When you scroll down you can see the Queue that was created. 

<p align="center" width="100%">
<img alt="Verify the Queue" src="../img/student/Quest2/ScrollDownAndSelect.jpg"  width="800">
</p>

## Create a Subscription in Azure Event Grid

1. Similar like before, in the Azure Portal search for EventsFromOnlineShop and select the Event Grid

<p align="center" width="100%">
<img alt="Select Event Grid" src="../img/student/Quest2/SelectEventGrid.jpg"  width="600">
</p>

2. Click on "+ Subscription" and create a new subscription for the Event Grid

<p align="center" width="100%">
<img alt="Create Event Grid Subscription" src="../img/student/Quest2/CreateEventSubscription.jpg"  width="800">
</p>

3. Start entering the details like, subscription name and select Azure Service Bus Queue

<p align="center" width="100%">
<img alt="Create Event Grid Subscription Details" src="../img/student/Quest2/EnterSubscriptionDetails.jpg"  width="600">
</p>

4. Click on "Select an endpoint" and make sure to select the Service Bus Queue for your Developer\<xxx> previous created. Then click on "Confirm Selection". 

<p align="center" width="100%">
<img alt="Select Endpoint" src="../img/student/Quest2/SelectEndpoint.jpg"  width="800">
</p>

5. Click on the Filter tab and click on "Add new filter"

<p align="center" width="100%">
<img alt="Add filter" src="../img/student/Quest2/AddFilter.jpg"  width="600">
</p>

6. For the filter, enter "data.createdby", select "Contains ins" and click "Add new values"

<p align="center" width="100%">
<img alt="Enter filter information" src="../img/student/Quest2/EnterFilters.jpg"  width="600">
</p>

7) Enter your SAP user, e.g. Developer\<xxx> and click on Create

<p align="center" width="100%">
<img alt="Enter Value and click on create" src="../img/student/Quest2/ValueAndCreate.jpg"  width="600">
</p>

### Test

Create and Order in the Online Shop and peek

## Connect the event to our Logic App

1. Open the Logic App created in Quest 1 and delete the first Trigger / Action (When a HTTP request is received)

<p align="center" width="100%">
<img alt="Delete the first action" src="../img/student/Quest2/LogicAppDesigner.jpg"  width="800">
</p>

2. Search for "Service Bus" and select the new Trigger action "Service Bus"

<p align="center" width="100%">
<img alt="Search Service Bus" src="../img/student/Quest2/SearchServiceBus.jpg"  width="600">
</p>

3. Select "When a message is received..."

<p align="center" width="100%">
<img alt="When a message is received" src="../img/student/Quest2/WhenAMessgeIsReceived.jpg"  width="600">
</p>

4. Enter a name and the Connecting string from the Service Bus Queue. Click on Create

<p align="center" width="100%">
<img alt="Enter a name and connection string" src="../img/student/Quest2/EnterNameAndConnectionString.jpg"  width="600">
</p>

```http
Endpoint=sb://servicebusforsaponlineshop.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=cIcJZtV87fyvpldm78OVcABI8LSw7QxRI+ASbCrll/w=
```

> **Note** - the Connection string can be retrieved from

<p align="center" width="100%">
<img alt="Copy connection string" src="../img/student/Quest2/CopyConnectionString.jpg"  width="800">
</p>

5. Select the Queue name that you previously created.

<p align="center" width="100%">
<img alt="Select Queue" src="../img/student/Quest2/SelectQueueName.jpg"  width="600">
</p>

6. Add another action

<p align="center" width="100%">
<img alt="Add action" src="../img/student/Quest2/AddAnotherAction.jpg"  width="600">
</p>

7. Add Initialize Variable

<p align="center" width="100%">
<img alt="Initialize Variable" src="../img/student/Quest2/InitalizeVariable.jpg"  width="500">
</p>

8. Use the Contet from the incoming Service Bus request

<p align="center" width="100%">
<img alt="Set message content" src="../img/student/Quest2/SetMessageContent.jpg"  width="800">
</p>

9. Add a Parse Json action

<p align="center" width="100%">
<img alt="Select Parse JSON structure" src="../img/student/Quest2/ParseJson.jpg"  width="400">
</p>

10. Use the previously initialized variable and use the Schema from below.

<p align="center" width="100%">
<img alt="Enter Schema" src="../img/student/Quest2/EnterSchema.jpg"  width="600">
</p>

```json
{
    "type": "object",
    "properties": {
        "subject": {
            "type": "string"
        },
        "eventtype": {
            "type": "string"
        },
        "id": {
            "type": "string"
        },
        "data": {
            "type": "object",
            "properties": {
                "ordernr": {
                    "type": "string"
                },
                "createdby": {
                    "type": "string"
                },
                "event": {
                    "type": "string"
                },
                "date": {
                    "type": "string"
                },
                "time": {
                    "type": "string"
                }
            }
        },
        "dataversion": {
            "type": "string"
        },
        "metadataversion": {
            "type": "string"
        },
        "eventtime": {
            "type": "string"
        },
        "topic": {
            "type": "string"
        }
    }
}
```

11. In the next step to call the RFC, replace the hard-coded order Nr with the `ordernr` field from the parsed message body.

<p align="center" width="100%">
<img alt="Enter Order NR" src="../img/student/Quest2/EnterOrderNr.jpg"  width="800">
</p>

### Test it!
1) Open up again the [Online Shop](https://vhcals4hcs.dummy.nodomain:44301/sap/bc/adt/businessservices/odatav4/feap?feapParams=C%C2%87u%C2%84C%C2%83%C2%84%C2%89C%C2%83xu%C2%88uHC%C2%87u%C2%84C%C2%8E%C2%89%7Ds%C2%83%C2%82%C2%80%7D%C2%82y%C2%87%7C%C2%83%C2%84s%C2%81%C2%87Es%C2%83HC%C2%87%C2%86%C2%8AxC%C2%87u%C2%84C%C2%8E%C2%89%7Ds%C2%83%C2%82%C2%80%7D%C2%82y%C2%87%7C%C2%83%C2%84s%C2%81%C2%87ECDDDEC77c%C2%82%C2%80%7D%C2%82ysg%7C%C2%83%C2%84777777ni%5Dscb%60%5DbYg%5CcdsagE77DDDE77ni%5Dscb%60%5DbYg%5CcdsagEscH&sap-ui-language=EN&sap-client=100)
<p align="center" width="100%">
<img alt="Enter Order NR" src="../img/student/Quest2/OnlineShopCreate.jpg"  width="800">
</p>

2) Fill in the required properties and click on Create
![Create Order](../img/student/Quest2/CreateOrder.jpg)

3) As soon as you click on create, head over to [Teams](https://teams.microsoft.com/) and check your channel. You should now see the Adaptive Card with your Order Item
![Adaptive Card](../img/student/Quest2/AdaptiveCard.jpg)

#### Behind the scene
1) When you go back to your the Azure Event Grid Topic (search for EventsFromOnlineShop), you can see the Event coming via the ABAP SDK for Azure when a new Order is crated in the Online Shop. At the bottom you can also see the Subscription, that you had created for this event. 
![Adaptive Card](../img/student/Quest2/EventGridCheckEvent.jpg)
>> Note: Since we have multiple users working on the same Online Shop, you will most likely see lots of different events coming in. 

2) In this subscription we had specified a filter for your Online Shop user, that would put incoming events into an Service Bus Queue. 
![Adaptive Card](../img/student/Quest2/EventGridSubscription.jpg)

3) Once an event is in this queue, we had created a Logic Apps trigger, that kicks off your specific flow to post an adaptive card in Teams. Open up your Logic App again and click on Run History. 
![Adaptive Card](../img/student/Quest2/RunsHistory.jpg)

4) From there you can see the previous runs of the Logic App. 
![Adaptive Card](../img/student/Quest2/LogicAppRunHistory.jpg)

## Where to next?

[< Quest 1](quest1.md) - **[🏠Home](../README.md)** - [ Quest 3 >](quest3.md)

[🔝](#)
