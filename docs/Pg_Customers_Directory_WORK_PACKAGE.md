# Pg_Customers_Directory Work Package

## 本次已完成（Phase 1）

這一輪只改造既有的 `AUTO_Customers_CRUD`，不新增手工 page schema，避免再次把整包 Appsmith source 弄壞。

匯入後你會看到這一頁顯示名稱已改成：
- `Pg_Customers_Directory`

但其內部 page id 仍沿用原本自動生成頁的骨架，目的是保持 Appsmith 匯入穩定。

## 這一頁現在能做什麼

1. 以 CRM 入口方式搜尋 customer
   - `customer_code`
   - `display_name`
   - `phone`
   - `email`
2. 只顯示第一線需要的目錄欄位
3. 點選某位 customer 後，在下方打開「客戶基本資料與追蹤備註」面板
4. 更新基本聯絡資料、來源、負責人、客戶備註與基本健康輪廓

## 這一頁刻意先不做的事

- 不在這一頁直接刪除 customer
- 不在這一頁直接暴露 internal UUID
- 不在這一頁硬做 child / family root 計算
- 不在這一頁假造 health summary timeline

## 目前前端暫時採用的安全做法

為了避免被後端 view/blocker 卡住，這一頁目前先直接讀 `crm_core.customers`，但前台呈現已收斂成 CRM customer directory 的樣子。

等後端確認好以下物件後，再切到正式 contract：
- `crm_views.customer_directory`
- `crm_views.customer_family_members`
- `crm_views.customer_latest_summary`
- `crm_app.create_customer(...)` 或 `app_contract/create_customer.sql`

## 下一階段（Phase 2）

下一頁應做：
- `Pg_Customers_Create`

原因：目前 auto-generated insert form 仍不是正式 CRM 建客頁，會暴露不該由前台手填的欄位。

## 後端需配合

目前這一頁不阻塞於後端新增物件；但若要進正式 CRM 化，請後端接著補：

1. `crm_views.customer_directory`
2. `crm_views.customer_family_members`
3. `crm_views.customer_latest_summary`
4. `create_customer` contract / function
5. `create_child_customer` contract / function
