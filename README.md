# TestRail (testrail)

TestRail is a web-based test case management and QA platform (originally by Gurock Software, now part of IDERA) for organizing test cases, running test runs and test plans, and recording test results across manual and automated testing. Its HTTP API (v2) exposes projects, suites, sections, cases, runs, plans, tests, results, milestones, configurations, and users, so teams can push automated results into a run, create and close test runs, and keep test cases in sync programmatically.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/testrail/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/testrail/refs/heads/main/apis.yml)

## Access Model (read this first)

TestRail is **not** a single shared public API on one hostname. There is one API definition (v2), but it runs against **your own TestRail instance**:

- **TestRail Cloud** — hosted per customer at `https://{instance}.testrail.io` (older Cloud instances use `.testrail.com`). Replace `{instance}` with your subdomain.
- **TestRail Server / Enterprise (self-hosted)** — runs on your own host, e.g. `https://testrail.yourcompany.com`.

**Unusual URL style.** Every call goes through TestRail's `index.php` front controller, so the API base is literally:

```
https://{instance}.testrail.io/index.php?/api/v2/
```

The `?/api/v2/...` is part of the path as TestRail sees it (a rewritten query string), not a normal REST path segment. A real request looks like:

```
GET https://example.testrail.io/index.php?/api/v2/get_run/1
```

**Authentication.** HTTP Basic auth. The username is your TestRail email; the password is either your account password or an **API key** generated under *My Settings*. The API must be enabled by an admin under *Administration > Site Settings > API* ("Enable API"). All requests and responses are JSON (`Content-Type: application/json`).

```
curl -H "Content-Type: application/json" \
  -u "user@example.com:APIkey" \
  "https://example.testrail.io/index.php?/api/v2/get_run/1"
```

Because the host is per-instance, the `baseURL` values in this catalog use the `{instance}` placeholder rather than a live shared endpoint.

## Tags

- Test Runs
- Test Management
- QA
- Test Cases
- Test Results
- Test Plans
- Testing
- Test Automation

## Timestamps

- **Created:** 2026-07-11
- **Modified:** 2026-07-11

## APIs

### TestRail Test Runs API

Create, retrieve, update, close, and delete test runs for a project. A test run is an execution of a set of test cases; `get_runs` lists runs for a project, `add_run` creates one (optionally scoped to specific cases), and `close_run` archives it and its results. This is the surface behind the "test runs" QA workflow.

