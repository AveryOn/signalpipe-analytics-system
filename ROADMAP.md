# SignalPipe Analytics System Roadmap

## Repository Foundation

### 001. Repository Structure

Create a repository structure for the ingestion API, event schemas, ClickHouse, batch processing, query API, metrics, logs, tracing, dashboards, tests, and documentation.

### 002. Runtime Configuration

Implement typed application configuration with environment variable validation at startup.

### 003. Environment Profiles

Support separate configurations for local, test, development, staging, and production environments.

### 004. Application Bootstrap

Create a unified application bootstrap with initialization of configuration, logging, ClickHouse, metrics, and graceful shutdown.

### 005. Health Endpoints

Implement endpoints for liveness, readiness, and checking availability of critical dependencies.

### 006. Error Model

Create a unified application error type with a code, message, HTTP status, and safe metadata.

### 007. Response Contract

Define a unified format for successful responses, errors, lists, and pagination metadata.

### 008. Development Environment

Prepare a local environment with the application, ClickHouse, Grafana, and required observability components.

### 009. Seed Data

Create a test event generator for local development and system demonstration.

### 010. System Documentation

Describe the system purpose, architecture, main data flows, and startup rules.

## Event Model

### 011. Base Event Schema

Define the base event structure with `eventId`, `eventName`, `timestamp`, `source`, `version`, and `properties`.

### 012. Event Metadata

Add standard metadata: `userId`, `tenantId`, `sessionId`, `requestId`, `traceId`, `environment`, and `service`.

### 013. Event Identity

Define rules for generating and validating a unique `eventId`.

### 014. Event Naming Convention

Record the event naming format and namespace usage rules.

### 015. Event Versioning

Implement event schema versioning.

### 016. Event Source Model

Define event source types: backend service, frontend application, worker, integration, and system process.

### 017. Event Category Model

Add event categories: business, product, technical, security, and audit.

### 018. Event Severity

Define event importance levels for technical and security scenarios.

### 019. Event Properties

Record allowed value types inside `properties`.

### 020. Nested Properties

Define rules for storing nested objects and arrays in events.

### 021. Event Size Limit

Add a maximum size limit for one event.

### 022. Event Timestamp Rules

Define rules for working with client timestamp, server timestamp, and ingestion timestamp.

### 023. Late Events

Define processing of events received significantly later than their occurrence time.

### 024. Future-Dated Events

Implement validation of events with a future timestamp.

### 025. Duplicate Events

Define rules for detecting and processing events with the same `eventId`.

### 026. Invalid Events

Define the storage or rejection format for invalid events.

### 027. Sensitive Properties

Create a list of prohibited or sensitive fields that cannot be stored in analytics events.

### 028. Property Sanitization

Implement removal or masking of sensitive values.

## Schema Registry

### 029. Event Schema Definition

Create a schema description format for each event type.

### 030. Schema Registry Storage

Implement storage for registered event schemas.

### 031. Schema Registration API

Create an API for registering a new event schema.

### 032. Schema Update API

Implement updating an existing schema with compatibility validation.

### 033. Schema Retrieval API

Create an API for retrieving a schema by event name and version.

### 034. Schema Listing API

Implement a list of all registered schemas.

### 035. Schema Status

Add draft, active, deprecated, and disabled statuses.

### 036. Schema Activation

Implement transition of a schema from draft to active.

### 037. Schema Deprecation

Implement schema deprecation without immediate deletion.

### 038. Backward Compatibility

Add backward compatibility validation between schema versions.

### 039. Breaking Change Detection

Detect removed required fields and incompatible type changes.

### 040. Schema Validation Service

Implement runtime validation of an event against a registered schema.

### 041. Unknown Event Policy

Define behavior when receiving an event of an unknown type.

### 042. Unknown Property Policy

Define whether additional properties absent from the schema are allowed.

### 043. Required Property Validation

Implement validation of required event properties.

### 044. Property Type Validation

Validate string, number, boolean, enum, array, object, and nullable values.

### 045. Schema Validation Errors

Create a structured validation error format.

### 046. Schema Cache

