# Customer Workspace CRM Frontend Redesign v2

這一版把 Appsmith app 從「自動生成 CRUD 頁集合」收斂成 Customer-first CRM 前台。

## 正式使用入口

1. Pg_Dashboard_Home：主管/營運首頁，只顯示關鍵指標與進入 Directory。
2. Pg_Customers_Directory：搜尋 customer_code / 姓名 / phone / email，點列進 Profile。
3. Pg_Customers_Detail：客戶健康管理主頁。以 customer_code 載入客戶 header、family members、latest summary、health timeline、open reviews。
4. Pg_Customers_Create：建立 root / standalone customer。
5. Pg_Customers_CreateChild：從 parent profile 建立 family member。

## AUTO CRUD 頁定位

AUTO_* 頁已保留，但定位改為 admin/debug，不是第一線人員入口。

## 已調整的前台原則

- Directory 只讀 crm_views.customer_directory，不寫入 view。
- 新增 customer 寫入 crm_core.customers，且不要求前台填 id/root_customer_id/customer_code。
- Detail 路由使用 customer_code。
- Create Child 只送 parent_customer_id，由 DB trigger 決定 root_customer_id / is_parent_customer。
- 不在前台重算 family/root。

## 後端待確認/補強

1. crm_views.customer_directory 必須包含：customer_id, customer_code, root_customer_code, display_name, status, phone, email, family_role, is_parent_customer, latest_submission_at, latest_assessment_at, open_review_count。
2. crm_views.customer_family_members 必須包含：root_customer_code, customer_code, display_name, status, family_role, is_parent_customer, phone, email。
3. crm_views.customer_latest_summary 必須以 customer_code 查詢。
4. crm_views.manual_review_open 必須包含 customer_code, review_ref, priority, review_status, assigned_to, created_at, notes。
5. 若你要讓 Appsmith 使用更安全的寫入方式，請後端提供 app_contract/create_customer.sql 與 app_contract/create_child_customer.sql 或 DB function。
6. 目前 HealthTimelineQuery 直接讀 crm_core.health_assessments / lifestyle_profiles / lab_results；若你不想讓 Appsmith 讀 core tables，請後端提供 crm_views.customer_health_timeline。

## 建議後端新增 view

```sql
CREATE OR REPLACE VIEW crm_views.customer_health_timeline AS
-- customer_code, event_type, event_at, title, status, details
-- union health_assessments / lifestyle_profiles / lab_results / intake_submissions
;
```

前端可再把 Pg_Customers_Detail 的 HealthTimelineQuery 改成只讀這個 view。
