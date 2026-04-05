# Zara 价格跟踪器

[![Bright Data](https://img.shields.io/badge/Powered%20by-Bright%20Data-blue?style=flat-square)](https://bright.cn)
[![Zara Price Tracker](https://img.shields.io/badge/Zara%20Price%20Tracker-Managed%20Solution-orange?style=flat-square)](https://bright.cn/products/insights/price-tracker/zara)
[![Python](https://img.shields.io/badge/Python-3.9%2B-yellow?style=flat-square)](https://python.org)

[![Bright Insights Price Tracker](https://raw.githubusercontent.com/danielshashko/bright-insights-assets/main/price-tracker-hero-v2.png)](https://bright.cn/products/insights/price-tracker/zara)

实时 Zara 价格跟踪——Inditex 旗下领先的快时尚品牌。两种入门方式：**全托管**情报平台，或用于构建您自有 pipeline 的**自助式 API**。

---

## 选项 1：Bright Insights - AI 驱动的价格跟踪（推荐）

**[Bright Insights](https://bright.cn/products/insights/price-tracker/zara)** 是 Bright Data 的全托管零售情报平台。无需构建 scraper，无需维护基础设施——只需将结构化、可直接分析的价格数据交付到仪表板、数据源或您的 BI 工具中。

**为什么团队选择 Bright Insights：**
- 🚀 **零配置** - 借助开箱即用的仪表板和数据源，几分钟内即可上线
- 🤖 **AI 驱动的推荐** - 对话式 AI 助手可将数百万数据点即时转化为可执行洞察
- ⚡ **实时监控** - 支持按小时到按天的刷新频率，并提供即时告警（email、Slack、webhook）
- 🌍 **无限扩展** - 任意网站、任意地区、任意刷新频率
- 🔗 **即插即用集成** - AWS、GCP、Databricks、Snowflake 等
- 🛡️ **全托管** - Bright Data 自动处理 schema 变更、站点更新和数据质量问题

**关键用例：**
- ✅ **跟踪季节性折扣** 以及 Zara 上的促销活动
- ✅ **监控竞争对手定价** 并识别趋势
- ✅ 当商品价格低于目标价格时，**自动触发价格提醒**
- ✅ 监控 MAP 政策合规性并检测价格违规
- ✅ 跟踪竞争对手促销及促销动态
- ✅ 将干净、统一的数据直接输入动态定价算法或 AI 模型

> **每月 $250 起 - [获取定制报价 →](https://bright.cn/products/insights/price-tracker/zara)**

---

## 选项 2：通过 Web Scraper API 自助使用

更希望构建您自己的 pipeline？Bright Data 的 **Web Scraper API** 为您提供对 Zara 产品数据的编程访问——包括价格、库存状态、评论等——无需管理代理或抓取基础设施。

### 前置条件

- Python 3.9 或更高版本
- 一个 [Bright Data account](https://bright.cn)（提供免费试用）
- 一个 Bright Data **API token**（[如何获取](https://docs.bright.cn/general/account/account-settings#api-token)）
- 一个用于 Zara 产品的 Bright Data **Web Scraper ID**（[在 Web Scrapers 控制面板中找到您的 ID](https://bright.cn/cp/scrapers)）

### 设置

1. **克隆此 repository**

   ```bash
   git clone https://github.com/bright-cn/zara-price-tracker.git
   cd zara-price-tracker
   ```

2. **安装依赖**

   ```bash
   pip install -r requirements.txt
   ```

3. **配置凭证**

   将 `.env.example` 复制为 `.env`，并填入您的值：

   ```bash
   cp .env.example .env
   ```

   ```env
   BRIGHTDATA_API_TOKEN=your_api_token_here
   BRIGHTDATA_DATASET_ID=your_dataset_id_here
   ```

   > **查找您的 Web Scraper ID**
   > 登录 [Bright Data Control Panel](https://bright.cn/cp/scrapers)，进入 **Web Scrapers**，
   > 搜索 “Zara”，然后复制 Web Scraper ID（格式：`gd_xxxxxxxxxxxx`）。

---

## 用法

### 1. 通过 URL 跟踪特定产品

传入 Zara 产品 URL 列表以获取结构化价格数据：

```python
from price_tracker import track_prices

urls = [
    "https://www.zara.com/us/en/sample-product-p12345678.html",
    # Add more product URLs here
]

results = track_prices(urls)
for item in results:
    print(f"{item.get('title')} - {item.get('final_price', item.get('price'))} {item.get('currency', '')}")
```

或直接运行：

```bash
python price_tracker.py
```

### 2. 通过关键词发现产品

查找与关键词搜索匹配的产品：

```python
from price_tracker import discover_by_keyword

results = discover_by_keyword("laptop", limit=50)
```

### 3. 通过分类 URL 浏览产品

从 Zara 分类页面收集所有产品：

```python
from price_tracker import discover_by_category

results = discover_by_category(
    "https://zara.com/category/example",
    limit=100,
)
```

---

## 输出字段

每条结果记录包含以下字段：

| Field | Description |
|-------|-------------|
| `url` | 产品页面 URL |
| `title` | 产品名称 |
| `brand` | 品牌 |
| `initial_price` | 原价 |
| `final_price` | 促销价 / 当前价格 |
| `currency` | 货币代码 |
| `discount` | 折扣 |
| `in_stock` | 库存状态 |
| `color` | 可选颜色 |
| `size` | 可选尺码 |
| `category` | 产品类别 / 部门 |
| `images` | 产品图片 URL |
| `description` | 产品描述 |
| `timestamp` | 采集时间戳 |

### 示例输出

```json
[
  {
    "url": "https://www.zara.com/us/en/sample-product-p12345678.html",
    "title": "Example Product Name",
    "brand": "Example Brand",
    "initial_price": 59.99,
    "final_price": 44.99,
    "currency": "USD",
    "discount": "25%",
    "in_stock": true,
    "rating": 4.5,
    "reviews_count": 1234,
    "images": ["https://zara.com/images/product1.jpg"],
    "description": "Product description text...",
    "timestamp": "2025-01-15T10:30:00Z"
  }
]
```

---

## 高级选项

`trigger_collection()` 函数接受可选参数来控制数据采集：

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | - | 返回的最大记录数 |
| `include_errors` | boolean | `true` | 在结果中包含错误报告 |
| `notify` | string (URL) | - | 当 snapshot 准备就绪时调用的 webhook URL |
| `format` | string | `json` | 输出格式：`json`、`csv` 或 `ndjson` |

示例（带选项）：

```python
from price_tracker import trigger_collection, get_results

inputs = [{"url": "https://www.zara.com/us/en/sample-product-p12345678.html"}]
snapshot_id = trigger_collection(inputs, limit=200, notify="https://your-webhook.com/hook")
results = get_results(snapshot_id)
```

---

## 资源

- 🌟 [Zara Price Tracker - Bright Insights (Managed)](https://bright.cn/products/insights/price-tracker/zara)
- 🔧 [Zara Scraper API](https://bright.cn/products/web-scraper/zara)
- 📖 [Bright Data Web Scraper API Documentation](https://docs.bright.cn/scraping-automation/web-scraper-api/overview)
- 🗄️ [Web Scrapers Control Panel](https://bright.cn/cp/scrapers)
- 🔑 [How to get an API token](https://docs.bright.cn/general/account/account-settings#api-token)
- 🌐 [Bright Data Homepage](https://bright.cn)

---

*使用 [Bright Data](https://bright.cn) 构建——行业领先的网络数据平台。*