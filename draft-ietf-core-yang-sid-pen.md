---
v: 3

docname: draft-bormann-core-yang-sid-pen-latest
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
  github: cabo/sid-pen

normative:
  RFC9254: yang-cbor
  RFC9595: core-sid
informative:
  RFC1065:

--- abstract

YANG-CBOR, RFC 9254 [^abs1-]

[^abs1-]: defines
        YANG Schema Item iDentifiers (YANG SID), globally unique 63-bit
        unsigned integers used to identify YANG items.
        RFC 9595 defines ways to allocate these SIDs on
        the basis of IANA registries.

[^abs2-]

[^abs2-]: The present specification uses these SID allocation
        mechanisms to allocate ranges with 100 000 63-bit SIDs each
        for each of the first 1 000 000 holders of IANA-registered
        Private Enterprise Numbers (PENs), as well as ranges with 10 000 32-bit SIDs each
        for each of the first 100 000 holders.

--- middle

# Introduction

YANG-CBOR, {{-yang-cbor}} [^abs1-]

[^abs2-]

IANA \[is requested to allocate/has allocated] 100 000 mega-ranges, for the SID numbers
300 000 000 000 to 399 999 999 999.

IANA also \[is requested to allocate/has allocated] 1000 mega-ranges, for the SID numbers
3 000 000 000 to 3 999 999 999.

The holder of a PEN ppp ppp then can use the SID numbers
3pp ppp p00 000 to 3pp ppp p99 999 for allocation in a scheme defined
by the holder.
The holder of a PEN pp ppp then can use the SID numbers
3 ppp pp0 000 to 3 ppp pp9 999 for allocation in a scheme defined
by the holder.

# Example

The Department for Mathematics and Computer Science of {{{Universität Bremen}}} holds PEN 30810.

This confers control over the SID range
303 081 000 000 up to
303 081 099 999,
as well as
3 308 100 000 up to
3 308 109 999 up to
to this PEN holder.

# Discussion

This allocation provides an extremely-low-threshold way for PEN holders
to get number space for the YANG SIDs used in their YANG modules.
It is, however, not always the approach to recommend to a module author:

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
  On the other hand, relative obscurity may be exactly what a PEN
  holder wants to achieve by using this mechanism.

Relying on the PEN registry might theoretically trigger a land-grab by
prospective writers of YANG modules.  However, PENs have been around
for decades {{RFC1065}} and such a land-grab hasn't occurred for the other
allocations implicitly provided by obtaining a PEN.

# IANA Considerations

This document allocates 100 000 63-bit and 1000 32-bit SID mega-ranges
as per {{Section 7.4 of -core-sid}}.

The contact for the allocation is: IETF CORE Working Group
      (core@ietf.org) or IETF Applications and Real-Time Area
      (art@ietf.org)

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

--- back

# Acknowledgments
{: numbered="false"}

This document was inspired by the discussion of the authors of
{{-yang-cbor}} and {{-core-sid}} how to handle {{{Rob Wilton's}}} feedback.
