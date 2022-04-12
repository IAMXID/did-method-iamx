# Abstract

This document descibes a method verify authorship of NFTs. In it's initial version it will be descibe the implementation for Cardano, but the implemntation is designed to be easily adopted to other blöockchains.

# Identity Credential

The proof of authorship is based an a set of attributes as Social Media Accounts, Email Adresses, Websites and Discord Servers etc. The attributes are called the credentialSubject.

```JSON
 "credentialSubject": [
  {
   "date": "Tue, 12 Apr 2022 06:50:25 GMT",
   "policyID": "xxyyzz",
   "version": "1.0.0",
   "description": "Teddy Troops NFT Identity by IAMX.ID",
   "accounts": {
    "twitter": {
     "handle": "<handle>",
     "email": "<account email>"
    },
    "discord": {
     "handle": "<handle>",
     "email": "<account email>"
    },
    "instagram": {
     "handle": "<handle>",
     "email": "<account email>"
    },
    "website": "<url to website>"
   }
  },
```

To enbale verification on ore proofs can be added to the Credential Document. The Proofs are added to the proof array and follow this pattern:

```JSON
"proof": [
{
    "type": "JsonWebSignature2020",
    "created": "2022-04-12T06:50:25.495Z",
    "verificationMethod": "did:iamx:cardano:z2J93VNNyGYqe8MNMjGnCZjo1egVSvAedLdu2zN8UfBCoRH1nutNWUG373L165u5iLy3oW85RFkd5mxs99HsmYHCbMdT477dCbEKDs92A81qp5Q",
    "proofPurpose": "assertionMethod",
    "proofValue": "eyJhbGciOiJFUzUxMiIsImNydiI6IlAtNTIxIiwidHlwIjoiSldTIn0.eyJkYXRlIjoiVHVlLCAxMiBBcHIgMjAyMiAwNjo1MDoyNSBHTVQiLCJwb2xpY3lJRCI6Inh4eXl6eiIsInZlcnNpb24iOiIxLjAuMCIsImRlc2NyaXB0aW9uIjoiVGVkZHkgVHJvb3BzIE5GVCBJZGVudGl0eSBieSBJQU1YLklEIiwiYWNjb3VudHMiOnsidHdpdHRlciI6eyJoYW5kbGUiOiI8aGFuZGxlPiIsImVtYWlsIjoiPGFjY291bnQgZW1haWw-In0sImRpc2NvcmQiOnsiaGFuZGxlIjoiPGhhbmRsZT4iLCJlbWFpbCI6IjxhY2NvdW50IGVtYWlsPiJ9LCJpbnN0YWdyYW0iOnsiaGFuZGxlIjoiPGhhbmRsZT4iLCJlbWFpbCI6IjxhY2NvdW50IGVtYWlsPiJ9LCJ3ZWJzaXRlIjoiPHVybCB0byB3ZWJzaXRlPiJ9fQ.ANxJIU2-vqEF4Mked6iHt5yFnx9O4Jv5RVVX0Cz9sXKHktTX-bezl4NykVOeqRsgPtWMzA9WcDb-UGESRvJtSfyAATu6fbZhS8gElPs47Q6N3rWKIkAP6VLbw1gryLS0qZCK1Bz5wNpnx8IMK5GFgbca2c39i4gGWTHsI6xUnW9uJ4n5"

},
{"... proof#2"},
{"... proof#3 ..."},
]

```

The type of the Verification is **JsonWebSignature2020**.

If more then one proof exists, every new proof is a signature of the first proof value of the previous proof.
Herby the proofs the different proofs add are linked via consecutive signatures and can be evaluated in that order.

The **verificationMethod** is a DID.
[Example NFT Identity Credential](IAMX_NFT_Identity_Credential_example.json)

# NFT with linked identity

The Identity is linked to each NFT via an attribute set called **nftIdentity**.

```JSON
        "nftIdentity": {
            "uri": "<asset fingerprint of NFT identity>",
          "sig": "NFT Policy ID signed per project DID"
        },
```

The signature is based on the DID of the issuer DID in the Identity Credential.
[Example of NFT with linked Identity](NFT_with_linked_Identity_example.json)
