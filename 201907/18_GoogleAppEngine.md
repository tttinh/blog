# Google App Engine

[Home](../README.md)

## What is App Engine

App Engine is a fully managed, serverless platform for developing and hosting web applications at scale. You as a developer focuses on your code, then let App Engine take care of provisioning servers and scaling your app instances based on demand.

There are two kind of environments to choose from:

- Standard Environment: you write it this way, we'll scale for you.
- Flexible Environment: it is Docker containers under the covers, so you can run any application that will run in a Docker container. Not fit for really low traffic sites, cause it keeps 2 VMs running under the covers.

Best fit:

- You think about code, HTTP requests, versions.
- Stateless serving applications.
- Scaling to high traffic.

## Why App Engine

With App Engine you can:

- Run multiple version of your application at the same time.
- Split traffic between multiple version of your application (roll out, A/B testing,...)
- Automatically scale up and down to zero, balance app instances across multiple available zone.
- Integrate with Stackdriver easily for: monitoring, logging, debugging and tracing.

## Some Facts

- Keep in mind that only an Owner can create App Engine applications in the project and add other people to the project.
- To build and deploy your application on Google App Engine, you need to enable Cloud Build API and App Engine Admin API. (Python3 standard environment)
