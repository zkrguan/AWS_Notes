# AWS Services Cheat Sheet

## Content Log


## AWS Cognito

**Authentication service from AWS.** This is a service that provided by AWS and the developer does not need to store any user information. Instead, all the user account info will be handled by AWS. 

( Remember the OG dot net developer said: "Don't handle the authentication by yourself, give the responsibility to the 3rd party." )

### A typical flow of using AWS congnito

![image](https://github.com/zkrguan/AWS_Notes/assets/97544709/3bc15830-f660-4d69-a261-1f51b7cb99f9)

You user clicking on the login -> got redirected to OAuth2 Cognito Authorization URL. 

Typing info and sent to cognito.

The request was sent to the AWS cognito.

AWS cognito sends back the token signed digitally. 

Frontend can send the requests with the token to the backend server.

The user will be able to access the resource.

If the token was tempered from AWS until it reaches to the Backend, the signing will not work. ( so no one could potentially change anything). Otherwise, the 

### JWT token 

A typical encoded json web token.

![image](https://github.com/zkrguan/AWS_Notes/assets/97544709/818c72c8-86ed-43d5-9fb6-54041f5e65bf)

base64URL -> Can be transferred in the header of a HTTP request. Everything is not useable until it is decoded. 

header is the red  -> how the token was signed (which algorithm was used)

payload (claim) is the purple -> claims about the user (like account info) and the token (issuer, expiration date, subject)

signature is the blue used to verfiy the token 
1. anyone can read the token but not alter 
2. Allows recipients of the token to verify that token has not been tampered with.
3. Based on public / private key encryption.                          

This is how it is decoded token looks like. 

![image](https://github.com/zkrguan/AWS_Notes/assets/97544709/ef3cc293-fef6-48be-bdad-fb06b38e7f22)

Just remember security is not from the claim, which means the claim does not contains password. The security is from the signing. 

Think about the College Logging process (although the service is provided by Azure) by they do have the same concept and share the similar flow.  

### OAuth2 URLs from/to Cognito

#### This is a typical OAuth2 URL to the AWS Cognito. It is trying to decide if the user is logged in. 

![image](https://github.com/zkrguan/AWS_Notes/assets/97544709/21647eed-d08b-4486-9c4e-5fcceba2e5da)

redirect_uri-> After authorized, where should I send you back to. You cannot send the user whererer you want, it must be configured in the AWS Cognito. 

response_type-> code will be explained at the end of the section.

client_id -> gen by Cognito when you configure the user pool. 

identity provider -> very straigh forward. 

scope -> which piece of info of JWT (JWT claim) do you want get back? (say I just want the user email)

state -> Client sent to Cognito. Cognito sent it back untouched to the client. Client compare the string. This is made for defending certain types of attacks. 

code_challenge and ..._method -> It is also for the safety. (remember shopify webhook safety method) 

#### If the user is not logged in

Redirect to the Cognito Login URL

![image](https://github.com/zkrguan/AWS_Notes/assets/97544709/e7cee02e-a353-43a5-b3e5-99d7793b6a74)

Amazon Cognito UI: User enters the info here

![image](https://github.com/zkrguan/AWS_Notes/assets/97544709/c5006133-2db1-458c-a3ef-b6d32918f43c)

#### If the user info matched 

Code is like a number while you are waiting for being seated. Or like a Costco Voucher can be redeemed for cash. 

Latter the client will use the code to exchange the token. 

![image](https://github.com/zkrguan/AWS_Notes/assets/97544709/fb4f3ecf-de8b-4bdc-88b2-ee21f298eda3)

#### Client exchanges access code for tokens

![image](https://github.com/zkrguan/AWS_Notes/assets/97544709/225d5bc5-93b4-415f-86fb-cc1f8e1c018b)

#### server will send the token back:

![image](https://github.com/zkrguan/AWS_Notes/assets/97544709/d31cc684-9b29-4bab-84b2-5b4f25e61860)

access token (OAuth2):
1. can be JWT or any type of string
2. Like an access card to acccess the resource.
3. Does not usually include user info.

Refresh token (OAuth2):
1.The access token will expire in expires_in seconds (3600 seconds = 1 hour)
2.We use this to trade for a new access toekn before the old one expires. (Isn't this similar to the UPS api token?)

Identity toekn (openID):
1. Always a JWT , which the client can parse for user details.
2. Like a Driver's license, it has claims about who I am.

**AWS Amplify does have a node package so you don't have to code it all by yourself.**
