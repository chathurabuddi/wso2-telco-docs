## Configure DEP for SSO

### Step 1 - Configuring the Carbon Console for SSO

Open the `<DEP_HOME>/repository/conf/security/authenticators.xml` file and give the configurations as shown below.

- Set `disabled` attributes in the `<Authenticator>` element to `false`.
- `ServiceProviderID` : The issuer name of the service provider. 
- `IdentityProviderSSOServiceURL` : URL of the Identity Server.

```xml 
<Authenticator name="SAML2SSOAuthenticator" disabled="false">
	<Priority>10</Priority>
	<Config>
		<Parameter name="LoginPage">/carbon/admin/login.jsp</Parameter>
		<Parameter name="ServiceProviderID">carbonserver</Parameter>
		<Parameter name="IdentityProviderSSOServiceURL">https://localhost:9444/samlsso</Parameter>
		<Parameter name="NameIDPolicyFormat">urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified</Parameter>
	</Config>
```

### Step 2 - Configuring Publisher/Store for SSO

To configure SSO for the API Publisher, open the 
`<DEP_HOME>/repository/deployment/server/jaggeryapps/publisher/site/conf/site.json` 
file and give the configurations as shown below.

```json  
ssoConfiguration" : {
    "enabled" : "true",
    "issuer" : "API_PUBLISHER",
    "identityProviderURL" : "https://localhost:9444/samlsso",
    "keyStorePassword" : "",
    "identityAlias" : "wso2carbon",
    "verifyAssertionValidityPeriod":"true",
    "timestampSkewInSeconds":"300",
    "audienceRestrictionsEnabled":"true",
    "responseSigningEnabled":"true",
    "assertionSigningEnabled":"true",
    "keyStoreName" :"",
    "signRequests" : "true",
    "assertionEncryptionEnabled" : "false",
    "idpInit" : "false",
    "idpInitSSOURL" : "https://localhost:9444/samlsso?spEntityID=API_PUBLISHER",
}
```

To configure SSO for the API Store, open the 
`<DEP_HOME>/repository/deployment/server/jaggeryapps/store/site/conf/site.json` 
file and change the ssoConfiguration section similarly. 
_This allows users to gain access to the Store applications directly, if they are authenticated against the Publisher._

```json  
ssoConfiguration" : {
    "enabled" : "true",
    "issuer" : "API_STORE",
    "identityProviderURL" : "https://localhost:9444/samlsso",
    "keyStorePassword" : "",
    "identityAlias" : "wso2carbon",
    "verifyAssertionValidityPeriod":"true",
    "timestampSkewInSeconds":"300",
    "audienceRestrictionsEnabled":"true",
    "responseSigningEnabled":"true",
    "assertionSigningEnabled":"true",
    "keyStoreName" :"",
    "signRequests" : "true",
    "assertionEncryptionEnabled" : "false",
    "idpInit" : "false",
    "idpInitSSOURL" : "https://localhost:9444/samlsso?spEntityID=API_STORE",
}
```

### Step 3 - Configure Identity Server as IDP for SSO

- Sign in to the WSO2 IS Management Console UI (`https://localhost:9444/carbon`). 
- Select `Add` under the `Service Providers` menu.
- Give service provider name as `API_PUBLISHER` and click `Register`.
- You are navigated to the detailed configuration page. Inside the 
`Inbound Authentication Configuration` section, expand `SAML2 Web SSO Configuration` 
and click `Configure`.
- Provide the configurations to register the `API_PUBLISHER` as the SSO service provider. 
    - Issuer: `API_PUBLISHER`
    - Assertion Consumer URL: `https://localhost:9443/publisher/jagg/jaggery_acs.jag`
    - Select the following options:
        - Enable Response Signing
        - Enable  Single Logout
    - Click Register once done.
- Similarly, provide the configurations to register the `API_STORE` as the SSO service provider.
    - Issuer: `API_STORE`
    - Assertion Consumer URL: `https://localhost:9443/store/jagg/jaggery_acs.jag`
    - Select the following options:
        - Enable Response Signing
        - Enable  Single Logout
    - Click Register once done.   