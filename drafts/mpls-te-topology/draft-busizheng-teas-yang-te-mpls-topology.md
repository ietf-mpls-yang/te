---
coding: utf-8

title: A YANG Data Model for MPLS-TE Topology

abbrev: MPLS-TE Topology YANG Model
docname: draft-busizheng-teas-yang-te-mpls-topology-04
workgroup: TEAS Working Group
category: std
ipr: trust200902

stand_alone: yes
pi: [toc, sortrefs, symrefs, comments]

author:
  -
    name: Italo Busi
    org: Huawei Technologies
    email: italo.busi@huawei.com
  -
    name: Aihua Guo
    org: Futurewei Inc.
    email: aihuaguo.ietf@gmail.com
  -
    name: Xufeng Liu
    org: IBM Corporation
    email: xufeng.liu.ietf@gmail.com
  -
    name: Tarek Saad
    org: Cisco Systems, Inc.
    email: tsaad.net@gmail.com
  -
    name: Rakesh Gandhi
    org: Cisco Systems, Inc.
    email: rgandhi@cisco.com

contributor:
  -
    name: Haomian Zheng
    org: Huawei Technologies
    email: zhenghaomian@huawei.com
  -
    name: Vishnu Pavan Beeram
    ins: V. Beeram
    org: Juniper Networks
    email: vbeeram@juniper.net
  -
    name: Igor Bryskin
    org: Individual
    email: i_bryskin@yahoo.com
  -
    name: Yanlei Zheng
    org: China Unicom
    email: zhengyanlei@chinaunicom.cn

--- abstract

  This document describes a YANG data model for Multi-Protocol Label
  Switching (MPLS) with Traffic Engineering (MPLS-TE) networks.

--- middle

