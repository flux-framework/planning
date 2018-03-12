## Section 3. Resource Services Use Cases

* 3.1: **Resource up/down status** \
  Configured resources are marked unavailable until they are known usable
  for a job.
  - Implementation: \
  Sets of resources are associated with each broker rank (that rank being
  the broker which will be capable of executing work on the resource set).
  Resources within the set assigned to a broker will be marked as
  down/unavailable until the broker is registered as live by broker monitoring
  (1.6)

* 3.2: **Withdraw Resource from Service** \
  An alternate state to "down" is provided to withdraw a resource or set
  of resources from being available for scheduling. Traditionally this
  is called the "drain" state or flag. The drain state can exist at the
  same time as any other state, and it is preserved until cleared. Withdrawing
  a reasource should also affect all of its children.
  - Implementation: \
  :warning: TDB.
 

* 3.3: **Resource Status Tool** \
  A resource query tool to summarize state of resource types in a Flux
  instance, e.g.:
  ```
  $ flux resource status "node"
  Node:
  total: 16
  up: 16
  down: 0
  drained: 0
  inuse: 0
  ```
  - Implementation: \
  An interface to query total resources in a given state will be offered
  by the system. Supported states will include configured, up, down, 
  drained, and in-use/allocated. One or all states should be able to be 
  queried in a single request. This query can be used to create the
  `flux resource status` tool.

