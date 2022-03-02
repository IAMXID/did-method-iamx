# NFT IAMX_NFT_identity_method v0.5

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

Core of this method is a new key called **identity** to be added to the body of the NFT. Example:
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
          "storageLocation": {
            "locationId": "depending on Ledger contains one of the following informations: fingerprint | url | tokenID",
            "type": "<transactionFingerprint | url | contract> // type of location"
          },
          "credentialHash": "CredentialDoc => sha256  // optional",
          "description": "https://iamx.id/nft_identity // optional"
        },

        "<other properties>": "<other_properties>"
      }
    },
    "version": "1.0"
  }
}

```
