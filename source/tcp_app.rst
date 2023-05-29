.. _getstartedwithtcpapp:

Get started with a TCP-type Client-Access App
=============================================

If you want to secure access to a single TCP application with a single hostname, you can use a TCP-type client-access application. 
This type of application, such as Microsoft Outlook, is created for a single TCP application (perApp) and supports TCP only. 
All TCP-type client-access applications are fully backward compatible with the latest Enterprise Application Access Cloud.

TCP-type client-access app lets you set up access control rules or services like the Enterprise Application Access solution used 
for secure access for HTTP applications.

.. figure:: /images/tcp_app.jpg 
   :alt: TCP type client-access application

   *Overview of TCP-type client-access application*

For each TCP-type client-access application:

(1) A WebSocket tunnel is created, and the traffic is directed to the Enterprise Application Access Cloud 
(2) Unlike a VPN, this tunnel does not provide access to the entire network. It only establishes connectivity to the 
    authorized application. From the Enterprise Application Access Cloud, traffic is directed to the connector through the firewall 
(3) Traffic is then forwarded to the application server 
(4) For example, if you configure the Outlook client as a client-access application, 
    this traffic is directed to the Microsoft Exchange server.

With this workflow, a single TCP application is secured by Enterprise Application Access. Users are able to access the application 
with the configured internal host. If there are multiple desktop applications, then multiple TCP client applications need to 
be configured.

Set up a TCP-type client-access application and learn how to:

- Create and install a connector in the Amazon AWS environment.

- Add users to the Enterprise Application Access Cloud directory.

- Create an ​Akamai​ identity provider (IdP) with EAA Client and Device Posture settings enabled.

- Associate the directory and deploy the IdP.

- Configure a TCP-type client-access application with Device Posture risk tiers and tags as ACLs. With tiers and tags, you 
  can restrict users that try to access the TCP-type app on risky or compromised devices.

- Add authentication with the IdP for the TCP-type application.

- Verify that a user can access the TCP-type application using the external hostname.

Depending on the user's operating system you need to:

`Set up EAA Client <https://techdocs.akamai.com/eaa/docs/deploy-eaa-client>`_ 

`Configure EAA Client <https://techdocs.akamai.com/eaa/docs/config-eaa-client>`_

`Create UDP and TCP applications. <https://techdocs.akamai.com/eaa/docs/install-eaa-client-udp-tcp>`_

Start with a simple configuration:

* Use the cloud directory in Enterprise Application Access (do not use any other Active Directory or LDAP).

* Use the ​Akamai​ cloud domain (do not use any custom external domain). You do not have to configure and upload certificates.

Create, download and install an Access Identity Connector
----------------------------------------------------------

You can configure EAA connectors in different virtual environments and cloud platforms. In this example, you create and deploy 
a connector for Amazon AWS environment for any access application.

1. Log in to Enterprise Center.
2. In the Enterprise Center navigation menu, select **Application Access > Clients & Connectors > Access and Identity Connectors**.
3. Click **Add New Connector** (+). Enter a connector name and an optional description.
4. Select a connector package **type**. The package type corresponds to the virtual environment where you are installing the connector. 
   For example, if you install the connector on the Amazon AWS environment, select package type **Amazon AWS EC2/VPC**.
5. Optionally, **Enable Debugging**, if you wish Akamai IT to have access to debug issues with your connector.
6. Click **Save Connector** (✓). The status of the connector changes to **Created (Download CloudFormation Template)**.
7. Click the connector on the connector list page.
8. The connector file opens in a separate browser window.
9. Download and save the connector file. The connector file is used later to set up the connector in the virtual environment.
10. Log in to your AWS console and select **AWS services menu > AWS CloudFormation > CREATE STACK**.

a. Under **Create Template**, select **Upload a template to Amazon S3**.
b. Click **Choose File** and select the downloaded **CloudFormation template** file.
c. Enter a stack name, NAT instance type, VPC ID, and subnet information and click **Next**.

.. note::
   For the NAT instance type recommended minimum is of m4.large.

d. Complete the configuration of tags, storage, and other features as needed. Since AWS does not use swap space for storage, 
   you need a minimum of 12 GB of RAM memory.

11. Click **CREATE**.
    Once the stack creation is complete, the connector instance starts and automatically connects to the EAA Cloud.
12. Next, return to the Enterprise Center.
13. In the Enterprise Center navigation menu, select **Application Access > Clients & Connectors > Access and Identity Connectors**.
14. Search for your connector and click **Approve** next to it.
15. Verify that the connector shows the private and public IP addresses assigned to it.
    The connector reaches out to the EAA Cloud after installation.
    The status changes to **Ready** and **Connector is running**.

