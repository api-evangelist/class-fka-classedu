---
name: Pull attendance and engagement reporting for a class
description: Retrieve attendance and engagement metrics for a Class (fka ClassEDU) class, paging through results.
api: openapi/class-fka-classedu-openapi.yml
operations: [getClasses, getAttendance, getMetrics]
---

# Pull attendance and engagement reporting for a class

Report on how a class ran using the Class Developer API.

## Auth
Send `Authorization: Bearer <api_key_secret>`. You need `class:read`, `attendance:read`, and `metrics:read` scopes.

## Steps
1. **Find the class** (optional) — `GET /api/v1/classes` (`getClasses`) to list classes, or pass `class_id`/`ext_class_id` to confirm a single one.
2. **Get attendance** — `GET /api/v1/reporting/attendance` (`getAttendance`) filtered by `class_id` or `ext_class_id`.
3. **Get engagement metrics** — `GET /api/v1/reporting/metrics` (`getMetrics`) for the same class.

## Paging
List and reporting endpoints page with `page` and `limit` (1-1000). Read `total_records`, `current_page`, `total_pages`, `next_page`, and `prev_page` from the response and loop until `next_page` is null. See `conventions/class-fka-classedu-conventions.yml`.

## Rules
- Timestamps are Unix time; zones are Time Zone IDs.
- Handle `403` as a missing scope on the API key and `404` as an unknown class id.
