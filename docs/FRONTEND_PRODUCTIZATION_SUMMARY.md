# Appsmith Customer Workspace productization summary

## Completed in this package

- Preserved the three Step 5 auto-generated CRUD pages.
- Added formal workspace pages: Pg_Dashboard_Home, Pg_Customers_Directory, Pg_Customers_Detail, Pg_Customers_Create, Pg_Customers_CreateChild.
- Updated Customers CRUD insert/update to stop asking operators for internal id, root_customer_id, or parent_customer_id.
- Formal read pages use crm_views.customer_directory instead of exposing raw internal UUIDs.
- Formal pages disable hard delete and steer deletion policy toward status = archived.

## Backend blockers / required follow-up

1. crm_views.customer_directory must exist with at least: customer_code, display_name, status, phone, email, family_role, is_parent_customer, root_customer_code, source_channel, owner_user_label, latest_submission_at, latest_assessment_at, open_review_count, created_at.
2. crm_views.customer_family_members must expose root_customer_code and public customer fields for the detail page family block.
3. crm_views.customer_latest_summary must expose one row per customer_code for the detail summary block.
4. Create-child is intentionally blocked until a backend contract/function exists. Recommended contract: app_contract/create_child_customer.sql or crm_app.create_child_customer(parent_customer_code text, display_name text, phone text, email text, family_role text, ...) RETURNS customer_code.
5. Health report pages depend on crm_core.health_report_headers and crm_core.health_report_items. If backend keeps only lab_results, either create compatibility views/tables or replace those pages with Lab Results pages.
6. Customer code auto-generation should treat empty string as missing, or frontend must always pass NULL. This package uses NULLIF(..., '').

## Import order suggestion

1. Pg_Dashboard_Home
2. Pg_Customers_Directory
3. Pg_Customers_Detail with URL query param ?customer_code=CH-000001
4. Pg_Customers_Create
5. AUTO pages for admin/debug only
