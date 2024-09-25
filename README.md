# BigQuery-DBT

This project demonstrates how to create a full ELT pipeline using BigQuery, dbt, and Looker Studio.

Data is loaded into the Data Warehouse (BigQuery) from a data lake (a GCP bucket), then loaded into the dataset. dbt is used to clean, transform, and load the data, resulting in a table that Looker Studio uses for creating visualizations.

## Setup Instructions

To run the project, you first need to populate a BigQuery dataset using the NYC TLC dataset. There are multiple ways to do this, but the easiest is to run the SQL commands found in the `/scripts` directory. You can execute these using the BigQuery web interface or the `bq` CLI.

### Step 1: Create a dbt Project

1. Set up a dbt project using dbt Cloud and connect it to BigQuery. You can refer to the [official documentation](https://docs.getdbt.com/guides/bigquery?step=1) for detailed steps.
   
2. For version control, using a Git provider has the added benefit of allowing you to run CI jobs on pull requests.

3. After creating the project, run:

   ``` dbt init ```
This will initialize the standard dbt directory structure.


### Step 2: Running dbt
To build the project with the full dataset, run the following command:

``` dbt build --vars "{'is_test_run': 'false'}" ```

Without specifying `is_test_run: false`, dbt will limit the results to only the first 100 rows for testing purposes.

### Step 3: Creating Environments
dbt creates a development environment by default, but you can also create a production environment.

You can set up jobs to ensure the pipeline runs on schedule in production. One type of job you can configure is a deployment job, which can check for data freshness, create documentation, build models, and execute the pipeline.

Another important job is the CI (Continuous Integration) job, which can be triggered whenever the codebase is altered.

### Step 4: Visualizing with Looker Studio
Once your data is ready, you can start creating visualizations using Looker Studio. To connect Looker Studio to BigQuery, you simply need to click a button if you're using the same Google account for all components.

After connecting to the desired table, be mindful that Looker Studio may automatically aggregate some columns. Review these before building your reports.

### Step 5: Creating Charts
Once your connection is established, add charts and metrics as you like. There are many options for visualizations in Looker Studio. For an example, refer to the `Trip_Analysis_2019-2020.pdf` file, which showcases charts with controls for date and service type.