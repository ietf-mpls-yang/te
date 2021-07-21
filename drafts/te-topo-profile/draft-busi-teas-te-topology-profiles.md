---
coding: utf-8

title: >
   Profiles for Traffic Engineering (TE) Topology Data Model and
   Applicability to non-TE Use Cases

abbrev: TE Topology Profiles
docname: draft-busi-teas-te-topology-profiles-03
workgroup: TEAS Working Group
category: info
ipr: trust200902

stand_alone: yes
pi: [toc, sortrefs, symrefs, comments]

author:
  -
    name: Italo Busi
    org: Huawei
    email: italo.busi@huawei.com
  -
    name: Xufeng Liu
    org: Volta Networks
    email: xufeng.liu.ietf@gmail.com
  -
    name: Igor Bryskin
    org: Individual
    email: i_bryskin@yahoo.com
  -
    name: Vishnu Pavan Beeram
    ins: V. Beeram
    org: Juniper Networks
    email: vbeeram@juniper.net
  -
    name: Tarek Saad
    org: Juniper Networks
    email: tsaad@juniper.net
  -
    name: Oscar Gonzalez de Dios
    ins: O. Gonzalez de Dios
    org: Telefonica
    email: oscar.gonzalezdedios@telefonica.com

contributor:
-
    name: Aihua Guo
    org: Futurewei Inc.
    email: aihuaguo.ietf@gmail.com
-
    name: Haomian Zheng
    org: Huawei
    email: zhenghaomian@huawei.com
-
    name: Sergio Belotti
    org: Nokia
    email: sergio.belotti@nokia.com

--- abstract

   This document describes how profiles of the Traffic Engineering (TE)
   Topology Model, defined in RFC8795, can be used to address
   applications beyond "Traffic Engineering".

--- middle

