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

5) In the preview screen you could now filter and transform the data. We will leave it as is, and just click on Load
![Preview Data](/student/Quest4/ExcelOdata4.jpg)

6) As a results we have the live data from the SAP system in Excel. If you go back and create another order item in the [Online Shop](https://vhcals4hcs.dummy.nodomain:44301/sap/bc/adt/businessservices/odatav4/feap?feapParams=C%C2%87u%C2%84C%C2%83%C2%84%C2%89C%C2%83xu%C2%88uHC%C2%87u%C2%84C%C2%8E%C2%89%7Ds%C2%83%C2%82%C2%80%7D%C2%82y%C2%87%7C%C2%83%C2%84s%C2%81%C2%87Es%C2%83HC%C2%87%C2%86%C2%8AxC%C2%87u%C2%84C%C2%8E%C2%89%7Ds%C2%83%C2%82%C2%80%7D%C2%82y%C2%87%7C%C2%83%C2%84s%C2%81%C2%87ECDDDEC77c%C2%82%C2%80%7D%C2%82ysg%7C%C2%83%C2%84777777ni%5Dscb%60%5DbYg%5CcdsagE77DDDE77ni%5Dscb%60%5DbYg%5CcdsagEscH&sap-ui-language=EN&sap-client=100) , you can just click on Refresh and get the latest data from SAP. 
![Refresh Data](/student/Quest4/ExcelOdata5.jpg)


[< Quest 3](quest3.md) - **[🏠Home](../README.md)** - [ Quest 5 >](quest5.md)
