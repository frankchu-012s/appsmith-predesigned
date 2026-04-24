# Pg_Customers_Directory Work Package

## Scope

- Work package: `Pg_Customers_Directory`
- Out of scope:
  - `Pg_Customers_Detail`
  - `Pg_Customers_Create`
  - `Pg_Customers_CreateChild`
  - dashboard and health-report productization

## Inputs Used

- `FRONTEND_APPSMITH_BUILD_PLAN.md`
- `FRONTEND_APP_PLAN.md`
- `CODEX_EXECUTION_AND_VALIDATION_PLAN.md`
- backend snapshot contracts:
  - `app_contract/customer_directory.sql`
  - `app_contract/search_customer.sql`
  - `docs/ARCHITECTURE.md`
  - `docs/SERVICE_CONTRACTS.md`
  - `docs/TESTING.md`
  - `VALIDATION_SUMMARY.md`

## Route And Data Contract

- Page name: `Pg_Customers_Directory`
- Route slug in this Appsmith app: `customers`
- Primary read contract: `crm_views.customer_directory`, aligned to `app_contract/customer_directory.sql`
- Search scope for this first package: `customer_code`, `display_name`, `email`, `phone`

## UI Intent

- Keep the existing Appsmith auto-generated card/table shell
- Remove direct exposure of raw internal fields from the visible table
- Reframe the page as a customer-first directory, not a raw table admin page
- Keep create/detail actions explicitly marked as next packages instead of leaving broken CRUD entry points

## Validation Notes

- `customer_code` stays the visible external identifier
- No visible internal UUID column remains in the directory table
- The page-level list query no longer reads directly from `crm_core.customers`
- Detail and create flows are intentionally staged for the next implementation packages
