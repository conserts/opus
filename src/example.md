<!--
SPDX-FileCopyrightText: 2022 Andreas Schmidt <andreas.schmidt@iese.fraunhofer.de>

SPDX-License-Identifier: CC-BY-SA-4.0
-->

# A Worked Example

This section of Opus describes a use case of ConSerts in detail, showing the different stages in safety engineering, the use of various engineering artifacts, as well as concrete details on the safety assurance for the concrete use case.
We use a *robotic bin-picking* scenario, where a human collaborates with the robot by providing or collecting boxes with material on the workbench.

The use case is provided by the [*KIT Intelligente Prozessautomation und Robotik (IAR-IPR)*](https://www.ipr.kit.edu/) and has been developed as part of the [*FabOS*](https://www.fab-os.org/) project.

Before we get started, we have to answer the question why we are not using traditional industrial safety engineering (e.g. static risk analysis, static safety functions, and fixed configurations).
With the continuous development towards bringing the Industry 4.0 idea to life, we see many applications that:

1. require close collaboration between humans and robots (which don't allow complete physical separation)
2. require regular changes in the setup (lot-size-one and other individualised production approaches)

With ConSerts, we are able to decouple components that form a concrete manufacturing system in an open and modular fashion.

This section is structured as follows:

* [System Description](./example/system.md), showing the details of the use case.

* [Preliminary Hazard- and Risk-Analysis](./example/safety_analysis.md), where we apply traditional, norm-driven safety engineering to identify relevant safety goals.

* [Assurance Case with GSN](./example/ac_gsn.md), where we argue how the collaborating systems can conceptually be operated safely.

* [Conditional Safety Certificates](./example/conserts.md) that contain the safety information that must be assessed at runtime.

* [Runtime Artifacts](./example/runtime.md) that can be used at run-time to compose and evaluate safety-relevant information.