Add a cache of active event schemas.

### 047. Schema Cache Invalidation

Implement cache updates after a schema change.

### 048. Schema Audit Log

Store the history of schema registration, modification, and disabling.

## Event Ingestion API

### 049. Single Event Endpoint

Implement an endpoint for receiving one event.

### 050. Batch Events Endpoint

Implement an endpoint for receiving an array of events.

### 051. Ingestion Authentication

Add authentication for ingestion clients.

### 052. Project API Keys

Implement API keys bound to a specific project or event source.

### 053. API Key Hashing

Store API keys only as hashes.

### 054. API Key Scopes

Add scopes for sending events and managing schemas.

### 055. Request Validation

Validate the ingestion request structure before processing individual events.

### 056. Event Validation

Validate each event through the schema registry.

### 057. Partial Batch Acceptance

Support partial batch acceptance when some events are valid and others are rejected.

### 058. Atomic Batch Mode

Add a mode in which the entire batch is rejected if at least one error exists.

### 059. Ingestion Response

Return the number of accepted, rejected, and duplicate events.

### 060. Client Event Indexes

Return indexes of erroneous events inside the batch.

### 061. Request Size Limit

Limit the maximum ingestion request size.

### 062. Batch Size Limit

Limit the maximum number of events in one batch.

### 063. Content Encoding

Support JSON and compressed ingestion requests.

### 064. Request Decompression

Implement safe processing of gzip-compressed requests.

### 065. Ingestion Rate Limiting

Limit the number of requests and events for one API key.

### 066. Tenant Quotas

Add daily and monthly ingestion limits by tenant or project.

### 067. Idempotency

Support an idempotency key for resending a batch.

### 068. Request Correlation

Add `requestId` and `traceId` to the ingestion operation.

### 069. Server Timestamp

Add a server-side ingestion timestamp.

### 070. Source IP Metadata

Optionally store safe metadata about the request source.

### 071. User Agent Metadata

Store the client SDK and its version without recording unnecessary user data.

### 072. Ingestion Timeout

Define the maximum ingestion request processing time.

### 073. Backpressure Response

Return a correct response when the internal buffer or ClickHouse is overloaded.

### 074. Ingestion Metrics

Collect the number of requests, events, errors, latency, and payload size.

## Event Normalization

### 075. Event Normalizer

Create a unified event normalization pipeline before storage.

### 076. Field Normalization

Normalize empty values, timestamps, identifiers, and enum fields.

### 077. Event Enrichment

Add server-side metadata to the event.

### 078. Environment Enrichment

Add environment, service name, and deployment version.

### 079. Geo Enrichment Abstraction

Prepare an abstraction for optional country or region detection by IP.

### 080. Device Metadata Normalization

Normalize device, browser, and operating system metadata.

### 081. Tenant Resolution

Resolve tenant and project from request credentials.

### 082. Schema Version Resolution

Resolve the current schema version if the client did not provide it.

### 083. Property Flattening

Implement controlled transformation of nested properties for analytical queries.

### 084. Reserved Property Names

Prohibit user events from overriding system fields.

### 085. Property Count Limit

Limit the maximum number of properties in an event.

### 086. String Length Limit

Limit the length of string values.

### 087. Numeric Range Validation

Validate allowed ranges of numeric values.

### 088. Normalization Metrics

Collect the number of changed, removed, and rejected fields.


## Buffering and Batch Processing

### 089. In-Memory Event Buffer

Implement a temporary event buffer before batch insert.

### 090. Buffer Capacity

Add a limit on the maximum number of events and occupied memory.

### 091. Time-Based Flush

Flush the buffer at a specified interval.

### 092. Size-Based Flush

Flush when the specified batch size is reached.

### 093. Manual Flush

Add the ability to force a flush.

### 094. Batch Builder

Build a ClickHouse batch with limits on size and row count.

### 095. Concurrent Batch Inserts

Support a limited number of parallel insert operations.

### 096. Insert Queue

Create a batch insert queue.

### 097. Queue Capacity

Add a maximum queue size and overload policy.

