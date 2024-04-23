# Snowpark Container Services PuPr Release Notes

This page contains release notes for the Public Preview of Snowpark Container Services (SPCS). The official documentation for Snowpark Container Services may be found [here](https://docs.snowflake.com/en/developer-guide/snowpark-container-services/overview).

To be notified about release notes, follow the instructions in the [README](https://github.com/Snowflake-Labs/spcs-updates/blob/main/README.md) to subscribe to Github releases for this repository. When there is a new release, this page will be updated and a new Github release will be created to notify you.

**Note**: Run `SELECT CURRENT_VERSION()` to see the current version of Snowflake in your account.

_Last updated: Mar 7, 2024_
<br><br>

## Release notes
| Feature | Description | Release date | Release |
|---------|-------------|--------------|---------|
| Compute Pool metrics | Compute Pool metrics are available as Public Preview for all Snowpark Container Services customers. Compute Pool metrics enable customers to observe the performance of nodes, services and jobs in the compute pool. Customers can now see the amount of free memory available for use by containers on a node or the amount of memory used by a specific container.  Customers can now optimize and troubleshoot their SPCS workloads for improved resource utilization and reduced cost. Additionally, SPCS Compute Pool metrics can be easily integrated with existing enterprise metrics collection, visualization and reporting services, such as Datadog and Prometheus. | April 21, 2024| 8.15|
| Compute Pool | Previously, a Snowpark Container Services Compute Pool may be incorrectly identified as `IDLE` and be suspended despite having `ACTIVE` services or jobs. Now, the show compute pools command output is corrected to display the `ACTIVE` state if there are any active services or jobs. Additionally, the Compute Pool’s auto-suspend timer will only start when there are no active services or jobs. | Mar 7, 2024| 8.9.2|
