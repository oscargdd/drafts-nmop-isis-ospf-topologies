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
  
#contributor:
#  
#  -
#    fullname: Olga Havel
#    org: Huawei
#    email: olga.havel@huawei.com
#  -
#    fullname: Pablo Pavon
#    org: "e-lighthouse"
#    email: "ppavon@e-lighthouse.com"



--- abstract

This document defines a YANG data model for representing an abstracted view of a network topology that contains Intermediate System to Intermediate System (IS-IS). This document augments the 'ietf-network' data model by adding IS-IS concepts and explains how the data model can be used to represent the IS-IS topology.

The YANG data model defined in this document conforms to the Network Management Datastore Architecture (NMDA).


--- middle

# Introduction

Network operators perform the capacity planning for their networks and run regular what-if scenarios analisys based on representations of the real network. Those what-if analysis and capacity planning processes require, among other information, a topological view (domains, nodes, links, network interconnection) of the deployed network. Such topological view requires on the one hand enough detail to carry out the required analysis but on the other hand abstracted enough to make those analysis viable.

This draft address the use case of modeling the topology of IP/MPLS networks that run IS-IS as IGP protocol. The draft builds on the ietf-network model in {{!RFC8345}}, enhanced in {{!RFC8346}} and {{!RFC8944}} which extend the generic network and network topology data models with topology attributes specific to Layer 3 and Layer 2. However, there is not any model that exposes Intermediate System to Intermediate System (IS-IS) specific network information. This information is required in the IP/MPLS planning process to properly assess the required network resources to meet the traffic demands in normal and failure scenarios.

This document defines a YANG data model for representing the IS-IS topology. The data model augments ietf-network module {{!RFC8345}} by adding the IS-IS information. The proposed YANG data model is used to export the topology of an IP/MPLS network from a network controller to an Operation Support System (OSS) tools with the aim of performing network planning and visualization.

This document explains the scope and purpose of the IS-IS topology model and how the topology and service models fit together.

The YANG data model defined in this document conforms to the Network Management Datastore Architecture {{!RFC8342}}.

## Terminology and Notations

This document assumes that the reader is familiar with IS-IS and the contents of {{!RFC8345}}. The document uses terms from those documents.

The terminology for describing YANG data models is found in {{!RFC7950}}, {{!RFC8795}} and {{!RFC8346}}.

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

# Use of IETF-Topology for Representing an IP/MPLS network domain

IP/MPLS Networks can contain multiple domain IGP domains. We can define an IGP domain as the collection of nodes and links that participate in the same IGP process. The topology information of a domain can be structured according to ietf-topology information model
For example, if BGP-LS is used to collect the information, the nodes and links that are announced with the same combination of AS number / are considered to belong to the same domain. 

If a node and/or layer termination point  participates in more than one IGP it will be present in multiple IGP domain networks.

The ietf-network instance MUST include the following properties to indicate it is a domain running an IGP instance:

A network-id that uniquely identifies such domain in the network.
The "network-types property should include the l3t:l3-unicast-topology, to indicate it is a network in which the nodes are capable of forwarding unicast packet. Also, this draft proposed to ade a new property, isis-topology, to indicate the topology being represented is running an IGP process. 

Also, should the topology include information such as bandwidth, delay information or color, it must include tet:te-topology.
To include delay and bandwdith performance measurements , MUST include tet-pkt:te-packet under the previous property
The supporting-network property can include the network-id of a base layer-3 network.
The node property should include the list of nodes as described below.
The ietf-network-topology:link MUST be present, with one link per each IP adjacency (one link for each direction of the adjancency).

# YANG Data Model for IS-IS Topology

The abstract (base) network data model is defined in the "ietf-network" module of {{!RFC8345}}. The ISIS-topology builds on the network data model defined in the "ietf-network" module {{!RFC8345}}, augmenting the nodes with IS-IS information, which anchor the links and are contained in nodes).

There is a set of parameters and augmentations that are included at the node level. Each parameter and description are detailed following:

