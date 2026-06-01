# Reliability Checklist — FIT4110 Lab 03 — Nhóm A1 (IoT Ingestion)

## 1. Functional tests

- [x] Có test cho endpoint health: `GET /health`.
- [x] Có test happy path cho endpoint chính: `POST /telemetry` (TemperatureReading).
- [x] Có test happy path cho loại cảm biến thứ 2: `POST /telemetry` (SmokeReading với oneOf + discriminator).
- [x] Có kiểm tra status code 2xx: `200` và `202`.
- [x] Có kiểm tra field quan trọng trong response: `status`, `service`, `eventId`, `ACCEPTED`, `deviceId`, `metricType`.
- [x] Có test đọc dữ liệu: `GET /telemetry/latest`.
- [x] Có test cập nhật trạng thái thiết bị: `POST /devices/{deviceId}/status`.

## 2. Auth tests

- [x] Có test request có token hợp lệ.
- [x] Có test thiếu token.
- [x] Có test sai token hoặc token rỗng.
- [x] Endpoint public được khai báo rõ nếu không cần auth: `GET /health`.
- [x] Test thể hiện đúng expected status `401/403` cho service thật; với mock, invalid-token case được skip có kiểm soát vì Prism không chứng minh auth middleware thật.

## 3. Negative tests

- [x] Có test thiếu field bắt buộc: thiếu `deviceId`.
- [x] Có test sai discriminator: `metricType=HUMIDITY` (không có trong oneOf).
- [x] Có test sai kiểu dữ liệu: `value` gửi dạng string `"thirty-six"`.
- [x] Lỗi trả về theo cùng một error model: `Problem` (RFC 9457).

## 4. Boundary tests

- [x] Có test min/max hoặc dữ liệu sát ngưỡng: `batteryLevel=100` (max theo schema).
- [x] Có test union type: `batteryLevel=null` (type: [number, "null"]).
- [x] Có test device không tồn tại: `POST /devices/SENSOR-X-999/status` → 404.
- [x] Có ghi chú kỳ vọng xử lý dữ liệu biên trong test-case matrix.

## 5. Reliability tests cơ bản

- [x] Có kiểm tra response time trong folder `06_Local_only_NonFunctional`.
- [x] Có mô tả timeout mong muốn: local response time dưới `1000ms`.
- [x] Có test rate limit: `POST /telemetry` trả `429 Too Many Requests`.
- [x] Có consumer-side smoke test với mock của nhóm A6 (Core Business): gọi `POST /events` và `GET /alerts/recent`.

## 6. Evidence

- [x] Collection export JSON: `postman/collections/FIT4110_lab03_iot_ingestion.postman_collection.json`.
- [x] Environment mock export JSON: `postman/environments/FIT4110_lab03_mock.postman_environment.json`.
- [x] Environment local export JSON: `postman/environments/FIT4110_lab03_local.postman_environment.json`.
- [x] Newman report XML/HTML: `reports/newman-report-mock.xml`, `reports/newman-report.html`.
- [x] Contract lint report: `reports/contract-lint-report.txt`.
- [x] Test-case matrix đã điền: `templates/test-case-matrix.csv`.
- [x] Biên bản handshake đã điền: `templates/consumer-provider-handshake.md`.

## Ghi chú

- Contract từ Lab 2 của Nhóm A1, bổ sung response 429 cho Lab 3.
- Consumer-side smoke test gọi Core Business (A6) — partner thật theo pairing matrix pair-05.
- Contract lint: Pass.
- Mock test: Pass (đã xuất report HTML).
