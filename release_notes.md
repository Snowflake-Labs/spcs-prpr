# Snowpark Container Services Limited PrPr Release Notes

This page contains release notes for past and upcoming behavior changes for the limited Private Preview of Snowpark Container Services (SPCS). 

SPCS is a new product offered by Snowflake, so it sees frequent updates and occasional behavior changes at this stage of the product development lifecycle. As such, this document is meant to keep early users informed of recent and upcoming user-visible changes. This page is considered the most up-to-date source of truth for user-facing changes; there is some delay in making changes to the [public documentation](https://docs.snowflake.com/LIMITEDACCESS/snowpark-containers/overview).

**Note**: Run `SELECT CURRENT_VERSION()` to see the current version of Snowflake in your account.

_Last updated: Sep 19, 2023 / (v7.33.1)_
<br><br>

## Upcoming changes
| Feature              | Description                                                                                                                                                                                                                                                                                                                                                                       | Release      |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ |
| Synchronous job runs | Enables running of jobs in a synchronous fashion. I.e., job execution (`EXECUTE SERVICE`) commands do not return until the job is complete. Returns with a message saying “Execution \<Query UUID\> completed successfully.”<br><br>Before this change, jobs executed asynchronously and job execution commands returned with “Execution submitted successfully as \<Query UUID\>.” | NOT RELEASED |
| Service auto-resume  | Enables users to specify the `AUTO_RESUME` boolean parameter to control whether to automatically resume a compute pool when a service or job is submitted to it.                                                                                                                                                                                                                | NOT RELEASED |
<br>

## Past changes
| Feature                                                                             | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Release | Reflected in official docs?                                                                                            |
| ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------- | ---------------------------------------------------------------------------------------------------------------------- |
| Defer Ingress URL display for 120s after Service creation                           | Enables behavior in which there is a 120 second delay after a service is created before ingress URL is provisioned.<br><br>After a service is created, running `SHOW SERVICES` will display “Endpoints provisioning in progress... check back in a few minutes” for the public endpoint URL to allow time for dns provisioning and prevent users from entering the url in browser when the dns entry isn't present.<br><br>If you have automation on service endpoint fetching, please update your scripts to wait for the provisioning.                                                                                                                                                                                                                                                                         | 7.32    | In progress                                                                                                            |
| Tag support                                                                         | `CREATE SERVICE` supports an option to associate tags with the service. See documentation for Snowflake tags [here](https://docs.snowflake.com/en/sql-reference/sql/create-tag).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | 7.32    | In progress                                                                                                            |
| Service creation from inline spec syntax change                                     | Enables SQL syntax change that allows creation of services and jobs from an inline specification. The new syntax is as follows:<br><br>Creating a service from an inline spec:<br>```CREATE SERVICE [ IF NOT EXISTS ] <name> IN COMPUTE POOL <compute_pool_name> FROM SPECIFICATION $$ ... $$;```<br><br>Creating a job from an inline spec:<br>```EXECUTE SERVICE IN COMPUTE POOL <compute_pool_name> FROM SPECIFICATION $$ … $$ [ USING ([ ARG1 = <arg1>, ARG2 = <arg2>, ... ])]```                                                                                                                                                                                                                                                                                                                             | 7.31    | In progress                                                                                                            |
| Disruptive Cluster Upgrade                                                          | Enables automatic disruptive cluster upgrades on all accounts.<br><br>As part of cluster upgrades, Snowflake will automatically deploy new OS, software, drivers and fix security vulnerabilities to compute pool nodes.<br>Customers should expect some downtime every 14 to 21 days after cluster creation/re-creation date during the maintenance period. The maintenance period runs every Tuesday, Wednesday, Thursday and Friday from 12 am to 5 am.<br><br>As part of the upgrades, all instances in the compute pools belonging to an account will be recreated. All services running in those compute pools will be recreated on the new instances. All jobs will be disrupted and customers will have to restart them once the Compute pool is ready. The expected downtime is approximately 30 minutes. | 7.30    | In progress                                                                                                            |
| Add startTime field to get_job_status and get_service_status for running containers | Add `startTime` field with service / job start timestamp to the response of `GET_JOB_STATUS()` and `GET_SERVICE_STATUS()`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | 7.29    | [Yes](https://docs.snowflake.com/LIMITEDACCESS/snowpark-containers/reference/service#system-get-service-status)        |
| Privilege limits on show / decribe service(s)                                       | Enforce privilege limits on the `SHOW SERVICES` and `DESCRIBE SERVICE` commands.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | 7.29    | [Yes](https://docs.snowflake.com/LIMITEDACCESS/snowpark-containers/reference/service#show-services)                    |
| Dynamic validation of GPU requests                                                  | Enables multi-GPU scenarios and dynamic validation of service GPU requests based on the number of GPUs available in the selected compute pool instance type. The previous service creation flow did not allow a service to request more than 1 GPU regardless of the number available in the compute pool, except by enabling it customer by customer basis.                                                                                                                                                                                                                                                                                                                                                                                                                                                     | 7.28    | In progress                                                                                                            |
| Deprecate ALTER COMPUTE POOL … STOP ALL SERVICES                                    | Deprecates syntax giving users the ability to stop all services within a given compute pool.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | 7.27    | [Yes](https://docs.snowflake.com/LIMITEDACCESS/snowpark-containers/working-with-compute-pool#managing-a-compute-pool)  |
| Compute pool suspension suspends its services                                       | Enables behavior in which suspending a compute pool suspends all of the services running inside of it.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | 7.27    | [Yes](https://docs.snowflake.com/LIMITEDACCESS/snowpark-containers/working-with-compute-pool#managing-a-compute-pool)  |
| GET_JOB_STATUS can wait for job terminal state                                      | Allows users to provide a `<timeout_secs>` param to `GET_JOB_STATUS()` which denotes the number of seconds to wait for the job to reach a terminal state (DONE or FAILED) before returning the status.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | 7.25    | [Yes](https://docs.snowflake.com/LIMITEDACCESS/snowpark-containers/reference/execute-service#system-get-job-status)    |
| CPU requests and limits                                                             | Gives users the ability to set CPU resource requests and limits for individual containers in the `containers.resources` field in the service or job spec.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | 7.23    | [Yes](https://docs.snowflake.com/LIMITEDACCESS/snowpark-containers/specification-reference#containers-resources-field) |
| Shared memory volumes                                                               | Introduces shared memory volumes                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | 7.22    | [Yes](https://docs.snowflake.com/LIMITEDACCESS/snowpark-containers/specification-reference)                          |
| Compute pool auto-suspend feature                                                   | Gives users the ability to set `AUTO_SUSPEND_SECS` parameter during compute pool creation to specify the number of seconds of inactivity after which the compute pool is automatically suspended.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | 7.22    | [Yes](https://docs.snowflake.com/LIMITEDACCESS/snowpark-containers/reference/compute-pool#create-compute-pool)         |
| Custom secrets                                                                      | Enables use of `containers.secrets` to pass Snowflake secrets objects to containers.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | 7.22    | [Yes](https://docs.snowflake.com/LIMITEDACCESS/snowpark-containers/specification-reference#containers-secrets-field)   |
| Add ACCOUNT_USAGE view                                                              | Creates ACCOUNT_USAGE views in user accounts to provide information on credit consumption by snowpark container services compute pools.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | 7.22    | [Yes](https://docs.snowflake.com/LIMITEDACCESS/snowpark-containers/accounts-orgs-usage-views)                          |