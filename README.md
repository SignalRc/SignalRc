# SignalRc
Signal RC starting point &amp; Discussions in Github

# Introduction
This document provides the official specification for Signal RC, version 1.0.0.
Please note that this release is currently under development and not yet deployed in production.

# What is Signal RC?

**Signal RC** is a modern, open data format designed for radio-controlled vehicle systems.
It is an Internet-friendly standard built on widely adopted web technologies such as JSON and WebSockets.
Signal RC is free and open-source software, and all source code is licensed under the Apache License, Version 2.0.
The project is developed openly with contributions from the RC community, and your ideas and feedback are always welcome.

Signal RC is engineered to integrate seamlessly with existing RC electronics and telemetry systems that may rely on serial, CAN-based, or proprietary communication protocols. It transforms and enhances telemetry, sensor data, and control information into a modern, web-friendly format that can be shared, processed, and visualized across web applications, mobile devices, and cloud services.

A typical Signal RC installation includes a gateway that converts native RC or sensor data into the Signal RC format, along with an optional Signal RC server. The server can offer additional capabilities such as data logging, analytics, cloud connectivity, and real-time monitoring.

# Data Model for Signal RC

The Signal RC Data Model defines a universal structure for telemetry and control information within radio-controlled vehicle systems. It is specified as a JSON schema. Refer to the Signal RC Data Model section for detailed  [documentation](doc/RcDataModel.md).

By defining a consistent data model in JSON, Signal RC simplifies the messaging layer and makes it easily extensible. Each data point in the model includes standardized units and metadata, ensuring that a specific value—such as vehicle speed or battery voltage—can always be found at a predictable location.

This approach also allows any compatible display, controller, or monitoring application to operate without prior knowledge of the full data model. A device implementing Signal RC can query the central Signal RC server to obtain all information required to interpret or display any data point. The metadata may include units of measurement, permitted ranges, alarm thresholds, and localized display names for every defined value in the system.

# Message Format

Signal RC messages use UTF-8 encoded JSON.
