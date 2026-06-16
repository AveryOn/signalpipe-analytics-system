# SignalPipe Analytics System Roadmap

## Repository Foundation

### 001. Repository Structure

Создать структуру репозитория для ingestion API, event schemas, ClickHouse, batch processing, query API, metrics, logs, tracing, dashboards, tests и документации.

### 002. Runtime Configuration

Реализовать типизированную конфигурацию приложения с проверкой environment variables при запуске.

### 003. Environment Profiles

Поддержать отдельные конфигурации для local, test, development, staging и production environments.

### 004. Application Bootstrap

Создать единый bootstrap приложения с инициализацией конфигурации, логирования, ClickHouse, metrics и graceful shutdown.

### 005. Health Endpoints

Реализовать endpoints для liveness, readiness и проверки доступности критических зависимостей.

### 006. Error Model

Создать единый тип ошибок приложения с кодом, сообщением, HTTP status и безопасной metadata.

### 007. Response Contract

Определить единый формат успешных ответов, ошибок, списков и pagination metadata.

### 008. Development Environment

Подготовить локальное окружение с приложением, ClickHouse, Grafana и необходимыми observability-компонентами.

### 009. Seed Data

Создать генератор тестовых событий для локальной разработки и демонстрации системы.

### 010. System Documentation

Описать назначение системы, архитектуру, основные потоки данных и правила запуска.

## Event Model

### 011. Base Event Schema

Определить базовую структуру события с `eventId`, `eventName`, `timestamp`, `source`, `version` и `properties`.

### 012. Event Metadata

Добавить стандартную metadata: `userId`, `tenantId`, `sessionId`, `requestId`, `traceId`, `environment` и `service`.

### 013. Event Identity

Определить правила генерации и валидации уникального `eventId`.

### 014. Event Naming Convention

Зафиксировать формат именования событий и правила использования namespace.

### 015. Event Versioning

Реализовать версионирование event schemas.

### 016. Event Source Model

Определить типы источников событий: backend service, frontend application, worker, integration и system process.

### 017. Event Category Model

Добавить категории событий: business, product, technical, security и audit.

### 018. Event Severity

Определить уровни важности событий для технических и security-сценариев.

### 019. Event Properties

Зафиксировать допустимые типы значений внутри `properties`.

### 020. Nested Properties

Определить правила хранения вложенных объектов и массивов в событиях.

### 021. Event Size Limit

Добавить ограничение максимального размера одного события.

### 022. Event Timestamp Rules

Определить правила работы с client timestamp, server timestamp и ingestion timestamp.

### 023. Late Events

Определить обработку событий, поступивших значительно позже времени их возникновения.

### 024. Future-Dated Events

Реализовать проверку событий с timestamp из будущего.

### 025. Duplicate Events

Определить правила обнаружения и обработки событий с одинаковым `eventId`.

### 026. Invalid Events

Определить формат хранения или отклонения невалидных событий.

### 027. Sensitive Properties

Создать список запрещённых или чувствительных полей, которые нельзя сохранять в analytics events.

### 028. Property Sanitization

Реализовать удаление или маскирование чувствительных значений.

## Schema Registry

### 029. Event Schema Definition

Создать формат описания схемы для каждого типа события.

### 030. Schema Registry Storage

Реализовать хранилище зарегистрированных event schemas.

### 031. Schema Registration API

Создать API регистрации новой event schema.

### 032. Schema Update API

Реализовать обновление существующей схемы с проверкой совместимости.

### 033. Schema Retrieval API

Создать API получения схемы по имени события и версии.

### 034. Schema Listing API

Реализовать список всех зарегистрированных schemas.

### 035. Schema Status

Добавить статусы draft, active, deprecated и disabled.

### 036. Schema Activation

Реализовать перевод схемы из draft в active.

### 037. Schema Deprecation

Реализовать устаревание схемы без немедленного удаления.

### 038. Backward Compatibility

Добавить проверку backward compatibility между версиями схемы.

### 039. Breaking Change Detection

Определять удалённые обязательные поля и несовместимые изменения типов.

### 040. Schema Validation Service

Реализовать runtime-валидацию события по зарегистрированной схеме.

### 041. Unknown Event Policy

Определить поведение при получении события неизвестного типа.

### 042. Unknown Property Policy

Определить разрешены ли дополнительные свойства, отсутствующие в схеме.

### 043. Required Property Validation

Реализовать проверку обязательных свойств события.

