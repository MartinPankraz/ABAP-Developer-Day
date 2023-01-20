# Prerequisites

- Ensure availability of SAP system S/4HANA 2022 or higher (e.g. using [SAP CAL](https://cal.sap.com/) - Fully activated Appliance)
- Ensure availability of [ABAPGit](https://abapgit.org/) on S4 to be able to import [ABAP SDK for Azure](https://github.com/microsoft/ABAP-SDK-for-Azure)
- Access to Azure subscription with rights to deploy resources (consider [free sign-up](https://azure.microsoft.com/free/) for easy sandboxing, note: sign-up is gated by credit card but no charges will occur)
- Access to Microsoft Teams and Office tenant (consider sign-up with [M365 Developer Program](https://developer.microsoft.com/microsoft-365/dev-program) for easy sandboxing)
- Ensure access to [SAP ABAP Development Tools](https://tools.eu1.hana.ondemand.com/#abap) 3.30 or higher on Eclipse for embedded Steampunk development

# Additional preparations (already done for a hosted Developer Day)
- Install the On-prem data Gateway with access to the SAP S/4HANA System. This is required to access RFC/BAPIs in the SAP system behind your firewall. In our case we have installed the required tools on the "Windows Remote Desktop" of the SAP S/4HANA 2022, Fully-Activated Appliance CAL system
## Downloading required files
- Download the [On-premises data gateway](https://www.microsoft.com/en-us/download/details.aspx?id=53127); Similar like the SAP Cloud Connector the On-premises data gateway can establish a connection from systems behind your firewall to Azure. 
- Download the [SAP .Net Conenctor](https://support.sap.com/en/product/connectors/msnet.html). Preferrably select the NCo 3.1, Complied with .NET Framework 4.6.2 version. You need an S-User to sign-in and download this file. 

## Installating  SAP .Net Connector
Start by installing the SAP .Net Connector. Just launch the SapNCox64Setup setup from the downloaded ZIP file and complete the installation. 

## Installing the on-premises data gateway
Run the GatewayInstall installation file previousy downloaded. 
> Note: You might need to up the .NET Framework to the latest version (in case of the CAL images, an update to .NET Framework 4.8 is required). The easiest way is to do this using the [.NET Framework 4.8 Web Installer}(https://dotnet.microsoft.com/en-us/download/dotnet-framework/thank-you/net48-web-installer)

