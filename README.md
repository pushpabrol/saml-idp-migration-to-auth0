# saml-idp-migration-auth0
This repo contains instructions on how to migrate SAML IDP from an platform like PING or Other to Auth0


## Problem statement
A customer that is using a third party IDP has setup 100s of SAML IDPs/Enterprise Connections. They want to migrate to Okta CIC but do not want to cause any changes for the partners/customers with whom they exchanged metadata to establish the IDP trust. The manual work involved with this setup is very high therefore a migration which allows this to happen with low touch is desirable


## How do we solve this?
  
    
  - The solution involves creating a proxy that runs on the ACS url for the Service Provider (source) and reposts the SAML response to the destination Service Provider
  
  <img width="738" alt="image" src="https://user-images.githubusercontent.com/7750618/182960288-2f84d4f6-4175-4a5e-b88f-001d956e791d.png">
  
  
  - Flow before routing
    <img width="1037" alt="image" src="https://user-images.githubusercontent.com/7750618/182974049-ea7ff3db-bcc9-4bdc-b127-88d037dad5d2.png">

  - Flow after routing is enabled via the proxy
  ![image](https://user-images.githubusercontent.com/7750618/183534744-6fd08734-e65d-416c-9db8-c4d2a3db2247.png)


  
  
  - Since this solution is in the middle of a SAML Authentication Response there are several key prerequisites for this to work.
    - Prerequisites:
      1. Each SAML IDP has a unique Entity ID
      2. Each SAML IDP has a unique ACS Url for the Source SP
      3. Each SAML IDP is not requiring a Signed Authentication Request from the SP (source) for SP initiated flows
      4. Each SAML IDP does not encrypt the SAML response uisng the public key of the SP (source)
      
      
  ## Source projects that contain the required code for this solution
  
  - The proxy source code that re-routes the SAML Response
    1. For this example we are using cloudflare as the proxy
    2. [Source code](https://github.com/pushpabrol/cf-worker-saml-proxy-externalsp-auth0)

  - The code using the Auth0 Management API to create the IDP/Enterprise SAML Connection
    1. [Source code](https://github.com/pushpabrol/auth0-create-saml-connection)

  - See the README within each project for steps
  - It is important to understand that in Auth0 we are disabling some of the checks such as destination and recipient to allow this to function

    
    
      
   
      
      
      


