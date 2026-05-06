---
title: "Akri: expose IoT / leaf devices as Kubernetes resources"
main_link: https://github.com/project-akri/akri
keywords: [akri, kubernetes, iot, edge, devices, ip-cameras, usb, gpu, fpga, cncf]
status: reviewed
---

# Akri: expose IoT / leaf devices as Kubernetes resources

**Main link:** <https://github.com/project-akri/akri>

Docs: <https://docs.akri.sh/>

## Summary

[Akri](https://github.com/project-akri/akri) is a CNCF Sandbox project (originally from Microsoft / DeisLabs) that lets a Kubernetes cluster *discover* and *schedule against* heterogeneous leaf devices — IP cameras (ONVIF / RTSP), USB devices (`/dev/video0`, USB serial, ...), OPC UA nodes, and embedded resources like GPUs and FPGAs. It runs as a controller + per-protocol discovery handlers + a per-node agent that turns each detected device into a Kubernetes [device-plugin](https://kubernetes.io/docs/concepts/extend-kubernetes/compute-extensions/device-plugins/) resource. From there, your pods declare `resources: { limits: { akri.sh/onvif-camera: "1" } }` and Kubernetes schedules them onto whichever node has a free camera attached.

The pitch: *"You name it, Akri finds it, you use it."*

## Insight

Akri solves a problem that genuinely hurts at the edge: a Kubernetes node has a USB camera plugged in or sees an RTSP stream on the LAN, and you want a pod to be scheduled on that specific node and use that specific device — without baking node selectors and host paths into your manifests. Without Akri, you end up with hand-maintained DaemonSets, bespoke labels per node, and brittle init containers that probe for hardware.

Reach for it when:

- You're running [[k3s]] or [k3os](https://k3os.io/) on edge nodes (Raspberry Pi, NVIDIA Jetson, industrial gateways) with attached sensors / cameras / serial devices.
- You have a *fleet* — Akri pays back its complexity once you have more than 3-4 nodes.
- Your devices speak ONVIF, OPC UA, udev (USB), or you can write a discovery handler for them.

Don't reach for it when:

- You have one server with one camera. Just hostPath the device into the pod and move on.
- The device protocol isn't ONVIF / OPC UA / udev and you don't want to write a Rust discovery handler.

The companion universe (also worth knowing): [[balena]] for the OS/fleet layer underneath, [Eclipse Mosquitto](https://mosquitto.org/) or another MQTT broker (see [[messaging/mqtt|mqtt]]) for telemetry pipes once Akri has connected the devices.

## Similar / related topics

- [[k3s]] — the lightweight Kubernetes Akri is most often paired with.
- [[balena]] — fleet-management OS at a layer below Kubernetes; complementary, not competing.
- [Kubernetes device-plugin API](https://kubernetes.io/docs/concepts/extend-kubernetes/compute-extensions/device-plugins/) — the underlying mechanism Akri builds on.
- [NVIDIA GPU device plugin](https://github.com/NVIDIA/k8s-device-plugin) — GPU-specific equivalent for the GPU case.
- [[messaging/mqtt|mqtt]] — common protocol for telemetry from devices Akri exposes.

## Internal links

<!-- reviewed -->

- [[k3s]] — typical Kubernetes substrate for Akri at the edge
- [[balena]] — complementary fleet-management OS layer
- [[messaging/mqtt|mqtt]] — telemetry transport often used alongside

## Keywords

`#kubernetes` `#akri` `#iot` `#edge` `#devices` `#ip-cameras` `#usb` `#gpu` `#fpga` `#cncf`

## References / raw notes

### What Akri is for (paraphrased from the project README)

> Akri lets you easily expose heterogeneous leaf devices (such as IP cameras and USB devices) as resources in a Kubernetes cluster, while also supporting the exposure of embedded hardware resources such as GPUs and FPGAs. Akri continually detects nodes that have access to these devices and schedules workloads based on them.

Built-in discovery handlers ship for **ONVIF** (IP cameras), **OPC UA** (industrial-IoT nodes), and **udev** (USB and other udev-discoverable devices). Custom protocols are supported by writing a discovery handler in Rust against the Akri SDK.

### Architecture in 30 seconds

- **Controller** — cluster-level pod that watches `Configuration` CRDs (which protocol to discover, what filter, what broker pod to spawn for each found device).
- **Agent** — per-node DaemonSet that runs the requested discovery handlers and registers found devices as device-plugin resources.
- **Broker pods** — per-device (or per-shared-device) pods that Akri spawns automatically once a device is found, to mediate access from your application pods.

### Useful entry points

- Repo: <https://github.com/project-akri/akri>
- Docs: <https://docs.akri.sh/>
- CNCF landscape entry: <https://landscape.cncf.io/?selected=akri>
- Original DeisLabs announcement (2020): <https://www.deislabs.io/posts/announcing-akri/>
