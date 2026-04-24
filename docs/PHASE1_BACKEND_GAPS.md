# Phase 1 backend gaps and follow-ups

## 目前不阻塞 Phase 1 的項目

這一版 `Pg_Customers_Directory` 已可直接使用 `crm_core.customers` 運作。

## 但要進正式 CRM / 客戶健康管理頁，後端仍需補齊

### 必要
- `crm_views.customer_directory`
- `crm_views.customer_family_members`
- `crm_views.customer_latest_summary`
- `crm_views.manual_review_open`

### 建議
- `crm_views.customer_health_timeline`

### 建立/寫入 contract
- `create_customer`
- `create_child_customer`

## 前端不應自行處理
- `customer_code` 生成
- `root_customer_id` / `parent_customer_id` 推導
- family/root 規則
- health timeline 聚合
- latest summary 聚合