### 044. Property Type Validation

Проверять string, number, boolean, enum, array, object и nullable значения.

### 045. Schema Validation Errors

Создать структурированный формат ошибок валидации.

### 046. Schema Cache

Добавить cache активных event schemas.

### 047. Schema Cache Invalidation

Реализовать обновление cache после изменения схемы.

### 048. Schema Audit Log

Сохранять историю регистрации, изменения и отключения schemas.

## Event Ingestion API

### 049. Single Event Endpoint

Реализовать endpoint приёма одного события.

### 050. Batch Events Endpoint

Реализовать endpoint приёма массива событий.

### 051. Ingestion Authentication

Добавить authentication для ingestion clients.

### 052. Project API Keys

Реализовать API keys, привязанные к конкретному проекту или источнику событий.

### 053. API Key Hashing

Хранить API keys только в виде hash.

### 054. API Key Scopes

Добавить scopes для отправки событий и управления schemas.

### 055. Request Validation

Проверять структуру ingestion request до обработки отдельных событий.

### 056. Event Validation

Валидировать каждое событие через schema registry.

### 057. Partial Batch Acceptance

Поддержать частичное принятие batch, когда часть событий валидна, а часть отклонена.

### 058. Atomic Batch Mode

Добавить режим, в котором весь batch отклоняется при наличии хотя бы одной ошибки.

### 059. Ingestion Response

Возвращать количество принятых, отклонённых и дублирующихся событий.

### 060. Client Event Indexes

Возвращать индексы ошибочных событий внутри batch.

### 061. Request Size Limit

Ограничить максимальный размер ingestion request.

### 062. Batch Size Limit

Ограничить максимальное количество событий в одном batch.

### 063. Content Encoding

Поддержать JSON и сжатые ingestion requests.

### 064. Request Decompression

Реализовать безопасную обработку gzip-сжатых запросов.

### 065. Ingestion Rate Limiting

Ограничить количество запросов и событий для одного API key.

### 066. Tenant Quotas

Добавить дневные и месячные лимиты ingestion по tenant или project.

### 067. Idempotency

Поддержать idempotency key для повторной отправки batch.

### 068. Request Correlation

Добавлять `requestId` и `traceId` к ingestion operation.

### 069. Server Timestamp

Добавлять server-side ingestion timestamp.

### 070. Source IP Metadata

Опционально сохранять безопасную metadata об источнике запроса.

### 071. User Agent Metadata

Сохранять client SDK и его версию без записи лишних пользовательских данных.

### 072. Ingestion Timeout

Определить максимальное время обработки ingestion request.

### 073. Backpressure Response

Возвращать корректный ответ при перегрузке внутреннего buffer или ClickHouse.

### 074. Ingestion Metrics

Собирать количество запросов, событий, ошибок, latency и payload size.

## Event Normalization

### 075. Event Normalizer

Создать единый pipeline нормализации событий перед сохранением.

### 076. Field Normalization

Нормализовать пустые значения, timestamp, identifiers и enum fields.

### 077. Event Enrichment

Добавлять server-side metadata к событию.

### 078. Environment Enrichment

Добавлять environment, service name и deployment version.

### 079. Geo Enrichment Abstraction

Подготовить abstraction для необязательного определения страны или региона по IP.

### 080. Device Metadata Normalization

Нормализовать device, browser и operating system metadata.

### 081. Tenant Resolution

Определять tenant и project по credentials запроса.

### 082. Schema Version Resolution

Определять актуальную версию схемы, если клиент её не передал.

### 083. Property Flattening

Реализовать контролируемое преобразование вложенных properties для аналитических запросов.

### 084. Reserved Property Names

Запретить пользовательским событиям переопределять системные поля.

### 085. Property Count Limit

Ограничить максимальное количество properties в событии.

### 086. String Length Limit

Ограничить длину строковых значений.

### 087. Numeric Range Validation

Проверять допустимые диапазоны числовых значений.

### 088. Normalization Metrics

Собирать количество изменённых, удалённых и отклонённых полей.

## Buffering and Batch Processing

### 089. In-Memory Event Buffer

Реализовать временный buffer событий перед batch insert.

### 090. Buffer Capacity

Добавить ограничение максимального количества событий и занимаемой памяти.

### 091. Time-Based Flush

Выполнять flush buffer через заданный интервал.

### 092. Size-Based Flush

Выполнять flush при достижении заданного размера batch.

### 093. Manual Flush

Добавить возможность принудительного flush.

