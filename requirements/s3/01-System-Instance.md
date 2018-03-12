## Section 1. System Instance Use Cases

* 1.1: **System Instance Bootstrap** \
  System instance is "launched" via standard system startup tools.
  - Implementation: \
  System instance brokers are started automatically by systemd or other system
  initialization and read configuration from static config files to aid in
  wireup. This system instance can tolerate disjoint startup and "resources"
  or ranks are noted as "down" by the instance until they join the comms
  session.

* 1.2: **System Instance Resource Configuration** \
  System instance reads schedulable resource information from a configuration
  file. Only resources so configured may be scheduled to jobs.
  - Implementation: \
  Use GraphML in the case of flux-sched resource service or simplified
  RDL+hwloc if using only lightweight flux-core testing scheduler.

* 1.3: **System Instance Default Connector** \
  A Flux command executed on a system with a system instance will by default
  use a connector to the system instance.
  - Implementation: \
  If no `FLUX_URI` set in environment, `flux`(1) command uses default system
  configuration (from `/etc/flux/conf.d/*.toml`?) to open standard default
  connector and URI.

* 1.4:  **KVS Resiliency** \
  KVS is resilent to loss of brokers hold master KVS data.
  - Implementation: \
  KVS will store and state save of last known good root hash, recoverable from
  a known location in case of restart/reboot of comms session and/or rank 0
  broker.

* 1.4: **Communications Resiliency** \
  The Flux system instance will be resilient to the loss of nodes or brokers
  internal to the comms network.
  - Implementation: \
  Flux comms network will transition to BGN or other network type with
  redundancy that allows loss of abitrary numbers of internal network nodes.

* 1.5: **Service Resiliency** \
  Important Flux services do not lose data when nodes/brokers are lost.
  - Implementation: \ 
  Services leverage KVS resiliency for important data (checkpoints), and
  similarly leverage network resiliency to remain available after internal
  node/broker loss.

* 1.6: **System Instance Broker Monitoring** \
  System instance will monitor and report broker ranks going up/down and
  generate events and liveness status query capability.
  - Implementation: :warning: \
  TBD

