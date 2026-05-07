---
title: "Rerun: time-aware multimodal data viewer for robotics & embodied AI"
main_link: https://rerun.io/
keywords: [rerun, robotics, multimodal, time-series, sensor-fusion, point-cloud, embodied-ai]
status: reviewed
review_date: 2026/05/03
---

# Rerun: time-aware multimodal data viewer for robotics & embodied AI

**Main link:** <https://rerun.io/>

Repo: <https://github.com/rerun-io/rerun> · Docs: <https://www.rerun.io/docs>

## Summary

[Rerun](https://rerun.io/) is an open-source viewer (Rust core, Python + Rust SDKs) for *time-aware multimodal data* — exactly the cocktail you get out of a robot or any sensor-rich system: RGB camera frames, depth maps, lidar scans, segmentation masks, 3D point clouds, 3D meshes, transforms (the robot's pose over time), bounding boxes, scalar timeseries, and arbitrary tensors, all aligned on a shared timeline. You log to it from Python (`rr.init("my_run"); rr.log("camera/rgb", rr.Image(arr))`) and the viewer either pops up locally or ingests an `.rrd` file later for replay.

## Insight

Rerun is what you reach for when:

- A normal debugger doesn't help because the bug is "the robot misperceived the world at frame 1473 when sunlight hit the lens". You need to *see* what the robot saw.
- A normal logger doesn't help because the interesting thing is the relationship between five streams over time, not a single line of text.
- A normal Grafana dashboard doesn't help because your data isn't (just) scalars — it's images, point clouds, transforms, geometry.

The killer move is the **shared time axis**: you scrub the timeline and every panel (the RGB feed, the segmentation overlay, the lidar scan, the 3D map, the scalar plot of motor torque) snaps to the same moment. This makes "what was the robot thinking right before it hit the wall?" a *visual* question, which is dramatically faster than reading log files.

It's also useful outside robotics — anywhere you have spatial+temporal data: AR/VR, drone flight logs, computer-vision pipelines, simulation, even some kinds of generative-media QA. The team explicitly markets it for "embodied AI" and that's where most of the recent investment is going.

## Similar / related topics

- [[visualization/grafana|grafana]] — for *scalar* time-series; complementary, not competing.
- [Foxglove Studio](https://foxglove.dev/) — closer competitor; came from the ROS world; comparable feature set.
- [PlotJuggler](https://github.com/facontidavide/PlotJuggler) — time-series plotter with strong ROS bag support; narrower scope.
- [[diagrams]] / [[manim]] — when you want a *static* or *pre-rendered* representation, not a live data view.
- [[groot]] — NVIDIA GR00T humanoid foundation model; one of the kinds of system whose internal state Rerun is good at visualising.

## Internal links

- [[visualization/grafana|grafana]] — for scalar metrics; pairs with Rerun for the multimodal side
- [[manim]] — for prepared explainer animations
- [[diagrams]] — for static architecture / scene diagrams

## Keywords

`#visualization` `#rerun` `#robotics` `#multimodal` `#time-series` `#sensor-fusion` `#point-cloud` `#embodied-ai`

## References / raw notes

### What Rerun is for (paraphrased from the project's pitch)

> Rerun is built to help you understand and improve complex processes that include rich multimodal data, like 2D, 3D, text, time series, tensors, etc. It is used in many industries, including robotics, simulation, computer vision, or anything that involves a lot of sensors or other signals that evolve over time.

The canonical example: a vacuum cleaner that keeps hitting walls. Logging text won't tell you why. Step-debugging won't either. What you need is a viewer that lets you see, at frame N:

- the RGB camera feed
- the depth image
- the lidar scan
- the segmentation overlay (what the perception network thought it saw)
- the 3D map of the room
- detected objects + confidence
- transforms (where the robot thought it was)
- scalars (motor torque, battery voltage, ...)

…all on one timeline you can scrub.

### Hello-world (Python)

```python
import numpy as np
import rerun as rr

rr.init("hello_world", spawn=True)            # spawns the viewer

# Log an RGB image
rr.log("camera/rgb", rr.Image(np.random.randint(0, 255, (480, 640, 3), dtype=np.uint8)))

# Log a 3D point cloud at the same time
points = np.random.rand(1000, 3).astype(np.float32)
colors = np.random.randint(0, 255, (1000, 3), dtype=np.uint8)
rr.log("world/points", rr.Points3D(points, colors=colors))

# Log a scalar over time
for t in range(100):
    rr.set_time_seconds("sim_time", t * 0.01)
    rr.log("metrics/torque", rr.Scalar(np.sin(t * 0.1)))
```

### Recording → replay

`rr.init("name", spawn=False)` + `rr.save("trace.rrd")` writes a recording you can re-open later (`rerun trace.rrd`), share with a teammate, or commit a small one to git as a regression test. Recordings query cleanly: there's a `rerun.dataframe` API for extracting clean per-frame DataFrames, which is useful for building training datasets straight from the visualisation traces.

### Useful entry points

- Site: <https://rerun.io/>
- Docs: <https://www.rerun.io/docs>
- Examples gallery: <https://www.rerun.io/examples>
- Source: <https://github.com/rerun-io/rerun>
