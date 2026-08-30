# C2GRAPH Application Lanes Guide

**Bahasa Indonesia:** [C2GRAPH-APPLICATION-LANES-GUIDE-ID.md](C2GRAPH-APPLICATION-LANES-GUIDE-ID.md)

> Public guide to C2GRAPH v1.2.0 scope lanes. Scope files specialize governance for a domain; they never replace root authority.

## 1. Why lanes exist

A backend API, database migration, mobile release, payment-sensitive change, and UI adjustment fail in different ways. C2GRAPH uses scoped rule surfaces so relevant controls can be loaded without expanding the root router into an unbounded document.

## 2. Publicly enumerated lanes

| Scope | Primary concerns |
| --- | --- |
| `backend-api.md` | endpoints, services, authn/authz, background jobs, integrations |
| `data-schema.md` | tables, columns, indexes, relations, migrations, seeds, heavy queries |
| `frontend-web.md` | pages, components, client routing/state, styling, assets |
| `infra-deploy.md` | environments, secrets, DNS, CDN, pipelines, runtime config, deploys |
| `mobile-android.md` | Android source, Gradle, Android resources, Play metadata |
| `mobile-ios.md` | iOS source, Xcode project/config, assets, entitlements |
| `mobile-native.md` | shared/cross-platform native mobile behavior |
| `payments-sensitive.md` | highest-standard payment-sensitive work |
| `qa-tooling.md` | tests, QA harnesses, browser automation, tool integration, CI |
| `ui-ux.md` | typography, color, spacing, icons, naming, copy tone, visual quality |
| `_TEMPLATE.md` | template for a new scoped lane |

## 3. Multi-lane work

One task may trigger more than one lane. Example: a checkout schema migration with frontend changes can require `payments-sensitive`, `data-schema`, `backend-api`, `frontend-web`, and QA controls. Loading multiple scopes does not authorize cross-lane writes; exact semantic scope and `ACTUAL_WRITE_SET` remain path-specific.

## 4. Mobile-specific rule

Mobile release governance differs from replaceable server deployments because a distributed binary cannot be rolled back in the same way. Platform-specific scope + build/release/device-QA/store-submission runbooks should be considered together.

## 5. Sensitive work

The `payments-sensitive` lane should be treated as a stricter policy overlay. Ambiguity is not a reason to broaden action. Evidence gaps should stop or narrow claims/actions according to the governing runbooks.

## 6. Adding a lane

A new lane should have:

1. deterministic triggers reflected by the router;
2. an explicit scope file based on `_TEMPLATE.md`;
3. no contradiction with root CORE rules;
4. relevant runbook links;
5. verification coverage and route validation;
6. a row in this public guide and the repository coverage matrix.