{: #introduction}

# Introduction

   There are many network scenarios being discussed in various IETF
   Working Groups (WGs) that are not classified as "Traffic Engineering"
   but can be addressed by a sub-set (profile) of the Traffic
   Engineering (TE) Topology YANG data model, defined in {{!RFC8795}}.

   Traffic Engineering (TE) is defined in {{?I-D.ietf-teas-rfc3272bis}} as aspects of
   Internet network engineering that deal with the issues of performance
   evaluation and performance optimization of operational IP networks.
   TE encompasses the application of technology and scientific
   principles to the measurement, characterization, modeling, and
   control of Internet traffic.

   The TE Topology Model is augmenting the Network Topology Model
   defined in {{!RFC8345}} with generic and technology-agnostic features
   that some are strictly applicable to TE networks, while others
   applicable to both TE and non-TE networks.

   Examples of such features that are applicable to both TE and non-TE
   networks are: inter-domain link discovery (plug-id), geo-
   localization, and admin/operational status.

   It is also worth noting that the TE Topology Model is quite an
   extensive and comprehensive model in which most features are
   optional. Therefore, even though the full model appears to be
   complex, at the first glance, a sub-set of the model (profile) can be
   used to address specific scenarios, e.g. suitable also to non-TE use
   cases.

   The implementation of such TE Topology profiles can simplify and
   expedite adoption of the full TE topology YANG data model, and allow
   for its reuse even for non-TE use case. The key question being
   whether all or some of the attributes defined in the TE Topology
   Model are needed to address a given network scenario.

   {{examples}} provides examples where profiles of the TE Topology Model
   can be used to address some generic use cases applicable to both TE
   and non-TE technologies.

{: #examples}

# Examples of non-TE scenarios

{: #uni-discovery}

## UNI Topology Discovery

   UNI Topology Discovery is independent from whether the network is TE
   or non-TE.

   The TE Topology Model supports inter-domain link discovery (including
   but not being limited to UNI link discovery) using the plug-id
   attribute. This solution is quite generic and does not require the
   network to be a TE network.

   The following profile of the TE Topology model can be used for the
   UNI Topology Discovery:

~~~~
   module: ietf-te-topology
     augment /nw:networks/nw:network/nw:network-types:
       +--rw te-topology!
     augment /nw:networks/nw:network/nw:node/nt:termination-point:
       +--rw te-tp-id?   te-types:te-tp-id
       +--rw te!
          +--rw admin-status?
          |       te-types:te-admin-status
          +--rw inter-domain-plug-id?          binary
          +--ro oper-status?                   te-types:te-oper-status
~~~~
{: #uni-discovery-tree title="UNI Topology"}

   The profile data model shown in {{uni-discovery-tree}} can be used to discover TE
   and non TE UNIs as well as to discover UNIs for TE or non TE
   networks.

   Such a UNI TE Topology profile model can also be used with
   technology-specific UNI augmentations, as described in section 3.

   For example, in {{?I-D.ietf-ccamp-eth-client-te-topo-yang}},
   the eth-svc container is defined to
   represent the capabilities of the Termination Point (TP) to be
   configured as an Ethernet client UNI, together with the Ethernet
   classification and VLAN operations supported by that TP.

   The {{?I-D.ietf-ccamp-otn-topo-yang}} provides another example, where:

-  the client-svc container is defined to represent the capabilities
   of the TP to be configured as an transparent client UNI (e.g.,
   STM-N, Fiber Channel or transparent Ethernet);

-  the OTN technology-specific Link Termination Point (LTP)
   augmentations are defined to represent the capabilities of the TP
   to be configured as an OTN UNI, together with the information
   about OTN label and bandwidth availability at the OTN UNI.

   For example, the UNI TE Topology profile can be used to model
   features defined in {{?I-D.ogondio-opsawg-uni-topology}}:

-  The inter-domain-plug-id attribute would provide the same
   information as the attachment-id attribute defined in
   {{?I-D.ogondio-opsawg-uni-topology}};

-  The admin-status and oper-status that exists in this TE topology
   profile can provide the same information as the admin-status and
   oper-status attributes defined in {{?I-D.ogondio-opsawg-uni-topology}}.

   Following the same approach in {{?I-D.ietf-ccamp-eth-client-te-topo-yang}}
   and {{?I-D.ietf-ccamp-otn-topo-yang}}, the type
   and encapsulation-type attributes can be defined by technology-
   specific UNI augmentations to represent the capability of a TP to be
   configured as a L2VPN/L3VPN UNI Service Attachment Point (SAP).

   The advantages of using a TE Topology profile would be having common
   solutions for:

-  discovering UNIs as well as inter-domain NNI links, which is
   applicable to any technology (TE or non TE) used at the UNI or
   within the network;

-  modelling non TE UNIs such as Ethernet, and TE UNIs such as OTN,
   as well as UNIs which can configured as TE or non-TE (e.g., being
   configured as either Ethernet or OTN UNI).

{: #admin-oper-state}

## Administrative and Operational status management

   The TE Topology Model supports the management of administrative and
   operational state, including also the possibility to associate some
   administrative names, for nodes, termination points and links. This
   solution is generic and also does not require the network to be a TE
   network.

   The following profile of the TE Topology Model can be used for
   administrative and operational state management:

~~~~
   module: ietf-te-topology
     augment /nw:networks/nw:network/nw:network-types:
       +--rw te-topology!
     augment /nw:networks/nw:network:
       +--rw te-topology-identifier
       |  +--rw provider-id?   te-global-id
       |  +--rw client-id?     te-global-id
       |  +--rw topology-id?   te-topology-id
       +--rw te!
          +--rw name?                     string
     augment /nw:networks/nw:network/nw:node:
       +--rw te-node-id?   te-types:te-node-id
       +--rw te!
          +--rw te-node-attributes
          |  +--rw admin-status?               te-types:te-admin-status
          |  +--rw name?                       string
          +--ro oper-status?                   te-types:te-oper-status
     augment /nw:networks/nw:network/nt:link:
       +--rw te!
          +--rw te-link-attributes
          |  +--rw name?                       string
          |  +--rw admin-status?               te-types:te-admin-status
          +--ro oper-status?                   te-types:te-oper-status
     augment /nw:networks/nw:network/nw:node/nt:termination-point:
       +--rw te-tp-id?   te-types:te-tp-id
       +--rw te!
          +--rw admin-status?                  te-types:te-admin-status
          +--rw name?                          string
          +--ro oper-status?                   te-types:te-oper-status
~~~~
{: #admin-oper-state-tree title="Generic Topology with admin and operational state"}

   The TE topology data model profile shown in {{admin-oper-state-tree}} is applicable to
   any technology (TE or non-TE) that requires management of the
   administrative and operational state and administrative names for
   nodes, termination points and links.

{: #geolocation}

## Geolocation

   The TE Topology model supports the management of geolocation
   coordinates for nodes and termination points. This solution is
   generic and does not necessarily require the network to be a TE
   network.

   The TE topology data model profile shown in {{geolocation-tree}}
   can be used to
   model geolocation data for networks.

~~~~
   module: ietf-te-topology
     augment /nw:networks/nw:network/nw:network-types:
       +--rw te-topology!
     augment /nw:networks/nw:network/nw:node/nt:termination-point:
       +--rw te-tp-id?   te-types:te-tp-id
       +--rw te!
          +--ro geolocation
             +--ro altitude?    int64
             +--ro latitude?    geographic-coordinate-degree
             +--ro longitude?   geographic-coordinate-degree
     augment /nw:networks/nw:network/nw:node:
       +--rw te-node-id?   te-types:te-node-id
       +--rw te!
          +--ro geolocation
             +--ro altitude?    int64
             +--ro latitude?    geographic-coordinate-degree
             +--ro longitude?   geographic-coordinate-degree
     augment /nw:networks/nw:network/nw:node/nt:termination-point:
       +--rw te-tp-id?   te-types:te-tp-id
       +--rw te!
          +--ro geolocation
             +--ro altitude?    int64
             +--ro latitude?    geographic-coordinate-degree
             +--ro longitude?   geographic-coordinate-degree
~~~~
{: #geolocation-tree title="Generic Topology with geolocation information"}

   This profile is applicable to any network technology (TE or non-TE)
   that requires management of the geolocation information for its nodes
   and termination points.

{: #overlay-underlay}

## Overlay and Underlay non-TE Topologies

   The TE Topology model supports the management of overlay/underlay
   relationship for nodes and links, as described in section 5.8 of
   {{!RFC8795}}. This solution is generic and does not require the network
   to be a TE network.

   The following TE topology data model profile can be used to manage
   overlay/underlay network data:

~~~~
   module: ietf-te-topology
     augment /nw:netorks/nw:network/nw:network-types:
       +--rw te-topology!
     augment /nw:networks/nw:network/nw:node:
       +--rw te-node-id?   te-types:te-node-id
       +--rw te!
          +--rw te-node-attributes
             +--rw underlay-topology {te-topology-hierarchy}?
                +--rw network-ref?   -> /nw:networks/network/network-id
     augment /nw:networks/nw:network/nt:link:
       +--rw te!
          +--rw te-link-attributes
             +--rw underlay {te-topology-hierarchy}?
                +--rw enabled?                     boolean
                +--rw primary-path
                   +--rw network-ref?
                   |       -> /nw:networks/network/network-id
                   +--rw path-element* [path-element-id]
                      +--rw path-element-id              uint32
                      +--rw (type)?
                         +--:(numbered-link-hop)
                         |  +--rw numbered-link-hop
                         |     +--rw link-tp-id    te-tp-id
                         |     +--rw hop-type?     te-hop-type
                         |     +--rw direction?    te-link-direction
                         +--:(unnumbered-link-hop)
                            +--rw unnumbered-link-hop
                               +--rw link-tp-id    te-tp-id
                               +--rw node-id       te-node-id
                               +--rw hop-type?     te-hop-type
                               +--rw direction?    te-link-direction
~~~~
{: #overlay-underlay-tree title="Generic Topology with overlay/underlay information"}

   This profile is applicable to any technology (TE or non-TE) when it
   is needed to manage the overlay/underlay information. It is also
   allows a TE underlay network to support a non-TE overlay network and,
   vice versa, a non-TE underlay network to support a TE overlay
   network.

{: #switching-limitations}

## Nodes with switching limitations

   A node can have some switching limitations where connectivity is not
   possible between all its TP pairs, for example when:

   -  the node represents a physical device with switching limitations;

   -  the node represents an abstraction of a network topology.

   This scenario is generic and applies to both TE and non-TE
   technologies.

   A connectivity TE Topology profile data model supports the management
   of the node connectivity matrix to represent feasible connections
   between termination points across the nodes. This solution is generic
   and does not necessarily require a TE enabled network.

   The following profile of the TE Topology model can be used for nodes
   with connectivity constraints:

~~~~
   module: ietf-te-topology
     augment /nw:networks/nw:network/nw:network-types:
       +--rw te-topology!
     augment /nw:networks/nw:network/nw:node:
       +--rw te-node-id?   te-types:te-node-id
       +--rw te!
          +--rw te-node-attributes
             +--rw connectivity-matrices
                +--rw number-of-entries?     uint16
                +--rw is-allowed?            boolean
                +--rw connectivity-matrix* [id]
                   +--rw id                  uint32
                   +--rw from
                   |  +--rw tp-ref?               leafref
                   +--rw to
                   |  +--rw tp-ref?               leafref
                   +--rw is-allowed?              boolean
~~~~
{: #switching-limitations-tree title="Generic Topology with connectivity constraints"}

   The TE topology data model profile shown in {{switching-limitations-tree}}
   is applicable to
   any technology (TE or non-TE) networks that requires managing nodes
   with certain connectivity constraints. When used with TE
   technologies, additional TE attributes, as defined in {{!RFC8795}}, can
   also be provided.

{: #augmentations}

# Technology-specific augmentations

   There are two main options to define technology-specific Topology
   Models which can use the attributes defined in the TE Topology Model
   {{!RFC8795}}.

   Both options are applicable to any possible profile of the TE
   Topology Model, such as those defined in {{examples}}.

   The first option is to define a technology-specific TE Topology Model
   which augments the TE Topology Model, as shown in {{te-augment-fig}}:

~~~~
                           +-------------------+
                           | Network Topology  |
                           +-------------------+
                                     ^
                                     |
                                     | Augments
                                     |
                         +-----------+-----------+
                         |      TE Topology      |
                         |       (profile)       |
                         +-----------------------+
                                     ^
                                     |
                                     | Augments
                                     |
                          +----------+----------+
                          | Technology-Specific |
                          |     TE Topology     |
                          +---------------------+
~~~~
{: #te-augment-fig title="Augmenting the TE Topology Model"}

   This approach is more suitable for cases when the technology-specific
   TE topology model provides augmentations to the TE Topology
   constructs, such as bandwidth information (e.g., link bandwidth),
   tunnel termination points (TTPs) or connectivity matrices. It also
   allows providing augmentations to the Network Topology constructs,
   such as nodes, links, and termination points (TPs).

   This is the approach currently used in {{?I-D.ietf-ccamp-eth-client-te-topo-yang}}
   and {{?I-D.ietf-ccamp-otn-topo-yang}}.

   It is worth noting that a profile of the technology-specific TE
   Topology model not using any TE topology attribute or constructs can
   be used to address any use case that do not require these attributes.
   In this case, only the te-topology presence container of the TE
   Topology Model needs to be implemented.

   The second option is to define a technology-specific Network Topology
   Model which augments the Network Topology Model and to rely on the
   multiple inheritance capability, which is implicit in the network-
   types definition of {{!RFC8345}}, to allow using also the generic
   attributes defined in the TE Topology model:

~~~~
                    +-----------------------+
                    |    Network Topology   |
                    +-----------------------+
                        ^               ^
                        |               |
           Augments +---+               +--+ Augments
                    |                      |
          +---------+---+       +----------+----------+
          | TE Topology |       | Technology-specific |
          |  (profile)  |       |  Network Topology   |
          +-------------+       +---------------------+
~~~~
{: #multi-inheritance-fig title="Augmenting the Network Topology Model with multi-inheritance"}

   This approach is more suitable in cases where the technology-specific
   Network Topology Model provides augmentation only to the constructs
   defined in the Network Topology Model, such as nodes, links, and
   termination points (TPs). Therefore, with this approach, only the
   generic attributes defined in the TE Topology Model could be used.

   It is also worth noting that in this case, technology-specific
   augmentations for the bandwidth information could not be defined.

   In principle, it would be also possible to define both a technology
   specific TE Topology Model which augments the TE Topology Model, and
   a technology-specific Network Topology Model which augments the
   Network Topology Model and to rely on the multiple inheritance
   capability, as shown in {{double-augment-fig}}:

~~~~
                    +-----------------------+
                    |    Network Topology   |
                    +-----------------------+
                        ^               ^
                        |               |
           Augments +---+               +--+ Augments
                    |                      |
          +---------+---+       +----------+----------+
          | TE Topology |       | Technology-specific |
          |  (profile)  |       |  Network Topology   |
          +-------------+       +---------------------+
                 ^                         ^
                 |                         |
                 | Augments                | References
                 |                         |
      +----------+----------+              |
      | Technology-Specific +--------------+
      |     TE Topology     |
      +---------------------+
~~~~
{: #double-augment-fig title="Augmenting both the Network and TE Topology Models"}

   This option does not provide any technical advantage with respect to
   the first option, shown in {{te-augment-fig}}, but could be useful to add
   augmentations to the TE Topology constructs and to re-use an already
   existing technology-specific Network Topology Model.

   It is worth noting that the technology-specific TE Topology model can
   reference constructs defined by the technology-specific Network
   Topology model but it could not augment constructs defined by the
   technology-specific Network Topology model.

{: #multi-inheritance}

## Multi-inheritance

As described in section 4.1 of {{RFC8345}}, the network types should be defined
using presence containers to allow the representation of network subtypes.

The hierachy of netwok subtypes can be single hierarchy, as shown in {{te-augment-fig}}.
In this case, each presence container contains at most one child presence container,
as shows in the JSON code below:

~~~~
{
  "ietf-network:ietf-network": {
    "ietf-te-topology:te-topology": {
      "example-te-topology": {}
    }
  }
}
~~~~

The hierachy of netwok subtypes can also be multi-hierarchy, as shown in {{multi-inheritance-fig}} and {{double-augment-fig}}.
In this case, one presence container can contain more than one child presence containers, as show in the JSON codes below:

~~~~
{
  "ietf-network:ietf-network": {
    "ietf-te-topology:te-topology": {}
    "example-network-topology": {}
  }
}
~~~~

~~~~
{
  "ietf-network:ietf-network": {
    "ietf-te-topology:te-topology": {
      "example-te-topology": {}
    }
    "example-network-topology": {}
  }
}
~~~~

Other examples of multi-hierarchy topologies are described in
{{?I-D.ietf-teas-yang-sr-te-topo}}.

{: #example-link}

## Example (Link augmentation)

   This section provides an example on how technology-specific
   attributes can be added to the Link construct:

~~~~
      +--rw link* [link-id]
         +--rw link-id            link-id
         +--rw source
         |  +--rw source-node?   -> ../../../nw:node/node-id
         |  +--rw source-tp?     leafref
         +--rw destination
         |  +--rw dest-node?   -> ../../../nw:node/node-id
         |  +--rw dest-tp?     leafref
         +--rw supporting-link* [network-ref link-ref]
         |  +--rw network-ref
         |  |       -> ../../../nw:supporting-network/network-ref
         |  +--rw link-ref       leafref
         +--rw example-link-attributes
         |   <...>
         +--rw te!
            +--rw te-link-attributes
               +--rw name?                             string
               +--rw example-te-link-attributes
               |   <...>
               +--rw max-link-bandwidth
                  +--rw te-bandwidth
                     +--rw (technology)?
                        +--:(generic)
                        |  +--rw generic?   te-bandwidth
                        +--:(example)
                           +--rw example?   example-bandwidth
~~~~
{: #example-link-tree title="Augmenting the Link with technology-specific attributes"}

   The technology-specific attributes within the example-link-attributes
   container can be defined either in the technology-specific TE
   Topology Model (Option 1) or in the technology-specific Network
   Topology Model (Option 2 or Option 3). These attributes can only be
   non-TE and do not require the implementation of the te container.

   The technology-specific attributes within the
   example-te-link-attributes container as well as the example
   max-link-bandwidth can only be defined in the technology-specific TE
   Topology Model (Option 1 or Option 3). These attributes can be TE or
   non-TE and require the implementation of the te container.

{: #implement}

# Implemented profiles

When a server implements a profile of the TE topology model, it is not clear how the server
can report to the client the subset of the model being implemented.

It is also worth noting that the supported profile may also depend on other attributes
(for example the network type).

In case the TE topology profile is reported by the server to the client, the server will report
in the operational datastore only the leaves which have been implemented, as described
in section 5.3 of {{!RFC8342}}.

More investigation is required in case the TE topology profile is configured by the client.

{: #security}

# Security Considerations

   This document provides only information about how the TE Topology
   Model, as defined in {{!RFC8795}}, can be profiled to address some
   scenarios which are not considered as TE.

   As such, this document does not introduce any additional security
   considerations besides those already defined in {{!RFC8795}}.

{: #iana}

# IANA Considerations

   This document requires no IANA actions.

{: numbered="false"}

# Acknowledgments

  The authors would like to thank Daniele Ceccarelli, Jonas Ahlberg and Scott Mansfield 
  for providing useful suggestions for this draft.

  This document was prepared using kramdown.

  Previous versions of this document was prepared using 2-Word-v2.0.template.dot.