### 094. Batch Builder

Формировать batch для ClickHouse с ограничением по размеру и количеству строк.

### 095. Concurrent Batch Inserts

Поддержать ограниченное количество параллельных insert operations.

### 096. Insert Queue

Создать очередь batch inserts.

### 097. Queue Capacity

Добавить максимальный размер очереди и политику перегрузки.

### 098. Retryable Insert Errors

Определить ошибки ClickHouse, после которых insert можно повторить.

### 099. Non-Retryable Insert Errors

Определить ошибки, при которых batch должен быть отклонён или помещён в dead letter storage.

### 100. Exponential Backoff

Реализовать retries с exponential backoff и jitter.

### 101. Batch Idempotency

Предотвращать повторную вставку одного batch после ambiguous failure.

### 102. Batch Identifiers

Назначать уникальный identifier каждому batch.

### 103. Failed Batch Storage

Сохранять failed batches для последующего анализа и replay.

### 104. Batch Replay

Реализовать повторную обработку failed batch.

### 105. Partial Insert Handling

Определить поведение при частичном успехе insert operation.

### 106. Buffer Shutdown Flush

Выполнять flush накопленных событий при graceful shutdown.

### 107. Forced Shutdown Policy

Определить поведение при невозможности завершить flush в пределах timeout.

### 108. Memory Pressure Handling

Замедлять ingestion или отклонять запросы при росте memory usage.

### 109. Buffer Metrics

Собирать размер buffer, возраст старейшего события и количество flush operations.

### 110. Batch Metrics

Собирать размер batch, insert latency, retries и failed batches.

## Optional Queue Integration

### 111. Queue Abstraction

Создать abstraction между ingestion API и storage worker.

### 112. Direct Insert Adapter

Реализовать режим прямой передачи событий во внутренний buffer.

### 113. Message Broker Adapter

Подготовить adapter для RabbitMQ, Kafka или Redis Streams.

### 114. Event Producer

Реализовать публикацию нормализованных событий в message broker.

### 115. Event Consumer

Реализовать consumer, читающий события и формирующий ClickHouse batches.

### 116. Consumer Group

Поддержать несколько consumer instances.

### 117. Delivery Semantics

Зафиксировать at-most-once, at-least-once или effectively-once модель доставки.

### 118. Consumer Offset Management

Реализовать подтверждение обработки сообщений после успешной вставки.

### 119. Consumer Retries

Повторять обработку временно неуспешных сообщений.

### 120. Dead Letter Queue

Перемещать необрабатываемые сообщения в DLQ.

### 121. DLQ Replay

Реализовать контролируемый replay сообщений из DLQ.

### 122. Queue Lag Metrics

Измерять lag, backlog и скорость обработки consumer.

## ClickHouse Foundation

### 123. ClickHouse Client

Создать типизированный adapter для выполнения ClickHouse queries и inserts.

### 124. Connection Configuration

Настроить hosts, credentials, database, timeout и compression.

### 125. Connection Health Check

Проверять доступность ClickHouse через readiness endpoint.

### 126. ClickHouse Migrations

Создать систему применения версионированных migrations.

### 127. Migration History

Хранить историю выполненных migrations.

### 128. Migration Lock

Предотвращать одновременное выполнение migrations несколькими instances.

### 129. Event Table

Создать основную таблицу хранения analytics events.

### 130. Raw Event Table

Создать таблицу для исходных или минимально преобразованных событий.

### 131. Invalid Event Table

Создать таблицу или отдельное хранилище для отклонённых событий.

### 132. Engine Selection

Исследовать и выбрать подходящий table engine для events.

### 133. ORDER BY Strategy

Определить ключ сортировки для типовых query patterns.

### 134. PARTITION BY Strategy

Определить partitioning событий по времени или tenant.

### 135. PRIMARY KEY Strategy

Определить primary key expression для ускорения аналитических запросов.

### 136. Data Types

Выбрать подходящие ClickHouse types для identifiers, timestamps, enums и properties.

### 137. LowCardinality Fields

Использовать `LowCardinality` для подходящих строковых измерений.

### 138. Nullable Fields

Определить правила использования Nullable и default values.

### 139. JSON Properties

Исследовать способы хранения динамических event properties.

### 140. Typed Event Columns

Выделить часто используемые аналитические поля в отдельные typed columns.

### 141. Compression Codecs

Настроить codecs для часто используемых типов данных.

### 142. Insert Format