### 098. Retryable Insert Errors

Define ClickHouse errors after which an insert can be retried.

### 099. Non-Retryable Insert Errors

Define errors for which the batch must be rejected or placed in dead letter storage.

### 100. Exponential Backoff

Implement retries with exponential backoff and jitter.

### 101. Batch Idempotency

Prevent duplicate insertion of one batch after an ambiguous failure.

### 102. Batch Identifiers

Assign a unique identifier to each batch.

### 103. Failed Batch Storage

Store failed batches for subsequent analysis and replay.

### 104. Batch Replay

Implement reprocessing of a failed batch.

### 105. Partial Insert Handling

Define behavior when an insert operation partially succeeds.

### 106. Buffer Shutdown Flush

Flush accumulated events during graceful shutdown.

### 107. Forced Shutdown Policy

Define behavior when the flush cannot be completed within the timeout.

### 108. Memory Pressure Handling

Slow ingestion or reject requests when memory usage grows.

### 109. Buffer Metrics

Collect buffer size, age of the oldest event, and number of flush operations.

### 110. Batch Metrics

Collect batch size, insert latency, retries, and failed batches.

## Optional Queue Integration

### 111. Queue Abstraction

Create an abstraction between the ingestion API and the storage worker.

### 112. Direct Insert Adapter

Implement a mode for direct transfer of events to the internal buffer.

### 113. Message Broker Adapter

Prepare an adapter for RabbitMQ, Kafka, or Redis Streams.

### 114. Event Producer

Implement publishing of normalized events to the message broker.

### 115. Event Consumer

Implement a consumer that reads events and builds ClickHouse batches.

### 116. Consumer Group

Support multiple consumer instances.

### 117. Delivery Semantics

Record the at-most-once, at-least-once, or effectively-once delivery model.

### 118. Consumer Offset Management

Implement acknowledgment of message processing after successful insertion.

### 119. Consumer Retries

Retry processing of temporarily unsuccessful messages.

### 120. Dead Letter Queue

Move unprocessable messages to a DLQ.

### 121. DLQ Replay

Implement controlled replay of messages from the DLQ.

### 122. Queue Lag Metrics

Measure lag, backlog, and consumer processing speed.

## ClickHouse Foundation

### 123. ClickHouse Client

Create a typed adapter for executing ClickHouse queries and inserts.

### 124. Connection Configuration

Configure hosts, credentials, database, timeout, and compression.

### 125. Connection Health Check

Check ClickHouse availability through the readiness endpoint.

### 126. ClickHouse Migrations

Create a system for applying versioned migrations.

### 127. Migration History

Store the history of executed migrations.

### 128. Migration Lock

Prevent simultaneous execution of migrations by multiple instances.

### 129. Event Table

Create the main table for storing analytics events.

### 130. Raw Event Table

Create a table for original or minimally transformed events.

### 131. Invalid Event Table

Create a table or separate storage for rejected events.

### 132. Engine Selection

Research and select an appropriate table engine for events.

### 133. ORDER BY Strategy

Define the sort key for typical query patterns.

### 134. PARTITION BY Strategy

Define event partitioning by time or tenant.

### 135. PRIMARY KEY Strategy

Define the primary key expression for accelerating analytical queries.

### 136. Data Types

Select appropriate ClickHouse types for identifiers, timestamps, enums, and properties.

### 137. LowCardinality Fields

Use `LowCardinality` for suitable string dimensions.

### 138. Nullable Fields

Define rules for using Nullable and default values.

### 139. JSON Properties

Research methods for storing dynamic event properties.

### 140. Typed Event Columns

Move frequently used analytical fields into separate typed columns.

### 141. Compression Codecs

Configure codecs for frequently used data types.

### 142. Insert Format

Select an efficient batch insert format.

### 143. Insert Compression

Use compression to reduce network traffic.

### 144. Deduplication Strategy

Define an event deduplication mechanism by `eventId`.

### 145. ReplacingMergeTree Study

Research the use of ReplacingMergeTree for deduplication.

### 146. MergeTree Behaviour

