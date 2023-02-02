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


9) From the top of the flow click on "Save" and "Run Trigger" to exectue the Flow. 



**[🏠Home](./README.md)** - [ Quest 2 >](quest2.md)

