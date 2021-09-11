# Connecting to Cloud SQL from Cloud Run (fully managed)

If you want to write a web service that runs on Google Cloud Run using Golang. And this service uses Google Cloud SQL as database. Then this is how it setups.

In this case, I use Postgres but other kind of database will be simillar.

## Follow Google Cloud SQL Guide

Detail instructions are [here](https://cloud.google.com/sql/docs/postgres/connect-run).

## Implementation

```go
var connectStr = fmt.Sprintf(
    "host=%v port=%v user=%v password=%v dbname=%v sslmode=disable",
    dbConfig.host,
    dbConfig.port,
    dbConfig.user,
    dbConfig.pass,
    dbConfig.db,
)

if err := orm.RegisterDriver("postgres", orm.DRPostgres); err != nil {
    panic(err)
}

beego.Info("Connect database with connect string: ", connectStr)
if err := orm.RegisterDataBase("default", "postgres", connectStr, 30); err != nil {
    panic(err)
}
```

I use beego framework, but the point is the connection string. Your database host should be: `/cloudsql/<CLOUD_SQL_INSTANCE>`, and your CLOUD_SQL_INSTANCE's format is **PROJECT_ID:REGION:INSTANCE_ID**.

That's everything you need to do to make your application runs on Google Cloud Run connect to your database which runs on Google Cloud SQL.

Remember your container must listen on port 8080! Please read this [document](https://cloud.google.com/run/docs/reference/container-contract) for more details.

