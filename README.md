# Copyright DID Method Specification

The did:cr method is used for unique identification of the copyright work(digital book, music, movie, etc.) and the copyright holder(creator of copyright work, trust management of copyright, collective management rrganization, etc.). 

## Status of this Document
This is a draft document. It may be periodically updated, replaced or removed without notice at any time.

## DID Method Name
The method name that identifies this DID method SHALL be: cr.

A DID that uses this method MUST begin with the following prefix: did:cr. Per the DID specification, this string MUST be in lowercase. The remainder of the DID, after the prefix, is specified below.

## Method Specific Identifier
The copyright DID scheme is defined by the following ABNF:
```sh
cr-did = "did:" cr-method-name ":" cr-type ":" cr-specific-id
cr-method-name	= "cr"
cr-type = "h" / "w" 
cr-specific-id	= cr-type ":" UUID
```
Refer to [RFC4122](https://www.ietf.org/rfc/rfc4122.txt) for UUID.

"h" and "w" in cr-type rule mean 'copyright holder' and 'copyright work', respectively.

The DID of the copyright holder and the copyright work are called CRH DID and CRW DID, respectively, hereafter.

Examples of CRH DID and CRW DID are as follows:
```sh
did:cr:h:809a1169-a990-40dc-a325-b95d03e5acd7
did:cr:w:4f75568d-6b09-4b07-ab99-7c9114b733b4
```

## DID Documents

### CRH DID Documents
refer [DID specification](https://w3c.github.io/did-core/)

### CRW DID Documents

If there is another identification scheme for identifying copyright work, the identifier by this identification scheme can be included in the [Also Known As](https://www.w3.org/TR/did-core/#also-known-as) property.

```json
{
  "id": "did:cr:w:4f75568d-6b09-4b07-ab99-7c9114b733b4",
  "alsoKnownAs": ["G500:1310377-1234567890", "10.1000/xyz123"],
}
```

The CRH DID may be specified as the DID controller of the CRW DID document. If more than one copyright holder exists, the DID controller can be specified as a CRH DID set.

In addition to the properties specified in the [DID specification](https://w3c.github.io/did-core/), the following properties exist addtionally.
- target property [optional]

  The information about asset(a copyright work or a collection of copyright works) may be optionally included. The asset can be any form of identifiable resource, such as one Video-on-Demand content, a collection of movies, or a specific chapter of a book.
 
  To describe the asset, the target property of the [Asset Class](https://www.w3.org/TR/odrl-model/#asset) described in the [W3C ODRL Information Model specification](https://www.w3.org/TR/odrl-model/) may be included.

  If the asset can be recognized and used individually, directly, and specifically(e.g., individual VOD content or an entire book), it can be identified by id property without target property. If the target property is used, it must have the same value as the id property. 

  The target asset is also declared as an AssetCollection to indicate the resource is a collection of resources. If the VoD collection consisting of several VoD contents is, the target asset of this VoD collection is declared as an AssetCollection. Refer [Asset Class](https://www.w3.org/TR/odrl-model/#asset) described in the [W3C ODRL Information Model specification](https://www.w3.org/TR/odrl-model/) for details.
	
```json
{
  "id": "did:cr:w:4f75568d-6b09-4b07-ab99-7c9114b733b4",
  "target": {
      "@type": "AssetCollection",
      "uid":  "http://example.com/vodcollection1011"
  },
}  
```

The CRW DID document may have the following service endpoints.

* Right Offer Service 
	
  The right offer represents rules(permission, prohibitions, and duties) that are being offered from the copyright holder of the work identified by this CRW DID. The right offer is typically used to make available policies to a wider audience, but does not grant any rules. As described in the [W3C ODRL Information Model specification](https://www.w3.org/TR/odrl-model/), the right offer is used for the creation of the right agreement between the assigner(e.g., copyright holder) and the assignee.(e.g., buyer). The right agreement is typically used to grant the terms of the rules between the assigner and the assignee.
  * type: 'RightOffer'
  * serviceEndpoint: indicates the location where the information of one ore more right offers is provided. 


* Work Metadata Service
  
  The CRW DID document does not provide metadata about the work itself (e.g., content type, genre, title, size/length, etc.). But, the metadata about the work can be expressed using several metadata standards(e.g., Dublin Core Metadata). The references to such metadata may be provided through this service endpoint.
  * type: 'WorkMetadata'
  * serviceEndpoint: indicates the location where the metadata of the copyright work is provided.

## CRUD Operation Definitions

### Create

The copyright holder can generate its CRH DID and DID documents and optionally register through Copyright DID Registrar. This specification does not require the CRH DID and DID document to be registered.

The copyright holder generally generates CRH DID and DID documents. It is recommended that the copyright holder creates its own CRH DID and DID document before generating the CRW DID and DID document.


In general, the DID of the copyright holder will be designated as the DID controller of the CRW DID document, but not necessarily. 


### Read/Resolve
Refer [DID Specification](https://www.w3.org/TR/did-core/) and [Decentralized Identifier Resolution](https://w3c-ccg.github.io/did-resolution/)


### Update
If a delegation of rights occurs(for example, when Alice delegate copyright/asset ownership to Bob), the DID controller must be changed from the DID of the delegator (Alice) to the DID of the delegatee (Bob). In this cases, the delegator (Alice) has the ability to change this CRW DID document. 

If there is a target property in the CRW DID document, it is recommended not to change this value once the target property is designated. If you want to change the target property, it is recommended to create a new CRW DID and DID document rather than changing the target property.

### Deactivate

When the CRH DID is deactivated, a CRW DID document that has this CRH DID as a DID controller must not exist. If a non-existent CRH DID is specified in the DID controller of the CRW DID document, the management for CRW DID document may become impossible.

## Security Considerations

The DID controller of the CRW DID document should be managed so that the DID of copyright work owner, such as the creator, can be designated. The copyright DID registrar may verify the authentication of the copyright holder when registering the CRW DID document.

It is strongly recommended that links specified in 'RightOffer' and WorkMetadata' service endpoint is integrity protected using solutions such as a hashlink [HASHLINK](https://datatracker.ietf.org/doc/html/draft-sporny-hashlink-07).



## Privacy Considerations
The assigner and assignee in the right offer provided through the 'RightOffer' service must also use the DID syntax, so that privacy protection for the DID subject can be preserved.


## References

- [DID Specification](https://www.w3.org/TR/did-core/)
- [W3C ODRL Information Model specification](https://www.w3.org/TR/odrl-model/)
- [Decentralized Identifier Resolution](https://w3c-ccg.github.io/did-resolution/)
- [HASHLINK](https://datatracker.ietf.org/doc/html/draft-sporny-hashlink-07)
