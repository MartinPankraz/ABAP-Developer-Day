# Quest 5 - Master's trail

[< Quest 4](quest4.md) - **[🏠Home](../README.md)** - [ Quest 6 >](quest6.md)

🌟🌟
🕒 1 h

We are following the official [Tutorial: Create a weather bot with Composer](https://learn.microsoft.com/composer/tutorial-create-weather-bot) -- however: instead of connecting to a Weather API, we are going to connect to our Online Store Service and retrieve additional information of a specific order.

## Create an Empty Bot in Composer

1. Start the Bot Framework Composer and click on "+ Create new" 

<p align="center" width="100%">
<img alt="Create Bot" src="../img/student/Quest5/CreateEmptyBot.jpg"  width="600">
</p>

2. Enter a name for the bot, e.g. "fetch_order_infos"

<p align="center" width="100%">
<img alt="Provide name" src="../img/student/Quest5/CreateBotName.jpg"  width="600">
</p>

> **Note**: You  might be asked to do a Tour of the Bot framework. Just skip this for now.

## Modify the Greeting

1. Select the Greeting trigger in the bot explorer and select the "Send a response" action
2. Select "Send a response"

<p align="center" width="100%">
<img alt="Send a response" src="../img/student/Quest5/SendAResponse.jpg"  width="600">
</p>

3. Click on the existing text and replace it with something like `Welcome to the SAP Online Shop Bot. Type "orders" to get started`

<p align="center" width="100%">
<img alt="replace response" src="../img/student/Quest5/ReplaceResonse.jpg"  width="600">
</p>

4. After that you can test the bot by clicking on "Start Bot" at the top of the window. When the Bot is running you can click on "Open Web Chat" to see the Greeting in a new window.

<p align="center" width="100%">
<img alt="Open Web Chat" src="../img/student/Quest5/OpenWebChat.jpg"  width="600">
</p>

> **Note**: You might get a warning regarding Firewall access. Click on Allow.
 
<p align="center" width="100%">
<img alt="Running Bot" src="../img/student/Quest5/FirstRunningBot.jpg"  width="600">
</p>

5. Close the Bot Framework Windows (not the Composer) by clicking on the "X"

<p align="center" width="100%">
<img alt="Close Bot" src="../img/student/Quest5/CloseBotDialog.jpg"  width="600">
</p>

## Create a new dialog

1. Select the "..." next to the fetch_order_infos bot in the Bot explorer and click on "+ Add a dialog"

<p align="center" width="100%">
<img alt="Add Dialog" src="../img/student/Quest5/AddADialog.jpg"  width="600">
</p>

2. Enter Name "get_order" and description "Get the details of a specific order" in the Create a dialog window and click on "OK"

<p align="center" width="100%">
<img alt="Add Dialog name" src="../img/student/Quest5/EnterDialogName.jpg"  width="400">
</p>

3. In the Bot explorer you can now see the new dialog "get_order" with a trigger "BeginDialog". Select this trigger, click on the "+" and select "Send a response"

<p align="center" width="100%">
<img alt="Add Send a Response" src="../img/student/Quest5/Dialog-SendAResponse.jpg"  width="600">
</p>

4. While the "Send a response" dialog is selected, click on "Add alternative" to enter your initial response for the step "get_order", e.g. "Let's lookup some order information"

> **Note** - If you cannot see the properties pane for the "Send a response", the Bot framwork migth still be running. In this case, close the windows of the running bot.

<p align="center" width="100%">
<img alt="Add Send a Response" src="../img/student/Quest5/Dialog-AddAlternative.jpg"  width="600">
</p>

## Start a dialog from a trigger

1. Select the fetch_order_infos dialog in the bot explorer and click on Change in the properties for "Recognizer Type/Dispatch type". Then select the "Regular expressioN" type and click on Done

<p align="center" width="100%">
<img alt="Change to regular expression" src="../img/student/Quest5/ChangeToRegularExpression.jpg"  width="600">
</p>

2. Now with the trigger changed to Regular expression, click on the "..." next to the fetch_order_infos and select "+ Add new trigger"

<p align="center" width="100%">
<img alt="Change to regular expression" src="../img/student/Quest5/AddNewTrigger.jpg"  width="700">
</p>

3. In the new windows select "Intent recognized" under trigger type and user order for the name and input of the regular expression

<p align="center" width="100%">
<img alt="order as trigger" src="../img/student/Quest5/CreateTrigger.jpg"  width="500">
</p>

4. Next we need to link this new "order" trigger to our Dialog. Start by selecting the order trigger in the box explorer, click on the "+" and select "Dialog management" -> "Begin a new dialog"

<p align="center" width="100%">
<img alt="Begin a new dialog" src="../img/student/Quest5/LinkTriggerToDialog.jpg"  width="600">
</p>

5. Having the "Begin a new dialog" step still selected, select "get_order" from the Dialog name dropdown.

<p align="center" width="100%">
<img alt="Link to get order" src="../img/student/Quest5/LinkTOGetOrder.jpg"  width="600">
</p>

6. With this we can again test the flow. Click on start bot / restart bot and open the Web Chat:

<p align="center" width="100%">
<img alt="Restart Bot" src="../img/student/Quest5/RestartBot.jpg"  width="600">
</p>

7. If you now trigger our flow (e.g. using something with order), you will get the response from our get_order dialog.

<p align="center" width="100%">
<img alt="Another test" src="../img/student/Quest5/SecondTest.jpg"  width="400">
</p>

## Prompt a user for input

1. In the next step we need to ask the user for an order number. For this select the BeginDialog step under "get_order" and click on the "+" sign after the "Send a response" step. From there Click on "Ask a question" and "Number".

<p align="center" width="100%">
<img alt="Ask for Number" src="../img/student/Quest5/AskForNumber.jpg"  width="600">
</p>

2. Select the "Prompt for a number" step and click on "Add alternative" to add a question, e.g. "What order ID are you looking for?"

> **Note** - You might need to Close the Chat window again.

<p align="center" width="100%">
<img alt="Ask for Number" src="../img/student/Quest5/EnterQuestion.jpg"  width="700">
</p>

<p align="center" width="100%">
<img alt="Ask for Number" src="../img/student/Quest5/EnterText.jpg"  width="400">
</p>

3. Next click on "User Input" and in the property field enter, user.orderID

<p align="center" width="100%">
<img alt="Ask Order ID" src="../img/student/Quest5/enterUserOrderID.jpg"  width="600">
</p>

## Call the Online Shop OData service

1. Now that we have all the information it is time to call the OData service. Under the User input dialog, click on the "+" and select "Access external resource" -> "Send an HTTP request"

<p align="center" width="100%">
<img alt="Send HTTP request" src="../img/student/Quest5/SendHTTPRequest.jpg"  width="600">
</p>

2. For the HTTP method, select "GET" and for the URL enter

```http
http://13.81.170.205:50000/sap/opu/odata4/sap/zui_onlineshop_ms1_o4/srvd/sap/zui_onlineshop_ms1/0001/Online_Shop?$filter=OrderID%20eq%20%27${user.orderID}%27
```

<p align="center" width="100%">
<img alt="Send HTTP request" src="../img/student/Quest5/EnterURL.jpg"  width="600">
</p>

> **Note** - For the order ID we user the property stored in the answer from the user stored in {user.orderID}

3. To authenticate with our S4H_EXT user, click on "Add new" in the Headers section and enter `Authorization` as the key and `Basic czRoX2V4dDpXZWxjb21lMQ==` as the value.

<p align="center" width="100%">
<img alt="Send HTTP request" src="../img/student/Quest5/EnterHeaders.jpg"  width="300">
</p>

> **Note** - Sometimes these properties are not stored. Before continuing with the next step, click on "Add New"  leave the focus of the current Value input field.

4. We expect a response in json and we want to store the results in a variable, enter `dialog.api_response` for the Result property and "json" for the Response type.

<p align="center" width="100%">
<img alt="Result properties" src="../img/student/Quest5/ResultProperties.jpg"  width="600">
</p>

5. With the response from the Online Shop we can now check if everything was OK. For that we create a if/else branch by clicking on the "+", selecting "Create a condition" and using the "Branch: If/else" action

<p align="center" width="100%">
<img alt="Create branch" src="../img/student/Quest5/CreateBranch.jpg"  width="600">
</p>

6. In the Branch: If/else dialog select "Write an expression" from the Condition drop down.

<p align="center" width="100%">
<img alt="Select Write Expression" src="../img/student/Quest5/WriteExpression.jpg"  width="600">
</p>

7. Enter the expression, which checks if the status code is 200.

```json
=dialog.api_response.statusCode == 200
```

<p align="center" width="100%">
<img alt="Select Write Expression" src="../img/student/Quest5/EnterExpression.jpg"  width="600">
</p>

8. In the True branch, click on the "+" and select "Manage Properties" -> "Set properties"

<p align="center" width="100%">
<img alt="Set properties" src="../img/student/Quest5/SetProperties.jpg"  width="600">
</p>

9. For each of the following properties, click on "Add new" under Assignment.

<p align="center" width="100%">
<img alt="Add new properties" src="../img/student/Quest5/AddNewProperties.jpg"  width="500">
</p>

<br>
<div style="margin-left: auto; margin-right: auto; width: 50%">

|Property|Value|
|---|---|
|dialog.orderID|=dialog.api_response.content.value[0].OrderID|
|dialog.Ordereditem|=dialog.api_response.content.value[0].Ordereditem|
|dialog.Purchasereqn|=dialog.api_response.content.value[0].Purchasereqn|
|dialog.Prstatus|=dialog.api_response.content.value[0].Prstatus|
|dialog.quantity|=dialog.api_response.content.value[0].quantity|
|dialog.DescriptionText|=dialog.api_response.content.value[0].DescriptionText|
</div>

<br>

<p align="center" width="100%">
<img alt="Properties added" src="../img/student/Quest5/PropertiesAdded.jpg"  width="600">
</p>

10. We should have all the information now so we can write a response to the user. Click on "+" in the True branch and click on "Send a response"

<p align="center" width="100%">
<img alt="Send response" src="../img/student/Quest5/SendAResponse-Withdata.jpg"  width="600">
</p>

11. Since we want to reply with an Adaptive Card, click on "+" and select Attachments

<p align="center" width="100%">
<img alt="Add Attachment" src="../img/student/Quest5/PlusAttachments.jpg"  width="400">
</p>

12. Then click on Add new attachment -> Create from template -> Adaptive Card

<p align="center" width="100%">
<img alt="Select Adaptive Card" src="../img/student/Quest5/AddAdaptiveCard.jpg"  width="400">
</p>

13. Copy and paste/replace the content from below in the Attachment field.

> **Note** - To learn more Adaptive Cards format, read the [documentation](https://docs.microsoft.com/adaptive-cards/getting-started/bots)

<br>
<details><summary><strong>⤵️AdaptiveCard code block</strong></summary>

```json

> To learn more Adaptive Cards format, read the documentation at
> https://docs.microsoft.com/en-us/adaptive-cards/getting-started/bots
- ```{
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.2",
    "type": "AdaptiveCard",
    "body": [
        {
            "type": "TextBlock",
            "text": "Order Item Information - ${dialog.orderID}",
            "wrap": true,
            "size": "Large",
            "weight": "Bolder",
            "color": "Accent"
        },
        {
            "type": "TextBlock",
            "text": "Information about Order ID: ${dialog.orderID} - ${dialog.Ordereditem}",
            "weight": "Bolder",
            "isSubtle": false
        },
        {
            "type": "FactSet",
            "facts": [
                {
                    "title": "Order ID",
                    "value": "${dialog.orderID}"
                },
                {
                    "title": "Order Item",
                    "value": "${dialog.Ordereditem}"
                },
                {
                    "title": "Quantity",
                    "value": "${dialog.quantity}"
                },
                {
                    "title": "Description",
                    "value": "${dialog.DescriptionText}"
                },
                {
                    "title": "PR Status",
                    "value": "${dialog.Prstatus}"
                },
                {
                    "title": "Purchase Requisition #",
                    "value": "${dialog.Purchasereqn}"
                }
            ]
        }
    ]
}```


```

</details>
<br>

<p align="center" width="100%">
<img alt="Content Added" src="../img/student/Quest5/ContentAdded.jpg"  width="400">
</p>

> **Warning** - Make sure that header "> To learn more Adaptive Cards..." is still there. 

14. Also add a response for the False branch using:

```json
Something went wrong: ${dialog.api_response.content.message}.
```

<p align="center" width="100%">
<img alt="Error response" src="../img/student/Quest5/SendError.jpg"  width="600">
</p>

15. In case of an error we will also clean-up the property to enable the user to enter another Order ID. In the Fail branch, under the Send a response, click on the "+", select "Manage properties" and click on "Delete a property"

<p align="center" width="100%">
<img alt="Error response" src="../img/student/Quest5/DeleteProperty.jpg"  width="600">
</p>

16. Enter "user.orderID" in the Properties of the Delete a property field.

<p align="center" width="100%">
<img alt="Delete order ID" src="../img/student/Quest5/DeleteOrderID.jpg"  width="600">
</p>

## Run the bot

Click again on "Restart bot" -> "Open Web Chat" to test the integration.

<p align="center" width="100%">
<img alt="Demo bot run" src="../img/student/Quest5/DemoBotRun.jpg"  width="400">
</p>

## (Optional) Publish the bot to Teams

Follow the steps outlined [here](https://learn.microsoft.com/en-us/composer/how-to-publish-bot?tabs=v2x) to publish the Bot to Azure and from there to Teams.

## Where to next?

[< Quest 4](quest4.md) - **[🏠Home](../README.md)** - [ Quest 6 >](quest6.md)

[🔝](#)
