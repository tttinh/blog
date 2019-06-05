# Setup GCP Connections on Airflow

[Home](../README.md)

## Create Google Cloud Service Account

Go to the console:

![console](img/console_service_account.png?raw=true)

Create the service account. Make sure the JSON private key has Editor's rights:

![console](img/create_service_account.png?raw=true)

Also, the service account needs to have permission to access the GCS bucket and Bigquery dataset.

After create succeed, a json key will be downloaded to your computer. Save it for next step.

## Create Airflow Connection

After having the GCP key, you need to create a connection in `Admin -> Connections` using your key.

In Airflow you need to define the *my_gcp_conn* named connection to your project:

![console](img/airflow_connection.png?raw=true)

Copy and paste the raw json of service account key file(the file you have downloaded in the above step) into `Keyfile JSON` field, supply the *project_id* and define the
minimum scope of `https://www.googleapis.com/auth/cloud-platform`.

Click `Save` and you are done.
