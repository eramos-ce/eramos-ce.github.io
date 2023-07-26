# *WIP* How to configure Liferay to authenticate to Keycloak via SAML

## Setup Liferay as a Service Provider

1. If you don't have a local dev environment yet, you can start one by simply running the following docker command.

      ```
      docker run -it --rm -p 8080:8080 liferay/dxp:latest
      ```

2. Once your environment is up, login as the default administrator user (i.e. test@liferay.com)
3. Open the SAML Admin page, at Control Panel --> SAML Admin
4. On the General tab, set the value of **Entity ID** to `liferay-saml` and click Save.
5. Click on Create Certificate.  You can either generate a new one by filling out the form, or click on Import Certificate to import an existing one.
     In this scenario, simply fill out the form.
6. Click on Download Certificate.

## Setup Keycloak Client

1. Create a new realm if you don't already have one.  In my case, I named it `liferay`.
2. Navigate to Manage -> Clients, then select Create client
3. Select a Client type of SAML, and Client ID of `liferay-saml`.
    * **NOTE** this Client ID must match Liferay's **Entity ID**.
4. Enter the following details for your site.
    * Root URL: http://localhost:8080
    * Valid redirect URIs:
        - http://localhost:8080/c/portal/saml/auth_redirect
        - http://localhost:8080/c/portal/saml/acs  
5. Save your changes.
6. Now open the Keys tab for the liferay-saml client.
7. On the **Signing keys config**, click on **Import key**.
8. Change the Archive format to Certificate PEM.
9. Click Browse and select the certificate downloaded on the previous section.

 ## Create Keycloak Users
 1. Navigate to Users
 2. Create a new user by clicking Add user
 3. Go to the Credentials tab and set the default password for the new users

## Configure Liferay SAML IdP    

