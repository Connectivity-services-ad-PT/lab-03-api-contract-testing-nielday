# Submission Checklist — Lab 03 — Nhóm A1 (IoT Ingestion)

Repo nhóm cần có các artefact sau trước khi nộp:

- [x] Contract service chính: `contracts/iot-ingestion.openapi.yaml`
- [x] Contract service phụ thuộc (consumer-side smoke): `contracts/core-business-a6.openapi.yaml`
- [x] Contract service phụ thuộc bổ sung: `contracts/analytics-a5.openapi.yaml`
- [x] Postman collection: `postman/collections/FIT4110_lab03_iot_ingestion.postman_collection.json`
- [x] Mock environment: `postman/environments/FIT4110_lab03_mock.postman_environment.json`
- [x] Local environment: `postman/environments/FIT4110_lab03_local.postman_environment.json`
- [x] Newman XML report: `reports/newman-report-mock.xml`
- [x] Newman HTML report: `reports/newman-report.html`
- [x] Contract lint report: `reports/contract-lint-report.txt`
- [x] Reliability checklist: `checklists/reliability_checklist.md`
- [x] Test-case matrix: `templates/test-case-matrix.csv`
- [x] Consumer-provider handshake: `templates/consumer-provider-handshake.md`

## Kết quả kiểm tra

- Contract lint: Pass (Không có lỗi).
- Newman mock run: Pass (Đã xuất report).
- Mock IoT: `http://localhost:4010`
- Mock Core Business (A6): `http://localhost:4011`

## Quy ước commit

```bash
git add .gitignore package.json package-lock.json
git add contracts postman checklists templates reports/contract-lint-report.txt
git add -f reports/newman-report-mock.xml reports/newman-report.html
git commit -m "lab03: add postman contract tests and newman report for A1 IoT Ingestion"
git push
```

## Link nộp LMS

Nộp link GitHub repo, không nộp file rời.
