---
name: Provision a class and enroll a learner
description: Create a class in Class (fka ClassEDU), enroll a learner, and generate their launch link.
api: openapi/class-fka-classedu-openapi.yml
operations: [createClass, createEnrollment, getLaunchLink]
---

# Provision a class and enroll a learner

Use the Class Developer API to stand up a class and get a learner into it.

## Auth
Send every request with `Authorization: Bearer <api_key_secret>` (per-organization API key). The base URL is your organization's Class subdomain, e.g. `https://your-organization.class.com`. You need the `class:write` and `enrollment:write` scopes.

## Steps
1. **Create the class** — `POST /api/v1/classes` (`createClass`). Provide `class_name` (required) and optionally `class_description`, `template_id`, `session_times[]` (Unix time `start_time`/`end_time`), `timezone` (Time Zone ID), `settings` (waiting_room, join_before_host, jbh_time), and `ext_class_id` so you can address the class by your own identifier later. Capture the returned `class_id`.
2. **Enroll the learner** — `POST /api/v1/class/enrollments` (`createEnrollment`). Reference the class by `class_id` or `ext_class_id`; supply `email`, `first_name`, `last_name`, `role`, and optionally `ext_person_id`.
3. **Get the launch link** — `GET /api/v1/class/enrollments/launch` (`getLaunchLink`) with the class and learner identifiers to return the learner's `launch_url`.

## Rules
- Address resources by either the Class-assigned id or your external id (`ext_class_id`, `ext_person_id`).
- Errors are HTTP status codes with a JSON `message` body: `400` validation, `401` bad key, `403` missing scope, `404` not found. See `errors/class-fka-classedu-problem-types.yml`.
- No idempotency key is available; use your external ids to keep operations repeatable.
