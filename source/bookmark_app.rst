.. _getstartedwithbookmarkapp:

Get started with a Bookmark App
================================

Bookmark applications are simple applications that you can publish as a link on the EAA Login Portal but are not formally 
secured behind Enterprise Application Access. A bookmark application is like a Hello World application. Set it up and 
learn how to add users to the Enterprise Application Access Cloud directory, create an identity provider (IdP), associate 
the directory and deploy the IdP, configure an application, and verify you can access the bookmark URL from the 
application inside Enterprise Application Access Cloud.


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

Next, configure your bookmark application.

Configure a bookmark application
---------------------------------

#. Log in to Enterprise Center.
#. In the Enterprise Center navigation menu, select **Application Access > Applications > Applications**.
#. Click **Add Application** (+).Enter the application name and description, and select Bookmark App in **Type**.
#. Click **Add Application**. Your application detail setting page opens.
#. In **Settings** select application category (optional) and enter a bookmark URL.
#. Click **Save**. Your application is configured.

Next, deploy your application.

Add an authentication source, and deploy the application
---------------------------------------------------------

Assign the previously deployed identity provider (IdP) and directory to the bookmark application and deploy your application.

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

Verify you can access the app from the EAA Login Portal
--------------------------------------------------------

1. Open a new tab and enter the login portal URL for your IdP.
2. Log in with your username and password you created and added to the cloud directory.
3. Click on your bookmark application.

You should be able to go to the URL you created.