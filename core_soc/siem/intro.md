# Introduction to SIEM

## What Is SIEM?

SIEM stands for **Security Information and Event Management**.

A SIEM is a security platform that collects and analyzes event and log data from multiple sources. It helps security teams search activity, correlate related events, detect suspicious behavior, generate alerts, investigate incidents, and produce reports.

Examples of SIEM and security analytics platforms include:

- Microsoft Sentinel
- Splunk Enterprise Security
- Elastic Security
- IBM QRadar
- Google Security Operations

> **Note:** The Elastic Stack, previously referred to as the ELK Stack, provides technologies for collecting, storing, searching, and visualizing data. Elastic Security adds SIEM and security analytics capabilities to the stack.

## Common Categories of Security Logs

Security logs can be grouped into several categories. Two common categories are host-based and network-based logs.

- **Host-Based Logs:** Record activity occurring on or involving an endpoint, server, or other host. Examples include operating system events, authentication activity, process execution, file activity, and endpoint security alerts.
- **Network-Based Logs:** Record communications between systems or between a system and an external network. Sources include firewalls, routers, proxies, DNS servers, VPN gateways, and intrusion detection or prevention systems.

Other important sources can include:

- Cloud platforms
- Identity providers
- Email security systems
- Business applications
- Software-as-a-service applications
- Threat intelligence platforms

These sources can produce large volumes of data in different formats. A SIEM helps centralize and analyze this information.

## Core SIEM Capabilities

Common SIEM capabilities include:

- **Centralized Data Collection:** Collects security events and logs from multiple sources into a searchable platform.
- **Parsing and Normalization:** Extracts relevant fields and maps data to consistent schemas when supported.
- **Correlation and Detection:** Evaluates events across different sources to identify suspicious patterns or known threats.
- **Search and Investigation:** Allows analysts to query security data and examine related activity.
- **Dashboards and Reporting:** Presents security information through visualizations, metrics, and reports.
- **Alert and Incident Management:** Generates alerts and may group related alerts into incidents or cases.
- **Threat Intelligence Enrichment:** Compares activity with information about known malicious addresses, domains, files, or other indicators.
- **Data Retention:** Stores security data according to operational, investigative, and compliance requirements.

A SIEM provides centralized visibility, but it does not automatically determine with certainty whether every event is malicious or legitimate. Detection results still require appropriate context, validation, and analyst review.

## How Does a SIEM Ingest and Process Logs?

Log ingestion is the process of collecting event data from different sources and making it available for search, detection, investigation, and reporting.

The exact process varies by SIEM platform and data source.

### 1. Data Collection

Organizations select collection methods based on the source, platform architecture, security requirements, and available integrations.

Common collection methods include:

- **Agents and Forwarders:** Lightweight software installed on endpoints, servers, or other systems to collect and transmit data.
- **Syslog and Other Network Protocols:** Devices send events over supported protocols to a SIEM, collector, or relay.
- **APIs and Native Connectors:** The SIEM retrieves or receives data from cloud platforms, security products, and SaaS applications through supported integrations.
- **Webhooks and HTTP-Based Collection:** Applications send events to an HTTP endpoint or event collector.
- **Collectors and Relays:** Intermediate systems receive data from multiple local sources before forwarding it to the SIEM.
- **File or Object Storage Collection:** The SIEM reads data from log files, storage accounts, or cloud object storage.
- **Manual Upload:** Some platforms allow analysts to upload supported data files for investigation or analysis.

### 2. Parsing and Transformation

After the SIEM receives the data, it may need to extract and organize relevant information.

- **Parsing:** Extracts fields from raw or semi-structured events, such as timestamps, usernames, source IP addresses, destination IP addresses, ports, processes, and actions.
- **Data Transformation:** Converts values, timestamps, field types, or message formats into forms the platform can process.
- **Normalization:** Maps similar information from different sources to a common schema or consistent set of fields.
- **Filtering:** Removes unwanted events or fields when configured, helping control noise, storage, and ingestion costs.
- **Enrichment:** Adds useful context, such as asset information, identity details, geolocation, vulnerability information, or threat intelligence.

Normalization does not always occur during ingestion. Some platforms also support normalization when the data is queried.

For example:

- Microsoft Sentinel can use the Advanced Security Information Model (ASIM) to apply normalized schemas through parsers, commonly at query time.
- Elastic Security uses the Elastic Common Schema (ECS) to structure security data consistently.
- Splunk can use the Common Information Model (CIM) to map data from different sources to shared data models.

### 3. Storage and Indexing

After collection and processing, the data is stored according to the platform's architecture.

- **Centralized Storage:** Data may be stored in indexes, search clusters, data lakes, cloud storage, or other repositories.
- **Indexing:** The platform organizes data so that it can be searched and retrieved efficiently.
- **Retention:** Data is retained for a configured period based on investigation, compliance, operational, and cost requirements.
- **Data Tiers:** Some platforms separate frequently searched data from older or less frequently accessed data.

Not every SIEM stores all data in the same way, and not all collected data is immediately indexed.

### 4. Detection and Correlation

Once the data is available, the SIEM can analyze it using one or more methods.

- **Detection Rules:** Compare events with predefined or custom conditions.
- **Correlation Rules:** Identify relationships among events from one or more data sources.
- **Behavioral Analytics:** Identify activity that differs from an established baseline.
- **Anomaly Detection:** Uses statistical or machine learning methods to identify unusual patterns.
- **Threat Intelligence Matching:** Compares observed indicators with known threat information.

When configured conditions are met, the SIEM may generate an alert. Some platforms can also group related alerts into an incident or case.

### 5. Investigation and Response

Security analysts use the SIEM to:

- Review alerts and incidents
- Search related events
- Build timelines
- Examine affected users and assets
- Validate whether activity is expected or suspicious
- Record investigation findings
- Escalate confirmed incidents
- Initiate or support response actions

Some SIEM platforms integrate with Security Orchestration, Automation, and Response (SOAR) capabilities. These integrations can automate activities such as enrichment, notification, ticket creation, or containment. Available response capabilities depend on the platform and its integrations.

## Important Considerations

A SIEM's effectiveness depends on more than simply collecting logs. Organizations should also consider:

- Whether the required data sources are connected
- Whether logging is enabled and configured correctly
- Whether events contain the fields needed for detection
- Whether timestamps are accurate and consistently handled
- Whether parsing and normalization are working as expected
- Whether detection rules are relevant to the environment
- Whether alerts contain enough context for investigation
- Whether data retention meets operational and compliance needs
- Whether ingestion volume and storage costs are controlled
- Whether analysts regularly tune and validate detections

A SIEM improves security visibility and analysis, but incomplete telemetry, incorrect configurations, or poorly tuned detections can limit the conclusions analysts can draw from the available data.
