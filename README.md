# dbt-cloud-sample-data
A repo to demo use of dbt cloud, with the snowflake sample data that loads with the snowflake free trial.

## Setup

* Get a 30 day free Snowflake trial: https://signup.snowflake.com/developers

Run the following in your new Snowflake account:
```sql
create role dbt_admin;
grant role sysadmin to role dbt_admin;
grant role dbt_admin to user <your-snowflake-username>;
grant usage on warehouse compute_wh to role dbt_admin;
use role dbt_admin;
use warehouse compute_wh;
create database reporting;
```

* Get a free dbt Cloud developer account: https://www.getdbt.com/pricing
* In setup of the dbt Cloud account, set the database to `REPORTING` and the warehouse to `COMPUTE_WH`.
* Enter a developer schema into Dbt cloud during setup (Dbt will prompt a suggested dbt_ + first initial + last name)

## Usage

In Develop mode in Dbt Cloud, go into pricing_summary_past_90.sql and run:
    Build +model (Upstream)

Go over to Snowflake and browse your databases and schemas to see that you now have two views that were created by Dbt:
* REPORTING.<YOUR_DEVELOPER_SCHEMA>.PRICING_SUMMARY_PAST_90
* REPORTING.<YOUR_DEVELOPER_SCHEMA>_STAGING.STG_LINEITEM

Query the views.

Setup a deployment in dbt Cloud (name it whatever you want).  Use the same `REPORTING` database, but a different schema such as `ANALYTICS`. Setup the deployment job and run it.

Go over to Snowflake and browse your databases and schemas to see that you now have two views that were created by Dbt:
* REPORTING.ANALYTICS.PRICING_SUMMARY_PAST_90
* REPORTING.ANALYTICS_STAGING.STG_LINEITEM

Query the views.
