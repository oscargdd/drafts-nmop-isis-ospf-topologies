---
title: A YANG Data Model for Intermediate System to intermediate System (IS-IS) Topology
abbrev: IS-IS Topology YANG
docname: draft-ogondio-opsawg-isis-topology-latest

stand_alone: true
ipr: trust200902
area: "Operations and Management"
wg: opsawg
category: std
submissiontype: IETF  # also: "independent", "IAB", or "IRTF"

consensus: true
v: 3

author:
  -
    fullname: Oscar González de Dios
    org: Telefonica
    email: oscar.gonzalezdedios@telefonica.com
  -
    fullname: Samier Barguil Giraldo
    org: Nokia
    email: samier.barguil_giraldo@nokia.com

  -
    fullname: Victor Lopez
    org: Nokia
    email: victor.lopez@nokia.com
  -
    fullname: Daniele Ceccarelli
    org: Cisco
    email: dceccare@cisco.com
  -
    fullname: Benoit Claise
    org: Huawei
    email: benoit.claise@huawei.com

contributor:
  -
    fullname: Olga Havel
    org: Huawei
    email: olga.havel@huawei.com
  -
    fullname: Pablo Pavon
    org: Universidad Politecnica de Cartegena
    email: pablo.pavon@upct.es

--- abstract

This document defines a YANG data model for representing an abstracted view of a network topology that contains Intermediate System to Intermediate System (IS-IS). This document augments the 'ietf-network' data model by adding IS-IS concepts and explains how the data model can be used to represent the IS-IS topology.

The YANG data model defined in this document conforms to the Network Management Datastore Architecture (NMDA).


--- middle

# Introduction

Network operators perform the capacity planning for their networks and run regular what-if scenarios analysis based on representations of the real network. Those what-if analysis and capacity planning processes require, among other information, a topological view (domains, nodes, links, network interconnection) of the deployed network.

This document defines a YANG data model representing an abstracted view of a network topology containing Intermediate System to Intermediate System (IS-IS). It covers the topology of IP/MPLS networks running IS-IS as Interior Gateway Protocol (IGP) protocol. The proposed YANG mode augments the "A YANG Data Model for Network Topologies" {{!RFC8345}} and"A YANG Data Model for Layer 3 Topologies" {{!RFC8346}} by adding IS-IS concepts. This YANG data model is used to export the IS-IS related topology directly from a network controller to an Operation Support System (OSS) tools.

Note that the YANG model is in this document strictly adhere to the concepts (and the YANG module) in "A YANG Data Model for Network Topologies" {{!RFC8345}} and"A YANG Data Model for Layer 3 Topologies" {{!RFC8346}}. While investigating the IS-IS topology, some limitations have discovered in {{!RFC8345}}, regarding how the digital map can be represented. Those limitations (and potential improvements) are covered in {{?I-D.draft-havel-opsawg-digital-map}}.

This document explains the scope and purpose of the IS-IS topology model and how the topology and service models fit together.
The YANG data model defined in this document conforms to the Network Management Datastore Architecture {{!RFC8342}}.

## Terminology and Notations

This document assumes that the reader is familiar with IS-IS and the contents of {{!RFC8345}}. The document uses terms from those documents.

The terminology for describing YANG data models is found in {{!RFC7950}}, {{!RFC8795}} and {{!RFC8346}}.

The term Digital Twin, Digital Map, Digital Map Modelling, Digital Map Model, Digital Map Data, and Topology are specified in {{?I-D.draft-havel-opsawg-digital-map}}.

## Requirements Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in  {{!RFC2119}}, {{!RFC8174}} when, and only when, they appear in all capitals, as shown here.

## Tree Diagram

Authors include a simplified graphical representation of the data model is used in {{ietf-l3-isis-topology-tree}} of this document.
The meaning of the symbols in these diagrams is defined in {{!RFC8340}}.

