# OktaSSO-NGFW
Enabling SSO via Okta directly onto an NGFW

## Formal Solution
Okta has a how to guide using their own integration. This works for authentication, but I was unable to get the Authorization working.
https://saml-doc.okta.com/SAML_Docs/How-to-Configure-SAML-2.0-for-Palo-Alto-Networks-Admin-UI.html

PLACEHOLDER: Insert link that refers to how to do authorization

## Customer Solution
Created my own integration element. Through this I was able to define the Admin Role and Access Domain.

In Okta, created a new application. With the following setup:
![alt text](https://github.com/mavrick01/OktaSSO-NGFW/blob/main/Okta%20Setup.png"Okta Setup")