The connector can do dialouts from the datacenter to the EAA Cloud, connecting the application inside the datacenter to the Internet 
through the EAA Cloud and the user.

Next, add users to the cloud directory.

Add users to the Cloud Directory
--------------------------------

You need to add users who can access the bookmark application to the cloud directory. Enterprise Center allows you to 
quickly configure an ​Akamai​ Cloud directory.

1. Log in to Enterprise Center.
2. In the Enterprise Center navigation menu, select **Application Access > Identity & Users > Directories**.
3. Select **Cloud Directory**.
4. In **Users**, click **Add New User (+)**. Enter email, first name, and last name.
5. Click **Send Invite**. New users receive an email to create a password and complete their account authorization.
6. Click **Save User Changes** (✓).

New users are now added to the Enterprise Application Access default directory, EAA Cloud directory.

Next, create an ​Akamai​ Identity provider (IdP).

Create an ​Akamai​ Identity Provider (IdP), associate the Cloud Directory, and deploy the IdP
---------------------------------------------------------------------------------------------

Identity providers (IdPs) manage user identity information and provide single sign-on (SSO) and multi-factor authentication (MFA).

Associate an IdP to the previously created directory to allow users to authenticate to your bookmark application.

1. Log in to Enterprise Center.
2. In the Enterprise Center navigation menu, select **Application Access > Identity & Users > Identity Providers**.
3. Click **Add Identity provider (+)**. Enter a name, description, and select the provider type as ​Akamai​, for example, IDP2.
4. Click **Continue**.
5. In **Settings > General > Identity server**  enter a URL. Leave all other settings set to default.
6. Click **Save**.
7. In **Directories**, click **Associate (+)**

.. figure:: /images/add_cloud_dir_idp.jpg

8. Select the **Cloud Directory** and click **Associate**. The cloud directory appears under the **Directories** tab.
9. Leave the default settings in the **Login portal** and click **Save**.
10. Hover over the **Status** of the identity provider, if there are no errors, **Ready for deployment** appears.

.. figure:: /images/idp_ready_to_deploy.jpg

11. Click **Deploy IDP**.
12. Now **Pending Changes** appears. All the pending deployment changes are shown. Make sure that IDP2 is selected. If you select any 
    other IdPs, they are simultaneously deployed. 

.. figure:: /images/pending_changes_tab_idp.jpg
   :alt: Pending Changes tab for Identity Provider
   :scale: 70 %

   *Pending Changes tab for the selected Identity Provider*


13. Click **Deploy**. 
14. Add a **Deploy Confirmation** message in the dialog box and click **Deploy**.

     The deployment may take several minutes to complete. When the deployment flow is complete, **Idp is Deployed** appears.


.. figure:: /images/idp_deployed.jpg
   :alt: Identity Provider Deployed
   :scale: 80 %

The cloud directory is now associated with the new identity provider. Users from to the directory can access applications 
associated with the identity provider.

Next, configure your TCP-type client-access application.

Configure a TCP-type client-access application with Device Posture ACLs
------------------------------------------------------------------------

1. Log in to Enterprise Center.
2. In the Enterprise Center navigation menu, select **Application Access > Applications > Applications**.
3. Click **Add Application** (+).Enter the application name and description,
   select **Client-Access App** for **Type**, and select 
   **TCP mode (single port, port mapping, load-balancing options)** for **Mode**.
4. Click **Add Application**. Your application detail setting page opens.
5. In **App Settings** configure the following:

   * **Application Host**. Enter the hostname of the client access application. This is the hostname that the native client uses to communicate with the application or application server. For example, if you are configuring a client like Outlook, this would be the hostname that is associated with Outlook accounts in your organization such as `mail.mydomain.com`, and is used to communicate with Microsoft Exchange.
   * **Port**. Specify the same port number, as you are going to add for the application App Server IP/FQDN. The EAA Client listens for traffic on this port from the user's computer.
   * **Endpoint Host**. Enter the external host of your application. This is the cloud endpoint for all communications between the client access application and Enterprise Application Access.


.. figure:: /images/tcp_app_domain_and_cloud_zones.jpg
   :alt: Configure TCP-type client-access app

