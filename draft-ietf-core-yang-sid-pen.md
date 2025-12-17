---
v: 3

docname: draft-ietf-core-yang-sid-pen-latest
title: >
  YANG-CBOR: Allocating SID ranges for PEN holders
abbrev: SID ranges for PEN holders
area: Applications and Real-Time Area (art)
wg: CoRE Working Group
kw: YANG-CBOR
cat: info
stream: IETF
# consensus: true
pi:
  strict: 'yes'
  toc: 'yes'
  tocdepth: '4'
  symrefs: 'yes'
  sortrefs: 'yes'
  compact: 'yes'
  subcompact: 'no'
author:
- ins: C. Bormann
  name: Carsten Bormann
  org: Universität Bremen TZI
  street: Postfach 330440
  city: D-28359 Bremen
  country: Germany
  phone: "+49-421-218-63921"
  email: cabo@tzi.org

venue:
  group: CoRE
  mail: core@ietf.org
  github: core-wg/sid-pen

normative:
  RFC9254: yang-cbor
  RFC9595: core-sid
#  IANA.enterprise-numbers: iana-pen
# Reference information as cached on the author's laptop, as long as we
# don't get https://github.com/ietf-tools/bibxml-service/issues/489 fixed:
  IANA.enterprise-numbers:
    -: iana-pen
    target: http://www.iana.org/assignments/enterprise-numbers
    title: Enterprise Numbers
    author:
    - org: IANA
    date: false
  IANA.yang-sid: # This one works!

informative:
  RFC1065:
  STD16: # RFC 1155, Section 3.1.4
  RFC5612: pen-doc
  I-D.ietf-core-yang-library: core-yang-library
  RFC8525: yang-library

--- abstract

