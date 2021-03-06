---
title: Connect Configuration Manager
titleSuffix: Configuration Manager
description: A how-to guide for connecting Configuration Manager with Desktop Analytics.
ms.date: 01/25/2019
ms.prod: configuration-manager
ms.technology: configmgr-other
ms.topic: conceptual
ms.assetid: 7ed389c3-a9ab-48ce-a5eb-27d52ee4fb94
author: aczechowski
ms.author: aaroncz
manager: dougeby
ROBOTS: NOINDEX
---

# How to connect Configuration Manager with Desktop Analytics 

> [!Note]  
> This information relates to a preview service which may be substantially modified before it's commercially released. Microsoft makes no warranties, express or implied, with respect to the information provided here.  

Desktop Analytics is tightly integrated with Configuration Manager. First, make sure the site is up to date to support the latest features. Then create the Desktop Analytics connection in Configuration Manager. Finally, monitor the health of the connection. 


## <a name="bkmk_hotfix"></a> Update the site

First, make sure that your Configuration Manager site is running at least version 1810. For more information, see [Install in-console updates](/sccm/core/servers/manage/install-in-console-updates).

You also need to install the version 1810 update rollup (4486457) to support integration with Desktop Analytics. For more information on this update, see [Update rollup for Configuration Manager current branch, version 1810](https://support.microsoft.com/help/4486457).

1. Update the site with the update rollup for version 1810. For more information, see [Install in-console updates](/sccm/core/servers/manage/install-in-console-updates).  

2. Update clients. To simplify this process, consider using automatic client upgrade. For more information, see [Upgrade clients](/sccm/core/clients/manage/upgrade/upgrade-clients#automatic-client-upgrade).  



## <a name="bkmk_connect"></a> Connect to the service

Use this procedure to connect Configuration Manager to Desktop Analytics, and configure device settings. This procedure is a one-time process to attach your hierarchy to the cloud service.  

1. In the Configuration Manager console, go to the **Administration** workspace, expand **Cloud Services**, and select the **Azure Services** node. Select **Configure Azure Services** in the ribbon.  

2. On the **Azure Services** page of the Azure Services Wizard, configure the following settings:  

    - Specify a **Name** for the object in Configuration Manager.  

    - Specify an optional **Description** to help you identify the service.  

    - Select **Desktop Analytics** from the list of available services.  
  
   Select **Next**.  

3. On the **App** page, select the appropriate **Azure environment**. Then select **Import** for the web app. Configure the following settings in the **Import Apps** window:  

    - **Azure AD Tenant Name**: This name is how it's named in Configuration Manager  

    - **Azure AD Tenant ID**: The **Directory ID** you copied from Azure AD   

    - **Client ID**: The **Application ID** you copied from the Azure AD app   

    - **Secret Key**: The key **Value** you copied from the Azure AD app   

    - **Secret Key Expiry**: The same expiration date of the key   

    - **App ID URI**: This setting should automatically populate with the following value: `https://cmmicrosvc.manage.microsoft.com/`  
  
   Select **Verify**, and then select **OK** to close the Import Apps window. Select **Next** on the App page of the Azure Services Wizard.  

4. On the **Diagnostic Data** page, configure the following settings:  

    - **Commercial ID**: this value should automatically populate with your organization's ID  

    - **Windows 10 diagnostic data level**: select at least **Enhanced (Limited)**  

    - **Allow Device Name in diagnostic data**: select **Enable**  

        > [!Note]  
        > Starting with Windows 10 version 1803, the device name isn't sent to Microsoft by default. If you don't send the device name, it appears in Desktop Analytics as "Unknown". This behavior can make it difficult to identify and assess devices.  

   Select **Next**. The **Available functionality** page shows the Desktop Analytics functionality that's available with the diagnostic data settings from the previous page. Select **Next** to continue or **Previous** to make changes.   

    ![Example Available Functionality page in the Azure Services Wizard](media/available-functionality.png)

5. On the **Collections** page, configure the following settings:  

    - **Display name**: The Desktop Analytics portal displays this Configuration Manager connection using this name. Use it to differentiate between different hierarchies. For example, *test lab* or *production*.  

    - **Target collection**: This collection includes all devices that Configuration Manager configures with your commercial ID and diagnostic data settings. It's the full set of devices that Configuration Manager connects to the Desktop Analytics service.  

    - **Devices in the target collection use a user-authenticated proxy for outbound communication**: By default, this value is **No**. If needed in your environment, set to **Yes**.   

    - **Select specific collections to synchronize with Desktop Analytics**: Select **Add** to include additional collections. These collections are available in the Desktop Analytics portal for grouping with deployment plans. Make sure to include pilot and pilot exclusion collections.  

        These collections continue to sync as their membership changes. For example, your deployment plan uses a collection with a Windows 7 membership rule. As those devices upgrade to Windows 10, and Configuration Manager evaluates the collection membership, those devices drop out of the collection and deployment plan.  

6. Complete the wizard.  

Configuration Manager creates a settings policy to configure devices in the Target Collection. This policy includes the diagnostic data settings to enable devices to send data to Microsoft. By default, clients update policy every hour. After receiving the new settings, it can be several hours more before the data is available in Desktop Analytics.



## <a name="bkmk_monitor"></a> Monitor connection health

Monitor the configuration of your devices for Desktop Analytics. In the Configuration Manager console, go to the **Software Library** workspace, expand the **Microsoft 365 Servicing** node, and select the **Connection Health** dashboard.  

For more information, see [Monitor connection health](/sccm/desktop-analytics/troubleshooting#monitor-connection-health).

Configuration Manager synchronizes any Desktop Analytics deployment plans within 15 minutes of creating the connection. In the Configuration Manager console, go to the **Software Library** workspace, expand the **Microsoft 365 Servicing** node, and select the **Deployment Plans** node. 



## Next steps

Advance to the next article to enroll devices to Desktop Analytics.
> [!div class="nextstepaction"]  
> [Enroll devices](/sccm/desktop-analytics/enroll-devices)  

