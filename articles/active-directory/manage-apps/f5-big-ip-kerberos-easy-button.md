---
title: Configure F5 BIG-IP Easy Button for Kerberos SSO
description: Learn to implement Secure Hybrid Access (SHA) with Single Sign-on to Kerberos applications using F5’s BIG-IP Easy Button guided configuration..
services: active-directory
author: CelesteDG
manager: martinco
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: how-to
ms.workload: identity
ms.date: 12/20/2021
ms.author: celested
ms.collection: M365-identity-device-management
---

# Tutorial: Configure F5 BIG-IP Easy Button for Kerberos SSO

In this article, learn to secure Kerberos-based applications with Azure Active Directory (Azure AD), through F5’s BIG-IP Easy Button guided configuration.

Integrating a BIG-IP with Azure Active Directory (Azure AD) provides many benefits, including:

* [Improved Zero Trust governance](https://www.microsoft.com/security/blog/2020/04/02/announcing-microsoft-zero-trust-assessment-tool/) through Azure AD pre-authentication   and [Conditional Access](/azure/active-directory/conditional-access/overview)

* Full SSO between Azure AD and BIG-IP published services

* Manage identities and access from a single control plane, the [Azure portal](https://portal.azure.com/)

To learn about all of the benefits, see the article on [F5 BIG-IP and Azure AD integration](./f5-aad-integration.md) and [what is application access and single sign-on with Azure AD](/azure/active-directory/active-directory-appssoaccess-whatis).

## Scenario description

This scenario looks at the classic legacy application using **Kerberos authentication**, also known as **Integrated Windows Authentication (IWA)**, to gate access to protected content.

Being legacy, the application lacks modern protocols to support a direct integration with Azure AD. The application can be modernized, but it is costly, requires careful planning, and introduces risk of potential downtime. Instead, an F5 BIG-IP Application Delivery Controller (ADC) is used to bridge the gap between the legacy application and the modern ID control plane, through protocol transitioning. 

Having a BIG-IP in front of the application enables us to overlay the service with Azure AD pre-authentication and headers-based SSO, significantly improving the overall security posture of the application.

> [!NOTE] 
> Organizations can also gain remote access to this type of application with [Azure AD Application Proxy](../app-proxy/application-proxy.md)

## Scenario architecture

The SHA solution for this scenario is made up of the following:

**Application:** BIG-IP published service to be protected by and Azure AD SHA. The application host is domain-joined and so is integrated with Active Directory (AD).

**Azure AD:** Security Assertion Markup Language (SAML) Identity Provider (IdP) responsible for verification of user credentials, Conditional Access (CA), and SAML based SSO to the BIG-IP. Through SSO, Azure AD provides the BIG-IP with any required session attributes.

**KDC:** Key Distribution Center (KDC) role on a Domain Controller (DC), issuing Kerberos tickets.

**BIG-IP:** Reverse proxy and SAML service provider (SP) to the application, delegating authentication to the SAML IdP before performing Kerberos-based SSO to the backend application.

SHA for this scenario supports both SP and IdP initiated flows. The following image illustrates the SP initiated flow.

   ![Scenario architecture](./media/f5-big-ip-kerberos-easy-button/scenario-architecture.png)

| Steps| Description|
| -------- |-------|
| 1| User connects to application endpoint (BIG-IP) |
| 2| BIG-IP APM access policy redirects user to Azure AD (SAML IdP) |
| 3| Azure AD pre-authenticates user and applies any enforced Conditional Access policies |
| 4| User is redirected to BIG-IP (SAML SP) and SSO is performed using issued SAML token |
| 5| BIG-IP requests Kerberos ticket from KDC |
| 6| BIG-IP sends request to backend application, along with Kerberos ticket for SSO |
| 7| Application authorizes request and returns payload |

## Prerequisites
Prior BIG-IP experience isn’t necessary, but you will need:

* An Azure AD free subscription or above

* An existing BIG-IP or [deploy a BIG-IP Virtual Edition (VE) in Azure](./f5-bigip-deployment-guide.md)

* Any of the following F5 BIG-IP license offers

    * F5 BIG-IP® Best bundle

    * F5 BIG-IP APM standalone license

    * F5 BIG-IP APM add-on license on an existing BIG-IP F5 BIG-IP® Local Traffic Manager™ (LTM)

    * 90-day BIG-IP full feature [trial license](https://www.f5.com/trial/big-ip-trial.php).

* User identities [synchronized](../hybrid/how-to-connect-sync-whatis.md) from an on-premises directory to Azure AD, or created directly within Azure AD and flowed back to your on-premises directory

* An account with Azure AD Application admin [permissions](/azure/active-directory/users-groups-roles/directory-assign-admin-roles#application-administrator)

* An [SSL Web certificate](./f5-bigip-deployment-guide.md) for publishing services over HTTPS, or use default BIG-IP certs while testing

* An existing Kerberos application or [setup an IIS (Internet Information Services) app](https://active-directory-wp.com/docs/Networking/Single_Sign_On/SSO_with_IIS_on_Windows.html) for KCD SSO

## BIG-IP configuration methods

There are many methods to configure BIG-IP for this scenario, including two template-based options and an advanced configuration. This tutorial covers the latest Guided Configuration 16.1 offering an Easy button template. With the Easy Button, admins no longer go back and forth between Azure AD and a BIG-IP to enable services for SHA. The deployment and policy management is handled directly between the APM’s Guided Configuration wizard and Microsoft Graph. This rich integration between BIG-IP APM and Azure AD ensures that applications can quickly, easily support identity federation, SSO, and Azure AD Conditional Access, reducing administrative overhead.

>[!NOTE] 
> All example strings or values referenced throughout this guide should be replaced with those for your actual environment.

## Register Easy Button

Before a client or service can access Microsoft Graph, it must be trusted by the [Microsoft identity platform.](/azure/active-directory/develop/quickstart-register-app)

This first step creates a tenant app registration that will be used to authorize the **Easy Button** access to Graph. Through these permissions, the BIG-IP will be allowed to push the configurations required to establish a trust between a SAML SP instance for published application, and Azure AD as the SAML IdP.

1. Sign-in to the [Azure AD portal](https://portal.azure.com/) using an account with Application Administrative rights

2. From the left navigation pane, select the **Azure Active Directory** service

3. Under Manage, select **App registrations > New registration**

4. Enter a display name for your application. For example, *F5 BIG-IP Easy Button*

5. Specify who can use the application > **Accounts in this organizational directory only**

6. Select **Register** to complete the initial app registration

7. Navigate to **API permissions** and authorize the following Microsoft Graph **Application permissions**:

   * Application.Read.All
   * Application.ReadWrite.All
   * Application.ReadWrite.OwnedBy
   * Directory.Read.All
   * Group.Read.All
   * IdentityRiskyUser.Read.All
   * Policy.Read.All
   * Policy.ReadWrite.ApplicationConfiguration
   * Policy.ReadWrite.ConditionalAccess
   * User.Read.All

8. Grant admin consent for your organization

9. In the **Certificates & Secrets** blade, generate a new **client secret** and note it down

10. From the **Overview** blade, note the **Client ID** and **Tenant ID**

## Configure Easy Button

Initiate the APM's **Guided Configuration** to launch the **Easy Button** Template.

1. Navigate to **Access > Guided Configuration > Microsoft Integration** and select **Azure AD Application**.

   ![Screenshot for Configure Easy Button- Install the template](./media/f5-big-ip-easy-button-ldap/easy-button-template.png)

2. Review the list of configuration steps and select **Next**

   ![Screenshot for Configure Easy Button - List configuration steps](./media/f5-big-ip-easy-button-ldap/config-steps.png)

3. Follow the sequence of steps required to publish your application.

   ![Configuration steps flow](./media/f5-big-ip-easy-button-ldap/config-steps-flow.png#lightbox)

### Configuration Properties

The **Configuration Properties** tab creates a BIG-IP application config and SSO object. Consider the **Azure Service Account Details** section to represent the client you registered in your Azure AD tenant earlier, as an application. These settings allow a BIG-IP's OAuth client to individually register a SAML SP directly in your tenant, along with the SSO properties you would normally configure manually. Easy Button does this for every BIG-IP service being published and enabled for SHA.

Some of these are global settings so can be re-used for publishing more applications, further reducing deployment time and effort.

1. Provide a unique **Configuration Name** so admins can easily distinguish between Easy Button configurations

2. Enable **Single Sign-On (SSO) & HTTP Headers**

3. Enter the **Tenant Id, Client ID,** and **Client Secret** you noted when registering the Easy Button client in your tenant.

   ![Screenshot for Configuration General and Service Account properties](./media/f5-big-ip-kerberos-easy-button/azure-configuration-properties.png)

Before you select **Next**, confirm the BIG-IP can successfully connect to your tenant.

### Service Provider

The Service Provider settings define the properties for the SAML SP instance of the application protected through SHA.

1. Enter **Host**. This is the public FQDN of the application being secured

2. Enter **Entity ID.** This is the identifier Azure AD will use to identify the SAML SP requesting a token

   ![Screenshot for Service Provider settings](./media/f5-big-ip-kerberos-easy-button/service-provider.png)

The optional **Security Settings** specify whether Azure AD should encrypt issued SAML assertions. Encrypting assertions between Azure AD and the BIG-IP APM provides additional assurance that the content tokens can’t be intercepted, and personal or corporate data be compromised.

3.	From the **Assertion Decryption Private Key** list, select **Create New**
 
   ![Screenshot for Configure Easy Button- Create New import](./media/f5-big-ip-oracle/configure-security-create-new.png)

4.	Select **OK**. This opens the **Import SSL Certificate and Keys** dialog in a new tab  

6.	Select **PKCS 12 (IIS) ** to import your certificate and private key. Once provisioned close the browser tab to return to the main tab.

   ![Screenshot for Configure Easy Button- Import new cert](./media/f5-big-ip-oracle/import-ssl-certificates-and-keys.png)

6.	Check **Enable Encrypted Assertion**

8.	If you have enabled encryption, select your certificate from the **Assertion Decryption Private Key** list. This is the private key for the certificate that BIG-IP APM will use to decrypt Azure AD assertions

10.	If you have enabled encryption, select your certificate from the **Assertion Decryption Certificate** list. This is the certificate that BIG-IP will upload to Azure AD for encrypting the issued SAML assertions.

   ![Screenshot for Service Provider security settings](./media/f5-big-ip-kerberos-easy-button/service-provider-security-settings.png)

### Azure Active Directory

This section defines all properties that you would normally use to manually configure a new BIG-IP SAML application within your Azure AD tenant. Easy Button provides a set of pre-defined application templates for Oracle PeopleSoft, Oracle E-business Suite, Oracle JD Edwards, SAP ERP as well as generic SHA template for any other apps. For this scenario select **F5 BIG-IP APM Azure AD Integration > Add.**

   ![Screenshot for Azure configuration add BIG-IP application](./media/f5-big-ip-kerberos-easy-button/azure-config-add-app.png)

#### Azure Configuration

1. Enter **Display Name** of app that the BIG-IP creates in your Azure AD tenant, and the icon that the users will see in [MyApps portal](https://myapplications.microsoft.com/).

2. Leave the **Sign On URL (optional)** blank to enable IdP initiated sign-on.

   ![Screenshot for Azure configuration add display info](./media/f5-big-ip-kerberos-easy-button/azure-config-display-name.png)

3. Select the refresh icon next to the **Signing Key** and **Signing Certificate** to locate the certificate you imported earlier
 
5. Enter the certificate’s password in **Signing Key Passphrase**

6. Enable **Signing Option** (optional). This ensures that BIG-IP only accepts tokens and claims that are signed by Azure AD

   ![Screenshot for Azure configuration - Add signing certificates info](./media/f5-big-ip-easy-button-ldap/azure-configuration-sign-certificates.png)

7. **User and User Groups** are dynamically queried from your Azure AD tenant and used to authorize access to the application. Add a user or group that you can use later for testing, otherwise all access will be denied

   ![Screenshot for Azure configuration - Add users and groups](./media/f5-big-ip-kerberos-easy-button/azure-configuration-add-user-groups.png)

#### User Attributes & Claims

When a user successfully authenticates to Azure AD, it issues a SAML token with a default set of claims and attributes uniquely identifying the user. The **User Attributes & Claims tab** shows the default claims to issue for the new application. It also lets you configure more claims.

As our AD infrastructure is based on a .com domain suffix used both, internally and externally, we don’t require any additional attributes to achieve a functional KCD SSO implementation. See the [advanced tutorial](./f5-big-ip-kerberos-advanced.md) for cases where you have multiple domains or user’s login using an alternate suffix. 

   ![Screenshot for user attributes and claims](./media/f5-big-ip-kerberos-easy-button/user-attributes-claims.png)

#### Additional User Attributes

The **Additional User Attributes** tab can support a variety of distributed systems requiring attributes stored in other directories, for session augmentation. Attributes fetched from an LDAP source can then be injected as additional SSO headers to further control access based on roles, Partner IDs, etc.

   ![Screenshot for additional user attributes](./media/f5-big-ip-kerberos-easy-button/additional-user-attributes.png)

>[!NOTE] 
>This feature has no correlation to Azure AD but is another source of attributes.

#### Conditional Access Policy

CA policies are enforced post Azure AD pre-authentication, to control access based on device, application, location, and risk signals.

The **Available Policies** view, by default, will list all CA policies that do not include user based actions.

The **Selected Policies** view, by default, displays all policies targeting All cloud apps. These policies cannot be deselected or moved to the Available Policies list as they are enforced at a tenant level.

To select a policy to be applied to the application being published:

1.	Select the desired policy in the **Available Policies** list
2.	Select the right arrow and move it to the **Selected Policies** list

Selected policies should either have an **Include** or **Exclude** option checked. If both options are checked, the selected policy is not enforced.

   ![Screenshot for CA policies](./media/f5-big-ip-kerberos-easy-button/conditional-access-policy.png)

>[!NOTE]
>The policy list is enumerated only once when first switching to this tab. A refresh button is available to manually force the wizard to query your tenant, but this button is displayed only when the application has been deployed.

### Virtual Server Properties

A virtual server is a BIG-IP data plane object represented by a virtual IP address listening for client requests to the application. Any received traffic is processed and evaluated against the APM profile associated with the virtual server, before being directed according to the policy results and settings.

1. Enter **Destination Address**. This is any available IPv4/IPv6 address that the BIG-IP can use to receive client traffic. A corresponding record should also exist in DNS, enabling clients to resolve the external URL of your BIG-IP published application to this IP, instead of the appllication itself. Using a test PC's localhost DNS is fine for testing.

2. Enter **Service Port** as *443* for HTTPS

3. Check **Enable Redirect Port** and then enter **Redirect Port**. It redirects incoming HTTP client traffic to HTTPS

4. The Client SSL Profile enables the virtual server for HTTPS, so that client connections are encrypted over TLS. Select the **Client SSL Profile** you created as part of the prerequisites or leave the default whilst testing

   ![Screenshot for Virtual server](./media/f5-big-ip-kerberos-easy-button/virtual-server.png)

### Pool Properties

The **Application Pool tab** details the services behind a BIG-IP, represented as a pool containing one or more application servers.

1. Choose from **Select a Pool.** Create a new pool or select an existing one

2. Choose the **Load Balancing Method** as *Round Robin*

3. For **Pool Servers** select an existing server node or specify an IP and port for the backend node hosting the header-based application

   ![Screenshot for Application pool](./media/f5-big-ip-oracle/application-pool.png)

Our backend application runs on HTTP port 80. You can switch this to 443 if your application runs on HTTPS.

#### Single Sign-On & HTTP Headers

Enabling SSO allows users to access BIG-IP published services without having to enter credentials. The **Easy Button wizard** supports Kerberos, OAuth Bearer, and HTTP authorization headers for SSO. You will need the Kerberos delegation account created earlier to complete this step. 

Enable **Kerberos** and **Show Advanced Setting** to enter the following:

* **Username Source:** Specifies the preferred username to cache for SSO. You can provide any session variable as the source of the user ID, but *session.saml.last.identity* tends to work best as it holds the Azure AD claim containing the logged in user ID

* **User Realm Source:** Required if the user domain is different to the BIG-IP’s kerberos realm. In that case, the APM session variable would contain the logged in user domain. For example,*session.saml.last.attr.name.domain*

   ![Screenshot for SSO and HTTP headers](./media/f5-big-ip-kerberos-easy-button/sso-headers.png)

* **KDC:** IP of a Domain Controller (Or FQDN if DNS is configured & efficient)

* **UPN Support:** Enable for the APM to use the UPN for kerberos ticketing 

* **SPN Pattern:** Use HTTP/%h to inform the APM to use the host header of the client request and build the SPN that it is requesting a kerberos token for.

* **Send Authorization:** Disable for applications that prefer negotiating authentication instead of receiving the kerberos token in the first request. For example, *Tomcat.*

   ![Screenshot for SSO method configuration](./media/f5-big-ip-kerberos-easy-button/sso-method-config.png)

### Session Management
The BIG-IPs session management settings are used to define the conditions under which user sessions are terminated or allowed to continue, limits for users and IP addresses, and corresponding user info. Refer to [F5's docs](https://support.f5.com/csp/article/K18390492) for details on these settings.

What isn’t covered here however is Single Log-Out (SLO) functionality, which ensures all sessions between the IdP, the BIG-IP, and the user agent are terminated as users sign off. When the Easy Button instantiates a SAML application in your Azure AD tenant, it also populates the Logout Url with the APM’s SLO endpoint. That way IdP initiated sign-outs from the Azure AD MyApps portal also terminate the session between the BIG-IP and a client.

Along with this the SAML federation metadata for the published application is also imported from your tenant, providing the APM with the SAML logout endpoint for Azure AD. This ensures SP initiated sign outs terminate the session between a client and Azure AD. But for this to be truly effective, the APM needs to know exactly when a user signs-out of the application.

If the BIG-IP webtop portal is used to access published applications then a sign-out from there would be processed by the APM to also call the Azure AD sign-out endpoint. But consider a scenario where the BIG-IP webtop portal isn’t used, then the user has no way of instructing the APM to sign out. Even if the user signs-out of the application itself, the BIG-IP is technically oblivious to this. So for this reason, SP initiated sign-out needs careful consideration to ensure sessions are securely terminated when no longer required. One way of achieving this would be to add an SLO function to your applications sign out button, so that it can redirect your client to either the Azure AD SAML or BIG-IP sign-out endpoint. The URL for SAML sign-out endpoint for your tenant can be found in **App Registrations > Endpoints**.

If making a change to the app is a no go, then consider having the BIG-IP listen for the application's sign-out call, and upon detecting the request have it trigger SLO. Refer to our [Oracle PeopleSoft SLO guidance](./f5-big-ip-oracle-peoplesoft-easy-button.md#peoplesoft-single-logout) for using BIG-IP irules to achieve this. More details on using BIG-IP iRules to achieve this is available in the F5 knowledge article [Configuring automatic session termination (logout) based on a URI-referenced file name](https://support.f5.com/csp/article/K42052145) and [Overview of the Logout URI Include option](https://support.f5.com/csp/article/K12056).

## Summary

This last step provides a breakdown of your configurations. Select **Deploy** to commit all settings and verify that the application now exists in your tenants list of Enterprise applications.

## Active Directory KCD configurations

For the BIG-IP APM to perform SSO to the backend application on behalf of users, KCD must be configured in the target AD domain. Delegating authentication also requires that the BIG-IP APM be provisioned with a domain service account.

Skip this section if your APM service account and delegation are already setup, otherwise log into a domain controller with an admin account.

For our scenario, the application is hosted on server **APP-VM-01** and is running in the context of a service account named **web_svc_account**, not the computer’s identity. The delegating service account assigned to the APM will be called **F5-BIG-IP**.

### Create a BIG-IP APM delegation account 

As the BIG-IP doesn’t support group Managed Service Accounts (gMSA), create a standard user account to use as the APM service account:

1. Replace the **UserPrincipalName** and **SamAccountName** values with those for your environment.

    ```New-ADUser -Name "F5 BIG-IP Delegation Account" -UserPrincipalName host/f5-big-ip.contoso.com@contoso.com -SamAccountName "f5-big-ip" -PasswordNeverExpires $true -Enabled $true -AccountPassword (Read-Host -AsSecureString "Account Password") ```

2. Create a **Service Principal Name (SPN)** for the APM service account to use when performing delegation to the web application’s service account.

     ```Set-AdUser -Identity f5-big-ip -ServicePrincipalNames @{Add="host/f5-big-ip.contoso.com"} ```

3. Ensure the SPN now shows against the APM service account.
     
     ```Get-ADUser -identity f5-big-ip -properties ServicePrincipalNames | Select-Object -ExpandProperty ServicePrincipalNames ```

4. Before specifying the target SPN that the APM service account should delegate to for the web application, you need to view its existing SPN config. Check whether your web application is running in the computer context or a dedicated service account. Next, query that account object in AD to see its defined SPNs. Replace <name_of_account> with the account for your environment. 

    ```Get-ADUser -identity <name_of _account> -properties ServicePrincipalNames | Select-Object -ExpandProperty ServicePrincipalNames ```

5. You can use any SPN you see defined against a web application’s service account, but in the interest of security it’s best to use a dedicated SPN matching the host header of the application. For example, as our web application host header is myexpenses.contoso.com we would add HTTP/myexpenses.contoso.com to the applications service account object in AD.

    ```Set-AdUser -Identity web_svc_account -ServicePrincipalNames @{Add="http/myexpenses.contoso.com"} ```

    Or if the app ran in the machine context, we would add the SPN to the object of the computer account in AD.

    ```Set-ADComputer -Identity APP-VM-01 -ServicePrincipalNames @{Add="http/myexpenses.contoso.com"} ```

With the SPNs defined, the APM service account now needs trusting to delegate to that service. The configuration will vary depending on the topology of your BIG-IP and application server.

### Configure BIG-IP and target application in same domain

1. Set trust for the APM service account to delegate authentication

    ```Get-ADUser -Identity f5-big-ip | Set-ADAccountControl -TrustedToAuthForDelegation $true ```

2. The APM service account then needs to know which target SPN it’s trusted to delegate to, Or in other words which service is it allowed to request a Kerberos ticket for. Set target SPN to the service account running your web application.

    ```Set-ADUser -Identity f5-big-ip -Add @{'msDS-AllowedToDelegateTo'=@('HTTP/myexpenses.contoso.com')} ```

If preferred, you can also complete these tasks through the Active Directory Users and Computers MMC (Microsoft Management Console) on a domain controller.

### BIG-IP and application in different domains

Starting with Windows Server 2012, cross domain KCD uses Resource-based constrained delegation (RCD). The constraints for a service have been transferred from the domain administrator to the service administrator. This allows the back-end service administrator to allow or deny SSO. This also introduces a different approach at configuration delegation, which is only possible using either PowerShell or ADSIEdit.

The **PrincipalsAllowedToDelegateToAccount** property of the applications service account (computer or dedicated service account) can be used to grant delegation from the BIG-IP. For this scenario, use the following PowerShell command on a Domain Controller DC (2012 R2+) within the same domain as the application.

If the **web_svc_account** service runs in context of a user account:

 ```$big-ip= Get-ADComputer -Identity f5-big-ip -server dc.contoso.com ```
 ```Set-ADUser -Identity web_svc_account -PrincipalsAllowedToDelegateToAccount $big-ip ```
 ```Get-ADUser web_svc_account -Properties PrincipalsAllowedToDelegateToAccount ```

If the **web_svc_account** service runs in context of a computer account:

 ```$big-ip= Get-ADComputer -Identity f5-big-ip -server dc.contoso.com ```
 ```Set-ADComputer -Identity web_svc_account -PrincipalsAllowedToDelegateToAccount $big-ip ```
 ```Get-ADComputer web_svc_account -Properties PrincipalsAllowedToDelegateToAccount ```

For more information, see [Kerberos Constrained Delegation across domains](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831477(v=ws.11)).

## Next steps

From a browser, **connect** to the application’s external URL or select the **application’s icon** in the [Microsoft MyApps portal](https://myapps.microsoft.com/). After authenticating to Azure AD, you’ll be redirected to the BIG-IP virtual server for the application and automatically signed in through SSO.

   ![Screenshot for App views](./media/f5-big-ip-kerberos-easy-button/app-view.png)

For increased security, organizations using this pattern could also consider blocking all direct access to the application, thereby forcing a strict path through the BIG-IP.

### Azure AD B2B guest access

[Azure AD B2B guest access](../external-identities/hybrid-cloud-to-on-premises.md) is supported for this scenario, by having guest identities flowed down from your Azure AD tenant to the directory the application uses for authorisation. Without a local representation of a guest object in AD, the BIG-IP would fail to recieve a kerberos ticket for KCD SSO to the backend application.

## Advanced deployment

There may be cases where the Guided Configuration templates lacks the flexibility to achieve more specific requirements. For those scenarios, see [Advanced Configuration for kerberos-based SSO](./f5-big-ip-kerberos-advanced.md).

Alternatively, the BIG-IP gives you the option to disable **Guided Configuration’s strict management mode**. This allows you to manually tweak your configurations, even though bulk of your configurations are automated through the wizard-based templates.

You can navigate to **Access > Guided Configuration** and select the **small padlock icon** on the far right of the row for your applications’ configs. 
 
   ![Screenshot for Configure Easy Button - Strict Management](./media/f5-big-ip-oracle/strict-mode-padlock.png)

At that point, changes via the wizard UI are no longer possible, but all BIG-IP objects associated with the published instance of the application will be unlocked for direct management.

>[!NOTE]
>Re-enabling strict mode and deploying a configuration will overwrite any settings performed outside of the Guided Configuration UI, therefore we recommend the advanced configuration method for production services.

## Troubleshooting

Failure to access a SHA protected application can be due to any number of factors. If troubleshooting kerberos SSO issues, be aware of the following.

* Kerberos is time sensitive, so requires that servers and clients be set to the correct time and where possible synchronized to a reliable time source

* Ensure the hostname for the domain controller and web application are resolvable in DNS

* Ensure there are no duplicate SPNs in your AD environment by executing the following query at the command line on a domain PC: setspn -q HTTP/my_target_SPN

You can refer to our [App Proxy guidance](../app-proxy/application-proxy-back-end-kerberos-constrained-delegation-how-to.md) to validate an IIS application is configured appropriately for KCD. F5’s article on [how the APM handles Kerberos SSO](https://techdocs.f5.com/en-us/bigip-15-1-0/big-ip-access-policy-manager-single-sign-on-concepts-configuration/kerberos-single-sign-on-method.html) is also a valuable resource.

### Log analysis

BIG-IP logging can help quickly isolate all sorts of issues with connectivity, SSO, policy violations, or misconfigured variable mappings. Start troubleshooting by increasing the log verbosity level.

1. Navigate to **Access Policy > Overview > Event Logs > Settings**

2. Select the row for your published application, then **Edit > Access System Logs**

3. Select **Debug** from the SSO list, and then select **OK**. 

Reproduce your issue, then inspect the logs, but remember to switch this back when finished as verbose mode generates lots of data. 

If you see a BIG-IP branded error immediately after successful Azure AD pre-authentication, it’s possible the issue relates to SSO from Azure AD to the BIG-IP.

1. Navigate to **Access > Overview > Access reports**

2. Run the report for the last hour to see logs provide any clues. The **View session variables** link for your session will also help understand if the APM is receiving the expected claims from Azure AD.

If you don’t see a BIG-IP error page, then the issue is probably more related to the backend request or SSO from the BIG-IP to the application. 

1. Navigate to **Access Policy > Overview > Active Sessions**

2. Select the link for your active session. The **View Variables** link in this location may also help determine root cause KCD issues, particularly if the BIG-IP APM fails to obtain the right user and domain identifiers from session variables.

See [BIG-IP APM variable assign examples]( https://devcentral.f5.com/s/articles/apm-variable-assign-examples-1107) and [F5 BIG-IP session variables reference]( https://techdocs.f5.com/en-us/bigip-15-0-0/big-ip-access-policy-manager-visual-policy-editor/session-variables.html) for more info.