Выбрать эффективный формат batch inserts.

### 143. Insert Compression

Использовать compression для уменьшения сетевого трафика.

### 144. Deduplication Strategy

Определить механизм дедупликации событий по `eventId`.

### 145. ReplacingMergeTree Study

Исследовать применение ReplacingMergeTree для дедупликации.

### 146. MergeTree Behaviour

Исследовать parts, merges, mutations и их влияние на производительность.

### 147. TTL Rules

Настроить автоматическое удаление или перемещение старых данных.

### 148. Storage Policies

Определить hot, warm и cold storage policies.

### 149. Table Size Metrics

Собирать количество rows, parts, compressed size и uncompressed size.

### 150. Merge Metrics

Наблюдать активные merges, mutation queue и background operations.

## Aggregations and Materialized Views

### 151. Hourly Event Aggregation

Создать materialized view с количеством событий по часу.

### 152. Daily Event Aggregation

Создать дневную агрегацию событий.

### 153. Event Name Aggregation

Агрегировать события по имени и времени.

### 154. Tenant Aggregation

Создать агрегаты по tenant и project.

### 155. Unique Users Aggregation

Реализовать расчёт приблизительного и точного количества уникальных пользователей.

### 156. Session Aggregation

Создать агрегаты по пользовательским сессиям.

### 157. Error Event Aggregation

Агрегировать технические ошибки по service, code и environment.

### 158. Latency Aggregation

Хранить percentiles latency для технических событий.

### 159. Funnel Aggregation

Подготовить структуру данных для анализа последовательности пользовательских событий.

### 160. Retention Aggregation

Подготовить данные для retention analysis.

### 161. Materialized View Backfill

Реализовать заполнение materialized views для существующих данных.

### 162. Aggregation Rebuild

Описать и реализовать пересоздание агрегатов после изменения логики.

### 163. Aggregation Consistency

Проверять согласованность raw events и aggregate tables.

### 164. Aggregate Query Benchmark

Сравнить запросы по raw table и pre-aggregated tables.

## Event Query API

### 165. Event Search Endpoint

Реализовать API поиска событий.

### 166. Filter by Event Name

Добавить фильтрацию по имени события.

### 167. Filter by Time Range

Добавить обязательное или ограниченное временное окно поиска.

### 168. Filter by Tenant

Ограничивать запросы tenant scope.

### 169. Filter by User

Добавить поиск событий конкретного пользователя.

### 170. Filter by Session

Добавить поиск событий внутри одной сессии.

### 171. Filter by Source

Поддержать фильтрацию по service, application или event source.

### 172. Property Filters

Реализовать фильтрацию по event properties.

### 173. Multiple Filters

Поддержать комбинацию нескольких фильтров.

### 174. Query Sorting

Добавить сортировку по времени и другим разрешённым полям.

### 175. Cursor Pagination

Реализовать cursor pagination для больших наборов событий.

### 176. Query Limits

Ограничить максимальное количество возвращаемых строк.

### 177. Query Timeout

Ограничить максимальное время выполнения запроса.

### 178. Query Complexity Validation

Отклонять чрезмерно дорогие или широкие analytics queries.

### 179. Parameterized Queries

Исключить конкатенацию пользовательских значений внутри SQL.

### 180. Query Builder

Создать безопасный builder разрешённых ClickHouse queries.

### 181. Event Count Endpoint

Реализовать получение количества событий за период.

### 182. Time Series Endpoint

Возвращать временной ряд событий с настраиваемым interval.

### 183. Unique Users Endpoint

Возвращать количество уникальных пользователей за период.

### 184. Top Events Endpoint

Возвращать наиболее часто встречающиеся события.

### 185. Property Breakdown Endpoint

Группировать события по указанному property.

### 186. Error Analytics Endpoint

Возвращать технические ошибки по code, service и environment.

### 187. Query Metadata

Возвращать duration, scanned rows и applied limits.

### 188. Query Metrics

Собирать количество запросов, latency, errors и scanned bytes.

## Analytics Query Features

### 189. Funnel Query

Реализовать анализ прохождения пользователей через последовательность событий.

### 190. Funnel Time Window

Ограничить максимальное время прохождения funnel.

### 191. Funnel Conversion

Рассчитывать conversion между шагами.

### 192. Retention Query

Реализовать cohort retention analysis.

### 193. Cohort Definition

Поддержать формирование cohort по первому событию или property.

### 194. Sessionization

