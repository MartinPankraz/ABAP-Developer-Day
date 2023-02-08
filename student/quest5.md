# Quest 5 - Master's trail
We are following the official [Tutorial: Create a weather bot with Composer](https://learn.microsoft.com/en-us/composer/tutorial-create-weather-bot) -- however: instead of connecting to a Weather API, we are going to connect to our Online Store Service and retrieve additional information of a specific order. 

### Create an Empty Bot in Composer
1) Start the Bot Framework Composer and click on "+ Create new" 
![Create Bot](Quest5/CreateEmptyBot.jpg)

2) Enter a name for the bot, e.g. "fetch_order_infos"
![Provide name](Quest5/CreateBotName.jpg)

### Modify the Greeting
1) Select the Greeting trigger in the bot explorer and select the "Send a response" action
2) Select "Send a response"
![Send a response](Quest5/SendAResponse.jpg)

2) Click on the existing text and replace it with something like "Welcome to the SAP Online Shop Bot. Type "orders" to get started." 
![replace response](Quest5/ReplaceResonse.jpg)

3) After that you can test the bot by clicking on "Start Bot" at the top of the window. When the Bot is running you can click on "Open Web Chat" to see the Greeting in a new window. 
![Open Web Chat](Quest5/OpenWebChat.jpg)
 
![Running Bot](Quest5/FirstRunningBot.jpg)

4) Close the Bot Framework Windows (not the Composer) by clicking on the "X"
![Close Bot](Quest5/CloseBotDialog.jpg)


### Create a new dialog
1) Select the "..." next to the fetch_order_infos bot in the Bot explorer and click on "+ Add a dialog"
![Add Dialog](Quest5/AddADialog.jpg)

2) Enter Name "get_order" and description "Get the details of a specific order" in the Create a dialog window and click on "OK"
![Add Dialog name](Quest5/EnterDialogName.jpg)

3) In the Bot explorer you can now see the new dialog "get_order" with a trigger "BeginDialog". Select this trigger, click on the "+" and select "Send a response"
![Add Send a Response](Quest5/Dialog-SendAResponse.jpg)

4) While the "Send a response" dialog is selected, click on "Add alternative" to enter your initial response for the step "get_order", e.g. "Let's lookup some order information"
>> Note: If you cannot see the properties pane for the "Send a response", the Bot framwork migth still be running. In this case, close the windows of the running bot. 
![Add Send a Response](Quest5/Dialog-AddAlternative.jpg)

### Start a dialog from a trigger
1) Select the fetch_order_infos dialog in the bot explorer and click on Change in the properties for "Recognizer Type/Dispatch type". Then select the "Regular expressioN" type and click on Done
![Change to regular expression](Quest5/ChangeToRegularExpression.jpg)

2) Now with the trigger changed to Regular expression, click on the "..." next to the fetch_order_infos and select "+ Add new trigger"
![Change to regular expression](Quest5/AddNewTrigger.jpg)

3) In the new windows select "Intent recognized" under trigger type and user order for the name and input of the regular expression
![order as trigger](Quest5/CreateTrigger.jpg)

4) Next we need to link this new "order" trigger to our Dialog. Start by selecting the order trigger in the box explorer, click on the "+" and select "Dialog management" -> "Begin a new dialog"
![Begin a new dialog](Quest5/LinkTriggerToDialog.jpg)

5) Having the "Begin a new dialog" step still selected, select "get_order" from the Dialog name dropdown. 
![Link to get order](Quest5/LinkTOGetOrder.jpg)

6) With this we can again test the flow. Click on start bot / restart bot and open the Web Chat:
![Restart Bot](Quest5/RestartBot.jpg)

7) If you now trigger our flow (e.g. using something with order), you will get the response from our get_order dialog. 
![Another test](Quest5/SecondTest.jpg)

### Prompt a user for input
1) In the next step we need to ask the user for an order number. For this select the BeginDialog step under "get_order" and click on the "+" sign after the "Send a response" step. From there Click on "Ask a question" and "Number". 
![Ask for Number](Quest5/AskForNumber.jpg)

2) Select the "Prompt for a number" step and click on "Add alternative" to add a question, e.g. "What order ID are you looking for?"
>> Note: You might  need to Close the Chat window again. 

![Ask for Number](Quest5/EnterQuestion.jpg)
![Ask for Number](Quest5/EnterText.jpg)

