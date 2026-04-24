# Restore package - Step 5 clean baseline

This package intentionally restores the Appsmith source to the clean Step 5 baseline that contains only the 3 Appsmith-generated AUTO CRUD pages:

- AUTO_Customers_CRUD
- AUTO_HealthReportHeaders_CRUD
- AUTO_HealthReportItems_CRUD

Why this package exists:

- The previous CRM frontend v2 package added hand-made Appsmith pages that may not match Appsmith's internal exported-page schema.
- To avoid compounding the issue, restore this baseline first.
- Do not continue importing the broken v2 package.

Next safe direction:

1. First confirm this Step 5 package imports and opens correctly.
2. Keep AUTO pages as admin/debug pages only.
3. Build CRM frontend incrementally inside Appsmith UI or by duplicating a known-good generated page, not by injecting brand-new page JSON from scratch.
4. Convert only one page at a time, starting with AUTO_Customers_CRUD -> Pg_Customers_Directory.
5. Do not delete or rename the original AUTO page until the converted page imports and runs.

Backend-dependent items that should not be faked in frontend:

- family/root calculation
- customer_code generation
- create_child_customer logic
- customer_latest_summary aggregation
- customer_health_timeline aggregation

Recommended backend contracts/views:

- crm_views.customer_directory
- crm_views.customer_family_members
- crm_views.customer_latest_summary
- crm_views.manual_review_open
- crm_views.customer_health_timeline (recommended)
- crm_app.create_customer(...) or app_contract/create_customer.sql
- crm_app.create_child_customer(...) or app_contract/create_child_customer.sql
