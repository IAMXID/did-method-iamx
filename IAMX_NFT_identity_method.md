# NFT IAMX_NFT_identity_method v1.0.0

# Status of This Document

This is a draft document and may be updated, replaced or obsoleted by other documents at any time. It is inappropriate to cite this document as other than work in progress.

# Contributions

[IAMX AG](https://iamx.id/)  
Dammstrasse 16  
6300 Zug  
Switzerland  
<https://iamx.id/>

Contact: <contact@imax.id>

## Contributors:

- Tim Brückmann
- Tim Heidfeld
- Jochen Leinberger
- Dennis Mittmann

# Abstract

This specification describes how identity and origin of an NFT can be verified based on Verifieable credentials and Decentralized Identifiers.

# NFT Metadat Identity tag

Core of this method is a new key called **identity** to be added to the body of the NFT.

- [NFT_with_Identity_example.json](./NFT_with_Identity_example.json)

```JSON
{
  "721": {
    "<policy_id>": {
      "<asset_name>": {
        "name": "<string>",

        "image": "<uri | array>",
        "mediaType": "image/<mime_sub_type>",

        "description": "<string | array>",

        "files": [
          {
            "name": "<string>",
            "mediaType": "<mime_type>",
            "src": "<uri | array>",
            "<other_properties>": "<other_properties>"
          }
        ],
        "identity": {
          "@context": [
            "https://github.com/IAMXID/did-method-iamx/blob/main/IAMX_NFT_identity_method.md"
          ],
          "version": "1.0.0",
          "storageLocation": {
            "url": "depending on Ledger contains one of the following informations: fingerprint | url | tokenID",
            "type": "<transactionFingerprint | url | contract> // type of location"
          },
          "credentialHash": "CredentialDoc => sha256  // optional",
          "description": "https://iamx.id/nft_identity // optional"
        },

        "<other properties>": "<other_properties>"
      }
    },
    "version": "1.0",
}

```

The Storage location will refer to a dataset with the **NFT Identity Credential**:

- [IAMX_NFT_Identity_3031.json](./IAMX_NFT_Identity_3031.json)
- [IAMX_NFT_Identity_3031_example.json](./IAMX_NFT_Identity_3031_example.json)

```JSON
{
  "3031": {
    "<policy_id>": {
      "identityCredential": {
        "payload": {
          "date": "Sun, 06 Mar 2022 22:41:17 GMT",
          "policyID": "xxyyzz",
          "version": "1.0.0",
          "description": "Teddy Troops NFT Identity by IAMX.ID",
          "accounts": {
            "twitter": { "handle": "<handle>", "email": "<account email>" },
            "discord": { "handle": "<handle>", "email": "<account email>" },
            "instagram": { "handle": "<handle>", "email": "<account email>" },
            "website": "<url to website>"
          }
        },
        "@context": "https://github.com/IAMXID/did-method-iamx",
        "signatures": [
          {
            "signature": "<payload => signed by Issuer rsa-sha256 in base58 encoding>",
            "signatureType": "rsa-sha256",
            "signatureEncoding": "base58",
            "DID": "did:iamx:cardanozggW2SuC7Phxth3SAjhtz7YuNfrcDhoRTz5WrSZ2xh38BTwf",
            "description": "Issuer",
            "storageLocation": {
              "uri": "whereDIDIsSaved",
              "type": "transactionFingerprint"
            }
          },
          {
            "signature": "<signature of Issuer => signed by Creator rsa-sha256 in base58 encoding>",
            "signatureType": "rsa-sha256",
            "signatureEncoding": "base58",
            "DID": "did:iamx:cardanozggW2SuC7Phxth3SAjhtz7YuNfrcDhoRTz5WrSZ2xh38BTwf",
            "description": "Creator",
            "storageLocation": {
              "uri": "whereIssuerDidIsSaved",
              "type": "transactionFingerprint"
            }
          },
          {
            "signature": "<signature of Creator => signed by Creator rsa-sha256 in base58 encoding>",
            "signatureType": "rsa-sha256",
            "signatureEncoding": "base58",
            "DID": "did:iamx:cardanozggW2SuC7Phxth3SAjhtz7YuNfrcDhoRTz5WrSZ2xh38BTwf",
            "description": "IAMX_test",
            "storageLocation": {
              "uri": "whereIAMXDidIsSaved",
              "type": "transactionFingerprint"
            }
          }
        ]
      }
    }
  }
}
```