Research parts, merges, mutations, and their impact on performance.

### 147. TTL Rules

Configure automatic deletion or movement of old data.

### 148. Storage Policies

Define hot, warm, and cold storage policies.

### 149. Table Size Metrics

Collect row count, part count, compressed size, and uncompressed size.

### 150. Merge Metrics

Observe active merges, the mutation queue, and background operations.

## Aggregations and Materialized Views

### 151. Hourly Event Aggregation

Create a materialized view with event counts by hour.

### 152. Daily Event Aggregation

Create a daily event aggregation.

### 153. Event Name Aggregation

Aggregate events by name and time.

### 154. Tenant Aggregation

Create aggregates by tenant and project.

### 155. Unique Users Aggregation

Implement calculation of approximate and exact unique-user counts.

### 156. Session Aggregation

Create aggregates by user sessions.

### 157. Error Event Aggregation

Aggregate technical errors by service, code, and environment.

### 158. Latency Aggregation

Store latency percentiles for technical events.

### 159. Funnel Aggregation

Prepare a data structure for analyzing sequences of user events.

### 160. Retention Aggregation

Prepare data for retention analysis.

### 161. Materialized View Backfill

Implement filling materialized views for existing data.

### 162. Aggregation Rebuild

Describe and implement rebuilding aggregates after logic changes.

### 163. Aggregation Consistency

Check consistency between raw events and aggregate tables.

### 164. Aggregate Query Benchmark

Compare queries against the raw table and pre-aggregated tables.

## Event Query API

### 165. Event Search Endpoint

Implement an event search API.

### 166. Filter by Event Name

Add filtering by event name.

### 167. Filter by Time Range

Add a mandatory or limited search time window.

### 168. Filter by Tenant

Restrict queries to tenant scope.

### 169. Filter by User

Add search for events of a specific user.

### 170. Filter by Session

Add search for events within one session.

### 171. Filter by Source

Support filtering by service, application, or event source.

### 172. Property Filters

Implement filtering by event properties.

### 173. Multiple Filters

Support a combination of multiple filters.

### 174. Query Sorting

Add sorting by time and other allowed fields.

### 175. Cursor Pagination

Implement cursor pagination for large event sets.

### 176. Query Limits

Limit the maximum number of returned rows.

### 177. Query Timeout

Limit maximum query execution time.

### 178. Query Complexity Validation

Reject excessively expensive or broad analytics queries.

### 179. Parameterized Queries

Eliminate concatenation of user values inside SQL.

### 180. Query Builder

Create a safe builder for allowed ClickHouse queries.

### 181. Event Count Endpoint

Implement retrieval of the number of events for a period.

### 182. Time Series Endpoint

Return an event time series with a configurable interval.

### 183. Unique Users Endpoint

Return the number of unique users for a period.

### 184. Top Events Endpoint

Return the most frequently occurring events.

### 185. Property Breakdown Endpoint

Group events by a specified property.

### 186. Error Analytics Endpoint

Return technical errors by code, service, and environment.

### 187. Query Metadata

Return duration, scanned rows, and applied limits.

### 188. Query Metrics

Collect the number of queries, latency, errors, and scanned bytes.

## Analytics Query Features

### 189. Funnel Query

Implement analysis of users progressing through an event sequence.

### 190. Funnel Time Window

Limit the maximum time for completing the funnel.

### 191. Funnel Conversion

Calculate conversion between steps.

### 192. Retention Query

Implement cohort retention analysis.

### 193. Cohort Definition

Support cohort formation by the first event or property.

### 194. Sessionization

Implement grouping events into sessions by a time timeout.

### 195. User Journey Query

Return an ordered sequence of user events.

### 196. Event Property Distribution

Calculate the distribution of a numeric property.

### 197. Percentile Query

Calculate p50, p90, p95, and p99 for numeric values.

### 198. Cardinality Query

Estimate the number of unique property values.

### 199. Comparison Query

Compare two time periods.

### 200. Time Zone Support

Support transforming time series into a specified timezone.

### 201. Query Cache

Add a cache for frequently repeated analytics queries.

