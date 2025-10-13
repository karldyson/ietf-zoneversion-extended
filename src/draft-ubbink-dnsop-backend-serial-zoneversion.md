%%%
title = "DNS Backend Serial Zone Version Option"
abbrev = "dns-backend-zoneversion"
area = "Internet"
workgroup = "DNSOP"

[seriesInfo]
status = "standard"
name = "Internet-Draft"
value = "draft-ubbink-dnsop-backend-serial-zoneversion-00"
stream = "IETF"

date = 2025-07-25T12:00:00Z

[[author]]
initials = "S.W.J."
surname = "Ubbink"
fullname = "Stefan Ubbink"
organization = "SIDN"
[author.address]
 email = "stefan.ubbink@sidn.nl"
 ui = "https://sidn.nl"
%%%

.# Abstract

The DNS Backend Serial Zone Version Opion is a way to get information about
the version of a DNS zone in the backend.
For example when a DNSSEC signer for a zone generates a new SOA serial, because
it has created new RRSIG records, the original data has not changed, but this
is not visible to anyone looking at the zone via DNS. This document will make it
possible to show the zone information which is the source of the presented data.


{mainmatter}

# Discussion Venues

This note is to be removed before publishing as an RFC.

Source for this draft and an issue tracker can be found at
https://github.com/SIDN/ietf-zoneversion-extended.git .


# Introduction

The DNS Backend Serial Zone Version Option is a way to get
information about the version of a DNS zone in the backend.
For example when a DNSSEC signer for a zone generates a new SOA serial, because
it has created new RRSIG records, the original data has not changed, but this
is not visible to anyone looking at the zone. 
This document makes it possible to retreive the backend version that is
the source of data in the DNS response.

# Terminology and Definitions {#terminology}

The key words "**MUST**", "**MUST NOT**", "**REQUIRED**",
"**SHALL**", "**SHALL NOT**", "**SHOULD**", "**SHOULD NOT**",
"**RECOMMENDED**", "**NOT RECOMMENDED**", "**MAY**", and
"**OPTIONAL**" in this document are to be interpreted as described in
BCP 14 [@!RFC2119;@!RFC8174] when, and only when, they appear in all
capitals, as shown here.

# Relation to the ZONEVERSION option

This document extends the original DNS Zone version Option [!@RFC9660] to
include an extra ZONEVERSION type.

# The backend serial in the zone

To make the backend serial available in the ZONEVERSION, it must be available
in the zone itself. Because creating an out of bound solution would be to much
work.
This document defines a label where the backend serial will be available to be used in the ZONEVERSION.

The backend serial will be published in a \_backend-version label under the zone apex.

For example:

    _backend-version.example.nl IN TXT "2025101099"

The setting of the value of the \_backend-version label is out of scope for
this document. The RDATA is not limited to a 32-bit number, like the
SOA-SERIAL. It can be any string.

There **MUST** only be one \_backend-version label.

# The backend serial ZONEVERSION type {#backend-serial}

This document defines a new ZONEVERSION option TYPE, which can be constructed
from RDATA in the zone.

The mnemonic for this type is "BACKEND-SERIAL".

The VERSION value for the BACKEND-SERIAL type **MUST** come from the RDATA of
the \_backend-version label in the zone. Unless the software already knows the
backend serial, then it **SHOULD** use that.
If there are multiple \_backend-version labels at the zone apex, these
**MUST** all be ignored.


# Security Considerations {#security}

When using the backend serial zoneversion option it will reveal a small bit of
information of your backend to the world. This should be a consius decission.

# IANA Considerations {#iana}

## ZONEVERSION TYPE value

This document defines a new item for the ZONEVERSION TYPE option, entitled
BACKEND-SERIAL (see (#backend-serial)), and assigns a value of <TBD> from the
ZONEVERSION TYPE Values space:

| ZONEVERSION TYPE | Mnemonic      | Reference       |
| ---------------- | ------------- | --------------- |
| <TBD>            |BACKEND-SERIAL | [this document] |

## \_backend-version Underscore Name

Per [@!RFC8552], IANA is requested to add the following entries to the
"Underscored and Globally Scoped DNS Node Names" registry:

| RR Type | \_NODE NAME       | Reference       |
| ------- | ----------------- | --------------- |
| TXT     | \_backend-version | [this document] |


# Acknowledgements

Many thanks to original authors of [@!RFC9660]. And the following reviewers:
Peter van Dijk.

{backmatter}

{#changelog}
# Change History (to be removed before publication)

* draft-ubbink-dnsop-backend-serial-zoneversion-00

> Use a better name for the draft.
> Use a different label for the backend version.
> Add security considerations

* draft-ubbink-zoneversion...

> Initial publication
