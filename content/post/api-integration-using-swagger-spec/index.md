---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Setup a mock server under 5 minutes using a Swagger spec"
subtitle: Using Prism to integrate APIs in your front end before they are ready
summary: Using Prism to integrate APIs in your front end before they are ready
authors: []
tags: ['frontend', 'prism', 'swagger', 'openapi']
categories: ['API integration']
date: 2020-10-24T11:18:55+05:30
lastmod: 2020-10-24T11:18:55+05:30
featured: false
draft: false
image:
  filename: featured.jpg
  focal_point: SMART
  preview_only: false
---

You are working on a front end app. You have started developing a feature but the API is not quite ready. The contracts are ready and the back end team have shared an OpenAPI Swagger spec with you. Do you need to wait for the APIs to be ready to start integrating in your front end? No.

You can setup a mock API server using the swagger spec you have, right now!

1. ### Make sure you have Node version > 12

    I find [this](https://medium.com/macoclock/update-your-node-js-on-your-mac-in-2020-948948c1ffb2) the fastest way to update.


2. ### Install Prism globally

    ```
    npm install -g @stoplight/prism-cli
    ```

3. ### Download the latest Swagger spec file

    Get the latest swagger spec file (Let's say `spec.yaml`) from the back end team and save it in a folder, say `specfications`.

4. ### Run Prism with the latest mock

    Go to the `specifications` directory and run,

    ```
    prism mock spec.yaml
    ```

    `spec.yaml` being the latest spec file.

This will start the mock server and you will see in the terminal (Or command prompt) the endpoint URL. Use this URL in your frontend.

If the endpoints in your Swagger spec need headers and metadata, you will need to pass them to the requests to successfully get a response, imitating the real API.

> Prism uses the examples set in the spec file and serves them when you hit a certain endpoint. This means that you will get the same response everytime you hit a particular endpoint. This is not ideal, but is a good stopgap until the actual APIs are ready.

If you tried this, I'd like to hear how it went ✌🏻