Реализовать объединение событий в сессии по временному timeout.

### 195. User Journey Query

Возвращать упорядоченную последовательность событий пользователя.

### 196. Event Property Distribution

Рассчитывать распределение числового property.

### 197. Percentile Query

Рассчитывать p50, p90, p95 и p99 для числовых значений.

### 198. Cardinality Query

Оценивать количество уникальных значений property.

### 199. Comparison Query

Сравнивать два временных периода.

### 200. Time Zone Support

Поддержать преобразование временных рядов в заданную timezone.

### 201. Query Cache

Добавить cache для часто повторяющихся analytics queries.

### 202. Query Cache Invalidation

Определить TTL и правила обновления query cache.

### 203. Saved Queries

Реализовать сохранение заранее подготовленных analytics queries.

### 204. Query Templates

Добавить параметризованные query templates для dashboards.

## Metrics

### 205. Metrics Endpoint

Реализовать Prometheus-compatible metrics endpoint.

### 206. HTTP Request Metrics

Собирать request count, latency, status codes и active requests.

### 207. Ingestion Metrics

Собирать accepted, rejected, duplicate и malformed events.

### 208. Validation Metrics

Собирать ошибки по event name, schema version и validation rule.

### 209. Buffer Metrics

Собирать buffer size, flush rate и oldest event age.

### 210. Batch Insert Metrics

Собирать batch size, insert latency, retries и failures.

### 211. ClickHouse Query Metrics

Собирать query latency, scanned rows, scanned bytes и errors.

### 212. Queue Metrics

Собирать queue depth, lag, publish rate и consume rate.

### 213. Runtime Metrics

Собирать memory, CPU, event loop delay и active handles.

### 214. Process Metrics

Собирать uptime, restarts и open file descriptors.

### 215. Business Metrics

Собирать количество активных tenants, projects и event volume.

### 216. Metric Naming Convention

Зафиксировать правила именования metrics и labels.

### 217. Metric Label Policy

Ограничить high-cardinality labels.

### 218. Histogram Buckets

Настроить buckets для HTTP, insert и query latency.

### 219. Metrics Registry

Создать единый registry metrics приложения.

### 220. Custom Metrics API

Создать внутренний API регистрации application metrics.

## Structured Logging

### 221. Structured Logger

Реализовать JSON logger для всех компонентов системы.

### 222. Log Levels

Определить trace, debug, info, warn, error и fatal уровни.

### 223. Standard Log Fields

Добавлять timestamp, level, service, environment, version и hostname.

### 224. Request Context

Добавлять requestId, traceId, tenantId и API key identifier.

### 225. Event Context

Добавлять eventId, eventName и schemaVersion к релевантным logs.

### 226. Batch Context

Добавлять batchId, batchSize и retry attempt.

### 227. ClickHouse Query Context

Добавлять queryId, operation type и duration.

### 228. Error Serialization

Сохранять stack, error code и cause без потери структуры.

### 229. Sensitive Data Sanitization

Удалять credentials, API keys, tokens и запрещённые event properties из logs.

### 230. Log Sampling

Добавить sampling для высокочастотных неошибочных logs.

### 231. Slow Operation Logging

Логировать медленные inserts, queries и ingestion requests.

### 232. Audit Logs

Отделить административные действия от обычных application logs.

### 233. Security Logs

Логировать authentication failures, quota violations и подозрительные запросы.

### 234. Logger Failure Behaviour

Не допускать падения основного процесса из-за ошибки logging transport.

## Tracing

### 235. Trace Context Model

Определить `traceId`, `spanId`, parent span и trace flags.

### 236. HTTP Ingestion Span

Создавать span для ingestion request.

### 237. Validation Span

Создавать span для schema lookup и event validation.

### 238. Normalization Span

Создавать span для normalization pipeline.

### 239. Buffer Span

Отслеживать добавление события в buffer и ожидание flush.

### 240. Batch Insert Span

Создавать span для ClickHouse insert.

### 241. Query Span

Создавать span для ClickHouse analytics query.

### 242. Queue Producer Span

Передавать trace context при публикации события в broker.

### 243. Queue Consumer Span

Продолжать trace при обработке сообщения consumer.

### 244. Trace Propagation

Передавать trace context через HTTP headers и message metadata.

### 245. Span Attributes

Добавлять безопасные attributes: tenant, eventName, batchSize и operation type.

### 246. Span Events

Записывать retries, validation failures и overload events внутри span.

### 247. Trace Sampling

