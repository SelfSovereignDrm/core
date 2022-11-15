# Copyright DID Method Specification

The did:cr method is used for unique identification of the copyright work(digital book, music, movie, etc.) and the copyright holder(e.g., creator of copyright work, Trust Management of Copyright, Collective Management Organization, etc.). 

## Status of this Document
This is a draft document. It may be periodically updated, replaced or removed without notice at any time.

## DID Method Name
The method name that identifies this DID method SHALL be: cr.

A DID that uses this method MUST begin with the following prefix: did:cr. Per the DID specification, this string MUST be in lowercase. The remainder of the DID, after the prefix, is specified below.

## Method Specific Identifier
The Copyright DID scheme is defined by the following ABNF:
```sh
cr-did = "did:" cr-method-name ":" cr-type ":" cr-specific-id
cr-method-name	= "cr"
cr-type = "h" / "w" 
cr-specific-id	= cr-type ":" UUID
```
Refer to [RFC4122](https://www.ietf.org/rfc/rfc4122.txt) for UUID.

"h" and "w" in cr-type rule name mean Holder and Work, respectively.

The following is an example of the Copyright Holder and Work DID:
```sh
did:cr:h:809a1169-a990-40dc-a325-b95d03e5acd7
did:cr:w:4f75568d-6b09-4b07-ab99-7c9114b733b4
```

## DID Documents

### Copyright Holder DID Documents
refer [DID specification](https://w3c.github.io/did-core/)

### Copyright Work DID Documents

If other identifier(s) for copyright work (ISBN, ISSN, DOI, EIDR, etc.) exist, the value(s) of this/these identifier(s) may be included in the [Also Known As](https://www.w3.org/TR/did-core/#also-known-as) property.

alsoKnownAs property can be seen in the following examples.
```json
{
  "id": "did:cr:w:4f75568d-6b09-4b07-ab99-7c9114b733b4",
  "alsoKnownAs": ["G500:1310377-1234567890", "10.1000/xyz123"],
}
```
The DID value of the copyright holder may be specified as the DID controller in copyright work DID document. If there is more than one copyright holder, it can be designated as a set of DID values. 

In addition to the properties specified in the [DID specification](https://w3c.github.io/did-core/), additional properties are as follows.
- target property [optional]

  The information about asset(a copyright work or a collection of copyright works) may be optionally included. The asset can be any form of identifiable resource, such as one Video-on-Demand content, a collection of movies, or several chapters of a book.
 
  To describe the asset, the target property of the [Asset Class](https://www.w3.org/TR/odrl-model/#asset) described in the [W3C ODRL Information Model specification](https://www.w3.org/TR/odrl-model/) may be included.

  If the asset can be recognized and used individually, directly, and specifically(e.g., individual VOD content or an entire book), it can be identified by id property without target property. If the target property is used, it must have the same value as the id property. 

  The target asset is also declared as an AssetCollection to indicate the resource is a collection of resources. If the VoD collection of several VoD contents is, the target asset of this VoD collection is declared as an AssetCollection. 
Refer [Asset Class](https://www.w3.org/TR/odrl-model/#asset) described in the [W3C ODRL Information Model specification](https://www.w3.org/TR/odrl-model/) for details.
```json
{
  "id": "did:cr:w:4f75568d-6b09-4b07-ab99-7c9114b733b4",
  "target": {
      "@type": "AssetCollection",
      "uid":  "http://example.com/vodcollection1011"
  },
}  
```

The copyright work DID document may have the following service endpoints.

* Right Offering Service 
	
  The right offer represents rules(permission, prohibitions, and duties) that are being offered from the copyright holder of the work identified by this DID. The right offer is typically used to make available policies to a wider audience, but does not grant any rules. As described in the [W3C ODRL Information Model specification](https://www.w3.org/TR/odrl-model/), the right offer is used for the creation of the right agreement between the assigner(e.g., copyright holder) and the assignee. The right agreement is typically used to grant the terms of the rules between the parties.
  * type: 'RightOffer'
  * serviceEndpoint: indicates the location where one ore more right offer information is provided.  

* Work Metadata Service
  
  The copyright work DID document does not provide metadata about the work itself (e.g., content type, genre, title, size/length, etc.). But, the metadata about the work can be expressed using several metadata standards(e.g., Dublin Core Metadata). The references to such metadata may be provided through this service endpoint.
  * type: 'workMetadata'
  * serviceEndpoint: indicates the location where metadata of copyright work is provided.

## CRUD Operation Definitions

### Create

The copyright holder can generate its DID and DID documents and optionally register through Copyright DID Registrar. This specification does not require the copyright holder DID and DID document to be registered.

In general, the copyright holder generates copyright work DID and DID documents. It is recommended that the copyright holder creates its own DID and DID document before generating the DID and DID document of the copyright work. Copyright DID registrar can perform authentication procedure for DID controller that point to copyright holder when registering the copyright work DID and DID document.

Care should be taken not to specify the DID value of the non-existent copyright holder in the DID Controller in the copyright work DID document.


### Read/Resolve
Refer [DID Specification](https://www.w3.org/TR/did-core/)and [Decentralized Identifier Resolution](https://w3c-ccg.github.io/did-resolution/)


### Update
If a right delegation occurs(for example, when Alice delegate copyright/asset ownership to Bob), the value of the DID controller may change from the DID of the delegator (Alice) to the DID of the delegatee (Bob). In this cases, the authority to change the current copyright work DID document belongs to the delegator (Alice). 

If there is a target property in the copyright work DID document, it is not recommended to change the value of the target property. If you want to change the target property, it is recommended to create a new copyright work DID and DID document rather than changing the target property.

Care should be taken not to specify the DID value of the non-existent copyright holder in the DID controller in the copyright work DID document.

### Deactivate

If the DID of the copyright holder is deactivated, care must be taken to prevent the existence of the copyright work DID and DID document with the DID controller as the DID of this copyright holder.
