> **Deprecation notice**: Grafana Agent has been deprecated and is now in
> Long-Term Support mode. We recommend migrating to the new [Grafana Alloy]
> collector, which is built on the foundation of Grafana Agent Flow.
>
> For more information, read our blog posts about Alloy and how to easily
> migrate from Agent to Alloy:
>
> * [Alloy announcement blog post](https://grafana.com/blog/2024/04/09/grafana-alloy-opentelemetry-collector-with-prometheus-pipelines/)
> * [Alloy FAQ](https://grafana.com/blog/2024/04/09/grafana-agent-to-grafana-alloy-opentelemetry-collector-faq/)
> * [Migrate to Alloy](https://grafana.com/docs/alloy/latest/tasks/migrate/)
>
> [Grafana Alloy]: https://github.com/grafana/alloy

<p align="center"><img src="docs/sources/assets/logo_and_name.png" alt="Grafana Agent logo"></p>

Grafana Agent is an OpenTelemetry Collector distribution with configuration
inspired by [Terraform][]. It is designed to be flexible, performant, and
compatible with multiple ecosystems such as Prometheus and OpenTelemetry.

Grafana Agent is based around **components**. Components are wired together to
form programmable observability **pipelines** for telemetry collection,
processing, and delivery.

> **NOTE**: This page focuses mainly on "[Flow mode][Grafana Agent Flow]," the
> Terraform-inspired revision of Grafana Agent.

Grafana Agent can collect, transform, and send data to:

* The [Prometheus][] ecosystem
* The [OpenTelemetry][] ecosystem
* The Grafana open source ecosystem ([Loki][], [Grafana][], [Tempo][], [Mimir][], [Pyroscope][])

[Terraform]: https://terraform.io
[Grafana Agent Flow]: https://grafana.com/docs/agent/latest/flow/
[Prometheus]: https://prometheus.io
[OpenTelemetry]: https://opentelemetry.io
[Loki]: https://github.com/grafana/loki
[Grafana]: https://github.com/grafana/grafana
[Tempo]: https://github.com/grafana/tempo
[Mimir]: https://github.com/grafana/mimir
[Pyroscope]: https://github.com/grafana/pyroscope

## Why use Grafana Agent?

* **Vendor-neutral**: Fully compatible with the Prometheus, OpenTelemetry, and
  Grafana open source ecosystems.
* **Every signal**: Collect telemetry data for metrics, logs, traces, and
  continuous profiles.
* **Scalable**: Deploy on any number of machines to collect millions of active
  series and terabytes of logs.
* **Battle-tested**: Grafana Agent extends the existing battle-tested code from
  the Prometheus and OpenTelemetry Collector projects.
* **Powerful**: Write programmable pipelines with ease, and debug them using a
  [built-in UI][UI].
* **Batteries included**: Integrate with systems like MySQL, Kubernetes, and
  Apache to get telemetry that's immediately useful.

[UI]: https://grafana.com/docs/agent/latest/flow/monitoring/debugging/#grafana-agent-flow-ui

## Getting started

Check out our [documentation][] to see:

* [Installation instructions][] for Grafana Agent Flow
* Details about [Grafana Agent Flow][]
* Steps for [Getting started][] with Grafana Agent Flow
* The list of Grafana Agent Flow [Components][]

[documentation]: https://grafana.com/docs/agent/latest/
[Installation instructions]: https://grafana.com/docs/agent/latest/flow/setup/install/
[Grafana Agent Flow]: https://grafana.com/docs/agent/latest/flow/
[Getting started]: https://grafana.com/docs/agent/latest/flow/getting_started/
[Components]: https://grafana.com/docs/agent/latest/flow/reference/components/

## Example

```river
// Discover Kubernetes pods to collect metrics from.
discovery.kubernetes "pods" {
  role = "pod"
}

// Collect metrics from Kubernetes pods.
prometheus.scrape "default" {
  targets    = discovery.kubernetes.pods.targets
  forward_to = [prometheus.remote_write.default.receiver]
}

// Get an API key from disk.
local.file "apikey" {
  filename  = "/var/data/my-api-key.txt"
  is_secret = true
}

// Send metrics to a Prometheus remote_write endpoint.
prometheus.remote_write "default" {
  endpoint {
    url = "http://localhost:9009/api/prom/push"

    basic_auth {
      username = "MY_USERNAME"
      password = local.file.apikey.content
    }
  }
}
```

We maintain an example [Docker Compose environment][] that can be used to
launch dependencies to play with Grafana Agent locally.

[Docker Compose environment]: ./example/docker-compose/

## Release cadence

A new minor release is planned every six weeks.

The release cadence is best-effort: if necessary, releases may be performed
outside of this cadence, or a scheduled release date can be moved forwards or
backwards.

Minor releases published on cadence include updating dependencies for upstream
OpenTelemetry Collector code if new versions are available. Minor releases
published outside of the release cadence may not include these dependency
updates.

Patch and security releases may be created at any time.

## Community

To engage with the Grafana Agent community:

* Chat with us on our community Slack channel. To invite yourself to the
  Grafana Slack, visit <https://slack.grafana.com/> and join the `#agent`
  channel.
* Ask questions on the [Discussions page][].
* [File an issue][] for bugs, issues, and feature suggestions.
* Attend the monthly [community call][].

[Discussions page]: https://github.com/grafana/agent/discussions
[File an issue]: https://github.com/grafana/agent/issues/new
[community call]: https://docs.google.com/document/d/1TqaZD1JPfNadZ4V81OCBPCG_TksDYGlNlGdMnTWUSpo

## Contribute

Refer to our [contributors guide][] to learn how to contribute.

[contributors guide]: ./docs/developer/contributing.md
# test change
