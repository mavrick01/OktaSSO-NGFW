# OktaSSO-NGFW
Enabling SSO via Okta directly onto an NGFW

## Formal Solution
Okta has a how to guide using their own integration. 
https://saml-doc.okta.com/SAML_Docs/How-to-Configure-SAML-2.0-for-Palo-Alto-Networks-Admin-UI.html

In added an Application by searching the App Catalog (Browse App Catalog)
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Adding%20App.png "Adding an App")

Search the App Catalog for `Palo Alto` (I found that adding more limits the list with the Admin UI in it)
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Browse%20Apps%20for%20Palo%20Alto.png "Search Palo Alto")

Select `Palo Alto Networks - Admin UI` you will see the summary page
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/App%20Summary.png "Palo Alto Networks - Admin UI App Summary")

Select `+Add Integration` and complete with your firewall details (renaming the App if you want). **NOTE** Remember to add 443 at the end
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Complete%20App%20Data.png "Palo Alto Networks - Admin UI Configuration")

Once in the App, goto the Sign-On Tab. Right at the bottom you can download the Meta Data file to load into the firewall.
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/App%20Sign%20On%20Page.png "App Sign On Tab")

You are not done yet - select the link  `Configure profile mapping`. This will open the profile mappings - just close that tab (you want the one behind it).
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Default%20Profile%20Page.png "The default profile mapping page")

This will show the profile editor page (*Alternatively you can access this through `Directory -> Profile Editor`*). Here we show it completed with the attributes. But you would have to select `+Add Attribute`
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Profile%20Editor%20completed.png "Profile Editor Page")

When you select `Add Attribute` you will be presented with this page:
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Add%20Profile%20Attributes.png "Add Profile Attributes")

In this add `adminrole` as shown, then select `Save and Add Attribute`.

Then add `accessdomain` and select `Save`. This will return you to the Profile Editor Page which should look as above.

Now go back to the Application Settings and select the `Assignments` tab. 
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/App%20Assignments.png "App Assignments")

Assign the User or Group you want to add. Here is where I am adding a Group.
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Assign%20Group.png "Assign Group")

It will pick up that there are Attributes added and ask you to enter them.
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Assign%20Roles.png "Assign Roles")

Now you can continue with the **Configure NGFW** section below

## Custom Solution
Created my own integration element. Through this I was able to define the Admin Role and Access Domain.

In Okta, created a new application. With the following setup:
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Okta%20Setup.png "Okta Setup")

Once this is setup, if you look on the right there is a Blue Button that says **"View SAML Setup Instructions"**

At the bottom of the screen is the optional Meta Data File. Copy the seection and save it in an xml file. Alternatively, make a note of the Entity ID, ACS URL (Identity Provider Single Sign-On URL) and download the certificate.

## Configure NGFW
Under `Device->Server Profiles`, Select SAML Identity Provider

![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Server%20Profile.png "Server Profile Setup")

Select Import and browse to the XML file.

Once imported edit the server Profile and change the highlighted items in red. If you do not import the Metadata file, you can enter the Entity ID and IDP Sign On URL (note you will have to import the certificate seperately if you do it manually). 

![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/SAML%20IDP%20Server%20Profile.png "SAML IDP Server Profile Setup")

*NOTE: the Sign-Off URL is the same as the Sign-On, the end is not `/sso/saml` but `/slo/saml`*

Next step is to create an Authentication Profile. NOTE: The Admin Role and Access Group Attributes
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Authentication%20Profile.png "Authentication Profile Setup")

*NOTE: You must have defined the SLO URL to Enable Single Logout*

You also need to add in the Advanced component (usually `any`)
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Authentication%20Profile%20Adv.png "Authentication Profile Setup Adavanced")

I also created an Admin role that maps to the configuration of the adminrole attributes in the Okta Setup.
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Admin%20Role.png "Admin Role")

Similarly there is a defined Access Domain that maps ot the Okta Setup:
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Access%20Domain.png "Access Domain")


Finally, you add the Authentication Profile under `Device->Setup->Authentication Settings`
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/raw/main/Authentication%20Settings.png "Authentication Settings")


#### Azure config
https://learn.microsoft.com/en-us/azure/active-directory/saas-apps/paloaltoadmin-tutorial




