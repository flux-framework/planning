## Section 2. Scheduler Use Cases

* 2.1: **Simple Job Submission** \
  Flux-supports simple job submission requesting only a number of cores,
  via `flux submit` or `flux run`.
  - Implementation: \
  `flux submit --cores=N` or `flux run --cores=N` create a simple jobspec
  with a flat request for a number of cores. That request is signed and
  sent along as job request.

* 2.2: **FIFO Scheduler** \
  Flux-scheduler will allocate jobs in first-in order.
  - Implementation: \
  TBD

* 2.3: **Interactive Jobs** \
  Interactive jobs will be submitted directly to an instance by selecting
  an "interactive" job class (name TDB?). This will be the default for
  `flux run`, with `flux submit` the class may be selected with a cmdline
  option.
  - Implementation: \
   Requested job class is denoted in a jobspec field. The default field
   value will place the job into the normal scheduling queue, but a
   setting of "interactive" (or other suitable name) will place the job
   into a queue that is optimized to reduce to queue wait-time by applying
   other constraints (smaller max core counts, reduced runtime). The scheduler
   may need to hold resources in reserve for interactive queues. The amount
   Needs slightly more design.

* 2.4: **Job Feasibility** \
  Jobs submitted that cannot run under the current configuration will
  be rejected during submission with an illustrative error message.
  - Implementation: \
   Job Feasibility checks are completed by a job ingest plugin or set of
   plugins that check various parameters and reject the job if those
   parameters do not satisfy feasibility. The flux error message returned
   to the user should have a space for a detailed reason string or
   message that can be printed by the job submission utility (not just
   an error number)

* 2.5: **List Jobs** \
  Scheduler should offer a service to return a list of queue-ordered jobs
  to any valid user.
  - Implementation: \
    TBD.

* 2.6: **Job Query Interface** \
  Job metadata availably only to instance scheduler should be available via
  a query interface. Some data about the job should be redacted in the case
  of a query from a user that is not also the owner of the job or have the
  admin role.
  - Implementation: \
    A generic "job" service or similar handles query calls on behalf of
    scheduler, and pulls relevant info from sched metadata in KVS (?)

* 2.7: **Job Cancellation** \
  A `flux job cancel` command should be offered to remove jobs from the
  queue. If the job is running, it would be terminated.
  - Implementation: \
    A generic "job" service or similar handles cancellation? Scheduler
    gets notified of cancellations in order to update queue.

* 2.8: **Job Modification** \
  Modification of some job parameters _may_ be allowed. There should
  be an interface for this, and a mechanism should be explored during S3
  timeframe, but perhaps this could be limited to timelimits or other
  example metadata just as proof-of-concept for S3.
  - Implementation: \
    Unknown. May require ability to modify or amend signed jobspec in KVS.
