
# Varnish Cache & Kubernetes basic overview
#### by sdarioz

## Introduction

Varnish Cache is an open source web application accelerator that can significantly improve the speed and scalability of web services. In this Markdown Notebook, we will cover the basics of how Varnish works and how to integrate it into a containerized application running on Kubernetes.

## How Varnish Cache Works

At a high level, Varnish sits in front of a web server and caches content close to the user.

- Varnish receives requests from clients
- Checks its cache for the requested content
    - If found, returns the cached version
    - If not found, passes the request to the origin server
- Caches the response from the origin server
- Returns the response to the client

This way, repeated requests can be served from Varnish's memory cache which is much faster than hitting the origin server directly.

## Benefits of Using Varnish

Some key benefits of adding a Varnish reverse proxy cache:

- Reduces load on the backend origin servers
- Improves response times for end users
- Handles spikes in traffic more gracefully
- Saves bandwidth
- Can serve stale content if origin is down

## Integrating Varnish with Kubernetes

To use Varnish on Kubernetes, we need to:

1. Run Varnish in a container
2. Expose it via a Kubernetes service
3. Configure routes to send traffic to Varnish first

We'll cover each step in more detail next.

## Running Varnish on Kubernetes

First, we need a Docker image with Varnish installed. We'll use the `emgag/varnish` image:

```
docker pull ghcr.io/emgag/varnish
```

Next, we can create a Kubernetes deployment:

```
// Varnish deployment config 
```

This runs Varnish with the default config. We'll also need a service to expose it:

```
// Varnish service config
```

## Routing Traffic to Varnish

Now that Varnish is running, we need to configure the ingress resource to route traffic to it:

```
// Ingress config to route traffic to Varnish first
```

This will send all external traffic to the Varnish service first before reaching the application.

## Configuring the Application

On the application side, we need to:

- Configure Varnish as the `PROXY_pass` for requests
- Allow caching of appropriate responses

With that, Varnish should be integrated and ready to accelerate our application!

## Summary

In this notebook we covered:

- How Varnish caches and accelerates web applications
- Running Varnish on Kubernetes
- Routing traffic through Varnish
- Configuring the application to leverage caching

Varnish can provide huge performance improvements for web apps on Kubernetes. Read on to see examples of implementing advanced Varnish configurations.


