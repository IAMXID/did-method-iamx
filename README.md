# IAMX DID Specification Draft v0.1

## Abstract

This specification describes a new DID IAMX for hosting DIDs on the Cardano blockchain, also referred to as IAMX DID. This specification conforms to the requirements specified in the [DIDs specification](https://www.w3.org/TR/did-core/) currently published by the W3C Credentials Community Group.

# 1. Conformance and Terminology

This specification assumes a fair degree of understanding of [W3C DIDs specification](https://www.w3.org/TR/did-core/).

The key words "MUST", "MUST NOT", "SHOULD", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [IETF RFC 2119](https://www.ietf.org/rfc/rfc2119).

# 2. IAMX DID Syntax

IAMX DID is a URI conforming to [IETF RFC 3986](https://www.ietf.org/rfc/rfc3986) and **SHOULD** be generated by each entity itself.

IAMX DID is generated in conformity with [W3C DIDs specification](https://www.w3.org/TR/did-core/).

IAMX LEDGER is a alphanumeric string

IAMX UID (specific-idstring) is conform to [IETF RFC 4122](https://www.ietf.org/rfc/rfc4122.txt)

The ABNF grammar used to generate the IAMX DID identifier is as follows:

```
iamx-did = did:iamx: ledger : specific-idstring
ledger = ledger identifyer
specific-idstring = xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx

```

`did:iamx:cardano:` denotes that IAMX DIDs are decentralized identifiers conforming to [W3C DIDs specification](https://www.w3.org/TR/did-core/), and are registered on the Cardano blockchain.

Below is an example of a IAMX DID:

```

did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453

```

# 2.2 IAMX DID document properties

An IAMX DID coument can have three distinct properties:

- **created**: poperty with the Date when the document was created.
- **registered**: poperty with the Date when the document was registered.
- **update**: poperty with the Date when the document was updated.
- **deactivated**: : poperty with the Date when the DID was deactivated.

A deactivated IAMX DID can no longer be used and cannot be reactivated for use.

## 2.5 IAMX DID Document

Each IAMX DID will have a corresponding IAMX DID Document, which is a set of data describing this IAMX DID.

Below is the basic structure of the IAMX DID Document:

```json
{
  "context": ["https://www.w3.org/ns/did/v1"],
  "id": "did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453",
  "authentication": {
    "id": "",
    "type": "",
    "controller": "",
    "publicKeyMultibase": ""
  },
  "credentials": []
}
```

# 3. Context

IAMX DID Documents **MUST** include a `@context` property.

The value of the @context property **MUST** be one or more URIs, where the value of the first URI **MUST** be [https://www.w3.org/ns/did/v1](https://www.w3.org/ns/did/v1). For more information, see [W3C DIDs specification](https://w3c.github.io/did-core/#production-0).

Below is an example in which the `@context` property has two values:

```json
{
  "@context": ["https://www.w3.org/ns/did/v1"]
}
```

## 3.1 Identifiers

iamx DID Documents **MUST** include an `id` property.

The value of the `id` property denotes the IAMX DID subject that the IAMX DID Document is about.

The value of `id` **MUST** be a valid IAMX DID. A IAMX DID **MUST** have exactly one DID subject.

```json
{
  "id": "did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453"
}
```

# 3.2 Public Keys

A IAMX DID Document **MAY** include a `publicKey` property to specify a set of public keys linked to that IAMX DID.

Public and private key pairs can be used for the identity management, authorization, and verification of IAMX DIDs. A IAMX DID can be linked to multiple public and private key pairs and one pair of public and private keys can also be used to manage multiple IAMX DIDs.

Every public key object linked to the `publicKey` property **MUST** include the fields `id`, `type`, `controller` and specific public key properties, and **MAY** include other additional properties.

Each linked public key has its own identifier specified using the field `id`. The value of `publicKey` **MUST NOT** contain multiple entries with the same `id`.

Bound public keys can be revoked. Revoked public keys **MUST NOT** be reactivated, but can still possess the original `id`.

The value of the `type` field represents the corresponding public key type. IAMX DID Documents support `BIP32-Ed25519`.

The value of the `controller` field which identifies the controller of the corresponding private key **MUST** be a valid IAMX DID, implying that the public key is controlled by this IAMX DID.

The encoding formats that IAMX DID Documents support include `publicKeyHex` and `AdaAddress`. Public keys of all types **MUST** be expressed in these two formats.

Below is a specific example of the `publicKey` property:

```json
{
  "publicKey": [
    {
      "id": "did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453#keys-1",
      "type": "BIP32-Ed25519",
      "controller": "did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453",
      "publicKeyHex": "0xfbf38de9fb40edcdab412094d24fa39a314f3d3f52f5860e2509c32522eda30161fe70dfc9f90434d64bd976ede4f112d4f2d8e34d28fe48281663219d2ddac6"
    },
    {
      "id": "did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453#keys-1",
      "type": "BIP32-Ed25519",
      "controller": "did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453",
      "AdaAddress": "addr1qxjq9aj8hy29jkp9dxeepe88ksayl4kqw7qe8et33j6ucxmj2ldj3f0f2l3xrk8ep7hwvde3wa8l9w4wkp8wxfshta2sya6pxt"
    }
  ]
}
```

# 3.4 Authentication

IAMX DID Documents **SHOULD** include an `authentication` property to specify a set of verification miamxds.

A IAMX DID subject could add the `authentication` property in its corresponding IAMX DID Document to denote that the subject has authorized a set of verification miamxds for the purpose of authentication.

The associated value **MUST** be an ordered set of one or more verification miamxds. Each verification miamxd in the `authentication` property **MAY** be embedded or referenced.

Below is an example which refers to authentication keys in the two way specified above:

```json
{
  ...
  "authentication": [
	"did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453#keys-1",
	{
	  "id": "did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453#keys-2",
	  "type": "BIP32-Ed25519",
	  "controller": "did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453",
	  "publicKeyHex": "03a835599850544b4c0a222d594be5d59cf298f5a3fd90bff1c8caa064523745f3"
	}
  ],
}
```

### 3.4.1 Authorization and Delegation

In a IAMX DID Document, an **OPTIONAL** `controller` property is included to assign one or more delegates.

A IAMX DID can be delegated to and controlled by one or more distinct IAMX DIDs.

A IAMX DID delegate has the necessary authorization to insert a new verification miamxd into the `authentication` property of the delegated IAMX DID.

It is worth noting that a IAMX DID Document can assign `controller` without defining the value of `authentication`.

The IAMX DID Document of the delegate **SHOULD** have a `authentication` property.

Below is a specific example representing that either `did:iamx:cardano:5Ee76017be7F983a520a778B413758A9DB49cBe9` or `did:iamx:cardano:9861eE37Ede3dCab070DF227155D86A7438d8Ed2` can act as a delegate of `did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453`:

```json
{
  ...
  "id": "did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453",
  "controller": [
	  "did:iamx:cardano:5Ee76017be7F983a520a778B413758A9DB49cBe9",
	  "did:iamx:cardano:9861eE37Ede3dCab070DF227155D86A7438d8Ed2"
  ],
}
```

# 3.5 Service Information

iamx DID Documents use an **OPTIONAL** `service` property to specify the service property.

iamx DID allows entities to add services and specify the relevant information of a service that is related to the particular IAMX DID, including fields such as the type of service and service endpoint.

This part is derived directly from [W3C DIDs specification](https://www.w3.org/TR/did-core/#services).

Below is a specific example:

```json
{
  ...
  "service": [
	{
	  "id": "did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453#some-service",
	  "type": "SomeServiceType",
	  "serviceEndpoint": "Some URL"
	}
  ]
}
```

# 3.6 DID methods

All methodes **SHOULD** include a property denoting the supported method which is executed against the current IAMX DID document. The property needs to specify a timestamp of the most recent change.

This part is derived directly from [W3C DIDs specification](https://www.w3.org/TR/did-core/#updated).

## 3.6.1 Creation

Creation of the DID including the private & public key is done **off chain** on a device controlled by the holder.
Within the metadata of the DID document a **created** property is added.
IAMX DID can be automatically created without registration for each Cardano address.

```json
{
  "created": "2019-06-30T12:00:00Z"
}
```

## 3.6.1 Registration

Registrationn DID will write the initial IAMX DID document on the blockchain ledger.
Within the metadata of the DID document a **registered** property is added.

```json
{
  "registered": "2019-06-30T12:00:00Z"
}
```

## 3.6.2 Update

Updating of an IAMX DID will generate new private & public key's and is done on a device controlled by the user.
Updating an IAMX DID Document is done by making a transaction which contains the old DID and writes a new DID as part of the transaction metadata into the blockchain ledger.
Within the metadata of the DID document a **updated** property is added

```json
{
  "updated": "2019-06-30T12:00:00Z"
}
```

## 3.6.3 Deactivation

Deactivating an IAMX DID Document is done by making a transaction which contains the old DID and writes a new DID as part of the transaction metadata into the blockchain ledger.
Within the metadata of the DID document a **deactivation** property is added.

```json
{
  "deactivated": "2019-06-30T12:00:00Z"
}
```

# 4. Security Considerations

The securiuty of the IAMX modell is based on the security of the underlying blockchain ledger. Currently the only supported blockchain is Cardano.

The Cardano security is based on the Ouroboros protocoll. Ouroboros features mathematically verifiable security against attackers. Security properties for the protocol are comparable to those achieved by the bitcoin blockchain protocol. For more Details on the protocoll can be found in the whitepaper:  
https://eprint.iacr.org/2016/889.pdf

IAMX DIDs will be stored in an address controlled by the holder. This enshures that third partys can't modify, register, update or deactivate the a DID document controlled by the holder.

## 4.1 Binding to Physical Identity

A IAMX DID document stored on the Blockchain will never contain any personal information. Ownership is proofen by:

- Controll over the blockchain address.
- Controll over of the private key which is related to the public key referened in the IAMX DID document.

## 4.2 DID document changes

As all IAMX DID methods are generated by a transaction which includes the previous DID the did documents changes can be monitored by observing the global state of the blockchain ledger.

# 5. Privacy Considerations

The DID document will **NEVER** contin any personal Data.
DIDs are pupose build, which means the holder will controll several DIDs for distinct Use Cases. This reduces the Potential for correlation based on usage.

# 4. Appendix

A simple example of a IAMX DID Document is as follows:

```json
{
  "context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suites/ed25519-2020/v1"
  ],
  "id": "did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453",
  "updated": "2019-06-30T12:00:00Z",
  "authentication": [
    "did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453#keys-1",
    {
      "id": "did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453#keys-2",
      "type": "BIP32-Ed25519",
      "controller": [
        "did:iamx:cardano:5Ee76017be7F983a520a778B413758A9DB49cBe9",
        "did:iamx:cardano:9861eE37Ede3dCab070DF227155D86A7438d8Ed2"
      ],
      "publicKeyHex": "03a835599850544b4c0a222d594be5d59cf298f5a3fd90bff1c8caa064523745f3"
    }
  ],
  "publicKey": [
    {
      "id": "did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453#keys-1",
      "type": "BIP32-Ed25519",
      "controller": "did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453",
      "publicKeyHex": "0xfbf38de9fb40edcdab412094d24fa39a314f3d3f52f5860e2509c32522eda30161fe70dfc9f90434d64bd976ede4f112d4f2d8e34d28fe48281663219d2ddac6"
    },
    {
      "id": "did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453#keys-1",
      "type": "BIP32-Ed25519",
      "controller": "did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453",
      "AdaAddress": "addr1qxjq9aj8hy29jkp9dxeepe88ksayl4kqw7qe8et33j6ucxmj2ldj3f0f2l3xrk8ep7hwvde3wa8l9w4wkp8wxfshta2sya6pxt"
    }
  ],
  "service": [
    {
      "id": "did:iamx:cardano:d36d6f76-e463-4e48-a97e-908edaee6453#some-service",
      "type": "SomeServiceType",
      "serviceEndpoint": "Some URL"
    }
  ],
  "credentials": [
    {
      "data": "debc879591e984d2f7521a414d1264a79135ea62fe97ad7abfffbaa2efd485ca",
      "publicKeyMultibase": "-----BEGIN RSA PUBLIC KEY-----\nMIIBCgKCAQEAs7Z/wvbS0Cg2AkdPJJ/cd8inoNUNJAZAIUlJPosyVruwMHLT707+\n6VTzV4qSQRMVQ1IJe22hQGLYqGvdN6TrXU5X3DsyyJxkBgHd35tOTSrUZDKOS7gX\n+yfj6YqRZvi+2Xfegj5kJX2hT/EOfYWO1KbFpjM+zovOQ9Um/ljwWsj9019Wv76m\nWhlpepKnR7k8tjHLdlM6iksbQltEpq79r2HEYj9hRKCoV5d1uWYd7906K1asN0zh\n8XroQESwu8Tp/kIhvXyP4WjxOJj2lh5ZpjtrJ3Qbk9ZPF7KNcAmEgqBo6fDz1Z9Q\ndlp6HTc2xuKPN1h913i5GdamCHpHphucMwIDAQAB\n-----END RSA PUBLIC KEY-----\n"
    }
  ]
}
```

## Normative References

[W3C-DID]

Decentralized Identifiers (DIDs) v1.0. W3C. Jul 2020. Working Draft. URL: https://www.w3.org/TR/did-core/

[RFC2119]

Key words for use in RFCs to Indicate Requirement Levels. S. Bradner. IETF. March 1997. Best Current Practice. URL: https://tools.ietf.org/html/rfc2119

[RFC4122]

A Universally Unique IDentifier (UUID) URN Namespace P. Leach July 2005 URL: https://www.ietf.org/rfc/rfc4122.txt

[RFC3986]

Uniform Resource Identifier (URI): Generic Syntax. T. Berners-Lee; R. Fielding; L. Masinter. IETF. JANUARY 2005. Standards Track. URL: https://tools.ietf.org/html/rfc3986
