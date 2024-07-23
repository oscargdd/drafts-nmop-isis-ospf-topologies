---
title: A YANG Data Model for Border Gateway Protocol (BGP) Topology
abbrev: BGP Topology YANG
docname: draft-ogondio-nmop-bgp-topology-latest

stand_alone: true
ipr: trust200902
area: "Operations and Management"
wg: nmop
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

#contributor:
#  -


--- abstract

This document defines a YANG data model for representing an abstracted view of a network topology that contains Border Gateway Protocol (BGP) information. This document augments the 'ietf-network' data model by adding BGP concepts. The aim of the model is to, from a SDN controller perspective, obtain iBGP and MP-BGP topology information from the network and to export it towards NBI interface to the service orchestration layer.

The YANG data model defined in this document conforms to the Network Management Datastore Architecture (NMDA).


--- middle

# Introduction

Network operators perform the capacity planning for their networks and run regular what-if scenarios analysis based on representations of the real network. Those what-if analysis and capacity planning processes require, among other information, a topological view (domains, nodes, links, network interconnection) of the deployed network.

This document defines a YANG data model representing an abstracted view of a network topology containing Border Gateway Protocol (BGP) with the following assumption:
* Areas can be explicit, depending on which IGP protocol is used.
+ Metrics can be provided to the operations team for greater control of the network.
- A view of the topology of the network built on the basis of the neighbors can be presented.

This YANG data model can be used to export the BGP related topology directly from a network controller to Operation Support System (OSS) tools or to a higher level controller.

This document defines a YANG data model for representing, managing and controlling the BGP topology. The data model augments ietf-network module {{!RFC8345}} by adding the BGP information.

This document explains the scope and purpose of the BGP  topology model and how the topology and service models fit together.

The YANG data model defined in this document conforms to the Network Management Datastore Architecture {{!RFC8342}}.

## Terminology and Notations

This document assumes that the reader is familiar with BGP and the contents of {{!RFC8345}}. The document uses terms from those documents.

The terminology for describing YANG data models is found in {{!RFC7950}}, {{!RFC8795}} and {{!RFC8346}}.

The term Digital Twin, Digital Map, Digital Map Modelling, Digital Map Model, Digital Map Data, and Topology are specified in {{?I-D.draft-havel-nmop-digital-map}}.

## Requirements Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in  {{!RFC2119}}, {{!RFC8174}} when, and only when, they appear in all capitals, as shown here.

## Tree Diagram

Authors include a simplified graphical representation of the data model specified in  {{ietf-l3-bgp-topology-tree}} of this document.
The meaning of the symbols in these diagrams is defined in {{!RFC8340}}.

## Prefix in Data Node Names

  In this document, names of data nodes and other data model objects are prefixed using the standard prefix associated with the corresponding YANG imported modules, as shown in the following table.