- **Human URL:** [https://support.testrail.com/hc/en-us/articles/7077874763156-Runs](https://support.testrail.com/hc/en-us/articles/7077874763156-Runs)
- **Base URL:** `https://{instance}.testrail.io/index.php?/api/v2`

#### Tags

- Test Runs
- Runs
- QA

#### Properties

- [API Reference](https://support.testrail.com/hc/en-us/articles/7077874763156-Runs)
- [OpenAPI](openapi/testrail-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/testrail.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/testrail.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### TestRail Test Results API

Read and record test results. Retrieve results for a test, a case, or an entire run, and push new results back — individually (`add_result`, `add_result_for_case`) or in bulk (`add_results`, `add_results_for_cases`). The bulk endpoints are the recommended way for automated test suites to report status, elapsed time, defects, and comments into a run.

- **Human URL:** [https://support.testrail.com/hc/en-us/articles/7077819312404-Results](https://support.testrail.com/hc/en-us/articles/7077819312404-Results)
- **Base URL:** `https://{instance}.testrail.io/index.php?/api/v2`

#### Tags

- Test Results
- Results
- Automation

#### Properties

- [API Reference](https://support.testrail.com/hc/en-us/articles/7077819312404-Results)
- [OpenAPI](openapi/testrail-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/testrail.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/testrail.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### TestRail Test Cases API

Manage the reusable test cases in a project's repository — list, get, create, update, copy, move, and delete cases, plus read case fields and case types. Bulk `get_cases` returns up to 250 records per page and supports filters for suite, section, and update history.

- **Human URL:** [https://support.testrail.com/hc/en-us/articles/7077292642580-Cases](https://support.testrail.com/hc/en-us/articles/7077292642580-Cases)
- **Base URL:** `https://{instance}.testrail.io/index.php?/api/v2`

#### Tags

- Test Cases
- Cases
- Test Management

#### Properties

- [API Reference](https://support.testrail.com/hc/en-us/articles/7077292642580-Cases)
- [OpenAPI](openapi/testrail-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/testrail.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/testrail.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### TestRail Tests API

Retrieve the individual tests generated inside a test run. A "test" is the instance of a case within a specific run, carrying current status and the run-time fields; `get_tests` lists them for a run and `get_test` fetches one by ID.

- **Human URL:** [https://support.testrail.com/hc/en-us/articles/7077990441108-Tests](https://support.testrail.com/hc/en-us/articles/7077990441108-Tests)
- **Base URL:** `https://{instance}.testrail.io/index.php?/api/v2`

#### Tags

- Tests
- Test Instances
- QA

#### Properties

- [API Reference](https://support.testrail.com/hc/en-us/articles/7077990441108-Tests)
- [OpenAPI](openapi/testrail-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/testrail.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/testrail.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### TestRail Test Plans API

Manage test plans, which group multiple test runs (often across configurations) under one milestone. Create and update plans, add and update plan entries (each entry becomes one or more runs across configurations), and close or delete plans.

- **Human URL:** [https://support.testrail.com/hc/en-us/articles/7077711537684-Plans](https://support.testrail.com/hc/en-us/articles/7077711537684-Plans)
- **Base URL:** `https://{instance}.testrail.io/index.php?/api/v2`

#### Tags

- Test Plans
- Plans
- Configurations

#### Properties

- [API Reference](https://support.testrail.com/hc/en-us/articles/7077711537684-Plans)
- [OpenAPI](openapi/testrail-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/testrail.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/testrail.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### TestRail Projects API

List, get, create, update, and delete projects — the top-level container that owns suites, cases, runs, plans, and milestones. Projects can be configured for single-suite, single-suite-with-baselines, or multi-suite modes.

- **Human URL:** [https://support.testrail.com/hc/en-us/articles/7077792415124-Projects](https://support.testrail.com/hc/en-us/articles/7077792415124-Projects)
- **Base URL:** `https://{instance}.testrail.io/index.php?/api/v2`

#### Tags

- Projects
- Administration

#### Properties

- [API Reference](https://support.testrail.com/hc/en-us/articles/7077792415124-Projects)
- [OpenAPI](openapi/testrail-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/testrail.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/testrail.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### TestRail Test Suites API

Manage test suites (collections of test cases) within a project — list, get, create, update, and delete suites. Used in multi-suite projects to organize cases by component, feature, or area.

- **Human URL:** [https://support.testrail.com/hc/en-us/articles/7077936624276-Suites](https://support.testrail.com/hc/en-us/articles/7077936624276-Suites)
- **Base URL:** `https://{instance}.testrail.io/index.php?/api/v2`

#### Tags

- Suites
- Test Cases
- Organization

#### Properties

- [API Reference](https://support.testrail.com/hc/en-us/articles/7077936624276-Suites)
- [OpenAPI](openapi/testrail-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/testrail.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/testrail.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### TestRail Sections API

Manage sections and subsections that group test cases inside a suite — list, get, create, update, and delete sections. Sections provide the folder-like hierarchy for organizing a case repository.

- **Human URL:** [https://support.testrail.com/hc/en-us/articles/7077083596436-Introduction-to-the-TestRail-API](https://support.testrail.com/hc/en-us/articles/7077083596436-Introduction-to-the-TestRail-API)
- **Base URL:** `https://{instance}.testrail.io/index.php?/api/v2`

#### Tags

- Sections
- Test Cases
- Organization

#### Properties

- [API Reference](https://support.testrail.com/hc/en-us/articles/7077083596436-Introduction-to-the-TestRail-API)
- [OpenAPI](openapi/testrail-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/testrail.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/testrail.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### TestRail Milestones API

Manage milestones used to track releases and deadlines — list, get, create, update, and delete milestones. Runs and plans can be associated with a milestone to report progress toward a release.

- **Human URL:** [https://support.testrail.com/hc/en-us/articles/7077723976084-Milestones](https://support.testrail.com/hc/en-us/articles/7077723976084-Milestones)
- **Base URL:** `https://{instance}.testrail.io/index.php?/api/v2`

#### Tags

- Milestones
- Planning

#### Properties

- [API Reference](https://support.testrail.com/hc/en-us/articles/7077723976084-Milestones)
- [OpenAPI](openapi/testrail-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/testrail.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/testrail.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### TestRail Configurations API

Manage configuration groups and configurations (for example browsers or operating systems) that plan entries expand across to generate multiple runs — list groups, create and update groups, and add, update, and delete individual configurations.

- **Human URL:** [https://support.testrail.com/hc/en-us/articles/7077083596436-Introduction-to-the-TestRail-API](https://support.testrail.com/hc/en-us/articles/7077083596436-Introduction-to-the-TestRail-API)
- **Base URL:** `https://{instance}.testrail.io/index.php?/api/v2`

#### Tags

- Configurations
- Test Plans

#### Properties

- [API Reference](https://support.testrail.com/hc/en-us/articles/7077083596436-Introduction-to-the-TestRail-API)
- [OpenAPI](openapi/testrail-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/testrail.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/testrail.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### TestRail Users API

Look up TestRail users — list all users, get a user by ID, or find a user by email address. Used to resolve assignee and creator IDs when creating runs, assigning tests, or recording results.

- **Human URL:** [https://support.testrail.com/hc/en-us/articles/7077083596436-Introduction-to-the-TestRail-API](https://support.testrail.com/hc/en-us/articles/7077083596436-Introduction-to-the-TestRail-API)
- **Base URL:** `https://{instance}.testrail.io/index.php?/api/v2`

#### Tags

- Users
- Administration

#### Properties

- [API Reference](https://support.testrail.com/hc/en-us/articles/7077083596436-Introduction-to-the-TestRail-API)
- [OpenAPI](openapi/testrail-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/testrail.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/testrail.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [Authentication](authentication/testrail-authentication.yml)
- [LinkedIn](https://www.linkedin.com/company/testrail)
- [Website](https://www.testrail.com)
- [Documentation](https://support.testrail.com/hc/en-us/articles/7077083596436-Introduction-to-the-TestRail-API)
- [Plans](plans/testrail-plans-pricing.yml)
- [Rate Limits](rate-limits/testrail-rate-limits.yml)
- [Fin Ops](finops/testrail-finops.yml)
- [Pricing](https://www.testrail.com/pricing/)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
