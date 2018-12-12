# Google Cloud Functions

Google Cloud Functions is a lightweight compute solution for developers to create single-purpose, stand-alone functions that respond to Cloud events without the need to manage a server or runtime environment.

## Types of Cloud functions

HTTP functions

You invoke HTTP functions from standard HTTP requests. These HTTP requests wait for the response and support handling of common HTTP request methods like GET, PUT, POST, DELETE and OPTIONS. When you use Cloud Functions, a TLS certificate is automatically provisioned for you, so all HTTP functions can be invoked via a secure connection.

Background functions

You can use background functions to handle events from your Cloud infrastructure, such as messages on a Cloud Pub/Sub topic, or changes in a Cloud Storage bucket.

For details, see Writing Background Functions.

## Background functions