{: #introduction}

# Introduction

  This document describes a YANG data model for Multi-Protocol Label
  Switching (MPLS) with Traffic Engineering (MPLS-TE) networks.

  This document also defines a collection of common data types and
  groupings in YANG data modeling language for MPLS-TE networks.  These
  derived common types and groupings are intended to be imported by the
  MPLS-TE topology model, defined in this document, as well as by the
  MPLS-TE tunnel model, defined in {{?I-D.ietf-teas-yang-te-mpls}}.

  Multi-Protocol Label Switching - Transport Profile (MPLS-TP) is a
  profile of the MPLS protocol that is used in packet switched
  transport networks and operated in a similar manner to other existing
  transport technologies (e.g., OTN), as described in {{?RFC5921}}. The YANG
  model defined in this document can also be for MPLS-TP networks.

## Tree Diagram

  A simplified graphical representation of the data model is used in
  {{mpls-te-topology-tree}} of this this document.  The meaning of the symbols in
  these diagrams is defined in {{!RFC8340}}.

{: #prefix}

## Prefixes in Data Node Names

  In this document, names of data nodes and other data model objects
  are prefixed using the standard prefix associated with the
  corresponding YANG imported modules, as shown in {{tab-prefixes}}.

| Prefix        | YANG module             | Reference
| rt-types      | ietf-routing-types      | {{!RFC8294}}
| tet           | ietf-te-topology        | {{!RFC8795}}
| tet-pkt       | ietf-te-topology-packet | {{!I-D.ietf-teas-yang-l3-te-topo}}
| te-packet-types | ietf-te-packet-types  | {{!I-D.ietf-teas-yang-l3-te-topo}}
| mte-types     | ietf-mpls-te-types      | This document
| tet-mpls      | ietf-te-mpls-topology   | This document
{: #tab-prefixes title="Prefixes and corresponding YANG modules"}

{: #mpls-te-types-overview}

# MPLS-TE Types Overview

  The module ietf-mpls-te-types contains the following YANG
  types and groupings which can be
  reused by MPLS-TE YANG models:

  load-balancing-type:

  > This identify defines the types of load-balancing algorithms used on
  bundled MPLS-TE link.

  te-mpls-label-hop:

  > This grouping is used for the augmentation of TE label for MPLS-TE
  path.

{: #mpls-te-topo-overview}

# MPLS-TE Topology Model Overview

  The MPLS-TE technology specific topology model augments the ietf-te-
  topology-packet YANG module, defined in {{!I-D.ietf-teas-yang-l3-te-topo}}, which in
  turns augment the generic ietf-te-topology YANG module, defined in
  {{!RFC8795}}, as shown in {{fig-mpls-te-topo}}.

~~~~ ascii-art
                  +------------------+    o: augment
     TE generic   | ietf-te-topology |
                  +------------------+
                            o
                            |
                            |
                            |
               +-------------------------+
     Packet TE | ietf-te-topology-packet |
               +-------------------------+
                            o
                            |
                            |
                            |
                +-----------------------+
     MPLS-TE    | ietf-te-mpls-topology |
                +-----------------------+
~~~~
{: #fig-mpls-te-topo title="Relationship between MPLS-TE, Packet-TE and TE topology models"}

  Given the guidance for augmentation in {{!RFC8795}}, the following
  technology-specific augmentations need to be provided:

  - A network-type to indicate that the TE topology is an MPLS-TE
    Topology, as follow:

~~~~
      augment /nw:networks/nw:network/nw:network-types/tet:te-topology
              /tet-pkt:packet:
        +--rw mpls-topology!
~~~~

  - TE Label Augmentations as described in {{mpls-te-label}}.

Note: TE Bandwidth Augmentations for paths, LSPs and links are provided by the ietf-te-topology-packet module, defined in {{!I-D.ietf-teas-yang-l3-te-topo}}.

{: #mpls-te-label}

## TE Label Augmentations

  In MPLS-TE, the label allocation is done by NE, information about
  label values availability is not necessary to be provided to the
  controller. Moreover, MPLS-TE tunnels are currently established
  within a single domain.

  Therefore this document does not define any MPLS-TE
  technology-specific augmentations, of the TE Topology model, for the
  TE label since no TE label related attributes should be instantiated
  for MPLS-TE Topologies.

  Open issue: shall this module allows the setup of MPLS-TE
  multi-domain tunnels?

{: #mpls-tp-topology}

## MPLS-TP Topology

  Multi-Protocol Label Switching - Transport Profile (MPLS-TP) is a
  profile of the MPLS protocol that is used in packet switched
  transport networks and operated in a similar manner to other existing
  transport technologies (e.g., OTN), as described in {{?RFC5921}}.

  Therefore YANG model defined in this document can also be applicable
  for MPLS-TP networks.

  However, as described in {{?RFC5921}}, MPLS-TP networks support
  bidirectional LSPs and require no ECMP and no PHP. When reporting the
  topology for an MPLS-TP network, additional information is required
  to indicate whether the network support these MPLS-TP
  characteristics.

  It is worth noting that {{!RFC8795}} is already capable to model TE
  topologies supporting either unidirectional or bidirectional LSPs:
  all bidirectional TE links can support bidirectional LSPs and all the
  links can support unidirectional LSPs and it is always possible to
  associated unidirectional LSPs as long as they belong to the same
  tunnel.

  When setting up bidirectional LSPs (e.g., MPLS-TP LSPs) only
  bidirectional TE Links are selected by path computation.

  In order to allow reporting that ECMP is not affecting forwarding the
  packets of a given LSP, the load-balancing-type attribute reports
  whether a LAG or TE Bundled Link performs load-balancing on a
  per-flow or per-top-label:

~~~~
    augment /nw:networks/nw:network/nt:link/tet:te:
      +--rw load-balancing-type?   mte-types:load-balancing-type
~~~~

  When setting up LSPs which do not requires ECMP (e.g., MPLS-TP LSPs)
  only Links that are not part of a LAG or TE Bundle or that performs
  per-top-label load balancing are selected by path computation.

  It is assumed that almost all the MPLS-TE nodes are capable to
  support Ultimate Hop Popping (UHP). However, if some interfaces are
  not able to support UHP, they can report it in the MPLS-TE topology:

~~~~
    augment /nw:networks/nw:network/nw:node/nt:termination-point
            /tet:te:
      +--ro uhp-incapable?   empty
~~~~

  When setting up LSPs which do not requires PHP (e.g., MPLS-TP LSPs)
  only the interfaces (LTPs) which are capable to support UHP in the
  destination node are selected by path computation.

{: #pck-te-types-yang}

# YANG model for common MPLS-TE Types

~~~~ yang
{::include ../../ietf-mpls-te-types.yang}
~~~~
{: #fig-mpls-te-types-yang title="MPLS-TE Types YANG model" sourcecode-markers="true" sourcecode-name="ietf-mpls-te-types@2022-11-07.yang"}

{: #mpls-te-topology}

# YANG model for MPLS-TE Topology

{: #mpls-te-topology-tree}

## YANG Tree

  {{fig-mpls-te-topology-tree}} below shows the tree diagram of the YANG model defined in
  module ietf-te-mpls-topology.yang.

~~~~ ascii-art
{::include ../../ietf-te-mpls-topology.tree}
~~~~
{: #fig-mpls-te-topology-tree title="MPLS-TE topology YANG tree" artwork-name="ietf-te-mpls-topology.tree"}

{: #mpls-te-topology-yang}

## YANG Code

~~~~ yang
{::include ../../ietf-te-mpls-topology.yang}
~~~~
{: #fig-mpls-te-topology-yang title="MPLS-TE topology YANG module" sourcecode-markers="true" sourcecode-name="ietf-te-mpls-topology@2022-11-07.yang"}

{: #security}

# Security Considerations

  To be added.

{: #iana}

# IANA Considerations

  To be added.

--- back

{: numbered="false"}

# Acknowledgments

  We thank Loa Andersson for providing useful suggestions for this draft.

  This document was prepared using kramdown.

  Previous versions of this document was prepared using 2-Word-v2.0.template.dot.
