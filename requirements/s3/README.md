# S3 Milestone: Informal Requirements and Use Cases

As an early delivery milestone for the Flux project, we propose a lightweight version of a Slurm replacement, which is suitable to function as a multi-user, system-level resource manager for a test cluster. The S3 version Flux will have the following high-level functionality, which is further detailed in the informal requirements and use cases of the pages below:

 * Multi-user
 * FIFO node/core only scheduler
 * System instance with limited resiliency (single node failover)
 * Static configuration

Specifically, the following requirements are culled from S4 for S3 Flux:

 * Preemption
 * Standy jobs
 * Scheduler "reason for not running"
 * Email notification
 * Job requeue
 * Accounting DB
 * Fairshare hierarchy
 * Partition, Association, and "QoS" limits
 * Job dependencies
 * Other job request parameters outside of number of cores

# Requirements and Use Cases

 * **S3-1** [System Instance](01-System-Instance.md)
 * **S3-2** [Scheduler](02-Scheduler.md)
 * **S3-3** [Resource Services](03-Resource-Services.md)
 * **S3-4** [Execution System](04-Execution.md)
 * **S3-5** [User Management](05-User-Management.md)
