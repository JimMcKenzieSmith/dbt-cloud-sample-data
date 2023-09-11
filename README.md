# dbt-cloud-sample-data
A repo to demo use of dbt cloud, with the snowflake sample data that is included with a snowflake free trial.

## Setup

* Fork this repo into your own github account
* Get a 30 day free Snowflake trial: https://signup.snowflake.com/developers

Run the following in your new Snowflake account:
```sql
use role accountadmin;
create role dbt_admin;
grant role sysadmin to role dbt_admin;
grant role dbt_admin to user <your-snowflake-username>;
grant usage on warehouse compute_wh to role dbt_admin;
use role dbt_admin;
use warehouse compute_wh;
create database reporting;
```

* Get a free dbt Cloud developer account: https://www.getdbt.com/pricing
* In setup of the dbt Cloud account:
    * Use the dashed account name which is found in Snowflake under Admin->Accounts when hovering over the account link. Example: `abcdefg-hij98765`
    * Set the database to `REPORTING` and the warehouse to `COMPUTE_WH`.
    * Enter a developer schema into Dbt cloud during setup (Dbt will prompt a suggested dbt_ + first initial + last name)

## Usage

Try creating a feature branch in the Develop area.  

In Develop mode in Dbt Cloud, go into `pricing_summary.sql` and run:
    Build +model (Upstream)

Now go over to Snowflake and browse your databases and schemas to see that you now have two views that were created by Dbt:
* REPORTING.<YOUR_DEVELOPER_SCHEMA>.PRICING_SUMMARY
* REPORTING.<YOUR_DEVELOPER_SCHEMA>_STAGING.STG_LINEITEM

Query the views.

Setup a deployment in dbt Cloud (name it whatever you want).  Use the same `REPORTING` database, but a different schema such as `ANALYTICS`. Setup the deployment job and run it.

Now go over to Snowflake and browse your databases and schemas to see that you now have two views that were created by Dbt:
* REPORTING.ANALYTICS.PRICING_SUMMARY
* REPORTING.ANALYTICS_STAGING.STG_LINEITEM

Query the views.

Try updating something, create a pull request, and merge to main.

## What We Have Accomplished

1. We setup Snowflake to have a role that dbt could use.
2. We setup dbt cloud to connect to our Snowflake account and build models from sample source data.
3. We have demonstrated that we can develop new features in an isolated feature branch that uses a developer named schema.  
4. We have also demonstrated that we can merge back to the main branch where the code can be officially deployed (to a QA environment or a Production environment). And the deployed code will use an officially named schema (in this case `analytics`) that runs only the main branch of code.
