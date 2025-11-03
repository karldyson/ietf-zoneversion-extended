%%%
title = "DNS Backend Zone Version Option"
abbrev = "dns-backend-zoneversion"
area = "Internet"
workgroup = "DNSOP"

[seriesInfo]
status = "standard"
name = "Internet-Draft"
value = "draft-ubbink-dnsop-backend-serial-zoneversion-00"
stream = "IETF"

date = 2025-11-02T12:00:00Z

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

This document defines a method for exposing a version identifier for the zone data in the backend via the DNS.

{mainmatter}

# Discussion Venues

This note is to be removed before publishing as an RFC.

Source for this draft and an issue tracker can be found at
https://github.com/SIDN/ietf-zoneversion-extended.git .

# Introduction

Often, the serial number is the identifier that is used to determine the "version" of a DNS zone.

However, things in the distribution path for a DNS zone may alter the serial
number even when the actual zone data doesn't change.

The notable example of this is when a DNSSEC signer performs signing activity
to maintain signatures and increments the SOA serial.

Whether the change of serial number is related to that signing activity,
and/or alteration of the zone data, is not visible to anyone looking at the zone via DNS.

Operators may want to be able to monitor and track the version of the zone
data that is being served by the DNS for reasons including tracking that the
correct version of the zone data is being seen by clients, and facilitating
alerting in the event that the zone data falls behind.

This document proposes a mechanism that facilitates exposing the backend zone data version via the DNS.

# Terminology and Definitions {#terminology}

The key words "**MUST**", "**MUST NOT**", "**REQUIRED**",
"**SHALL**", "**SHALL NOT**", "**SHOULD**", "**SHOULD NOT**",
"**RECOMMENDED**", "**NOT RECOMMENDED**", "**MAY**", and
"**OPTIONAL**" in this document are to be interpreted as described in
BCP 14 [@!RFC2119;@!RFC8174] when, and only when, they appear in all
capitals, as shown here.

# Relation to the ZONEVERSION option

This document extends the original DNS Zone Version Option [!@RFC9660] to
include an extra ZONEVERSION type.

# The backend version in the zone

Operators may have a variety of middleboxes involved in the distribution of DNS zones
to servers that ultimately serve the DNS zone to client networks.

Therefore, to make the backend version available in the ZONEVERSION, it is stored in the zone itself.

This document defines a label where the backend version will be available to be used in the ZONEVERSION.

The backend version will be published in a \_backend-version label at the zone apex.

For example:

    _backend-version.example.nl IN TXT "2025101099"

The setting of the value of the \_backend-version label is out of scope for
Inclusion of a \_backend-version label in any given DNS zone is **OPTIONAL**
however if present there **MUST** be no more than ONE.

# The backend version ZONEVERSION type {#backend-version}

This document defines a new ZONEVERSION option TYPE, which can be constructed
from RDATA in the zone.

The mnemonic for this type is "BACKEND-VERSION".

The VERSION value for the BACKEND-VERSION type **MUST** come from the RDATA of
the \_backend-version label in the zone. Unless the software already knows the
backend serial, then it **SHOULD** use that.
If there are multiple \_backend-version labels at the zone apex, these
**MUST** all be ignored.

# Security Considerations {#security}

Operators should be aware that this option could expose a small detail of the backend
versioning of data, and decide whether this is acceptable within their risk appetite.

# IANA Considerations {#iana}

## ZONEVERSION TYPE value

This document defines a new item for the ZONEVERSION TYPE option, entitled
BACKEND-VERSION (see (#backend-version)), and assigns a value of <TBD> from the
ZONEVERSION TYPE Values space:

| ZONEVERSION TYPE | Mnemonic        | Reference       |
| ---------------- | --------------- | --------------- |
| <TBD>            | BACKEND-VERSION | [this document] |

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

# Appendix A - Other Considerations (to be removed before publication)

This name is currently illegal in GTLDs per ICANN Registry Agreement:

https://itp.cdn.icann.org/en/files/registry-agreements/base-registry-agreement-21-01-2024-en.html#exhibitA

The author is advised that there is a process that can be followed that would
permit the operator of GTLD DNS zones to include this record.