3) Next click on "User Input" and in the property field enter, user.orderID
![Ask Order ID](Quest5/enterUserOrderID.jpg)

### Call the Online Shop OData service
1) Now that we have all the information it is time to call the OData service. Under the User input dialog, click on the "+" and select "Access external resource" -> "Send an HTTP request"
![Send HTTP request](Quest5/SendHTTPRequest.jpg)

2) For the HTTP method, select "GET" and for the URL enter 
```http
http://13.81.170.205:50000/sap/opu/odata4/sap/zui_onlineshop_ms1_o4/srvd/sap/zui_onlineshop_ms1/0001/Online_Shop?$filter=OrderID%20eq%20%27${user.orderID}%27
```
![Send HTTP request](Quest5/EnterURL.jpg)
>> Note: For the order ID we user the property stored in the answer from the user stored in {user.orderID}

3) To authenticate with our S4H_EXT user, click on "Add new" in the Headers section and enter "Authorization" as the key and "Basic czRoX2V4dDpXZWxjb21lMQ==" as the value. 
![Send HTTP request](Quest5/EnterHeaders.jpg)
>>Note: Sometimes these properties are not stored. Before continuing with the next step, click on somewhere else to leave the focus of the Value input field.


4) Since we expect a response in json and we want to store the results in a variable, enter "dialog.api_response" for the Result property and "json" for the Response type. 
![Result properties](Quest5/ResultProperties.jpg)

5) With the response from the Online Shop we can now check if everything was OK. For that we create a if/else branch by clicking on the "+", selecting "Create a condition" and using the "Branch: If/else" action
![Create branch](Quest5/CreateBranch.jpg)

6) In the Branch: If/else dialog select "Write an expression" from the Condition drop down. 
![Select Write Expression](Quest5/WriteExpression.jpg)

7) enter the expression, 
```
=dialog.api_response.statusCode == 200
```
![Select Write Expression](Quest5/EnterExpression.jpg)

8) in the True branch, click on the "+" and select "Manage Properties" -> "Set properties" 
![Set properties](Quest5/SetProperties.jpg)

9) For each of the following properties, click on "Add new" under Assignment. 
![Add new properties](Quest5/AddNewProperties.jpg)

|Property|Value|
|---|---|
|dialog.orderID|=dialog.api_response.content.value[0].OrderID|
|dialog.Ordereditem|=dialog.api_response.content.value[0].Ordereditem|
|dialog.Purchasereqn|=dialog.api_response.content.value[0].Purchasereqn|
|dialog.Prstatus|=dialog.api_response.content.value[0].Prstatus|
|dialog.quantity|=dialog.api_response.content.value[0].quantity|
|dialog.DescriptionText|=dialog.api_response.content.value[0].DescriptionText|
![Properties added ](Quest5/PropertiesAdded.jpg)

10) We should have all the information now so we can write a response to the user. Click on "+" in the True branch and click on "Send a resposne" 
![Send response](Quest5/SendAResponse-Withdata.jpg)

11) As before add a text for the response:
```
Order ID ${dialog.orderID} is an order of ${dialog.Ordereditem} (${dialog.DescriptionText}) with a quantity of ${dialog.quantity}. The purchase requisition is ${dialog.Prstatus} ${dialog.Purchasereqn}
```
![Success response](Quest5/SuccessResponse.jpg)
>> Note: We can also create a more beautiful adaptive card here like the one we sent to Teams from Logic Apps before. 

12) Also add a response for the False branch using:
```
Something went wrong: ${dialog.api_response.content.message}.
``` 
![Error response](Quest5/SendError.jpg)

13) In case of an error we will also clean-up the property to enable the user to enter another Order ID. In the Fail branch, under the Send a response, click on the "+", select "Manage properties" and click on "Delete a property"
![Error response](Quest5/DeleteProperty.jpg)

14) Enter "user.orderID" in the Properties of the Delete a property field. 
![Delete order ID](Quest5/DeleteOrderID.jpg)

### Run the bot
1) Click again on "Restart bot" -> "Open Web Chat" to test the integration. 
![Demo bot run](Quest5/DemoBotRun.jpg)

### Publish the bot to Teams
ToDo--ToDo--ToDo--ToDo--ToDo--
As a next step you could publish the bot to Azure Bot Framework and from there make it also available in Teams. 

[< Quest 4](quest4.md) - **[🏠Home](../README.md)** - [ Quest 6 >](quest6.md)


