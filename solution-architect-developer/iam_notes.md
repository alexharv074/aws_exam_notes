# IAM

IAM universal - no regions.

Consists of:
- users
- groups
- roles
- policy documents: these are JSON docs that can be applied to: users, groups or roles.

---

# STS

Temporary access to AWS resources

Federation (typically with AD)
- uses SAML
- temporary access based on the AD creds. No need to be an IAM user
- SSO allows access to the AWS console without IAM creds

Federation with mobile apps
- Use Facebook/Amazon/Google or other OpenID providers log in.

Cross account (users from other AWS accounts)

Key terms:
- federation: joining users from one domain e.g. IAM with users from another e.g. AD, Facebook etc.
- identity: a user of a service like Facebook etc.
- identity broker: a service that takes an identity from one domain and joins it (federates it) to another.
- identity store: e.g. AD, Facebook, Google etc.

Flow:
1. Enter username/password
2. App calls the Identity Broker - this is a custom function that sits between the App, LDAP & STS.
3. LDAP directory is queried to validate the username/password.
4. Identity Broker calls `GetFederationToken()` with an IAM policy + duration (1-36 hours).
5. STS returns 4 values: access key, secret access key, token & duration.
6. Identity Broker returns these 4 things to the App.
7. App uses the creds to talk to S3.

Exam scenarios:

1. Using username and password. Flow is as above.
2. Same as above except using IAM Roles that are returned by LDAP instead of username and password.

---

# AD Federation

Need to know the flow. To do. Cloud Guru slide is too confusing.

> The user navigates to ADFS webserver. The user enter in their single sign on credentials. The user's web browser receives a SAML assertion from the AD server. The user's browser then posts the SAML assertion to the AWS SAML end point for SAML and the AssumeRoleWithSAML API request is used to request temporary security credentials. 5) The user is then able to access the AWS Console.

The AWS sign-in endpoint for SAML is https://signin.aws.amazon.com/saml

SAML stands for Security Assertion Markup Language.

Remember API call is named AssumeRoleWithSAML.

---

# WIF (web identity federation)

Just know that you can authenticate your applications using Facebook, LinkedIn, Google, amazon.com etc.

Flow:

1. Go to Facebook and verify our identity with Facebook and Facebook returns an Access Token.
2. Use that token to obtain Temporary Security Credentials from AWS. Provider ID = graph.facebook.com RoleArn = arn:aws:iam::877950674958:role/WebIdFed_Facebook Role Session Name web-identity-federation
3. Access AWS with access key and secret key.

> A user authenticates with facebook first. They are then given an ID token by facebook. An API call, AssumeRoleWithWebIdentity, is then used in conjunction with the ID token. A user is then granted temporary security credentials.

Remember API call is named AssumeRoleWithWebIdentity.

---