### 202. Query Cache Invalidation

Define TTL and query cache refresh rules.

### 203. Saved Queries

Implement saving predefined analytics queries.

### 204. Query Templates

Add parameterized query templates for dashboards.


## Metrics

### 205. Metrics Endpoint

Implement a Prometheus-compatible metrics endpoint.

### 206. HTTP Request Metrics

Collect request count, latency, status codes, and active requests.

### 207. Ingestion Metrics

Collect accepted, rejected, duplicate, and malformed events.

### 208. Validation Metrics

Collect errors by event name, schema version, and validation rule.

### 209. Buffer Metrics

Collect buffer size, flush rate, and oldest event age.

### 210. Batch Insert Metrics

Collect batch size, insert latency, retries, and failures.

### 211. ClickHouse Query Metrics

Collect query latency, scanned rows, scanned bytes, and errors.

### 212. Queue Metrics

Collect queue depth, lag, publish rate, and consume rate.

### 213. Runtime Metrics

Collect memory, CPU, event loop delay, and active handles.

### 214. Process Metrics

Collect uptime, restarts, and open file descriptors.

### 215. Business Metrics

Collect the number of active tenants, projects, and event volume.

### 216. Metric Naming Convention

Record metric and label naming rules.

### 217. Metric Label Policy

Limit high-cardinality labels.

### 218. Histogram Buckets

Configure buckets for HTTP, insert, and query latency.

### 219. Metrics Registry

Create a unified application metrics registry.

### 220. Custom Metrics API

Create an internal API for registering application metrics.

## Structured Logging

### 221. Structured Logger

Implement a JSON logger for all system components.

### 222. Log Levels

Define trace, debug, info, warn, error, and fatal levels.

### 223. Standard Log Fields

Add timestamp, level, service, environment, version, and hostname.

### 224. Request Context

Add requestId, traceId, tenantId, and API key identifier.

### 225. Event Context

Add eventId, eventName, and schemaVersion to relevant logs.

### 226. Batch Context

Add batchId, batchSize, and retry attempt.

### 227. ClickHouse Query Context

Add queryId, operation type, and duration.

### 228. Error Serialization

Store stack, error code, and cause without losing structure.

### 229. Sensitive Data Sanitization

Remove credentials, API keys, tokens, and prohibited event properties from logs.

### 230. Log Sampling

Add sampling for high-frequency non-error logs.

### 231. Slow Operation Logging

Log slow inserts, queries, and ingestion requests.

### 232. Audit Logs

Separate administrative actions from regular application logs.

### 233. Security Logs

Log authentication failures, quota violations, and suspicious requests.

### 234. Logger Failure Behaviour

Prevent the main process from crashing because of a logging transport error.

## Tracing

### 235. Trace Context Model

Define `traceId`, `spanId`, parent span, and trace flags.

### 236. HTTP Ingestion Span

Create a span for the ingestion request.

### 237. Validation Span

Create a span for schema lookup and event validation.

### 238. Normalization Span

Create a span for the normalization pipeline.

### 239. Buffer Span

Track adding an event to the buffer and waiting for flush.

### 240. Batch Insert Span

Create a span for a ClickHouse insert.

### 241. Query Span

Create a span for a ClickHouse analytics query.

### 242. Queue Producer Span

Propagate trace context when publishing an event to the broker.

### 243. Queue Consumer Span

Continue the trace when the consumer processes the message.

### 244. Trace Propagation

Propagate trace context through HTTP headers and message metadata.

### 245. Span Attributes

Add safe attributes: tenant, eventName, batchSize, and operation type.

### 246. Span Events

Record retries, validation failures, and overload events inside the span.

### 247. Trace Sampling

Configure head-based sampling.

### 248. Error Trace Sampling

Preserve traces of failed operations with a higher probability.

### 249. OpenTelemetry Integration

Integrate OpenTelemetry-compatible tracing.

### 250. Trace Exporter

Connect a local backend for viewing traces.

## Grafana Dashboards

### 251. System Overview Dashboard

Create a dashboard of the overall state of ingestion, storage, queries, and runtime.