6. Additionally, select one of these domains:

   * **Use your domain**. If you use your own custom domain, provide a certificate configured as a complete bundle with all the subordinates (having the full chain of trust), otherwise you get a web-socket error. To use an uploaded certificate, select Use uploaded certificates and select the previously uploaded certificate.
   * **Use Akamai domain**. If you use an ​Akamai​ domain no certificate is needed. For the purpose of this Get Started Guide, let's select the Akamai domain.
   * **Akamai Cloud Zone**. The cloud zone should be a geographic location that is closest to the data center where your application server resides. The ​Akamai​ Cloud Zone can start with Client-* like in Client-US-East, Client-US-West. It is the zone closest to the application in the data center.

7. Click **Save**.
8. To add connectors to service your application, go to **Connectors**.
9. Click **Associate connector** and select one or more connectors. Click **Associate**.  
   To remove a connector click **Disassociate** next to it.The associated connector appears under the **Connectors**.

.. note::
   The connector must be running to serve the application.

9. In the **Server Settings**, configure the following:

   a. **Application Server IP/FQDN**. Enter the IP address or fully qualified domain name (FQDN).
   b. **Port**. Enter the port of the TCP application. It should be the same port number as you entered in the previous steps in the **App Settings**.

.. figure:: /images/tcp_app_server_settings.jpg
   :alt: Configure TCP App Server settings

   *Configure TCP-type Client-Access App Server settings - FQDN and Port*   
 

10. To configure multiple applications servers for load balancing go to **Server load balancing**, click **Add New Server** (+). 
    Enterprise Application Access supports various load balancing techniques including round-robin, session or cookie stickiness, 
    and source IP hash in **Advanced** settings.
11. To configure access control rules of who can access your application and when they can access, go to **Access**  tab, enable **Access**.

.. note::
   For Device Posture ACLs to work, set up EAA Client with Device Posture enabled.

12. To create a new rule, click **Add Rule** (+) in **Deny Access Rules**. To edit an existing rule, click **Edit Rule**. The rule setting dialog appears.

    a. In **Rule Name** enter a name for the rule and click **Add**.
    b. In **Type** select **Device Risk Tier**.
    c. In **Operator** menu leave `is`, the default value for Device Posture risk tiers.
    d. In **Value** select **High**.

13. Click **Add Criteria (+)** to configure the following:

   a. In **Type** select **Device Risk Tag**.
   b. In **Operator** menu leave `is NOT`, the default value for Device Posture risk tags.
   c. In **Value** select a tag. For example, you can apply a tag that groups devices that are not **IOS devices**.
   d. Click **Add**.

14. Click **Save Rule**.

.. figure:: /images/DP_rule-v1.jpg
   :alt: Device Posture ACL rule

   With this ACL configuration, when a user accesses the TCP-type client-access application from a machine that falls into a high-risk tier, and not a IOS mobile device, that's basically a Windows PC with low risk tier is allowed to access to the application.


15. Next, configure the authentication source and deploy your application.

Add an authentication source, and deploy the application
---------------------------------------------------------

Assign the previously deployed identity provider (IdP) and directory to the Web application and deploy your application.

1. Log in to Enterprise Center.
2. In the Enterprise Center navigation menu, select **Application Access > Applications > Applications**.
3. Select your application to open it.
4. Click the **Authentication** tab and add the authentication for your application:  

   a. Click **Enable Authentication**.
   b. Select Identity provider from the list.
   c. Click **Assign Directory** and select the Cloud Directory from the list.
   d. Click **Associate**.

    The directory appears under the **Assigned Directories**.

    .. figure:: /images/add_cloud_dir_idp2.jpg
    

5. Leave the **History** in the application configuration default and click **Save**.
6. Hover over the Deployment symbol, if all configurations are correct, **Ready for Deployment** appears.
7. To deploy the application, click **Deploy Application**.
8. Now **Pending Changes** appears. All the pending deployment changes are shown. Make sure you selected your application. If you select any others, 
   they are simultaneously deployed.

   .. figure:: /images/app_ready_to_deploy.jpg 

9. Click **Deploy**, add a **Deploy Confirmation** message in the dialog box, and click **Deploy**.

   The deployment may take several minutes to complete. When completed, **App Deployed** appears.

      .. figure:: /images/app_deployed.jpg
         :scale: 90 %

Next, verify if you can access the application.

Verify you can access the TCP-type client-access Application from the EAA Login Portal
---------------------------------------------------------------------------------------

1. Open a new tab and enter the external host name URL you created for the web application in the previous steps.
   For example, `https://mail.mydomain.stage.akamai-access.com`.
2. Log in with your username and password you created and added to the cloud directory.
3. If the user accesses the `mail.mydomain` app on a computer that was qualified as a high-risk device, they 
   are denied access, since you set a corresponding ACL, otherwise they are allowed to access the TCP-type 
   client-access application.