## Prefix in Data Node Names

  In this document, names of data nodes and other data model objects are prefixed using the standard prefix associated with the corresponding YANG imported modules, as shown in the following table.

| Prefix | Yang Module            | Reference    |
| ------ | ---------------------- | ------------ |
| isisnt | ietf-l3-isis-topology  | RFCXXX       |
| yang   | ietf-yang-types        | {{!RFC6991}} |
{: #tab-prefixes title="Prefixes and corresponding YANG modules"}

RFC Editor Note:
Please replace XXXX with the RFC number assigned to this document.
Please remove this note.

# Use Cases

This information is required in the IP/MPLS planning process to properly assess the required network resources to meet the traffic demands in normal and failure scenarios. Network operators perform the capacity planning for their networks and run regular what-if scenarios analysis based on representations of the real network. Those what-if analysis and capacity planning processes require, among other information, a topological view (domains, nodes, links, network interconnection) of the deployed network.

The standardization of an abstracted view of the IS-IS topology model as NorthBound Interface (NBI) of Software Defined Networking (SDN) controllers allows the inject this information into third party tools covering specialized cases.

The IS-IS topological model should export enough IS-IS information to permit these tools simulating the IP routing. By adding the traffic demand, ideally at the IP flow level, we can simulate the traffic growth and its effect on the routing. That is, simulating how IP-level traffic demands would be forwarded, after IS-IS convergence is reached, and from there estimating, using appropriate mathematical models, related KPIs like the occupation in the links or end-to-end latencies.

In summary, the network-wide view of the IS-IS topology enables multiple use cases:

* Network design: verifying that the actual deployed IS-IS network conforms to the planned design
+ Failure analysis. Systematic and massive test of the network under multiple simulated failure situations, evaluating the network fault tolerance properties, and using mathematical models to derive statistical network availability metrics.
+ What-if analysis. Estimation of the network KPIs in modified network situations. For instance, failure situations, traffic anomaly situations, addition or deletion of new adjacencies, IGP weight reconfigurations.
- Capacity planning. Dimensioning or redesign of the IP infrastructure to satisfy target KPI metrics under existing or forecasted traffic demands.

## Relationship with the IS-IS YANG Model


{{!RFC9130}} specifies a YANG data model that can be used to configure and manage the IS-IS protocol on network elements. This data model covers the configuration of an IS-IS routing protocol instance, as well as the retrieval of IS-IS operational states.
{{!RFC9130}} is still expected to be used for individual network elements configuration and monitoring. On the other hand, the proposed YANG model in this document covers the abstracted view of the entire network topology containing IS-IS. As such, this model is available via the NBI of SDN controllers.

## Relationship with Digital Map

As described in {{?I-D.draft-havel-opsawg-digital-map}}, the Digital Map provides the core multi-layer topology model and data for the digital twin and connects them to the other digital twin models and data.

The Digital Map Modelling defines the core topological entities, their role in the network, core properties, and relationships both inside each layer and between the layers.

The Digital Map Model is a basic topological model that is linked to other functional parts of the digital twin and connects them all: configuration, maintenance, assurance (KPIs, status, health, symptoms), Traffic Engineering (TE), different behaviors and actions,  simulation, emulation, mathematical abstractions, AI algorithms, etc.

As such the IGP topology of the Digital Map (in this case, IS-IS) is just one of the layers of the Digital Map, for specific user (the network operator in charge of the IGP) for specific IGP use cases as described before.

# Use of IETF-Topology for Representing an IP/MPLS network domain

IP/MPLS Networks can contain multiple domain IGP domains. We can define an IGP domain as the collection of nodes and links that participate in the same IGP process. The topology information of a domain can be structured according to ietf-network-topology data model {{!RFC8345}}. For example, if BGP-LS is used to collect the information, the nodes and links that are announced with the same combination of AS number / domain ID are considered to belong to the same domain.

If a node and/or layer termination point  participates in more than one IGP it will be present in multiple IGP domain networks.

The ietf-network instance MUST include the following properties to indicate it is a domain running an IGP instance:

A network-id that uniquely identifies such domain in the network.
The "network-types" property should include the l3t:l3-unicast-topology, to indicate it is a network in which the nodes are capable of forwarding unicast packet. Also, this draft proposed to add a new property, "isis-topology", to indicate the topology being represented is running the IS-IS IGP process.

Also, should the topology include information such as bandwidth, delay information or color, it must include tet:te-topology.
To include delay and bandwdith performance measurements , MUST include tet-pkt:te-packet under the previous property
The supporting-network property can include the network-id of a base layer-3 network.
The node property should include the list of nodes as described below.
The ietf-network-topology:link MUST be present, with one link per each IP adjacency (one link for each direction of the adjancency).

# YANG Data Model for IS-IS Topology

The abstract (base) network data model is defined in the "ietf-network" and "ietf-network-topology" modules of {{!RFC8345}}.
The L3 topology module is defined in the "ietf-l3-unicast-topology" module of {{!RFC8346}}.
The ietf-l3-isis-topology builds on the data models defined in {{!RFC8345}} and  {{!RFC8346}}, augmenting the nodes with IS-IS information.

There is a set of parameters and augmentations that are included at the node level. Each parameter and description are detailed following:

* Network-types: Its presence identifies the IS-IS topology type. Thus, the network type MUST be isis-topology.
+ IS-IS timer attributes: Identifies the node timer attributes configured in the network element. They are LSP lifetime and the LSP refresh interval.
- IS-IS status: contains the IS-IS status attributes (level, area-address and neighbours).

The following figure is based on the Figure 1 from {{!RFC8346}}, where the example-ospf-topology is relaced with ietf-l3-isis-topology and where
arrows show how the modules augment each other.

{: #ietf-l3-isis-topology-module-structure}
~~~~
                      +-----------------------------+
                      |  +-----------------------+  |
                      |  |      ietf-network     |  |
                      |  +----------^------------+  |
                      |             |               |
                      |  +-----------------------+  |
                      |  | ietf-network-topology |  |
                      |  +----------+------------+  |
                      +-------------^---------------+
                                    |
                                    |
                       +------------^-------------+
                       | ietf-l3-unicast-topology |
                       +------------^-------------+
                                    |
                                    |
                        +-----------^-----------+
                        | ietf-l3-isis-topology |
                        +-----------------------+
~~~~
{: #fig-ietf-l3-isis-topology-module-structure title="IS-IS Topology module structure"}

Theere are some limitations in the {{!RFC8345}} that are explained in more detail in {{!I-D.draft-havel-opsawg-digital-map}}.
The current version of the ietf-l3-isis-topology module is based on the current version of {{!RFC8345}}.
The following will be addressed when {{!RFC8345}} is extended to support the identified limitations:
- Both IS-IS domain and IS-IS areas could be modelled as networks
- The IS-IS Areas will be connected via IS-IS links
- IS-IS nodes could belong to multiple IS-IS networks

There is a set of parameters and augmentations that are included at the network level.
- Network-types: Its presence identifies the IS-IS topology type. Thus, the network type MUST be isis-topology.

There is a set of parameters and augmentations that are included at the node level. Each parameter and description are detailed following:
- IS-IS node core attributes: contains the IS-IS core attributes (system-id, level, area-address).
- IS-IS timer attributes: Identifies the node timer attributes configured in the network element. They are LSP lifetime and the LSP refresh interval.

There is a set of parameters and augmentations that are included at the link level. Each parameter and description are detailed following:
- IS-IS link level. The level must be the same as the termination points at each end for Level 1 and Level 2 interfaces. There may be 2 links
between the Level1-2 IS-IS interfaces, one for Level 1 adjacency and one for Level 2 adjacency.
- IS-IS link metric. Added on top of metric1 and metric2 of the l3-link-attributes

There is a  set of parameters and augmentations are included at the termination point level. Each parameter is listed as follows:
- Interface-type: point-to point or braodcast
- Level. The level must be the same as for the node, except when node is Level 1-2 and the interfaces can only be Level 1 or Level 2.
- Passive mode

{: #ietf-l3-isis-topology-tree}


# IS-IS Topology Tree Diagram

{{fig-ietf-l3-isis-topology-tree}} below shows the tree diagram of the YANG data model defined in module ietf-l3-isis-topology.yang ({{fig-ietf-isis-topolopy-yang}}).

~~~~
module: ietf-l3-isis-topology

  augment /nw:networks/nw:network/nw:network-types:
    +--rw isis-topology!
  augment /nw:networks/nw:network/nw:node/l3t:l3-node-attributes:
    +--rw isis-node-attributes
       +--rw system-id?              ietf-isis:system-id
       +--rw level?                  ietf-isis:level
       +--rw area-address*           ietf-isis:area-address
       +--rw lsp-lifetime?           uint16
       +--rw lsp-refresh-interval?   uint16
    +--rw isis-timer-attributes
    |  +--rw lsp-lifetime?           uint16
    |  +--rw lsp-refresh-interval?   uint16
    +--rw isis-status
       +--rw level?          ietf-isis:level
       +--rw area-address*   ietf-isis:area-address
       +--rw system-id?      ietf-isis:system-id
       +--ro neighbors*      inet:ip-address
  augment /nw:networks/nw:network/nt:link/l3t:l3-link-attributes:
    +--rw isis-termination-point-attributes
       +--rw interface-type?   ietf-isis:interface-type
       +--rw level?            ietf-isis:level
       +--rw metric?           uint32
       +--rw is-passive?       boolean
  augment /nw:networks/nw:network/nw:node/nt:termination-point/l3t:l3-termination-point-attributes:
    +--rw isis-termination-point-attributes
       +--rw interface-type?   ietf-isis:interface-type
       +--rw level?            ietf-isis:level
       +--rw metric?           uint32
       +--rw is-passive?       boolean
~~~~
{: #fig-ietf-l3-isis-topology-tree title="IS-IS Topology tree diagram"}

# YANG Model for IS-IS topology

This module imports types from {{!RFC8343}} and {{!RFC8345}}. Following the YANG model is presented.

~~~~
<CODE BEGINS> file "ietf-l3-isis-topology@2022-10-24.yang"
module ietf-l3-isis-topology {
  yang-version 1.1;
  namespace
    "urn:ietf:params:xml:ns:yang:ietf-l3-isis-topology";
  prefix "isisnt";

  import ietf-network {
    prefix "nw";
    reference
      "RFC 8345: A YANG Data Model for Network Topologies";
  }

  import ietf-network-topology {
    prefix "nt";
    reference
      "RFC 8345: A YANG Data Model for Network Topologies";
  }

  import ietf-l3-unicast-topology {
    prefix "l3t";
    reference
      "RFC 8346: A YANG Data Model for Layer 3 Topologies";
  }

  import ietf-isis {
    prefix "ietf-isis";
    reference
      "RFC 9130: YANG Data Model for the IS-IS Protocol";
  }

  import ietf-inet-types {
    prefix "inet";
    reference
      "RFC 6991: Common YANG Data Types";
  }

  organization
    "IETF OPSA (Operations and Management Area) Working Group";
  contact
    "WG Web:  <https://datatracker.ietf.org/wg/opsawg/>
    WG List:  <mailto:opsawg@ietf.org>

    Editor:   Oscar Gonzalez de Dios
              <mailto:oscar.gonzalezdedios@telefonica.com>
    Editor:   Samier Barguil
              <mailto:samier.barguilgiraldo.ext@telefonica.com>
    Editor:   Victor Lopez
              <mailto:victor.lopez@nokia.com>
    Editor:   Benoit Claise
              <mailto:benoit.claise@huwaei.com>";
  description
    "This module defines a model for Layer 3 IS-IS
     topologies.

     Copyright (c) 2022 IETF Trust and the persons identified as
     authors of the code.  All rights reserved.

     Redistribution and use in source and binary forms, with or
     without modification, is permitted pursuant to, and subject to
     the license terms contained in, the Revised BSD License set
     forth in Section 4.c of the IETF Trust's Legal Provisions
     Relating to IETF Documents
     (https://trustee.ietf.org/license-info).

     This version of this YANG module is part of RFC XXXX
     (https://www.rfc-editor.org/info/rfcXXXX); see the RFC itself
     for full legal notices.";

  revision 2022-09-21 {
    description
      "Initial version";
    reference
      "RFC XXXX: A YANG Data Model for Intermediate System to
       Intermediate System (IS-IS) Topology";
  }

  grouping isis-topology-type {
    description "Identifies the topology type to be IS-IS.";
    container isis-topology {
      presence "indicates IS-IS topology";
      description
        "The presence of the container node indicates IS-IS
        topology";
    }
  }

  grouping isis-link-attributes {
     description "Identifies the IS-IS link attributes.";
     container isis-link-attributes {
     leaf metric {
      type uint32 {
         range "0 .. 16777215";
       }
      description
        "This type defines wide style format of IS-IS metric.";
     }
     leaf level {
      type ietf-isis:level;
      description
        "Level of an IS-IS node - can be level-1,
        level-2 or level-all.";
    }
    }
  }

  grouping isis-node-attributes {
    description "isis node scope attributes";
    container isis-node-attributes {
    leaf system-id {
        type ietf-isis:system-id;
        description
          "System-id of the node.";
      }
    leaf level {
        type ietf-isis:level;
        description
          "Level of an IS-IS node - can be level-1,
          level-2 or level-all.";
      }
    leaf-list area-address {
        type ietf-isis:area-address;
        description
          "List of areas supported by the protocol instance.";
      }
    leaf lsp-lifetime {
        type uint16 {
           range "1..65535";
         }
        units "seconds";
        description
          "Lifetime of the router's LSPs in seconds.";
      }
    leaf lsp-refresh-interval {
        type uint16 {
           range "1..65535";
         }
        units "seconds";
        description
          "Refresh interval of the router's LSPs in seconds.";
      }
    }
  }

  grouping isis-termination-point-attributes {
    description "IS-IS termination point scope attributes";
    container isis-termination-point-attributes {
       description
      "Indicates the termination point from the
      which the IS-IS is configured. A termination
      point can be a physical port, an interface, etc.";

    leaf interface-type {
      type ietf-isis:interface-type;
      description
        "Type of adjacency (broadcast or point-to-point) to be established
        for the interface.
        This dictates the type of hello messages that are used.";
    }

    leaf level {
      type ietf-isis:level;
      description
        "Level of an IS-IS node - can be level-1,
        level-2 or level-all.";
    }

    leaf is-passive{
      type boolean;
      description
        "Indicates whether the interface is in passive mode (IS-IS
        not running but network is advertised).";
      }
    }
  }

  augment "/nw:networks/nw:network/nw:network-types" {
    description
      "Introduces new network type for L3 Unicast topology";
    uses isis-topology-type;
  }

  augment "/nw:networks/nw:network/nw:node/l3t:l3-node-attributes" {
    when "/nw:networks/nw:network/nw:network-types/isisnt:isis-topology" {
      description
        "Augmentation parameters apply only for networks with
        isis topology";
    }
    description
      "isis node-level attributes ";
    uses isis-node-attributes;
  }

  augment "/nw:networks/nw:network/nt:link/l3t:l3-link-attributes" {
    when "/nw:networks/nw:network/nw:network-types/isisnt:isis-topology" {
      description
        "Augmentation parameters apply only for networks with
        IS-IS topology";
    }
    description
      "Augments topology link configuration";
    uses isis-link-attributes;
  }



  augment "/nw:networks/nw:network/nw:node/nt:termination-point"+
  "/l3t:l3-termination-point-attributes" {
    when "/nw:networks/nw:network/nw:network-types/isisnt:isis-topology" {
      description
        "Augmentation parameters apply only for networks with
        IS-IS topology";
    }
    description
      "Augments topology termination point configuration";
    uses isis-termination-point-attributes;
  }
}
<CODE ENDS>
~~~~
{: #fig-ietf-isis-topolopy-yang title="IS-IS Topology YANG module"}

# Security Considerations


  The YANG module specified in this document defines a schema for data that is designed to be accessed via network management protocols such as NETCONF {!RFC6241}} or RESTCONF {{!RFC8040}}. The lowest NETCONF layer is the secure transport layer, and the mandatory-to-implement secure transport is Secure Shell (SSH) {{!RFC6242}}. The lowest RESTCONF layer is HTTPS, and the mandatory-to-implement secure transport is TLS {{!RFC8446}}.

  The Network Configuration Access Control Model (NACM) {{!RFC8341}} provides the means to restrict access for particular NETCONF or RESTCONF users to a preconfigured subset of all available NETCONF or RESTCONF protocol operations and content.

 There are a number of data nodes defined in this YANG module that are writable/creatable/deletable (i.e., config true, which is the default). These data nodes may be considered sensitive or vulnerable in some network environments. Write operations (e.g., edit-config) to these data nodes without proper protection can have a negative effect on network operations.

# IANA Considerations

  This document registers the following namespace URIs in the IETF XML registry {{!RFC3688}}:

    --------------------------------------------------------------------
    URI: urn:ietf:params:xml:ns:yang:ietf-l3-isis-topology
    Registrant Contact: The IESG.
    XML: N/A, the requested URI is an XML namespace.
    --------------------------------------------------------------------

   This document registers the following YANG module in the YANG Module Names registry {{!RFC6020}}:

    --------------------------------------------------------------------
    name:         ietf-l3-isis-topology
    namespace:    urn:ietf:params:xml:ns:yang:ietf-l3-isis-topology
    maintained by IANA: N
    prefix:       ietf-l3-isis-topology
    reference:    RFC XXXX
    --------------------------------------------------------------------



--- back

{: numbered="true"}

# Implementation Status

   Note to the RFC-Editor: Please remove this section before publishing.

## Implementation Status in Telefonica Group
   The Yang based topology model proposed in this draft is being used today in one of the Telefonica
   operations to export the Multi-vendor IP/MPLS topology based on multiple IS-IS domains to several
   Operation Support System tools for visualization, capacity planning and simulation. A commercial
   controller has implemented the exposure of the information. It is one of the building blocks to
   expose the network capabilities, together with other models which cover the inventory and service
   provisioning in a vendor-agnostic fashion.

## Huawei Digital Map PoC Status

   As mentioned in {{?I-D.draft-havel-opsawg-digital-map}}, a Digital Map PoC with a real lab has been built, based on multi-
   vendor devices, with {{!RFC8345}} as the base YANG module for the topology building blocks. This PoC successfully modelled
   IS-IS routing (among other technologies and layers), but it needs to be further aligned with this latest developments in this draft.

## Implementation Status in E-lighthouse Network Solutions

E-lighthouse Network Solutions (https://e-lighthouse.com/) implementation is consuming the IS-IS network topology information
exported by a commercial controller, using the Yang model proposed in this draft. It is able to simulate the network behavior
under different changes, covering the what-if, failure analysis, dimensioning and other use cases mentioned in this draft.

# Acknowledgments
{:numbered="false"}
The authors would like to thank Pierre Francois for the review and suggestions the document.

This work is partially supported by the European Commission under grant agreement No. 101092766  (ALLEGRO Project) and Horizon 2020 Secured autonomic traffic management for a Tera of SDN flows (Teraflow) project (grant agreement number 101015857).
