---
stand_alone: true
ipr: trust200902
docname: draft-bormann-core-yang-sid-pen-latest
title: >
  YANG-CBOR: Allocating SID ranges for PEN holders
abbrev: SID ranges for PEN holders
area: Applications and Real-Time Area (art)
wg: Internet Engineering Task Force
kw: CBOR
cat: info
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

normative:
  I-D.ietf-core-yang-cbor: yang-cbor
  I-D.ietf-core-sid: core-sid

--- abstract

[^abs1-]

[^abs1-]: YANG-CBOR, RFC XXXX (draft-ietf-core-yang-cbor) defines
        YANG Schema Item iDentifiers (YANG SID), globally unique 63-bit
        unsigned integers used to identify YANG items.
        RFC YYYY (draft-ietf-core-sid) defines ways to allocate these SIDs on
        the basis of IANA registries.

[^abs2-]

[^abs2-]: The present specification uses these SID allocation mechanisms
        to allocate 100 000 SIDs for each holder of the first 1 000 000
        holders of IANA-registered Private Enterprise Numbers (PENs).

--- middle

# Introduction

[^abs1-]

[^abs2-]

We allocate 100 000 mega-ranges, for the SID numbers
300 000 000 000 to 399 999 999 999.

The holder of a PEN ppp ppp then can use the SID numbers
3pp ppp p00 000 to 3pp ppp p99 999 for allocation in a scheme defined
by the holder.

# Example

The Department for Mathematics and Computer Science of {{{Universität Bremen}}} holds PEN 30810.

This confers control over the SID range
303 081 000 000 up to
303 081 099 999 to this department.

# IANA Considerations

This document allocates 100 000 SID mega-ranges as per {{Section 7.4 of
-core-sid}}.

The contact for the allocation is: IETF CORE Working Group
      (core@ietf.org) or IETF Applications and Real-Time Area
      (art@ietf.org)

The allocation policy inside the mega-range is "private".
The URL is that of the present specification.

The management of the SID blocks of 100 000 SIDs each, 10 such blocks
for each mega-range 3nn nnn 000 000, is delegated to the PEN holder
for nnn nnx, where x is the sequence number of the SID block in the
mega-range.

The technical capacity to ensure the sustained operation of the
registry for a period of at least 10 years (as required for registries
of class "private") is derived from the capacity of IANA to maintain
the PEN number registry.

--- back

# Acknowledgments
{: numbered="false"}

This document was inspired by the discussion of the authors if
{{-yang-cbor}} and {{-core-sid}} how to handle {{{Rob Wilton's}}}