### 252. Ingestion Dashboard

Show requests, accepted events, rejected events, throughput, and latency.

### 253. Schema Validation Dashboard

Show validation errors by event name, schema version, and reason.

### 254. Buffer Dashboard

Show buffer size, flush frequency, and oldest event age.

### 255. Batch Insert Dashboard

Show batch sizes, insert latency, retries, and failures.

### 256. ClickHouse Dashboard

Show query latency, parts, merges, storage size, and resource usage.

### 257. Query API Dashboard

Show query count, latency, timeout, and scanned bytes.

### 258. Queue Dashboard

Show backlog, lag, publish rate, consume rate, and DLQ size.

### 259. Runtime Dashboard

Show CPU, memory, event loop delay, and process restarts.

### 260. Error Dashboard

Show application errors, ClickHouse errors, and failed batches.

### 261. Tenant Usage Dashboard

Show ingestion volume and query usage by tenant.

### 262. Event Volume Dashboard

Show the most frequent events and volume changes over time.

### 263. SLO Dashboard

Show availability, ingestion latency, error rate, and storage delay.

### 264. Dashboard Variables

Add filters by environment, service, tenant, and event name.

### 265. Dashboard Provisioning

Store Grafana dashboards in the repository and load them automatically.

### 266. Dashboard Documentation

Describe the purpose of each dashboard and interpretation of panels.

## Alerts and SLO

### 267. Ingestion Availability SLI

Define an indicator of ingestion request success.

### 268. Ingestion Latency SLI

Measure the time from the HTTP request until the event is accepted by the system.

### 269. Storage Delay SLI

Measure the delay between ingestion timestamp and the event appearing in ClickHouse.

### 270. Query Availability SLI

Define analytics query success.

### 271. Query Latency SLI

Measure query API latency.

### 272. Ingestion SLO

Record target ingestion availability and latency.

### 273. Query SLO

Record target analytics query indicators.

### 274. Error Budget

Calculate the error budget for ingestion and the query API.

### 275. High Rejection Rate Alert

Create an alert for a sharp increase in invalid events.

### 276. Buffer Saturation Alert

Create an alert for buffer saturation.

### 277. Insert Failure Alert

Create an alert for repeated ClickHouse insert failures.

### 278. Storage Delay Alert

Create an alert for increased event delivery time to ClickHouse.

### 279. Queue Lag Alert

Create an alert for increased consumer lag.

### 280. DLQ Growth Alert

Create an alert for growth of the dead letter queue.

### 281. ClickHouse Parts Alert

Create an alert for an excessive number of active parts.

### 282. Query Latency Alert

Create an alert for growth in p95 and p99 query latency.

### 283. Memory Alert

Create an alert for sustained process memory growth.

### 284. Event Loop Delay Alert

Create an alert for blocking of the Node.js event loop.

### 285. Alert Severity

Define warning, critical, and informational severity.

### 286. Alert Runbook Links

Link each alert to an operational runbook.

## Security

### 287. Authentication Threat Model

Conduct threat analysis for ingestion and query authentication.

### 288. Tenant Isolation

Guarantee tenant scope in all ingestion and query operations.

### 289. Query Authorization

Check the user's right to access the analytics project.

### 290. Schema Administration Authorization

Restrict schema registration and modification with administrative permissions.

### 291. API Key Rotation

Implement secure replacement of an ingestion API key.

### 292. API Key Revocation

Implement immediate revocation of a compromised key.

### 293. Request Replay Protection

Limit repeated use of signed or timestamped requests.

### 294. Payload Abuse Protection

Protect the ingestion API from excessive nesting, huge strings, and compression bombs.

### 295. Query Injection Protection

Prevent arbitrary ClickHouse SQL from being passed through the public API.

### 296. Sensitive Event Detection

Reject events containing passwords, tokens, card data, and other prohibited fields.

### 297. Data Encryption in Transit

Use TLS for API, ClickHouse, and observability connections.

### 298. Data Retention Security

Delete data after the permitted retention period ends.

### 299. Audit Trail

