## Section 4. Execution Use Cases

* 4.1: **Interactive Parallel Job** \
  An "interactive" parallel job is an execution of `flux run` in which the
  `flux run` (or shell frontend process) stays resident and does not terminate
  until all tasks of the parallel job have exited and IO has completed.

* 4.2: **Background Parallel Job** \
  A background parallel job is a parallel job with no front end process,
  e.g. `flux run` resident during execution. I/O, exit status and state
  are streamed to the KVS, and events generated during major state
  transitions.
  

* 4.3: **Batch Execution** \
  A "batch job" is a script run under the identity of the batch user with
  access to a resource set within the system. A batch job allows parallel
  jobs to be executed on the resource set by one or more individual `flux run`
  invocations from within the batch script. When the script ends the batch
  job will be considered complete.
  - Implementation: \ 
    In Flux a batch job is a _background_ parallel job running an Flux instance,
    with the batch script as the initial program of this instance. A tool
    to submit batch jobs, e.g. `flux submit` should therefore take a
    script argument along with other options and create a jobspec that
    results in running one copy of `flux start` on every node of the
    granted resource set, with a copy of the batch script as the argument
    to `flux start`.

* 4.4: **Interactive Job with No Arguments** \
  In a batch script environment, `flux run COMMAND` with no extra resource
  arguments should start with a copy of the jobspec that created the
  enclosing instance. This implements standard practice in Slurm and possibly
  other batch script environments, where default arguments to `srun` are
  set by the batch script environment. This also allows a single batch script
  to be used under different allocated resource set configurations.
  - Implementation: \
    Simplest approach is for `flux run` to default to a jobspec which
    requests allocation of all extant resources in the current instance.
    (We may want a shorthand jobspec stanza for this.) In Flux however,
    at least the "task slot shape" will be a required argument. To get
    around this, there would need to be a default task slot specification
    that can be passed down to a sub-instance (which doesn't affect the
    task slot of "Node" for the instance itself).

* 4.5: **I/O To and From Files** \
   `flux run` supports sending stderr/stdout to files in addition to
    KVS, and supports redirecting stdin to one or more tasks from a file.
    Default: broadcast stdin to all tasks, stderr/out copied to frontend
    stderr/out or simply held in KVS for background job.

* 4.6: **Process co-location** \
   `flux run` should have a mode to launch work on existing resources
   owned by the requesting user, without submission to a job queue. One
   use case here is co-locating debugger processes or other tools with
   one task per node or one task per existing task. Processes of the
   co-located job will need to be run within the same namespace as the
   targeted job.
   - Implementation: :warning: \
    TBD. It would be convenient if co-located jobs could reuse the same
    KVS namespace as the running job, leveraging existing job shell(s) to
    launch multiple parallel programs. The co-located program should be
    able to be managed as its own parallel execution however, and ehancing
    job shell to manage multiple parallel jobs at once may not be as
    elegant as invoking another job shell which can enter namespace of
    the exising job. In either case, KVS job schema may need to either
    be reworked to allow multiple jobs to be managed per namespace, or
    to allow copying(?) of an existing namespace.
    
* 4.7: **Administrative Execution** \
  Users with "admin" role should be able to execute `flux run` or other
  similar tool without scheduler involvement.
  - Implementation: \
    Admin users will need to have access to create KVS namespace for
    jobs on the fly, with arbitrary R. Then `flux run` works as normal.
    A range of job identifiers could be reserved for these jobs.
    Another approach would be to stick to `flux exec` for this purpose.
