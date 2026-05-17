# 📦 Supply Chain & Inventory Performance Dashboard
> End-to-end data pipeline and analytics solution built on Microsoft Fabric and Power BI Service.

---

## 🏗️ Architecture Overview

```
Local CSV Dataset
      ↓
Dataflow Gen2 (Managed Connection & Gateway)
      ↓# 📦 Supply Chain & Inventory Performance Dashboard
> End-to-end data pipeline and analytics solution built on Microsoft Fabric and Power BI Service.

---

## 🏗️ Architecture Overview

```
Local CSV Dataset
      ↓
Dataflow Gen2 (Managed Connection & Gateway)
      ↓
🔷 Bronze Lakehouse  ──  Raw ingestion
      ↓
Silver Notebook (PySpark)
      ↓
🥈 Silver Lakehouse  ──  Cleaned & validated Delta table
      ↓
Gold Notebook (PySpark)
      ↓
🥇 Gold Lakehouse  ──  Star schema (4 Delta tables)
      ↓
Power BI Semantic Model (Direct Lakehouse)
      ↓
📊 Power BI Service Dashboard
      ↑
⏰ Scheduled Refresh via Fabric Pipeline — 3:00 AM daily
```

---

## 📂 Repository Contents

| File | Description |
|---|---|
| `Silver_.ipynb` | PySpark notebook — data cleaning & validation (Silver layer) |
| `Gold.ipynb` | PySpark notebook — star schema creation (Gold layer) |
| `Inventory_Dashboard.pbix` | Power BI Desktop report file |
| `DataflowGen_2_Local_Data.pqt` | Dataflow Gen2 Power Query definition |
| `Inventory_Refresh_Pipeline.zip` | Fabric pipeline for scheduled refresh |
| `Dashboard.PNG` | Screenshot of the final Power BI dashboard |
| `Inventory_Model.PNG` | Screenshot of the Power BI semantic model |
| `schedule_refresh.PNG` | Screenshot of the Fabric Monitor showing successful runs |

---

## 🔷 Step 1 — Bronze Layer: Raw Ingestion

**Tool:** Dataflow Gen2 with Managed Connection & Gateway

- Connected local CSV dataset (`Inventory_SupplyChain_Dataset`) to the **Bronze Lakehouse**
- Used Dataflow Gen2 to manage credentials, gateway, and scheduled load
- Raw table: `Suppy_BronzeLakeHouse.dbo.Inventory_SupplyChain_Dataset`
- Dataset shape: **1,200 rows × 15 columns**

---

## 🥈 Step 2 — Silver Layer: Clean & Validate

**Notebook:** `Silver_.ipynb` | **Runtime:** Synapse PySpark (Microsoft Fabric)

### Actions performed
1. Read raw table from Bronze Lakehouse
2. Checked dataset shape (1,200 rows, 15 columns) and printed schema
3. Verified null counts — **zero nulls** across all 15 columns
4. Dropped any rows with null values (`dropna()`) as a safety measure
5. Standardised all column names → lowercase with underscores
6. Renamed `cost_of_goods_sold__cogs_` → `COGS`
7. Saved cleaned data to **Silver Lakehouse** as Delta table: `Cleaned_Inventory_SupplyChain_Dataset`

### Silver schema (15 columns)
`date`, `region`, `category`, `supplier`, `warehouse`, `order_status`, `units_sold`, `inventory_level`, `transportation_cost`, `order_accuracy`, `lead_time__days_`, `backorder`, `COGS`, `average_inventory`, `warehouse_capacity`

---

## 🥇 Step 3 — Gold Layer: Star Schema

**Notebook:** `Gold.ipynb` | **Lakehouse:** `Supply_Gold_LakeHouse`

### Dimension tables built

| Table | Key | Columns |
|---|---|---|
| `dim_product` | `product_id` | `category`, `supplier` |
| `dim_warehouse` | `warehouse_id` | `region`, `warehouse` |
| `master_date` | `date` | `date`, `year`, `month` |

### Fact table

**`fact_supplychain`** — joins all three dimensions via foreign keys
Columns: `date`, `product_id`, `warehouse_id`, `order_status`, `units_sold`, `inventory_level`, `transportation_cost`, `order_accuracy`, `lead_time__days_`, `backorder`, `COGS`, `average_inventory`, `warehouse_capacity`

All four tables saved to the Gold Lakehouse as **Delta format** with overwrite mode.

---

## 📊 Step 4 — Power BI Semantic Model & Dashboard

**Connection type:** Direct Lakehouse (no data import)

### Model relationships
- `fact_supplychain` → `dim_product` (many-to-one via `product_id`)
- `fact_supplychain` → `dim_warehouse` (many-to-one via `warehouse_id`)
- `fact_supplychain` → `master_date` (many-to-one via `date`)

### Dashboard KPIs

| Metric | Value |
|---|---|
| Total Inventory Value | 179,537.70M |
| Warehouse Utilisation Rate | 34.08% |
| Inventory Turnover | 28.2K |
| Order Accuracy Rate | 91.33% |

### Dashboard visuals
- Warehouse Capacity Utilisation (gauge)
- Transportation Cost by Region & Category (clustered bar)
- Annual Units Sold Trend (line chart, 2020–2024)
- Average Lead Time by Category (donut chart)
- Order Fulfillment Status — Fulfilled: 838 | Pending: 248 | Canceled: 114
- Inventory Level by Region & Category (horizontal bar)

---

## ⏰ Step 5 — Scheduled Refresh

**Pipeline:** `Inventory_Refresh_Pipeline`
**Schedule:** Every day at **3:00 AM**
**Monitored via:** Microsoft Fabric Monitor

All pipeline runs show ✅ **Succeeded** status.

---

## 🛠️ Tech Stack

`Microsoft Fabric` · `PySpark` · `Delta Lake` · `Dataflow Gen2` · `Power BI Service` · `Fabric Pipelines` · `Synapse Notebooks`

---

## 📸 Screenshots

**Dashboard**
![Dashboard](Dashboard.PNG)

**Semantic Model**
![Model](Inventory_Model.PNG)

**Pipeline Monitor**
![Monitor](schedule_refresh.PNG
🔷 Bronze Lakehouse  ──  Raw ingestion
      ↓
Silver Notebook (PySpark)
      ↓
🥈 Silver Lakehouse  ──  Cleaned & validated Delta table
      ↓
Gold Notebook (PySpark)
      ↓
🥇 Gold Lakehouse  ──  Star schema (4 Delta tables)
      ↓
Power BI Semantic Model (Direct Lakehouse)
      ↓
📊 Power BI Service Dashboard
      ↑
⏰ Scheduled Refresh via Fabric Pipeline — 3:00 AM daily
```