Record management of schemas, keys, quotas, and saved queries.

### 300. Security Incident Events

Create special events for security monitoring.

## Reliability

### 301. Graceful Shutdown

Correctly stop the HTTP server, buffer, workers, metrics, and ClickHouse connections.

### 302. Dependency Startup Order

Start the application only after critical dependencies are initialized.

### 303. ClickHouse Outage Handling

Define ingestion behavior during temporary ClickHouse unavailability.

### 304. Queue Outage Handling

Define producer and consumer behavior when the broker is unavailable.

### 305. Grafana Outage Independence

Guarantee that Grafana unavailability does not affect ingestion.

### 306. Metrics Export Failure

Prevent primary processing from stopping because of metrics backend problems.

### 307. Retry Budget

Limit the total number of retries and their impact on the system.

### 308. Circuit Breaker

Add a circuit breaker for ClickHouse and external observability exporters.

### 309. Bulkhead Isolation

Separate resources for ingestion, batch insertion, and the query API.

### 310. Load Shedding

Reject part of the requests under overload instead of allowing uncontrolled memory growth.

### 311. Read-Only Degraded Mode

Support the query API while ingestion is temporarily stopped.

### 312. Ingestion-Only Degraded Mode

Support event accumulation while the query subsystem is temporarily unavailable.

### 313. Poison Event Handling

Isolate an event that continuously causes a processing error.

### 314. Failed Batch Recovery

Create a procedure for recovering failed batches.

### 315. Data Consistency Check

Compare the number of accepted, published, and stored events.

### 316. Duplicate Rate Monitoring

Track the proportion of repeatedly received events.

### 317. Clock Skew Monitoring

Track the difference between client timestamp and server timestamp.

### 318. Disaster Recovery

Describe restoration of ClickHouse, schemas, and configuration after environment loss.


## Performance Testing

### 319. Ingestion Benchmark

Measure single-event ingestion throughput.

### 320. Batch Ingestion Benchmark

Measure throughput with different batch sizes.

### 321. Validation Benchmark

Compare the cost of validating simple and complex schemas.

### 322. Normalization Benchmark

Measure the cost of enrichment and property normalization.

### 323. Buffer Benchmark

Check the impact of buffer size and flush interval.

### 324. Insert Batch Size Benchmark

Find an efficient ClickHouse batch size.

### 325. Concurrent Insert Benchmark

Determine the optimal number of parallel inserts.

### 326. Compression Benchmark

Compare ingestion and inserts with and without compression.

### 327. Query Benchmark

Measure latency of the main analytics queries.

### 328. Raw vs Aggregate Benchmark

Compare queries against raw and aggregate tables.

### 329. High Cardinality Benchmark

Research the impact of high-cardinality properties.

### 330. Wide Event Benchmark

Research events with a large number of properties.

### 331. Large Payload Benchmark

Check processing of events close to the maximum size.

### 332. Sustained Load Test

Conduct a long-running test of stable ingestion load.

### 333. Spike Test

Check the system during a sharp short-term increase in events.

### 334. Stress Test

Find throughput limits and behavior after saturation.

### 335. Recovery Test

Check system recovery after overload ends.

### 336. Performance Report

Record results, bottlenecks, and recommended limits.

## Testing

### 337. Event Schema Unit Tests

Check base schemas and event model rules.

### 338. Validation Unit Tests

Check required fields, types, enums, and nested properties.

### 339. Normalization Unit Tests

Check transformation of timestamps, metadata, and properties.

### 340. Sanitization Unit Tests

Check removal of sensitive data.

### 341. Batch Builder Unit Tests

Check batch formation by size and row count.

### 342. Retry Unit Tests

Check backoff, jitter, and retry limits.

### 343. Query Builder Unit Tests

Check generation of allowed queries and filters.

### 344. Tenant Isolation Tests

Check impossibility of access to another tenant's data.

### 345. Ingestion Integration Tests

Check the complete flow from HTTP request to buffer.

### 346. ClickHouse Integration Tests

Check migrations, inserts, and queries on a real ClickHouse instance.

### 347. Batch Insert Integration Tests