Настроить head-based sampling.

### 248. Error Trace Sampling

Сохранять traces ошибочных операций с более высокой вероятностью.

### 249. OpenTelemetry Integration

Интегрировать OpenTelemetry-compatible tracing.

### 250. Trace Exporter

Подключить локальный backend для просмотра traces.

## Grafana Dashboards

### 251. System Overview Dashboard

Создать dashboard общего состояния ingestion, storage, queries и runtime.

### 252. Ingestion Dashboard

Показать requests, accepted events, rejected events, throughput и latency.

### 253. Schema Validation Dashboard

Показать validation errors по event name, schema version и причине.

### 254. Buffer Dashboard

Показать buffer size, flush frequency и oldest event age.

### 255. Batch Insert Dashboard

Показать batch sizes, insert latency, retries и failures.

### 256. ClickHouse Dashboard

Показать query latency, parts, merges, storage size и resource usage.

### 257. Query API Dashboard

Показать query count, latency, timeout и scanned bytes.

### 258. Queue Dashboard

Показать backlog, lag, publish rate, consume rate и DLQ size.

### 259. Runtime Dashboard

Показать CPU, memory, event loop delay и process restarts.

### 260. Error Dashboard

Показать application errors, ClickHouse errors и failed batches.

### 261. Tenant Usage Dashboard

Показать ingestion volume и query usage по tenant.

### 262. Event Volume Dashboard

Показать наиболее частые события и изменение volume во времени.

### 263. SLO Dashboard

Показать availability, ingestion latency, error rate и storage delay.

### 264. Dashboard Variables

Добавить filters по environment, service, tenant и event name.

### 265. Dashboard Provisioning

Хранить Grafana dashboards в репозитории и загружать автоматически.

### 266. Dashboard Documentation

Описать назначение каждого dashboard и интерпретацию panels.

## Alerts and SLO

### 267. Ingestion Availability SLI

Определить показатель успешности ingestion requests.

### 268. Ingestion Latency SLI

Измерять время от HTTP request до принятия события системой.

### 269. Storage Delay SLI

Измерять задержку между ingestion timestamp и появлением события в ClickHouse.

### 270. Query Availability SLI

Определить успешность analytics queries.

### 271. Query Latency SLI

Измерять latency query API.

### 272. Ingestion SLO

Зафиксировать целевую availability и latency ingestion.

### 273. Query SLO

Зафиксировать целевые показатели analytics queries.

### 274. Error Budget

Рассчитывать error budget для ingestion и query API.

### 275. High Rejection Rate Alert

Создать alert на резкий рост невалидных событий.

### 276. Buffer Saturation Alert

Создать alert на заполнение buffer.

### 277. Insert Failure Alert

Создать alert на repeated ClickHouse insert failures.

### 278. Storage Delay Alert

Создать alert на рост времени доставки событий в ClickHouse.

### 279. Queue Lag Alert

Создать alert на рост consumer lag.

### 280. DLQ Growth Alert

Создать alert на увеличение dead letter queue.

### 281. ClickHouse Parts Alert

Создать alert на чрезмерное количество active parts.

### 282. Query Latency Alert

Создать alert на рост p95 и p99 query latency.

### 283. Memory Alert

Создать alert на устойчивый рост process memory.

### 284. Event Loop Delay Alert

Создать alert на блокировку Node.js event loop.

### 285. Alert Severity

Определить warning, critical и informational severity.

### 286. Alert Runbook Links

Связать каждый alert с operational runbook.

## Security

### 287. Authentication Threat Model

Провести анализ угроз для ingestion и query authentication.

### 288. Tenant Isolation

Гарантировать tenant scope во всех ingestion и query operations.

### 289. Query Authorization

Проверять право пользователя на доступ к analytics project.

### 290. Schema Administration Authorization

Ограничить регистрацию и изменение schemas административными permissions.

### 291. API Key Rotation

Реализовать безопасную смену ingestion API key.

### 292. API Key Revocation

Реализовать немедленный отзыв скомпрометированного ключа.

### 293. Request Replay Protection

Ограничить повторное использование подписанных или timestamped requests.

### 294. Payload Abuse Protection

Защитить ingestion API от чрезмерной вложенности, огромных строк и compression bombs.

### 295. Query Injection Protection

Не допускать передачу произвольного ClickHouse SQL через публичный API.

### 296. Sensitive Event Detection

Отклонять события, содержащие password, token, card data и другие запрещённые поля.