* Network-types: Its presence identifies the IS-IS topology type. Thus, the network type MUST be isis-topology.
+ IS-IS timer attributes: Identifies the node timer attributes configured in the network element. They are LSP lifetime and the LSP refresh interval.
- IS-IS status: contains the IS-IS status attributes (level, area-address and neighbours).


There is a second set of parameters and augmentations are included at the termination point level. Each parameter is listed as follows:

* Interface-type
+ Level
+ Metric
- Passive mode

{: #ietf-l3-isis-topology-tree}

# IS-IS Topology Tree Diagram

{{fig-ietf-l3-isis-topology-tree}} below shows the tree diagram of the YANG data model defined in module ietf-l3-isis-topology.yang ({{ietf-l3-isis-topology-yang}}).

~~~~
module: ietf-l3-isis-topology
  augment /nw:networks/nw:network/nw:network-types:
    +--rw isis-topology!
  augment /nw:networks/nw:network/nw:node/l3t:l3-node-attributes:
    +--rw isis-timer-attributes
    |  +--rw lsp-lifetime?           string
    |  +--rw lsp-refresh-interval?   string
    +--rw isis-status
       +--rw level?          ietf-isis:level
       +--rw area-address*   ietf-isis:area-address
       +--ro neighbours*     inet:ip-address
  augment .../nt:termination-point/l3t:l3-termination-point-attributes:
    +--rw isis-termination-point-attributes
       +--rw interface-type?   identityref
       +--rw level?            ietf-isis:level
       +--rw metric?           uint64
       +--rw is-passive?       boolean
~~~~
{: #fig-ietf-l3-isis-topology-tree title="IS-IS Topology tree diagram"}

{: #ietf-l3-isis-topology-yang}

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
      "RFC 6991: Common YANG Data Types";
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
              <mailto:victor.lopez@nokia.com>";
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

  grouping isis-node-attributes {
    description "isis node scope attributes";
    container isis-timer-attributes {
      description
        "Contains node timer attributes";
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
    container isis-status {
      description
        "Contains the IS-IS status attributes";
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

      leaf system-id {
        type ietf-isis:system-id;
        description
          "System-id of the node.";
      }

      leaf-list neighbors {
        type inet:ip-address;
        config false;
        description
          "Topology flags";
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
        "Type of adjacency to be established for the interface. This
        dictates the type of hello messages that are used.";
    }

    leaf level {
      type ietf-isis:level;
      description
        "Level of an IS-IS node - can be level-1,
        level-2 or level-all.";
    }

    leaf metric {
      type uint32 {
         range "0 .. 16777215";
       }
      description
        "This type defines wide style format of IS-IS metric.";
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
    uses isis-termination-point-attributes;
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

  The YANG module specified in this document defines a schema for data that is designed to be accessed via network management protocols such as NETCONF {{!RFC6241}} or RESTCONF {{!RFC8040}}.  The lowest NETCONF layer is the secure transport layer, and the mandatory-to-implement secure transport is Secure Shell (SSH) {{!RFC6242}}. The lowest RESTCONF layer is HTTPS, and the mandatory-to-implement secure transport is TLS {{!RFC5246}}.

   The NETCONF access control model {{!RFC6536}} provides the means to restrict access for particular NETCONF or RESTCONF users to a preconfigured subset of all available NETCONF or RESTCONF protocol operations and content.

   There are a number of data nodes defined in this YANG module that are writable/creatable/deletable (i.e., config true, which is the default).  These data nodes may be considered sensitive or vulnerable   in some network environments.  Write operations (e.g., edit-config) to these data nodes without proper protection can have a negative effect on network operations.

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

# Implementation Status

This section will be used to track the status of the implementations of the model. It is aimed at being removed if the document becomes RFC.

--- back

{: numbered="false"}

# Acknowledgments

This work is partially supported by the European Commission under   Horizon 2020 Secured autonomic traffic management for a Tera of SDN
 flows (Teraflow) project (grant agreement number 101015857).