Check flush, retries, and failed batch storage.

### 348. Schema Registry Integration Tests

Check registration, versioning, and cache invalidation.

### 349. Queue Integration Tests

Check producer, consumer, acknowledgment, and DLQ.

### 350. Query API Integration Tests

Check filters, pagination, grouping, and time series.

### 351. Metrics Tests

Check registration and updating of metrics.

### 352. Logging Tests

Check structured fields and sanitization.

### 353. Tracing Tests

Check trace propagation between ingestion, queue, and storage.

### 354. Failure Injection Tests

Simulate ClickHouse outage, timeout, and connection reset.

### 355. Duplicate Delivery Tests

Check behavior during repeated event delivery.

### 356. Graceful Shutdown Tests

Check buffer flush and completion of active operations.

## Operations

### 357. Application Runbook

Create instructions for starting, stopping, and checking the application.

### 358. ClickHouse Runbook

Describe health, storage, parts, merges, and query load checks.

### 359. High Rejection Runbook

Describe investigation of growth in validation errors.

### 360. Buffer Saturation Runbook

Describe diagnostics of internal buffer saturation.

### 361. Insert Failure Runbook

Describe actions when data cannot be written to ClickHouse.

### 362. Queue Lag Runbook

Describe diagnostics of consumer lag.

### 363. DLQ Runbook

Describe viewing, correcting, and replaying dead letter events.

### 364. Slow Query Runbook

Describe finding and optimizing slow ClickHouse queries.

### 365. Storage Growth Runbook

Describe monitoring storage usage and changing retention.

### 366. Data Replay Runbook

Describe safe reloading of events.

### 367. Schema Rollback Runbook

Describe recovery after an incorrect schema update.

### 368. API Key Incident Runbook

Describe revocation and rotation of a compromised key.

### 369. Backup Procedure

Create a procedure for backing up schemas and critical metadata.

### 370. Restore Procedure

Check system restoration from backup.

## Final System Tasks

### 371. Complete Ingestion Pipeline

Build the complete flow: API request, authentication, validation, normalization, buffering, and ClickHouse insert.

### 372. Complete Queued Pipeline

Build the flow through a broker: ingestion producer, consumer, batch insert, retries, and DLQ.

### 373. Complete Query Platform

Build the query API with filters, time series, aggregations, pagination, and tenant isolation.

### 374. Complete Observability Stack

Connect metrics, structured logs, traces, dashboards, and alerts.

### 375. Production Dashboard Package

Prepare a complete set of Grafana dashboards for ingestion, ClickHouse, queries, and runtime.

### 376. Production Alert Package

Prepare alerts with severity, description, and runbook links.

### 377. Event SDK

Create a small TypeScript SDK for sending single and batch events.

### 378. SDK Retry Policy

Add retries, timeout, and idempotency to the client SDK.

### 379. SDK Buffering

Add a local batch buffer and periodic flush to the SDK.

### 380. SDK Shutdown Flush

Ensure accumulated events are sent when the application terminates.

### 381. Demo Application

Create an application that generates business and technical events.

### 382. Demo Analytics Scenario

Show ingestion, queries, a dashboard, metrics, logs, and a trace of one end-to-end flow.

### 383. Simulated Production Incident

Simulate increased ingestion latency or ClickHouse insert failures.

### 384. Incident Investigation

Conduct an investigation through metrics, logs, traces, and ClickHouse system tables.

### 385. Incident Postmortem

Document impact, timeline, root cause, resolution, and preventive actions.

### 386. Architecture Documentation

Describe components, data flows, storage model, reliability, and security boundaries.

### 387. Performance Report

Record throughput limits, recommended batch sizes, and query limits.

### 388. Security Review

Check authentication, tenant isolation, payload sanitization, and query safety.

### 389. Operational Readiness Review

Check health checks, dashboards, alerts, runbooks, backups, and graceful shutdown.

### 390. Final Analytics Case Study

Create a complete description of a production-style analytics system: ingestion, schemas, buffering, ClickHouse, querying, observability, failures, performance, and operations.
