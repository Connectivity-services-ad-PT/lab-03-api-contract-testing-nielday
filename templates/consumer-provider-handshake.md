# Consumer–Provider Handshake

## Thông tin chung

- Lab: FIT4110 Lab 03
- Ngày: 2026-06-01
- Provider team: `Nhóm A6 — Core Business`
- Consumer team: `Nhóm A1 — IoT Ingestion`
- Provider service: Core Business REST API (fallback endpoint cho IoT event)
- Consumer service: IoT Ingestion contract test suite / consumer-side smoke client

## Contract

- Contract file: `contracts/core-business-a6.openapi.yaml`
- Mock base URL: `http://localhost:4011`
- Auth method: Bearer token, dùng biến Postman `{{authToken}}`
- Endpoint được test:
  - `POST /events` — gửi sensor event (SensorReadingEvent)
  - `GET /alerts/recent` — đọc danh sách cảnh báo gần đây

## Smoke test

### Request 1: POST /events

```http
POST /events
Authorization: Bearer {{authToken}}
Content-Type: application/json
```

```json
{
  "eventType": "SENSOR_READING",
  "eventId": "f47ac10b-58cc-4372-a567-0e02b2c3d479",
  "occurredAt": "2026-05-24T08:30:00Z",
  "source": "service-iot-ingestion",
  "deviceId": "SENSOR-TEMP-B2-01",
  "sensorType": "temperature",
  "value": 38.5,
  "unit": "celsius"
}
```

### Expected response

```json
{
  "eventId": "f47ac10b-58cc-4372-a567-0e02b2c3d479",
  "acceptedAt": "2026-05-24T08:30:01Z"
}
```

### Request 2: GET /alerts/recent

```http
GET /alerts/recent?limit=5
Authorization: Bearer {{authToken}}
```

### Expected response

```json
{
  "items": [
    {
      "id": "0196fb3d-4ad7-7d1e-9f49-5d5148d2babc",
      "sourceService": "service-iot-ingestion",
      "alertType": "SENSOR_THRESHOLD_EXCEEDED",
      "severity": "HIGH",
      "message": "Cam bien khoi vuot nguong",
      "status": "OPEN",
      "createdAt": "2026-05-24T08:35:00Z"
    }
  ]
}
```

## Kết quả

- [x] Consumer gọi mock thành công (POST /events → 201).
- [x] Consumer parse được field cần dùng: `eventId`, `acceptedAt`.
- [x] Consumer đọc được danh sách alert (GET /alerts/recent → 200, items array).
- [x] Consumer hiểu lỗi 4xx/5xx provider trả về qua `Problem` trong contract Core Business.
- [x] Có Newman report: `reports/newman-report.html`, `reports/newman-report-mock.xml`.

## Ghi chú thay đổi hợp đồng

| Nội dung | Trước | Sau | Người đồng ý |
|---|---|---|---|
| Smoke test provider phụ thuộc | Chưa có bằng chứng chạy được | Thêm request `POST {{coreBusinessMockUrl}}/events` và `GET {{coreBusinessMockUrl}}/alerts/recent` trong folder `05_Consumer_side_Smoke` | Provider (A6) và Consumer (A1) |
| Field consumer cần parse | Chưa ghi rõ | `eventId`, `acceptedAt`, `items` | Provider (A6) và Consumer (A1) |
| Auth trong test | Có nguy cơ hardcode token | Dùng `Authorization: Bearer {{authToken}}` từ environment | Provider (A6) và Consumer (A1) |

## Xác nhận

- Provider representative: `Nhóm A6 — Core Business`
- Consumer representative: `Nhóm A1 — IoT Ingestion`

## Evidence

- Newman mock report: `reports/newman-report.html`
- Newman XML report: `reports/newman-report-mock.xml`
- Collection: `postman/collections/FIT4110_lab03_iot_ingestion.postman_collection.json`
- Mock environment: `postman/environments/FIT4110_lab03_mock.postman_environment.json`
