# Quest 4 - Accountant's detour

In the next step we want to integrate the Ordered Items in Excel. Unfortunately Excel does not yet support OData v4. Luckily creating an OData v2 services is just a click away with RAP. 

Using this approach we have created the OData v2 service: http://vhcals4hcs.dummy.nodomain:50000/sap/opu/odata/sap/ZUI_ONLINESHOP_MS1_O2/Online_Shop 
>> Note: we are using here HTTP and Port 50000 (not HTTPS), because we have not installed official certificates. 

1) Open Microsoft Excel on your VM

2) From a clean Excel Worksheet, navigate to Data and select 'Get Data' -> Open From Other Sources -> From OData Feed
![Connect to OData](/student/Quest4/ExcelOdata1.jpg)

3) When prompted enter the URL http://vhcals4hcs.dummy.nodomain:50000/sap/opu/odata/sap/ZUI_ONLINESHOP_MS1_O2/Online_Shop 
![Enter OData feed](/student/Quest4/ExcelOdata2.jpg)
>> Note: you can also open this URL in a browser first to validate that you get the Ordered Items in JSON 

4) In order to authetnicate with the SAP System, select Basic and enter your Username and Password
![Authentication](/student/Quest4/ExcelOdata3.jpg)

5) In the prevew screen you could now filter and transform the data. We will leave it as is, and just click on Load
![Preview Data](/student/Quest4/ExcelOdata4.jpg)

6) As a results we have the live data from the SAP system in Excel. If you go back and create another order item, you can just click on Refresh and get the latest data from SAP. 
![Refresh Data](/student/Quest4/ExcelOdata5.jpg)


[< Quest 3](quest3.md) - **[🏠Home](../README.md)** - [ Quest 5 >](quest5.md)
