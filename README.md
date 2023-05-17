# Secure.SAML

This library provides utility SAML class to generate SAML 2.0 as XMLDocument & base64 encoded string for parameters specified below-

- `key="recipient"` is Recipient or Consumer URL
- `key="issuer"` is string Issuer Name or domain
- `key="audienceRestrictions"` is a (name/value) dictonary for Audience Restrictions
- `key="userIdentity"` is string user Identity or subject
- `key="customAttributes"` is a (name/value) dictonary for custom attributes to pass
- `key="signatureType"` is string whether to sign Response or Assertion.

```csharp
var saml = new SAML(Func<X509Certificate2> certificateFactory);

var parameters = new Parameters
{
  Issuer = "http://ninjacorp.com",
  Recipient = "https://xyz.target-link.co.uk:443/saml/api",
  AudienceRestrictions = new[] { "xyz.target-link.co.uk" },
  NamedId = "NIN0123456",
  NameIdFormat = NameIdFormat.Unspecified,                           // Default - Unspecified
  Attributes = new Dictionary<string, string> { { "Custom_key", "value" } },
  SignatureType = SignType.Response,                                 // Default - Response
  NotOnOrAfterInMins = 10,                                           // Default - 10 minutes
  SigningAlgorithm = SigningAlgorithm.SHA512,                        // Supports - SHA1, SHA256 & SHA512 (default).
  SamlId = Guid.Parse("95AD6A84-95C1-4B39-AE5E-FE1E700C406C"),       // Optional, defaults to new guid.
  AssertionId = Guid.Parse("B3CA912A-4A6B-4F31-9FD8-FC5E55837656"),  // Optional, defaults to new guid.
  Timestamp = DateTime.Parse("2018-02-27T09:36:44.0665619Z")         // Optional, defaults to DateTime.UtcNow
};

var xmlDocument = saml.Create(parameters);
var base64EncodedString = saml.CreateEncoded(parameters);

```

Example Generated SAML :-

```xml
<samlp:Response xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
                ID="_95ad6a8495c14b39ae5efe1e700c406c"
                Version="2.0"
                IssueInstant="2018-02-27T09:36:44Z"
                Destination="https://xyz.target-link.co.uk:443/saml/api"
                xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
	<saml:Issuer>http://ninjacorp.com</saml:Issuer>
	<Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
		<SignedInfo>
			<CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
			<SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha512"/>
			<Reference URI="#_95ad6a8495c14b39ae5efe1e700c406c">
				<Transforms>
					<Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
					<Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#">
						<InclusiveNamespaces PrefixList="#default saml ds xs xsi"
						                     xmlns="http://www.w3.org/2001/10/xml-exc-c14n#"/>
					</Transform>
				</Transforms>
				<DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha512"/>
				<DigestValue>SLRX7pQuCZwnqc033B5ohdF7If9zYy8ez1uaDb7E7QKYYHRbIuZ8xDNGTSDI/1WmjkcPiGy8PoCu7l2TflaxVg==</DigestValue>
			</Reference>
		</SignedInfo>
		<SignatureValue>EN/W5jihCeYjUMO3T2I83N6J+gtWsyo1nFCyFacD88TE/00aSATsXC/koju3p+wO1h5xxiGW9mk3kOEOKCUKjekZ7Oub4irCz1xUJ2WmDM1h/+uxb9yFrflnVt8CRuUdfOQpTDAXqS4ENQn26ZsrH9iQ3oPDZcTHqIgwTRWCzR0=</SignatureValue>
		<KeyInfo>
			<X509Data>
				<X509Certificate>MIIB/DCCAWWgAwIBAgIQ39LYOFEy9K1A+f1T4C/ELzANBgkqhkiG9w0BAQQFADAWMRQwEgYDVQQDEwtYWVogQ29tcGFueTAeFw0wNDEyMzExNzAwMDBaFw0wOTEyMzExNzAwMDBaMBYxFDASBgNVBAMTC1hZWiBDb21wYW55MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQC3E55S/VquStRieuJ39TM6HkKh47pC+x3XklZ+gmIPHk2XRbUuOCnJunxnesChjDJ2H0tP1usHoPU2jJbfNffEJRrVw8zDavvVqiye4hHGaSL3i7BDOChzKeQY/8yifIMFUIK7DOKwfQDUbJf662gac6u0AmNv/CNdIpECWUHokQIDAQABo0swSTBHBgNVHQEEQDA+gBDHk2UyyDjvEL4gr3OaFlNBoRgwFjEUMBIGA1UEAxMLWFlaIENvbXBhbnmCEN/S2DhRMvStQPn9U+AvxC8wDQYJKoZIhvcNAQEEBQADgYEAIqaguk7RrjeJJtq44DSFatuGtYxASy/MXtdbHhuiYIRNNBgBPB3NWYHVBrZnftBmbHz1Ur61x7ZWYPqezvKhyKZNgHHkbL0O35MHEYNNJhDLdw0QVn4QkZL5MhLHU+8zcaMWTERlQN3rQTAg4paz5oSVDMQyPbUAC/xsquUP44E=</X509Certificate>
			</X509Data>
		</KeyInfo>
	</Signature>
	<samlp:Status>
		<samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
	</samlp:Status>
	<saml:Assertion Version="2.0"
	                ID="_b3ca912a4a6b4f319fd8fc5e55837656"
	                IssueInstant="2018-02-27T09:36:44Z">
		<saml:Issuer>http://ninjacorp.com</saml:Issuer>
		<saml:Subject>
			<saml:NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:unspecified">NIN0123456</saml:NameID>
			<saml:SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
				<saml:SubjectConfirmationData NotOnOrAfter="2018-02-27T09:46:44Z"
				                              Recipient="https://xyz.target-link.co.uk:443/saml/api"/>
			</saml:SubjectConfirmation>
		</saml:Subject>
		<saml:Conditions NotBefore="2018-02-27T09:35:44Z"
		                 NotOnOrAfter="2018-02-27T09:46:44Z">
			<saml:AudienceRestriction>
				<saml:Audience>xyz.target-link.co.uk</saml:Audience>
			</saml:AudienceRestriction>
		</saml:Conditions>
		<saml:AuthnStatement AuthnInstant="2018-02-27T09:36:44Z">
			<saml:AuthnContext>
				<saml:AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</saml:AuthnContextClassRef>
			</saml:AuthnContext>
		</saml:AuthnStatement>
		<saml:AttributeStatement>
			<saml:Attribute Name="Custom_key"
			                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
				<saml:AttributeValue>value</saml:AttributeValue>
			</saml:Attribute>
		</saml:AttributeStatement>
	</saml:Assertion>
</samlp:Response>
```
