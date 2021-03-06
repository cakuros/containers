---

copyright:
  years: 2014, 2020
lastupdated: "2020-04-30"

keywords: kubernetes, iks, envoy, sidecar, mesh, bookinfo

subcollection: containers

---

{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}


# About the managed Istio add-on
{: #istio-about}

Istio on {{site.data.keyword.containerlong}} provides a seamless installation of Istio, automatic updates and lifecycle management of Istio control plane components, and integration with platform logging and monitoring tools.
{: shortdesc}

With one click, you can get all Istio core components and additional tracing, monitoring, and visualization up and running. Istio on {{site.data.keyword.containerlong_notm}} is offered as a managed add-on, so {{site.data.keyword.cloud_notm}} automatically keeps all your Istio components up-to-date.

## What is Istio?
{: #istio_ov_what_is}

[Istio](https://www.ibm.com/cloud/istio){: external} is an open service mesh platform to connect, secure, control, and observe microservices on cloud platforms such as Kubernetes in {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

When you shift monolith applications to a distributed microservice architecture, a set of new challenges arises such as how to control the traffic of your microservices, do dark launches and canary rollouts of your services, handle failures, secure the service communication, observe the services, and enforce consistent access policies across the fleet of services. To address these difficulties, you can leverage a service mesh. A service mesh provides a transparent and language-independent network for connecting, observing, securing, and controlling the connectivity between microservices. Istio provides insights and control over the service mesh by so that you can manage network traffic, load balance across microservices, enforce access policies, verify service identity, and more.

For example, using Istio in your microservice mesh can help you:
- Achieve better visibility into the apps that run in your cluster.
- Deploy canary versions of apps and control the traffic that is sent to them.
- Enable automatic encryption of data that is transferred between microservices.
- Enforce rate limiting and attribute-based whitelist and blacklist policies.

An Istio service mesh is composed of a data plane and a control plane. The data plane consists of Envoy proxy sidecars in each app pod, which mediate communication between microservices. The control plane consists of Pilot, Mixer telemetry and policy, and Citadel, which apply Istio configurations in your cluster. For more information about each of these components, see the [`istio` add-on description](#istio_ov_components).

<br />


## What is Istio on {{site.data.keyword.containerlong_notm}}?
{: #istio_ov_addon}

Istio on {{site.data.keyword.containerlong_notm}} is offered as a managed add-on that integrates Istio directly with your Kubernetes cluster.
{: shortdesc}

**What does this look like in my cluster?**</br>
When you install the Istio add-on, the Istio control and data planes use the network interfaces that your cluster is already connected to. Configuration traffic flows over the private network within your cluster, and does not require you to open any additional ports or IP addresses in your firewall. If you expose your Istio-managed apps with an Istio Gateway, external traffic requests to the apps flow over the public network interface.

**How does the update process work?**</br>
The Istio version in the managed add-on is tested by {{site.data.keyword.cloud_notm}} and approved for the use in {{site.data.keyword.containerlong_notm}}. Additionally, the Istio add-on simplifies the maintenance of your Istio control plane so you can focus on managing your microservices. {{site.data.keyword.cloud_notm}} keeps all your Istio components up-to-date by automatically rolling out patch updates to the most recent version of Istio that is supported by {{site.data.keyword.containerlong_notm}}.

Whenever the managed Istio add-on is updated, make sure that you [update your `istioctl` client and the Istio sidecars for your app](/docs/containers?topic=containers-istio#update_client_sidecar) to match the Istio version of the add-on. You can check whether the versions of your `istioctl` client and the Istio add-on control plane match by running `istioctl version`.

If you need to use the latest version of Istio or customize your Istio installation, you can install the open source version of Istio by following the steps in the [Quick Start with {{site.data.keyword.cloud_notm}} tutorial](https://istio.io/docs/setup/platform-setup/ibm/){: external}.
{: tip}

<br />


## What comes with the Istio add-on?
{: #istio_ov_components}

In Kubernetes version 1.16 and later clusters, you can install the generally available managed Istio add-on, which runs Istio version 1.4.
{: shortdesc}

The Istio add-on installs the core components of Istio. For more information about any of the following control plane components, see the [Istio documentation](https://istio.io/docs/concepts/what-is-istio/){: external}.
* `Envoy` proxies inbound and outbound traffic for all services in the mesh. Envoy is deployed as a sidecar container in the same pod as your app container.
* `Mixer` provides telemetry collection and policy controls.
  * Telemetry pods are enabled with a Prometheus endpoint, which aggregates all telemetry data from the Envoy proxy sidecars and services in your app pods.
  * Policy pods enforce access control, including rate limiting and applying whitelist and blacklist policies.
* `Pilot` provides service discovery for the Envoy sidecars and configures the traffic management routing rules for sidecars.
* `Citadel` uses identity and credential management to provide service-to-service and end-user authentication.
* `Galley` validates configuration changes for the other Istio control plane components.

To provide extra monitoring, tracing, and visualization for Istio, the add-on also installs [Prometheus](https://prometheus.io/){: external}, [Grafana](https://grafana.com/){: external}, [Jaeger](https://www.jaegertracing.io/){: external}, and [Kiali](https://kiali.io/){: external}.

<br />


## Limitations
{: #istio_limitations}

Review the following limitations for the managed Istio add-on.
{: shortdesc}

* When you enable the managed Istio add-on, you cannot use `IstioControlPlane` resources to customize the Istio control plane installation. Only the `IstioControlPlane` resources that are managed by IBM are supported.
* You cannot modify the `istio` configuration map in the `istio-system` namespace. This configuration map determines the Istio control plane settings after the managed add-on is installed.
* The following features are not supported in the managed Istio add-on:
  * [Policy enforcement](https://istio.io/docs/tasks/policy-enforcement/enabling-policy/){: external}
  * [Any features by the community that are in alpha or beta release stages](https://istio.io/about/feature-stages/){: external}
