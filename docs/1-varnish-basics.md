
# Varnish basic configs overview
#### by sdarioz

In the first part of this notebook, we covered the basics of running Varnish on Kubernetes. Now let's look at some more advanced configuration to take full advantage of Varnish's caching capabilities.

## Customizing the Varnish Configuration

The default Varnish configuration provided by the `emgag/varnish` image works well out of the box. But for more control, we can mount a custom `default.vcl` file:

```
// Example deployment with custom VCL mount
```

This allows us to tweak things like:

- Caching rules - what responses to cache and for how long
- Request handling - routing, filtering, rewrites
- Backends - load balancing, health checks, failovers

## Cache Invalidation

To keep the cache fresh, we need ways to proactively invalidate stale content:

**Purges** - Manually clearing cached content, eg. when content changes

**Bans** - Blocking URLs from being cached based on conditions

Common approaches include:

- Configuring purge/ban via HTTP requests to Varnish
- Using a caching proxy like Envoy to send purges
- Invalidating via service mesh sidecars

We could also trigger purges from our CD pipeline using Varnish's CLI.

## Edge Side Includes

Varnish supports Edge Side Includes (ESI) for assembling cached fragments into pages.

For example, a blog homepage could cache:

- The header/footer
- The sidebar
- Each post fragment

The full page is assembled from the cached components. This way the header/sidebar can stay cached while new posts are added.

## Streaming

Varnish can enable streaming of large responses like video files. This allows the content to be sent to clients immediately without waiting for the full response.

Streaming requires handling Range requests and configuring ESI correctly.

## Health Checks

To detect origin outages, we can configure health checks in Varnish:

```
// backends with health checks
```

This continuously pings the origin and marks backends as healthy/unhealthy to enable failovers.

## Varnish Modules

Varnish has a robust extension system via Varnish Modules (VMODs). There are 100+ modules providing advanced capabilities like:

- Header manipulation
- URL rewriting
- Access control
- Authentication
- GeoIP handling
- JSON parsing

VMODs allow implementing complex edge logic while keeping the core fast and simple.

----
# Next:
## Integrating with Tools
### Observability

There are modules available for integrating Varnish with tools like:

- Prometheus - export metrics for monitoring
- Grafana - visualize metrics and analytics
- ELK - logging and analytics with the Elastic stack

This provides observability into cache performance and how users interact with the edge.

## Summarizing

With advanced configuration, Varnish can become a highly customizable high-performance edge for applications. 

Features like fine-grained caching, streaming, health checks, and extensions provide powerful benefits. Integrating analytics tools gives visibility into the edge to optimize it.

Hopefully this gives you foundations for leveraging Varnish's capabilities when accelerating your web applications on Kubernetes.