---

## 📂 Repository Contents

| File | Description |
|---|---|
| `Silver_.ipynb` | PySpark notebook — data cleaning & validation (Silver layer) |
| `Gold.ipynb` | PySpark notebook — star schema creation (Gold layer) |
| `Inventory_Dashboard.pbix` | Power BI Desktop report file |
| `DataflowGen_2_Local_Data.pqt` | Dataflow Gen2 Power Query definition |
| `Inventory_Refresh_Pipeline.zip` | Fabric pipeline for scheduled refresh |
| `Dashboard.PNG` | Screenshot of the final Power BI dashboard |
| `Inventory_Model.PNG` | Screenshot of the Power BI semantic model |
| `schedule_refresh.PNG` | Screenshot of the Fabric Monitor showing successful runs |

---

## 🔷 Step 1 — Bronze Layer: Raw Ingestion

**Tool:** Dataflow Gen2 with Managed Connection & Gateway

- Connected local CSV dataset (`Inventory_SupplyChain_Dataset`) to the **Bronze Lakehouse**
- Used Dataflow Gen2 to manage credentials, gateway, and scheduled load
- Raw table: `Suppy_BronzeLakeHouse.dbo.Inventory_SupplyChain_Dataset`
- Dataset shape: **1,200 rows × 15 columns**

