---
v: 3

docname: draft-ietf-core-yang-sid-pen-latest
title: >
  YANG-CBOR: Allocating SID ranges for PEN holders
abbrev: SID ranges for PEN holders
area: Applications and Real-Time Area (art)
wg: Internet Engineering Task Force
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
  IANA.enterprise-numbers: iana-pen
informative:
  RFC1065:
  STD16: # RFC 1155, Section 3.1.4
  I-D.ietf-core-yang-library: core-yang-library
  RFC8525: yang-library

--- abstract

YANG-CBOR, RFC 9254 [^abs1-]

[^abs1-]: defines
        YANG Schema Item iDentifiers (YANG SID), globally unique 63-bit
        unsigned integers used to identify YANG items.
        RFC 9595 defines ways to allocate these SIDs on
        the basis of IANA registries.

[^abs2-]

[^abs2-]: The present specification employs these SID allocation
        mechanisms to allocate ranges with 100 000 63-bit SIDs each
        for each of the first 1 000 000 holders of IANA-registered
        Private Enterprise Numbers (PENs), as well as ranges with 10 000 32-bit SIDs each
        for each of the first 100 000 holders.

--- middle

# Introduction

YANG-CBOR {{-yang-cbor}} [^abs1-]

[^abs2-]

IANA \[is requested to allocate/has allocated] 100 000 mega-ranges, for the SID numbers
300 000 000 000 to 399 999 999 999.

IANA also \[is requested to allocate/has allocated] 1000 mega-ranges, for the SID numbers
3 000 000 000 to 3 999 999 999.

Private Enterprise Numbers (PENs) are registered in
{{IANA.enterprise-numbers}} in a low-threshold, low-overhead
registration process.
At the time of writing (~ 37 years after
creating this registry), around 65 000 PENs are registered.
We speak of the registrant for a PEN as the "PEN holder".

The present specification makes the following SID ranges available to
certain (current or future) PEN holders for allocation in a scheme defined
by the holder:

* The holder of a PEN ppp ppp (< 1 000 000) can use the SID numbers
3pp ppp p00 000 to 3pp ppp p99 999.
* The holder of a PEN pp ppp (< 100 000) can use the SID numbers
3 ppp pp0 000 to 3 ppp pp9 999.

# Example

The Department for Mathematics and Computer Science of {{{Universität Bremen}}} holds PEN 30810.

To this PEN holder, the present specification confers control over the
SID ranges:

* 3**03 081 0**00 000 up to 3**03 081 0**99 999, and
* 3 **308 10**0 000 up to 3 **308 10**9 999.

(The plaintext form of this document shows "*" characters around the
digits conveying the PEN, which are shown in **boldface** in the
typographic forms.)

# Discussion

This allocation provides an extremely-low-threshold (zero-interaction)
way for PEN holders to get number space for the YANG SIDs used in
their YANG modules.
If a PEN is not already available to the entity needing such number
space, it can be obtained in a very low-threshold process.
Employing this number space is, however, not always the approach to
recommend to a module author:

* The large space uses 64-bit numbers.  While this is of relatively little
  consequence due to the delta-encoding used for SIDs in YANG-CBOR, a
  few further bytes can be saved by allocating the SIDs in one of the
  mega-ranges that are specifically allocated by an organization
  (which, for the first 2000 or so, will lead to 32-bit outer deltas).
* For the first 100 000 PEN holders, there also is a smaller space that
  uses 32-bit numbers.
  This space is likely to run out before or around 2040; the
  expectation is that by that time there will be enough opportunities
  to request ranges from a megarange operator that this mechanism is
  no longer needed.
* This space has no infrastructure to discover the YANG module behind
  a SID.  Of course, each PEN holder can provide such infrastructure,
  but even then the problem remains how to find that infrastructure
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
{{Section 3.1.4 of RFC1155@STD16}}), and such a land-grab hasn't
occurred for the other allocations implicitly provided by obtaining a
PEN.

# IANA Considerations

This document allocates 100 000 63-bit and 1000 32-bit SID mega-ranges
as per {{Section 7.4 of -core-sid}}.

The contact for the allocation is: IETF CORE Working Group
      (core@ietf.org) or IETF Applications and Real-Time Area
      (art@ietf.org).

The allocation policy inside the mega-range is "private".
The URL is that of the present specification.

The management of the SID blocks of 100 000 SIDs each, 10 such blocks
for each mega-range 3nn nnn 000 000, is delegated to the PEN holder
for nnn nnx, where x is the sequence number of the SID block in the
mega-range (i.e., the PEN holder for nnn nnx controls SID
3nn nnn x00 000 to 3nn nnn x99 999).

Similarly, the management of the SID blocks of 10 000 SIDs each, 100 such blocks
for each mega-range 3 nnn 000 000, is delegated to the PEN holder
for nn nxx, where x is the sequence number of the SID block in the
mega-range (i.e., the PEN holder for nn nxx controls SID
3 nnn xx0 000 to 3 nnn xx9 999).

The technical capacity to ensure the sustained operation of the
registry for a period of at least 10 years (as required for registries
of class "private") is derived from the capacity of IANA to maintain
the PEN number registry.

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
{{-yang-cbor}} and {{-core-sid}} how to handle {{{Rob Wilton's}}} feedback.