### 297. Data Encryption in Transit

Использовать TLS для API, ClickHouse и observability connections.

### 298. Data Retention Security

Удалять данные после завершения допустимого retention period.

### 299. Audit Trail

Фиксировать управление schemas, keys, quotas и saved queries.

### 300. Security Incident Events

Создать специальные события для security monitoring.

## Reliability

### 301. Graceful Shutdown

Корректно останавливать HTTP server, buffer, workers, metrics и ClickHouse connections.

### 302. Dependency Startup Order

Запускать приложение только после инициализации критических зависимостей.

### 303. ClickHouse Outage Handling

Определить поведение ingestion при временной недоступности ClickHouse.

### 304. Queue Outage Handling

Определить поведение producers и consumers при недоступности broker.

### 305. Grafana Outage Independence

Гарантировать, что недоступность Grafana не влияет на ingestion.

### 306. Metrics Export Failure

Не допускать остановки основной обработки при проблемах metrics backend.

### 307. Retry Budget

Ограничить общее количество retries и их влияние на систему.

### 308. Circuit Breaker

Добавить circuit breaker для ClickHouse и внешних observability exporters.

### 309. Bulkhead Isolation

Разделить ресурсы ingestion, batch insertion и query API.

### 310. Load Shedding

Отклонять часть запросов при перегрузке вместо неконтролируемого роста memory.

### 311. Read-Only Degraded Mode

Поддержать query API при временной остановке ingestion.

### 312. Ingestion-Only Degraded Mode

Поддержать накопление событий при временной недоступности query subsystem.

### 313. Poison Event Handling

Изолировать событие, постоянно вызывающее ошибку обработки.

### 314. Failed Batch Recovery

Создать процедуру восстановления failed batches.

### 315. Data Consistency Check

Сравнивать количество принятых, опубликованных и сохранённых событий.

### 316. Duplicate Rate Monitoring

Отслеживать долю повторно поступающих событий.

### 317. Clock Skew Monitoring

Отслеживать разницу между client timestamp и server timestamp.

### 318. Disaster Recovery

Описать восстановление ClickHouse, schemas и configuration после потери окружения.

## Performance Testing

### 319. Ingestion Benchmark

Измерить throughput single-event ingestion.

### 320. Batch Ingestion Benchmark

Измерить throughput при разных размерах batch.

### 321. Validation Benchmark

Сравнить стоимость валидации простых и сложных schemas.

### 322. Normalization Benchmark

Измерить стоимость enrichment и property normalization.

### 323. Buffer Benchmark

Проверить влияние buffer size и flush interval.

### 324. Insert Batch Size Benchmark

Найти эффективный размер ClickHouse batch.

### 325. Concurrent Insert Benchmark

Определить оптимальное количество параллельных inserts.

### 326. Compression Benchmark

Сравнить ingestion и insert с compression и без неё.

### 327. Query Benchmark

Измерить latency основных analytics queries.

### 328. Raw vs Aggregate Benchmark

Сравнить запросы к raw и aggregate tables.

### 329. High Cardinality Benchmark

Исследовать влияние высококардинальных properties.

### 330. Wide Event Benchmark

Исследовать события с большим количеством properties.

### 331. Large Payload Benchmark

Проверить обработку событий близких к максимальному размеру.

### 332. Sustained Load Test

Провести длительный тест стабильной ingestion нагрузки.

### 333. Spike Test

Проверить систему при резком кратковременном росте событий.

### 334. Stress Test

Найти пределы throughput и поведение после насыщения.

### 335. Recovery Test

Проверить восстановление системы после завершения перегрузки.

### 336. Performance Report

Зафиксировать результаты, bottlenecks и рекомендуемые limits.

## Testing

### 337. Event Schema Unit Tests

Проверить базовые схемы и правила event model.

### 338. Validation Unit Tests

Проверить required fields, types, enums и nested properties.

### 339. Normalization Unit Tests

Проверить преобразование timestamps, metadata и properties.

### 340. Sanitization Unit Tests

Проверить удаление sensitive data.

### 341. Batch Builder Unit Tests

Проверить формирование batch по размеру и количеству строк.

### 342. Retry Unit Tests

Проверить backoff, jitter и retry limits.

### 343. Query Builder Unit Tests

Проверить генерацию разрешённых queries и filters.

### 344. Tenant Isolation Tests

Проверить невозможность доступа к данным другого tenant.

### 345. Ingestion Integration Tests

