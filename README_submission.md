# README Submission

Kết quả practice: student benchmark PASS 11/11, no-memory PASS 2/11. `reports/comparison.md` cho thấy evidence hit rate tăng từ 18.2% lên 100.0%.

Layer quan trọng nhất trong bộ test này là long-term memory. Layer này có 4 case trực tiếp: E02, E03, E08, E09, và còn cung cấp evidence cho mixed case E07. Long-term giữ preference Python của Minh, open loop benchmark report lúc 16:00, user isolation của Lan, và cập nhật mới nhất cho BLUEBIRD-42.

Trade-off giữa Context Block/Zep và Redis+Qdrant: Zep có user-scoped graph, Context Block, facts/episodes kèm provenance và xử lý conflict/recency tốt hơn, nên phù hợp cho cross-session recall. Redis+Qdrant dễ kiểm soát, rẻ và minh bạch hơn, nhưng phải tự làm schema, TTL, user isolation, ranking, deletion và audit.

Guardrail chống memory poisoning: chỉ durable-write khi user đã opt-in, minimize PII trước khi ingest, lưu source/timestamp/confidence/scope, và review các preference hoặc task high-impact trước khi ghi. Heartbeat/background job chỉ được compact hoặc de-duplicate, không được tự cấp quyền hay tạo instruction mới.

Phân tích benchmark: không có layer student nào thấp nhất vì tất cả đều đạt 100%. No-memory fail các durable layer vì không retrieve được cross-session, episodic và semantic evidence. Query retrieve nhiều token nhất là E03 với 1381 tokens. E07 cần kết hợp long-term preference `Python` với semantic payment rule `Idempotency-Key`. Student avg token reduction là 14.2%; no-memory reduction 81.8% nhưng hit rate chỉ 18.2%, vì nó tiết kiệm token bằng cách retrieve rỗng.

E08 dùng recency/scope: Python vẫn dùng cho ORCHID-27, nhưng BLUEBIRD-42 mới hơn và project-specific nên backend phải là TypeScript + NestJS. E10 cho thấy compaction tốt phải giữ constraint/TODO durable note `REVIEW-DEADLINE-1600`, không chỉ tóm tắt hội thoại cũ.

## Ảnh minh chứng

![Golden 20/20](submission/golden.png)

![Long-term](submission/long_term.png)

![Episodic](submission/episodic.png)

![Semantic](submission/semantic.png)

![Privacy](submission/privacy.png)
