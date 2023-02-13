# Prerequisites

- Ensure availability of SAP system S/4HANA 2022 or higher (e.g. using [SAP CAL](https://cal.sap.com/) - Fully activated Appliance) with **static** IP address
- Ensure availability of [ABAPGit](https://abapgit.org/) on S4 to be able to import [ABAP SDK for Azure](https://github.com/microsoft/ABAP-SDK-for-Azure)
- Access to Azure subscription with rights to deploy resources (consider [free sign-up](https://azure.microsoft.com/free/) for easy sandboxing, note: sign-up is gated by credit card but **no charges will occur**)
- Access to Microsoft Teams and Office tenant (consider sign-up with [M365 Developer Program](https://developer.microsoft.com/microsoft-365/dev-program) for easy sandboxing)
- Ensure access to [SAP ABAP Development Tools](https://tools.eu1.hana.ondemand.com/#abap) 3.30 or higher on Eclipse for embedded Steampunk development

## Additional preparations (already done for a hosted Developer Day)

- Install the On-prem data Gateway with access to the SAP S/4HANA System. This is required to access RFC/BAPIs in the SAP system behind your firewall. In our case we have installed the required tools on the "Windows Remote Desktop" of the SAP S/4HANA 2022, Fully-Activated Appliance CAL system

### Configuration on the Azure Subscription
- Create a Azure Event Grid Topic, e.g. EventsFromOnlineShop
- Create a Service Bus Queue, e.g. ServiceBusForSAPOnlineShop

### Downloading required files

- Download the [On-premises data gateway](https://www.microsoft.com/en-us/download/details.aspx?id=53127); Similar like the SAP Cloud Connector the On-premises data gateway can establish a connection from systems behind your firewall to Azure. 
- Download the [SAP .Net Conenctor](https://support.sap.com/en/product/connectors/msnet.html). Preferably select the NCo 3.1, Complied with .NET Framework 4.6.2 version. You need an S-User to sign-in and download this file. 

### Installing  SAP .Net Connector

Start by installing the SAP .Net Connector. Just launch the SapNCox64Setup setup from the downloaded ZIP file and complete the installation. 

### Installing the on-premises data gateway

Run the GatewayInstall installation file previously downloaded. In the last step you need to sign-in with your Azure Active Directory users (that is also used for your Azure subscription, so that we can later also use Logic Apps). Please make sure that during the installation you specify the Azure region from which you will also use your Logic Apps later on in the Azure subscription. 
![OPDG Select region](/student/prerequisite/OPDG-Region.jpg)


> **Note** - You might need to up the .NET Framework to the latest version (in case of the CAL images, an update to .NET Framework 4.8 is required). The easiest way is to do this using the [.NET Framework 4.8 Web Installer}(https://dotnet.microsoft.com/en-us/download/dotnet-framework/thank-you/net48-web-installer)


### Install ABAP SDK for Azure
1) Go to Transaction SE38 -> ZABAP_GIT_STANDALONE

2) Create Package

3) Enter GitUrl 
The ABAP SDK for Azure is hosted on GitHut at xxx. Use URL https://github.com/microsoft/ABAP-SDK-for-Azure.git to fetch the required files

4) Confirm Inactive Objects
![Inactive Obejcts](/student/prerequisite/ABAPGit-InactiveObjects.jpg)

### Prepare the Client

In a hosted event we have prepared a VM for you so that you can find all the required tools for this workshop already pre-installed. If you do it on your own, you might need to install these tools on your computer. 

1) Browser
We recommend that you install a Browser like Microsoft Edge or Google Chrome and run it in private / incognito mode. By this you ensure that there is no mix-up with other (corporate) identities that you might you already used. 

2) Bot Framework
Follow the instructions outlined [here](https://learn.microsoft.com/en-us/composer/install-composer) to install the Bot Framework Composer on your client. 
In our case we have already installed the [.NET 3.1.426 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/sdk-3.1.426-windows-x64-installer) and also the [Composer](https://github.com/microsoft/BotFramework-Composer/releases/download/v2.1.2/BotFramework-Composer-2.1.2-windows-setup.exe) for Windows. 


3) Microsoft Excel
Althought most of the itnegration that we do also run in a Browser (e.g. the Teams or Word integration) we recommend to install the rich-client version of Excel. In this version it is very easy to connect to the OData service and get the data live in your Excel sheet. 

