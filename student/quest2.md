# Quest 2 - Apprentice's curious road

[< Quest 1](quest1.md) - **[🏠Home](../README.md)** - [ Quest 3 >](quest3.md)

Now that we have the Logic App in place to fetch data from the SAP system and create an Adaptive Card in Teams, we want to trigger this flow automatically from the SAP System once a new order is placed in the Online Shop.

For this we will enhance the SAP System to react on the custom event triggered by the Online Shop. This event will then be picked up by the ABAP SDK for Azure and sent to an Azure Service Bus. From there our Logic App from Quest 1 can be triggered.

## Create a Queue in Azure Service Bus

1. Search from the Azure Portal for "ServiceBusForSAPOnlineShop" and select the Service Bus

<p align="center" width="100%">
<img alt="Select Service Bus" src="img/student/Quest2/SelectServiceBus.jpg"  width="600">
</p>

2. Create a new Queue by clicking on "+ Queue" and enter a Queue name, like Developer\<XXX>

<p align="center" width="100%">
<img alt="Create Queue" src="img/student/Quest2/CreateQueue.jpg"  width="600">
</p>

3. When you scroll down you can see the Queue that was created. 

<p align="center" width="100%">
<img alt="Verify the Queue" src="img/student/Quest2/ScrollDownAndSelect.jpg"  width="600">
</p>

## Create a Subscription in Azure Event Grid

1. Similar like before, in the Azure Portal search for EventsFromOnlineShop and select the Event Grid

<p align="center" width="100%">
<img alt="Select Event Grid" src="img/student/Quest2/SelectEventGrid.jpg"  width="600">
</p>

2. Click on "+ Subscription" and create a new subscription for the Event Grid

<p align="center" width="100%">
<img alt="Create Event Grid Subscription" src="img/student/Quest2/CreateEventSubscription.jpg"  width="600">
</p>

3. Start entering the details like, subscription name and select Azure Service Bus Queue

<p align="center" width="100%">
<img alt="Create Event Grid Subscription Details" src="img/student/Quest2/EnterSubscriptionDetails.jpg"  width="600">
</p>

4. Click on "Select an endpoint" and make sure to select the Service Bus Queue for your Developer\<xxx> previous created. Then click on "Confirm Selection". 

<p align="center" width="100%">
<img alt="Select Endpoint" src="img/student/Quest2/SelectEndpoint.jpg"  width="600">
</p>

5. Click on the Filter tab and click on "Add new filter"

<p align="center" width="100%">
<img alt="Add filter" src="img/student/Quest2/AddFilter.jpg"  width="600">
</p>

6. For the filter, enter "data.createdby", select "Contains ins" and click "Add new values"

<p align="center" width="100%">
<img alt="Enter filter information" src="img/student/Quest2/EnterFilters.jpg"  width="600">
</p>

7) Enter your SAP user, e.g. Developer\<xxx> and click on Create

<p align="center" width="100%">
<img alt="Enter Value and click on create" src="img/student/Quest2/ValueAndCreate.jpg"  width="600">
</p>

### Test

Create and Order in the Online Shop and peek

## Connect the event to our Logic App

1. Open the Logic App created in Quest 1 and delete the first Trigger / Action (When a HTTP request is received)

<p align="center" width="100%">
<img alt="Delete the first action" src="img/student/Quest2/LogicAppDesigner.jpg"  width="600">
</p>

2. Search for "Service Bus" and select the new Trigger action "Service Bus"

<p align="center" width="100%">
<img alt="Search Service Bus" src="img/student/Quest2/SearchServiceBus.jpg"  width="600">
</p>

3. Select "When a message is received..."

<p align="center" width="100%">
<img alt="When a message is received" src="img/student/Quest2/WhenAMessgeIsReceived.jpg"  width="600">
</p>

4. Enter a name and the Connecting string from the Service Bus Queue. Click on Create

<p align="center" width="100%">
<img alt="Enter a name and connection string" src="img/student/Quest2/EnterNameAndConnectionString.jpg"  width="600">
</p>

```http
Endpoint=sb://servicebusforsaponlineshop.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=cIcJZtV87fyvpldm78OVcABI8LSw7QxRI+ASbCrll/w=
```

> **Note** - the Connection string can be retrieved from

<p align="center" width="100%">
<img alt="Copy connection string" src="img/student/Quest2/CopyConnectionString.jpg"  width="600">
</p>

5. Select the Queue name that you previously created.

<p align="center" width="100%">
<img alt="Select Queue" src="img/student/Quest2/SelectQueueName.jpg"  width="600">
</p>

6. Add another action

<p align="center" width="100%">
<img alt="Add action" src="img/student/Quest2/AddAnotherAction.jpg"  width="600">
</p>

7. Add Initialize Variable

<p align="center" width="100%">
<img alt="Initialize Variable" src="img/student/Quest2/InitalizeVariable.jpg"  width="600">
</p>

8. Use the Contet from the incoming Service Bus request

<p align="center" width="100%">
<img alt="Set message content" src="img/student/Quest2/SetMessageContent.jpg"  width="600">
</p>

9. Add a Parse Json action

<p align="center" width="100%">
<img alt="Select Parse JSON structure" src="img/student/Quest2/ParseJson.jpg"  width="600">
</p>

10. Use the previously initialized variable and use the Schema from below.

<p align="center" width="100%">
<img alt="Enter Schema" src="img/student/Quest2/EnterSchema.jpg"  width="600">
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
<img alt="Enter Order NR" src="img/student/Quest2/EnterOrderNr.jpg"  width="600">
</p>

### Test it!



## Where to next?

[< Quest 1](quest1.md) - **[🏠Home](../README.md)** - [ Quest 3 >](quest3.md)

[🔝](#)
