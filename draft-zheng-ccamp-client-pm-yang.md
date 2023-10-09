---
title: "A YANG Data Model for Client Signal Performance Monitoring"
abbrev: "PM YANG for Client Signal"
category: std

docname: draft-zheng-ccamp-client-pm-yang-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Routing"
workgroup: "CCAMP Working Group"
venue:
  group: "Common Control and Measurement Plane"
  type: "Working Group"
  mail: "ccamp@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/ccamp/"
  github: "haomianzheng/ccamp-client-pm-yang"
  latest: "https://haomianzheng.github.io/ccamp-client-pm-yang/draft-zheng-ccamp-client-pm-yang.html"

author:
  -
    name: Chaode Yu
    org: Huawei Technologies
    email: yuchaode@huawei.com
  -
    name: Haomian Zheng
    org: Huawei Technologies
    street: H1, Huawei Xiliu Beipo Village, Songshan Lake
    city: Dongguan
    region: Guangdong
    code: 523808
    country: China
    email: zhenghaomian@huawei.com
  -
    name: Italo Busi
    org: Huawei Technologies
    country: Italy
    email: italo.busi@huawei.com
  -
    name: Victor Lopez
    org: Nokia
    country: Spain
    email: victor.lopez@nokia.com
  -
    name: Oscar Gonzalez de Dios
    ins: O. Gonzalez de Dios
    org: Telefonica
    country: Spain
    email: oscar.gonzalezdedios@telefonica.com

contributor:

normative:

informative:


--- abstract

A transport network is a server-layer network to provide connectivity
services to its client.  Given the client signal is configured, the
followup function for performance monitoring, such as latency and bit
error rate, would be needed for network operation.

This document describes the data model to support the performance
monitoring functionalities.

--- middle

# Introduction

Client-layer network and server-layer network have been respectively
modeled to allow the tunnels carrying the client traffic.  Server-
layers are modeled as tunnels with various switching technologies,
such as OTN in {{?I-D.ietf-ccamp-otn-tunnel-model}} and WSON in
{{?I-D.ietf-ccamp-wson-tunnel-model}}.  Client-layers are modeled as
client signals according to the client-signal identities specified in
{{!I-D.ietf-ccamp-layer1-types}}.  These client signals can be
configured to existing tunnels via the client signal configuration
model specified in {{!I-D.ietf-ccamp-client-signal-yang}}.

In the network operation, the operator is interested in monitoring
their instantiated client signal over tunnels.  The objective of such
monitoring is to complete timely adjustment once there is abnormal
statistic which may result in failure of the client signal.  The
parameters specified in the performance monitoring model can be
collected for the operation need.  The OAM mechanism, can be
configured together with the performance monitoring model.

# Terminology and Notations

A simplified graphical representation of the data model is used in
this document.  The meaning of the symbols in the YANG data tree
presented later in this document is defined in {{?RFC8340}}.  They are
provided below for reference.

- Brackets "\[" and "]" enclose list keys.

- Abbreviations before data node names: "rw" means configuration
  (read-write) and "ro" state data (read-only).

- Symbols after data node names: "?" means an optional node, "!"
  means a presence container, and "*" denotes a list and leaf-list.

- Parentheses enclose choice and case nodes, and case nodes are also
  marked with a colon (":").

- Ellipsis ("...") stands for contents of subtrees that are not
  shown.

# Model Relationship

{{!I-D.ietf-ccamp-client-signal-yang}} has specified the two models for
the client signal configuration, module ietf-trans-client-service for
transparent client service and module ietf-eth-tran-service for
Ethernet service.  Basically the client signal types in this document
is consistent with ietf-eth-tran-types, and focus on different
functionality.  On the perspective of operator, the modules in
{{!I-D.ietf-ccamp-client-signal-yang}} can be used to configure the
service given any underlay tunnels, while the operation about
monitoring the performance on given service can be achieved by using
the model in this document.

Consideration on Key Performance Information (KPI) monitoring for
Virtual Network (VN) and tunnels has been specified in
{{!I-D.ietf-teas-actn-pm-telemetry-autonomics}}.  Usually the monitoring
on the tunnels are the VNs should be separately deployed for the
network operation, but it is possible to have common parameters that
are both needed for the VN/TE and the configured services.  Common
types are imported in both modules.

