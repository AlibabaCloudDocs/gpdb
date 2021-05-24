# Planning

The Technical White Paper describes the core features and ecosystem integration tools of AnalyticDB for PostgreSQL, including the technical architecture, module component and interaction, common scenarios, product forms, performance, cost, availability, reliability, scalability, and security. AnalyticDB for PostgreSQL plans to invest more to improve the cloud native architecture and features, improve cost efficiency, and improve performance in hybrid transaction/analytical processing \(HTAP\) scenarios.

-   To improve the cloud native architecture and features, AnalyticDB for PostgreSQL supports an architecture in which computing is decoupled from storage and scaling can be completed within seconds without impacts on the service.
-   To improve cost efficiency, AnalyticDB for PostgreSQL continues to optimize its execution engine, optimizers, and storage engine to support high-throughput, low-latency, and high-concurrency reads and writes. AnalyticDB for PostgreSQL introduces real-time materialized views, parallel queries, and result set cache for optimization in specific scenarios. AnalyticDB for PostgreSQL also supports hierarchical storage to reduce storage costs.
-   To improve performance in HTAP scenarios, AnalyticDB for PostgreSQL supports multi-replica query extension and hybrid row-column storage. Row stores are used for transactional processing, whereas column stores are used for analytical processing. This way, a single system is used for both online transaction processing and analysis.

