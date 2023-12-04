# A URI Scheme for Fediverse Resources

## Abstract

This document describes the "fedi" URI scheme.

The posts and accounts that populate the Fediverse, and certain other constructs such as hashtags, are "resources" in the sense described by [RFC3986](https://datatracker.ietf.org/doc/html/rfc3986). Since they are normally accessed over HTTP and HTTPS transports, they are conventionally identified by "https" URIs. However, the process by which Fediverse resources are retrieved and presented to users is more complex than a straightforward HTTPS request. The presence of the "fedi" scheme signals that this more complex process is appropriate.

## Terminology

The term "Fediverse" is loosely defined, but for the purposes of this document, it means the collection of servers and clients which are federated together based on the ActivityPub protocol.

The terms MUST and SHOULD are used in IETF style.

## Fediverse URIs In An HTTP World

Suppose that Ash has an account on a Fediverse instance at example.com. Their account would conventionally be identified in Fediverse messages as "@ash@example.com". Suppose Ash successfully posts a cat photo to their Fediverse account. From the HTTP point of view, its URI might be "https://example.com/@ash/111506589124254741". Suppose Lee has a Fediverse account at example.org, and follows Ash. Lee's Fediverse client would display Ash's post and allow Lee to boost it, reply to it, like it, mute or block Ash, and so on.

Suppose that Lee wants to share the post with their friend Pat so, using their Web browser, copies the URI and emails it to Pat. When Pat clicks on it, they will be taken to example.com and see the post. Even though Pat has a Fediverse account at example.net, because the browser is not a Fediverse client, they will not be able to boost, reply, or otherwise interact with it.

In this situation, Pat can copy the URI, paste it into their Fediverse client's "search" field, and eventually be able to interact with it, but this is non-intuitive and shouldn't be necessary.

This problem applies to URIs for accounts as well as posts. Should Lee email Pat saying "You should follow https://example.com/@ash", Pat won't be able to use that URI easily.

The "fedi" URI scheme described here is designed to help solve both these problems.

## Using "fedi" URIs

Given the "fedi" scheme, the URI for Ash's account would be "fedi://ash@example.com/" and for their cat picture, "fedi://ash@example.com/111506589124254741".

The URI identifying the #fediverse hashtag would be "fedi://hashtag/fediverse" and the URI identifying a search for "calico cat" would be "fedi://search/calico%20cat".

When a Web user-agent (including mobile Fediverse apps) encounters a "fedi" URI, the following would be an appropriate series of actions:

The user agent notices the scheme and dispatches to a scheme-specific module that knows how to handle these URIs.
That module would ascertain whether there is an active local Fediverse client and, if so, how to send requests to such a client. This document does not specify how this task is to be accomplished. On both Web browsers and mobile operating systems, it is possible to install scheme-selected protocol handlers, but other methods for doing this can be imagined.
The module would dispatch the information in the URI to the local Fediverse client. This document does not specifiy the interface between the user-agent module and the Fediverse client. Probably the simplest way to accomplish this is simply to send the URI as received, but other methods can be imagined.
The local Fediverse client would present the Fediverse account or post to the user, and allow interaction with it.

## Syntax of "fedi" URIs

This section specifies the syntax of "fedi" URIs using the terms "Scheme", "Authority", "User Information", "Host", "Port", "Path", "Query", and "Fragment", as specified in RFC3986].

The Scheme of a "fedi" URI MUST be "fedi".

There are two kinds of "fedi" URIs: Account-based and account-free.

In an account-based "fedi" URI, the Authority MUST include User Information whose value is an account name (the "Account") on a Fediverse instance (the "Instance"), and the Host must identify the Instance as described in section 3.2 of RFC3986. The Authority MUST NOT include a Port. In an account-based "fedi" URI, if a Path component is present, its value SHOULD be the unique identifier of a post by the Author on the Instance. Such a URI identifies an individual post on the Instance. This document does not limit the syntax of the Path beyond the constraints described in RFC3986.

If an account-based "fedi" URI does not contain a Path, it identifies the Author's account.

In an account-free "fedi" URI, the Authority MUST be one of a fixed set of strings, to be specified in subsequent specifications, which also specifies the allowed value for the Path for each allowed Authority string, and MUST NOT include User Information or a Port.

In a "fedi" URI, the Query and Fragment are reserved for use in future specifications.
