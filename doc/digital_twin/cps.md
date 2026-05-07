---
title: Cyber-Physical Systems (CPS) — academic framing
main_link: https://www.sciencedirect.com/science/article/pii/S2351978917304067
keywords: [cps, cyber-physical-systems, industry-4.0, manufacturing, digital-twin, smart-manufacturing]
status: reviewed
review_date: 2026/05/03
---

# Cyber-Physical Systems (CPS) — academic framing

**Main link:** <https://www.sciencedirect.com/science/article/pii/S2351978917304067>

## Summary

A **cyber-physical system (CPS)** is the academic / Industry 4.0 framing of the same idea industry calls a digital twin: a tight integration of computational models, networking, and physical processes where the cyber and physical halves continuously sense, communicate, and actuate on each other. The two key papers in this folder review CPS-based production systems and the *correlation/comparison* between digital twins and CPS in smart-manufacturing contexts.

## Insight

Treat **CPS** and [[definition|digital twin]] as the *same underlying concept seen from different communities*: CPS is the term you'll see in academic, NSF, and EU-Horizon funding contexts; "digital twin" is the term used by industrial vendors (Siemens, IBM, NVIDIA, BMW). The differences are mostly emphasis: CPS literature foregrounds **control loops, real-time semantics, formal methods, and safety**; digital-twin literature foregrounds **lifecycle visualisation and operational decision support**. If you're searching the literature for foundational ideas (sensor fusion, distributed control, formal modelling), use "CPS"; if you're looking for industrial case studies, use "digital twin". A CPS-aware approach will save you from the digital-twin literature's habit of glossing over hard real-time / safety constraints.

## Similar / related topics

- [[definition]] — Industry framing of essentially the same concept.
- [[isc2022]] — NVIDIA + BMW supercomputing-scale realisation of CPS principles.
- **Industry 4.0** — German government strategic initiative that brought CPS into manufacturing discourse.
- **IEC 61499** — function-block standard for distributed industrial control (a CPS-aligned alternative to PLCs).
- **NSF CPS programme** — major US funder for academic CPS research.

## Internal links

- [[definition]] — Industrial framing of the same concept.
- [[isc2022]] — Concrete industrial deployment realising CPS ideas at scale.
- [[bmw]] — BMW iFACTORY case study.

## Keywords

`#cps` `#cyber-physical-systems` `#industry-4.0` `#manufacturing` `#digital-twin` `#smart-manufacturing`

## References / raw notes

### Key papers

- **A Review of the Roles of Digital Twin in CPS-based Production Systems** — Procedia Manufacturing, 2017: <https://www.sciencedirect.com/science/article/pii/S2351978917304067>

  Surveys how digital twins fit into CPS-based production: control loops, condition monitoring, predictive maintenance, what-if simulation. Good entry point if you want the academic literature's framing of digital twins.

- **Digital Twins and Cyber–Physical Systems toward Smart Manufacturing and Industry 4.0: Correlation and Comparison** — Engineering, Elsevier, 2019: <https://www.sciencedirect.com/science/article/pii/S209580991830612X>

  Direct head-to-head: terminology overlap, where the two concepts diverge, how Industry 4.0 reference architectures (RAMI 4.0) accommodate both.

### CPS taxonomy at a glance

| Layer | What it is | Examples |
|-------|-----------|----------|
| Physical | The thing being controlled | Robot arm, turbine, vehicle, plant |
| Sensor | Data acquisition | IMU, vision, force/torque, flow, temperature |
| Network | Data transport | EtherCAT, OPC-UA, MQTT, ROS DDS, 5G |
| Compute / decision | Models, control, AI | PLC + edge ML + cloud back-haul |
| Actuator | Acting back on the physical | Servo, valve, switch, robot effector |

The defining trait is the **closed loop** — the model doesn't just observe, it acts.