| Prefix | Yang Module            | Reference    |
| ------ | ---------------------- | ------------ |
| bgpnt  | ietf-l3-bgp-topology   | RFCXXX       |
| yang   | ietf-yang-types        | {{!RFC6991}} |
{: #tab-prefixes title="Prefixes and corresponding YANG modules"}

RFC Editor Note:
Please replace XXXX with the RFC number assigned to this document.
Please remove this note.

# Use Cases

Use cases for this document are the same than explained in {{?I-D.draft-ogondio-nmop-isis-topology}}. Here are included for completeness and discussion. Future versions may consider removing them.

This information is required in the IP/MPLS planning process to properly assess the required network resources to meet the traffic demands in normal and failure scenarios. Network operators perform the capacity planning for their networks and run regular what-if scenarios analysis based on representations of the real network. Those what-if analysis and capacity planning processes require, among other information, a topological view (domains, nodes, links, network interconnection) of the deployed network.

The standardization of an abstracted view of the BGP topology model as NorthBound Interface (NBI) of Software Defined Networking (SDN) controllers allows the unified query of the BGP topology in order to inject this information into third party tools covering specialized cases.

The BGP topological model should export enough BGP information to permit these tools to simulate the IP routing. By mapping the traffic demand, ideally at the IP flow level, to the topology, we can simulate the traffic growth, evaluating this way its effect on the routing and quality of service. That is, simulating how IP-level traffic demands would be forwarded, after BGP convergence is reached, and from there estimating, using appropriate mathematical models, related KPIs like the occupation in the links or end-to-end latencies.

In summary, the network-wide view of the BGP topology enables multiple use cases:

* Network design: verifying that the actual deployed BGP network conforms to the planned design.
+ Capacity planning. Dimensioning or redesign of the IP infrastructure to satisfy target KPI metrics under existing or forecasted traffic demands.
+ What-if analysis. Estimation of the network KPIs in modified network situations. For instance, failure situations, traffic anomaly situations, addition or deletion of new adjacencies, IGP weight reconfigurations, etc.
- Failure analysis. Systematic and massive test of the network under multiple simulated failure situations, evaluating the network fault tolerance properties, and using mathematical models to derive statistical network availability metrics.

## Relationship with the BGP YANG Model

TBD

## Relationship with Digital Map

As described in {{?I-D.draft-havel-nmop-digital-map}}, the Digital Map provides the core multi-layer topology model and data for the digital twin and connects them to the other digital twin models and data.

The Digital Map Modelling defines the core topological entities, their role in the network, core properties, and relationships both inside each layer and between the layers.

The Digital Map Model is a basic topological model that is linked to other functional parts of the digital twin and connects them all: configuration, maintenance, assurance (KPIs, status, health, symptoms), Traffic Engineering (TE), different behaviors and actions,  simulation, emulation, mathematical abstractions, AI algorithms, etc.

As such the BGP topology of the Digital Map is just one of the layers of the Digital Map, for specific user (the network operator in charge of the BGP) for the specific use cases as described before.

# YANG Data Model for BGP Topology

The abstract (base) network data model is defined in the "ietf-network" module of {{!RFC8345}}. The BGP-topology builds on the network data model defined in the "ietf-network" module {{!RFC8345}}, augmenting the nodes with BGP information, which anchor the links and are contained in nodes.

There is a set of parameters and augmentations that are included at the node level. Each parameter and description are detailed following:

* Network-types /restconf/data/ietf-network:networks/network/network-types: Its presence identifies the BGP topology type. Thus, the network type MUST be bgp-topology.
+ Local-as /restconf/data/ietf-networks/network/node/l3t:l3-node-attributes/bgp:local-as: Identifies the Local-AS configured in the Network-Element.
+ Neighbors /restconf/data/ietf-network:networks/network/node/l3t:l3-node-attributes/bgp:neighbours: list of Neighbors of the Node. Each Neighbor has the same set of parameters to describe the BGP session.
+ Neighbour neighbour is identified by the IP address. Description for troubleshooting purposes.
+ Peer-As attributes/bgp:neighbours/bgp:peer-as: Autonomous System of the peer. In case of iBGP sessions the Local -As and Peer-As is the same.
- Address-Family: Address Families shared by the nodes in the session. This is a leaf-list, because more than one AFI + SAFI address family can be shared. The options are alligned to the ones available in the IETF-BGP-TYPES (ipv4-unicast, ipv6-unicast, ipv4-labeled-unicast, ...)

The following figure is based on the Figure 1 from {{!RFC8346}}, where the example-bgp-topology is replaced with ietf-l3-bgp-topology and where
arrows show how the modules augment each other.

{: #ietf-l3-bgp-topology-module-structure}
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
                        | ietf-l3-bgp-topology  |
                        +-----------------------+
~~~~
{: #fig-ietf-l3-bgp-topology-module-structure title="BGP Topology module structure"}


{: #ietf-l3-bgp-topology-tree}

# BGP Topology Tree Diagram

{{fig-ietf-l3-bgp-topology-tree}} below shows the tree diagram of the YANG data model defined in module ietf-l3-bgp-topology.yang ({{fig-ietf-l3-bgp-topolopy-yang}}).

~~~~
{::include Yang/Tree/ietf-l3-bgp-topology.tree}
~~~~
{: #fig-ietf-l3-bgp-topology-tree title="BGP Topology tree diagram"}

# YANG Model for BGP topology

This module imports types from {{!RFC8343}} and {{!RFC8345}}.

~~~~
<CODE BEGINS> file "ietf-l3-bgp-topology@2024-07-23.yang"
{::include Yang/ietf-l3-bgp-topology.yang}<CODE ENDS>
~~~~
{: #fig-ietf-l3-bgp-topolopy-yang title="BGP Topology YANG module"}

# Security Considerations


  The YANG module specified in this document defines a schema for data that is designed to be accessed via network management protocols such as NETCONF {!RFC6241}} or RESTCONF {{!RFC8040}}. The lowest NETCONF layer is the secure transport layer, and the mandatory-to-implement secure transport is Secure Shell (SSH) {{!RFC6242}}. The lowest RESTCONF layer is HTTPS, and the mandatory-to-implement secure transport is TLS {{!RFC8446}}.

  The Network Configuration Access Control Model (NACM) {{!RFC8341}} provides the means to restrict access for particular NETCONF or RESTCONF users to a preconfigured subset of all available NETCONF or RESTCONF protocol operations and content.

 There are a number of data nodes defined in this YANG module that are writable/creatable/deletable (i.e., config true, which is the default). These data nodes may be considered sensitive or vulnerable in some network environments. Write operations (e.g., edit-config) to these data nodes without proper protection can have a negative effect on network operations.

# IANA Considerations

  This document registers the following namespace URIs in the IETF XML registry {{!RFC3688}}:

    --------------------------------------------------------------------
    URI: urn:ietf:params:xml:ns:yang:ietf-l3-bgp-topology
    Registrant Contact: The IESG.
    XML: N/A, the requested URI is an XML namespace.
    --------------------------------------------------------------------

   This document registers the following YANG module in the YANG Module Names registry {{!RFC6020}}:

    --------------------------------------------------------------------
    name:         ietf-l3-bgp-topology
    namespace:    urn:ietf:params:xml:ns:yang:ietf-l3-bgp-topology
    maintained by IANA: N
    prefix:       ietf-l3-bgp-topology
    reference:    RFC XXXX
    --------------------------------------------------------------------

# Implementation Status

This section will be used to track the status of the implementations of the model. It is aimed at being removed if the document becomes RFC.

--- back

{: numbered="false"}

# Acknowledgments
{:numbered="false"}

The authors of this document would like to thank XXX.