---

## 🥈 Step 2 — Silver Layer: Clean & Validate

**Notebook:** `Silver_.ipynb` | **Runtime:** Synapse PySpark (Microsoft Fabric)

### Actions performed
1. Read raw table from Bronze Lakehouse
2. Checked dataset shape (1,200 rows, 15 columns) and printed schema
3. Verified null counts — **zero nulls** across all 15 columns
4. Dropped any rows with null values (`dropna()`) as a safety measure
5. Standardised all column names → lowercase with underscores
6. Renamed `cost_of_goods_sold__cogs_` → `COGS`
7. Saved cleaned data to **Silver Lakehouse** as Delta table: `Cleaned_Inventory_SupplyChain_Dataset`

### Silver schema (15 columns)
`date`, `region`, `category`, `supplier`, `warehouse`, `order_status`, `units_sold`, `inventory_level`, `transportation_cost`, `order_accuracy`, `lead_time__days_`, `backorder`, `COGS`, `average_inventory`, `warehouse_capacity`

---

## 🥇 Step 3 — Gold Layer: Star Schema

**Notebook:** `Gold.ipynb` | **Lakehouse:** `Supply_Gold_LakeHouse`

### Dimension tables built

| Table | Key | Columns |
|---|---|---|
| `dim_product` | `product_id` | `category`, `supplier` |
| `dim_warehouse` | `warehouse_id` | `region`, `warehouse` |
| `master_date` | `date` | `date`, `year`, `month` |

### Fact table

**`fact_supplychain`** — joins all three dimensions via foreign keys
Columns: `date`, `product_id`, `warehouse_id`, `order_status`, `units_sold`, `inventory_level`, `transportation_cost`, `order_accuracy`, `lead_time__days_`, `backorder`, `COGS`, `average_inventory`, `warehouse_capacity`

All four tables saved to the Gold Lakehouse as **Delta format** with overwrite mode.

---

## 📊 Step 4 — Power BI Semantic Model & Dashboard

**Connection type:** Direct Lakehouse (no data import)

### Model relationships
- `fact_supplychain` → `dim_product` (many-to-one via `product_id`)
- `fact_supplychain` → `dim_warehouse` (many-to-one via `warehouse_id`)
- `fact_supplychain` → `master_date` (many-to-one via `date`)

### Dashboard KPIs

| Metric | Value |
|---|---|
| Total Inventory Value | 179,537.70M |
| Warehouse Utilisation Rate | 34.08% |
| Inventory Turnover | 28.2K |
| Order Accuracy Rate | 91.33% |

### Dashboard visuals
- Warehouse Capacity Utilisation (gauge)
- Transportation Cost by Region & Category (clustered bar)
- Annual Units Sold Trend (line chart, 2020–2024)
- Average Lead Time by Category (donut chart)
- Order Fulfillment Status — Fulfilled: 838 | Pending: 248 | Canceled: 114
- Inventory Level by Region & Category (horizontal bar)

---

## ⏰ Step 5 — Scheduled Refresh

**Pipeline:** `Inventory_Refresh_Pipeline`
**Schedule:** Every day at **3:00 AM**
**Monitored via:** Microsoft Fabric Monitor

All pipeline runs show ✅ **Succeeded** status.

---

## 🛠️ Tech Stack

`Microsoft Fabric` · `PySpark` · `Delta Lake` · `Dataflow Gen2` · `Power BI Service` · `Fabric Pipelines` · `Synapse Notebooks`

---

## 📸 Screenshots

**Dashboard**
![Dashboard](Dashboard.PNG)

**Semantic Model**
![Model](Inventory_Model.PNG)

**Pipeline Monitor**
![Monitor](schedule_refresh.PNG)
