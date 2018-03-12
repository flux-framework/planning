## Section 5. User Management Use Cases

* 5.1: **Unix Users** \
  The system Flux instance will support only users in local password file
  of the cluster on which it is run. Users added will be able to immediately
  submit work to the instance and run jobs.
  - Implementation: \
    System instance userdb will initialize users from `/etc/passwd` file on
    startup. The active user service on one rank will watch the passwd file
    and  add/remove users in userdb as they appear and disappear.

* 5.2: **Administrative users** \
  Administrative users may have extra permissions within the instance, such
  as direct access to the KVS or ability to adjust scheduler priority, etc.
  These users are denoted by an "admin" user role in the userdb. For the
  purposes of S3, this role should be automatically applied via configuration.
  - Implementation: \
    Configuration of system instance should be able to denote list of user
    names with admin role (e.g. `user.admins = [ ... ]`) or add admin role
    to all users in a given group (e.g. `user.admin-groups = [ "staff" ]`).