YANG-CBOR (RFC 9254, "Encoding of Data Modeled with YANG in the
Concise Binary Object Representation (CBOR)") [^abs1a-] RFC 9595
("YANG Schema Item iDentifier (YANG SID)") [^abs1b-]

[^abs1a-]: defines
        YANG Schema Item iDentifiers (YANG SID), globally unique 63-bit
        unsigned integers used to identify YANG items.

[^abs1b-]: defines ways to allocate these SIDs on
        the basis of IANA registries.

[^abs2-]

[^abs2-]: The present specification employs these SID allocation
        mechanisms to allocate ranges with 100 000 SIDs
        (representation size 64 bits) each
        for each of the holders of IANA-registered
        Private Enterprise Numbers (PENs) < 1 000 000, as well as ranges with
        10 000 SIDs (representation size 32 bits) each
        for each of the holders of PENs < 100 000.

[^status]

[^status]:\\
    The present revision –04 is intended to address the feedback from
    the AD review and the IETF last call.
    \\
    Note that due to a regression in the `bib.ietf.org` service
    (<https://github.com/ietf-tools/bibxml-service/issues/489>), the
    reference [IANA.enterprise-numbers] may come out as "\*\*\* BROKEN
    REFERENCE \*\*\*" in some CI systems; this will certainly be fixed in
    the course of further processing.

--- middle

# Introduction

YANG-CBOR {{-yang-cbor}} [^abs1a-] {{-core-sid}} [^abs1b-]

[^abs2-]

IANA \[is requested to allocate/has allocated] a mega-range with
100 billion SIDs (representation size 64 bits), for the SID numbers 300 000 000 000 to 399 999 999 999.

IANA also \[is requested to allocate/has allocated] a mega-range with
1 billion SIDs (representation size 32 bits), for the SID numbers 3 000 000 000 to 3 999 999 999.

Private Enterprise Numbers (PENs) are registered in
{{IANA.enterprise-numbers}} in a low-threshold, low-overhead
registration process.
At the time of writing (~ 37 years after
creating this registry), around 65 000 PENs are registered.
In this document, the registrant for a PEN is referred to as the "PEN holder".

The present specification makes the following SID ranges available to
certain (current or future) PEN holders for allocation in a scheme defined
by the holder:

* The holder of a PEN ppp ppp (< 1 000 000) can use the SID numbers
3pp ppp p00 000 to 3pp ppp p99 999.
* The holder of a PEN pp ppp (< 100 000) can use the SID numbers
3 ppp pp0 000 to 3 ppp pp9 999.

# Example

{{-pen-doc}} has allocated Enterprise Number 32473 "for use in examples
in RFCs, books, documentation, and the like".

If this Enterprise Number had an actual PEN holder, the present
specification would confer control to it over the SID ranges:

* 3**03 247 3**00 000 up to 3**03 247 3**99 999, and
* 3 **324 73**0 000 up to 3 **324 73**9 999.

(The plaintext form of this document shows "*" characters around the
digits conveying the PEN, which are shown in **boldface** in the
typographic forms.)

As Enterprise Number 32473 is intended to be used in documentation,
the SIDs in the two SID ranges listed here for the documentation PEN
are consequently also available for use in documentation.

# Discussion

This allocation provides an extremely-low-threshold (zero-interaction)
way for PEN holders to get number space for the YANG SIDs used in
their YANG modules.
If a PEN is not already available to the entity needing such number
space, it can be obtained in a very low-threshold process.
Employing this number space is, however, not always the approach to
recommend to a module author:

* In the larger of the two spaces, each SID number needs a
  representation size of 64 bits ("64-bit SIDs").
  (This larger representation size of the absolute value of the SID is
  of comparatively little consequence due to the delta-encoding used for
  SIDs in YANG-CBOR.)
* For the holders of PENs < 100 000, there additionally is a smaller space
  where each SID number needs a representation size of 32 bits
  ("32-bit SIDs").
  PEN numbers that have access to this space (PEN < 100 000) are likely to run out
  before or around 2040; the
  expectation is that by that time there will be enough opportunities
  to request SID ranges within mega-ranges allocated by other registrants that
  this mechanism is less needed.
* This space has no infrastructure to discover the YANG module behind
  a SID.  Of course, each PEN holder can provide such infrastructure,
  but even then the problem remains of how to find that infrastructure
  for a SID.  (Search engines may mitigate this somewhat.)
  On the other hand, in some cases this relative obscurity may be exactly what a PEN
  holder wants to achieve by using this mechanism.

  If obscurity is not the intention, one or both of the following
  approaches are encouraged:

   * The PEN holder can provide a public repository where their YANG
     models can be found alongside the applicable SID files.
     Such a repository may be easy to set up using a popular git forge
     such as, at the time of writing, GitHub.

   * Implementations that employ PEN-based SIDs can facilitate
     information discovery by providing {{-core-yang-library}} or
     another form of YANG library {{-yang-library}}.

Relying on the PEN registry might theoretically trigger a land-grab by
prospective writers of YANG modules.
However, PENs have been around for decades (see {{Section 3.1.4 of
RFC1065}}, which continues to be in force with no technical changes as
{{Section 3.1.4 of RFC1155@STD16}}), and such a land-grab has not
occurred for the other allocations implicitly provided by obtaining a
PEN.

# IANA Considerations

[^replace-xxxx]: RFC Ed.: throughout this section, please replace
    RFC-XXXX with the RFC number of this specification and remove this
    note.

[^replace-xxxx]

As per {{Section 6.3 of -core-sid}},
in the {{sid-mega-range ("YANG-SID Mega-Ranges")<IANA.yang-sid}}
registry within the "YANG SIDs" registry group {{IANA.yang-sid}},
this document allocates two mega-ranges, one with 1 billion
SIDs ranging from 3 000 000 000 up to 3 999 999 999 (32-bit
representation size), and
one with 100 billion
SIDs ranging from 300 000 000 000 up to 399 999 999 999 (64-bit
representation size),
as summarized in {{tab-allocations}}.

| Entry Point        | Size               | Allocation | Org<br>Name | URL                                 |
| 3 000 000 000   | 1 000 000 000   | Private    | IANA     | https://rfc-editor.org/info/rfcxxxx |
| 300 000 000 000 | 100 000 000 000 | Private    | IANA     | https://rfc-editor.org/info/rfcxxxx |
{: #tab-allocations title="YANG-SID Mega-Range Allocations for use by PEN holders"}

IANA is requested to mark the following ranges as reserved for documentation:

* 303 247 300 000 up to 303 247 399 999
* 3 324 730 000 up to 3 324 739 999

An additional contact for the allocation is: IETF CORE Working Group
(core@ietf.org) or IETF Applications and Real-Time Area
(art@ietf.org).

The allocation policy inside the mega-range is "private".
The URL is that of the present specification.

The management of the SID block of 100 000 SIDs each, ranging from
3pp ppp p00 000 to 3pp ppp p99 999, is delegated to the PEN holder
for PEN ppp ppp (i.e., the PEN holder for ppp ppp controls SID
3pp ppp p00 000 to 3pp ppp p99 999).

Similarly, the management of the SID block of 10 000 SIDs each,
ranging from 3 ppp pp0 000 to 3 ppp pp9 999, is delegated to the PEN holder
for PEN pp ppp (i.e., the PEN holder for pp ppp controls SID
3 ppp pp0 000 to 3 ppp pp9 999).

{{Section 6.3.2 of -core-sid}} requires an organization that requests an
entry in the "YANG-SID Mega-Ranges" registry to ensure the technical
capacity to manage the SID ranges within those mega-ranges for a
period of at least 10 years (Private ranges).
The individual SID ranges within the mega-ranges allocated in this
document are assigned through the registration of PEN numbers.
The technical capacity to ensure the sustained operation of the PEN
number registry is derived from the demonstrated capacity of IANA to
maintain this registry as well as the importance of a functioning PEN
number registry in other contexts.

# Security Considerations

{{Section 5 (Security Considerations) of RFC9595}} applies, as well as
{{Section 8 (Security Considerations) of RFC9254}}.
In particular, the fact that a certain Private Enterprise Number
appears in a SID is not an indicator of provenance, i.e., it does not
guarantee that the SID or underlying YANG model actually does
originate from the holder of that PEN.
The requirement to ascertain the authoritative source of this
information, as discussed in the above security considerations, remains.

--- back

# Acknowledgments
{: numbered="false"}

This document was inspired by the discussion of the authors of
{{-yang-cbor}} and {{-core-sid}} on how to handle {{{Rob Wilton's}}} feedback.
