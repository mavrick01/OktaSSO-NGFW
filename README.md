# OktaSSO-NGFW
Enabling SSO via Okta directly onto an NGFW

## Formal Solution
Okta has a how to guide using their own integration. This works for authentication, but I was unable to get the Authorization working.
https://saml-doc.okta.com/SAML_Docs/How-to-Configure-SAML-2.0-for-Palo-Alto-Networks-Admin-UI.html

PLACEHOLDER: Insert link that refers to how to do authorization

## Customer Solution
Created my own integration element. Through this I was able to define the Admin Role and Access Domain.

In Okta, created a new application. With the following setup:
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Okta%20Setup.png "Okta Setup")

Once this is setup, if you look on the right there is a Blue Button that says **"View SAML Setup Instructions"**

At the bottom of the screen is the optional Meta Data File. Copy the seection and save it in an xml file. Alternatively, make a note of the Entity ID, ACS URL (Identity Provider Single Sign-On URL) and download the certificate.

Under `Device->Server Profiles`, Select SAML Identity Provider
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Server%20Profile.png "Server Profile Setup")

Select Import and browse to the XML file.

Once imported edit the server Profile and change the highlighted items in red. If you do not import the Metadata file, you can enter the Entity ID and IDP Sign On URL (note you will have to import the certificate seperately if you do it manually). 

![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/SAML%20IDP%20Server%20Profile.png "SAML IDP Server Profile Setup")

*NOTE: the Sign-Off URL is the same as the Sign-On, the end is not `/sso/saml` but `/slo/saml`*

Next step is to create an Authentication Profile. NOTE: The Admin Role and Access Group Attributes
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Authentication%20Profile.png "Authentication Profile Setup")

You also need to add in the Advanced component (usually `any`)
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Authentication%20Profile%20Adv.png "Authentication Profile Setup Adavanced")

I also created an Admin role that maps to the configuration of the adminrole attributes in the Okta Setup.
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Admin%20Role.png "Admin Role")

Similarly there is a defined Access Domain that maps ot the Okta Setup:
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Access%20Domain.png "Access Domain")


Finally, you add the Authentication Profile under `Device->Setup->Authentication Settings`
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Authentication%20Settings.png "Authentication Settings")


####Azure config####
https://learn.microsoft.com/en-us/azure/active-directory/saas-apps/paloaltoadmin-tutorial