VPN-level parameters and their monitoring have been defined in
{{!I-D.www-bess-yang-vpn-service-pm}}.  This module focus on the
performance on the topology at different layer or the overlay
topology between VPN sites.  On the other hand, this document is
focusing on the performance of the service configured between
Customer Ends (CE).

# Consideration on Monitoring Parameters

There can be multiple groups of parameters for monitoring, such as
latency, bit error rate (BER).  Some of these parameters are layer-
dependent, for example, packet loss is only applicable in packet
networks are won't be neede for layer 1 OTN and layer 0 WSON.

This document starts with the specification of the latency
measurement for both Ethernet service and client signal service.  In
the future version additional parameters would be added into the data
model in the same approach as the latency in the current version.  A
candidate list of parameters to be monitored include: Latency, Packet
Loss, Bit Error Rate (BER), Jitter, Bandwidth, Byte/Packet number and
so on.

# OAM Configuration

The operation, administration and maintenance protocols and data
models have been specified in {{!RFC8531}} for the connection-oriented
network.  The model is referenced in this work to develop an
Ethernet-specific OAM models, which is augmenting the service
performance monitoring data model.

The definitions of OAM terminologies, such as maintainence
Maintenance Domain (MD), Maintenance Association (MA), and
Maintenance End Points (MEP), can be found in {{!RFC8531}} as well.

# YANG Model for Performance Monitoring

## YANG Tree for Performance Monitoring

~~~~ ascii-art
{::include ietf-service-pm.tree}
~~~~
{: #fig-service-pm-tree title="YANG Tree for Performance Monitoring" artwork-name="ietf-service-pm.tree"}

## YANG Tree for OAM Configuration

~~~~ ascii-art
{::include ietf-eth-service-oam.tree}
~~~~
{: #fig-eth-service-oam-tree title="YANG Tree for OAM Configuration" artwork-name="ietf-eth-service-oam.tree"}

# YANG Code for Performance Monitoring

## The Performance Monitoring YANG Code

~~~~ yang
{::include ietf-service-pm.yang}
~~~~
{: #fig-service-pm-yang title="Performance Monitoring YANG Code"
sourcecode-markers="true" sourcecode-name="ietf-service-pm@2021-07-07.yang"}

## The OAM Configuration YANG Code

~~~~ yang
{::include ietf-eth-service-oam.yang}
~~~~
{: #fig-eth-service-oam-yang title="OAM Configuration YANG Code"
sourcecode-markers="true" sourcecode-name="ietf-eth-service-oam@2021-07-10.yang"}

# IANA Considerations

This document requests IANA to register the following URIs in the "ns" subregistry within the "IETF XML Registry" {{!RFC3688}}. Following the format in {{!RFC3688}}, the following registrations are requested.

~~~~
      URI: urn:ietf:params:xml:ns:yang:ietf-service-pm
      Registrant Contact: The IESG
      XML: N/A; the requested URI is an XML namespace.

      URI: urn:ietf:params:xml:ns:yang:ietf-eth-service-oam
      Registrant Contact: The IESG
      XML: N/A; the requested URI is an XML namespace.
~~~~

This document requests IANA to register the following YANG modules in the "IANA Module Names" {{!RFC6020}}. Following the format in {{!RFC6020}}, the following registrations are requested:

~~~~
      name:         ietf-service-pm
      namespace:    urn:ietf:params:xml:ns:yang:ietf-service-pm
      prefix:       svc-pm
      reference:    RFC XXXX (This document)

      name:         ietf-eth-service-oam
      namespace:    urn:ietf:params:xml:ns:yang:ietf-eth-service-oam
      prefix:       eth-oam
      reference:    RFC XXXX (This document)
~~~~

RFC Editor: Please replace XXXX with the RFC number assigned to this document.

# Manageability Considerations

TBD.

# Security Considerations

The data following the model defined in this document is exchanged
via, for example, the interface between an orchestrator and a
transport network controller.  The security concerns mentioned in
{{!I-D.ietf-ccamp-client-signal-yang}} also applies to this document.

The YANG module defined in this document can be accessed via the
RESTCONF protocol defined in {{!RFC8040}}, or maybe via the NETCONF
protocol {{!RFC6241}}.

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
