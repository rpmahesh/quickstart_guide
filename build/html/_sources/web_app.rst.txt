.. _getstartedwithwebapp:

Get started with a Web App
===========================

Web applications are used by an enterprise and reside in the app server within the data center.

To provide secure access with Enterprise Application Access (EAA), attach an access and identity connector to the web application server 
inside the data center. The connector does a dialout to the Enterprise Application Access Cloud on port 443. The users connect to the 
Enterprise Application Access Login Portal URL in their web browsers. They provide their credentials once to gain access to all of the 
applications.

  .. figure:: /images/web_app.jpg 

Set up a web application and learn how to: 

- Create and install a connector in Amazon AWS environment.

- Add users to the EAA Cloud directory, create an Akamai identity provider (IdP).

- Associate the directory and deploy the IdP.

- Configure a web application (HTTPS application) with time-based access control rules (ACL) for restricted access to 
  certain users at specific times.

- Add authentication with the identity provider (IdP) for the web application.

- Verify if the user can access the web application using the external hostname.

Start with a simple configuration:

- Use the cloud directory in EAA (do not use any other Active Directory or LDAP).

- Use the EAA cloud domain (do not use any custom external domain). You do not have to configure and 
  upload certificates.

Also, we skip for now application services like URL rewrite rules for optimized content delivery, URL path-based policies, 
and use of Internet Content Adaptation Protocol (ICAP) for offload-processing of internet-based content by dedicated servers.

The authentication mechanism for the web application server is set to **none** (default). Additional configuration is needed 
if you want to use any of the advanced application-facing authentication mechanisms like SAML 2.0, WS Federation, and 
OpenID Connect 1.0.


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

Next, configure your Web (HTTPS) application.

Configure a Web application
----------------------------

1. Log in to Enterprise Center.
2. In the Enterprise Center navigation menu, select **Application Access > Applications > Applications**.
3. Click **Add Application** (+).Enter the application name and description, and select **New Access App** in **Type**.
4. Click **Add Application**. Your application detail setting page opens.
5. In **App Settings** for **Akamai Cloud Zone**, select the cloud zone located closest to the datacenter where your 
   application resides.
6. In **External Host** enter the external host name for the application where the users can access the application. You can use 
   your own domain or enter a ​Akamai​ domain:

     * If you select **Use Akamai domain**, enter a URL, for example, https://sample-web-app.go.akamai-access.com.
     * If you use the **​Akamai​ domain**, you don't need to configure a certificate.

   If you select Use your domain, enter your own domain.

.. figure:: /images/web_app_external_host.jpg
   :alt: Configure External App Hostname

   *Configure the external hostname for your Web app and selecting the cloud zone*

.. note::
   If you use your own domain, you need to add a certificate and associate the certificate for your own domain and set up a 
   CNAME redirect for the application.

7. To add connectors to service your application, go to **Connectors**.
8. Click **Associate connector** and select one or more connectors. Click **Associate**.  
   To remove a connector click **Disassociate** next to it.The associated connector appears under the **Connectors**.

.. note::
   The connector must be running to serve the application.

9. In the **Server Settings**, configure the following:

   a. **Verify Origin Server Certificate** (off-by-default). Allows you to do the origin server certificate validation (recommended). 
      Also, select a root CA certificate.
   b. **ROOT CA Certificate**. Choose the root CA certificate with the full bundle you uploaded into Enterprise Center after 
      uploading a certificate in **Certificates > Certificates**.
   c. **Application Server IP/FQDN**. In Protocol select **https** (default) for secure web traffic or **http** for http traffic. 
      In **Host: Port** enter a valid internal IP address for the server or the fully qualified domain name (FQDN) that you use to 
      access this web server when inside your company's network. Also, enter an IP port number.

.. figure:: /images/web_app_server_settings.jpg
   :alt: Configure App Server settings

   *Configure App Server settings and FQDN*   

.. note::
   If no port is specified, port `443` is the default port.  

10. To configure multiple applications servers for load balancing go to **Server load balancing**, click **Add New Server** (+). 
    Enterprise Application Access supports various load balancing techniques including round-robin, session or cookie stickiness, 
    and source IP hash in **Advanced** settings.
11. To configure access control rules of who can access your application and when they can access, go to **Access**  tab, enable **Access**.
12. To create a new rule, click **Add Rule** (+). To edit an existing rule, click **Edit Rule**. A modal window appears.
13. In **Rule Name** enter a name for the rule and click **Add**.
14. In **Type** select **Time**.
15. In **Operator** select either `is`` or `is not`.
16. In **Value** enter the value if applicable, or select the value for the access control type.
17. Click **Time** to configure the time-based settings:

   a. In **Start Time** and **End Time** enter a time in hh:mm, AM-PM format.
   b. Select **time zone**.
   c. Select the days of the week for when you want to deny access.

18. Click **Save Rule**.

.. figure:: /images/acl_user_time.jpg
   :alt: ACL rule

   *For example, ACL Rule: RestrictUserAccessRule 1, that restricts User A from accessing the application from 1:00 - 2:00 AM, Saturdays, PST.*

19. You can optionally configure the following settings for your web application:

   * **Services**. Add any services like compression, URL rewrite rules, ICAP, and URL path-based policies.

   * **Advanced**. You can configure more advanced settings.

      * If **Application-facing authentication mechanism** is **SAML 2.0**, also configure **SAML Settings**.
      * If **Application-facing authentication mechanism** is **WS-Federation**, also configure **WS-Federation Settings**.
      * If **Application-facing authentication mechanism** is **Open ID Connect 1.0**, also configure **OpenID**.

20. Next, configure the authentication source and deploy your application.

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

Verify you can access the Web Application from the EAA Login Portal
--------------------------------------------------------------------

1. Open a new tab and enter the external host name URL you created for the web application in the previous steps.
   For example, `https://sample-web-app.go.akamai-access.com`.
2. Log in with your username and password you created and added to the cloud directory.
3. If you are User A, you should be able to access the web application in the non-restricted access days and times. 