Проверить полный flow от HTTP request до buffer.

### 346. ClickHouse Integration Tests

Проверить migrations, inserts и queries на реальном ClickHouse.

### 347. Batch Insert Integration Tests

Проверить flush, retries и failed batch storage.

### 348. Schema Registry Integration Tests

Проверить регистрацию, versioning и cache invalidation.

### 349. Queue Integration Tests

Проверить producer, consumer, acknowledgment и DLQ.

### 350. Query API Integration Tests

Проверить filters, pagination, grouping и time series.

### 351. Metrics Tests

Проверить регистрацию и обновление metrics.

### 352. Logging Tests

Проверить structured fields и sanitization.

### 353. Tracing Tests

Проверить trace propagation между ingestion, queue и storage.

### 354. Failure Injection Tests

Имитировать ClickHouse outage, timeout и connection reset.

### 355. Duplicate Delivery Tests

Проверить поведение при повторной доставке событий.

### 356. Graceful Shutdown Tests

Проверить flush buffer и завершение активных операций.

## Operations

### 357. Application Runbook

Создать инструкцию запуска, остановки и проверки приложения.

### 358. ClickHouse Runbook

Описать проверку health, storage, parts, merges и query load.

### 359. High Rejection Runbook

Описать расследование роста validation errors.

### 360. Buffer Saturation Runbook

Описать диагностику заполнения internal buffer.

### 361. Insert Failure Runbook

Описать действия при невозможности записывать данные в ClickHouse.

### 362. Queue Lag Runbook

Описать диагностику consumer lag.

### 363. DLQ Runbook

Описать просмотр, исправление и replay dead letter events.

### 364. Slow Query Runbook

Описать поиск и оптимизацию медленных ClickHouse queries.

### 365. Storage Growth Runbook

Описать контроль storage usage и изменение retention.

### 366. Data Replay Runbook

Описать безопасную повторную загрузку событий.

### 367. Schema Rollback Runbook

Описать восстановление после некорректного schema update.

### 368. API Key Incident Runbook

Описать отзыв и rotation скомпрометированного ключа.

### 369. Backup Procedure

Создать процедуру резервного копирования schemas и критической metadata.

### 370. Restore Procedure

Проверить восстановление системы из backup.

## Final System Tasks

### 371. Complete Ingestion Pipeline

Собрать полный поток: API request, authentication, validation, normalization, buffering и ClickHouse insert.

### 372. Complete Queued Pipeline

Собрать flow через broker: ingestion producer, consumer, batch insert, retries и DLQ.

### 373. Complete Query Platform

Собрать query API с filters, time series, aggregations, pagination и tenant isolation.

### 374. Complete Observability Stack

Связать metrics, structured logs, traces, dashboards и alerts.

### 375. Production Dashboard Package

Подготовить полный набор Grafana dashboards для ingestion, ClickHouse, queries и runtime.

### 376. Production Alert Package

Подготовить alerts с severity, description и runbook links.

### 377. Event SDK

Создать небольшой TypeScript SDK для отправки single и batch events.

### 378. SDK Retry Policy

Добавить retries, timeout и idempotency в client SDK.

### 379. SDK Buffering

Добавить локальный batch buffer и периодический flush в SDK.

### 380. SDK Shutdown Flush

Обеспечить отправку накопленных событий при завершении приложения.

### 381. Demo Application

Создать приложение, генерирующее business и technical events.

### 382. Demo Analytics Scenario

Показать ingestion, запросы, dashboard, metrics, logs и trace одного end-to-end flow.

### 383. Simulated Production Incident

Смоделировать рост ingestion latency или ClickHouse insert failures.

### 384. Incident Investigation

Провести расследование через metrics, logs, traces и ClickHouse system tables.

### 385. Incident Postmortem

Оформить impact, timeline, root cause, resolution и preventive actions.

### 386. Architecture Documentation

Описать компоненты, потоки данных, storage model, reliability и security boundaries.

### 387. Performance Report

Зафиксировать пределы throughput, рекомендуемые batch sizes и query limits.

### 388. Security Review

Проверить authentication, tenant isolation, payload sanitization и query safety.

### 389. Operational Readiness Review

Проверить health checks, dashboards, alerts, runbooks, backups и graceful shutdown.

### 390. Final Analytics Case Study

Собрать законченное описание production-style analytics system: ingestion, schemas, buffering, ClickHouse, querying, observability, failures, performance и эксплуатация.
