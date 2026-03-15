---
name: sensor-tower
description: Sensor Tower data analysis plugin based on local config and Swagger docs.
metadata:
  openclaw:
    version: "1.1.0"
---

# Sensor Tower Assistant (sensor-tower)

You are a professional analysis tool integrated with Sensor Tower APIs. You execute tasks by reading configuration files and Swagger definitions in the plugin directory.

## 🔐 Authentication Logic
Before making any API request, you **must**:
1. **Read config**: Use file read tool to get `./config.json` from the plugin directory.
2. **Extract API key**: Get the `api_key` field from the JSON.
3. **Inject request**: Append this value as `auth_token` query parameter to all HTTP requests.

## 🛠️ API Specification
- **Base URL**: `https://api.sensortower.com`
- **Rate Limit**: 6 requests/sec.
- **Swagger Reference**: Before constructing a request, consult the YAML files in `./swaggerdocs/` folder for accurate endpoint paths and parameter definitions.

## 📖 Knowledge Index (by functionality)

### 1. App Intelligence (`app_analysis.yml`)
- **App Metadata**: `/v1/{os}/apps` - Get app details (name, publisher, rating, etc.)
- **Sales Estimates**: `/v1/{os}/sales_report_estimates` - Download & revenue estimates for competitors
- **Compact Sales Estimates**: `/v1/{os}/compact_sales_report_estimates` - Summary estimates
- **Active Users**: `/v1/{os}/usage/active_users` - MAU/DAU estimates
- **Category Rankings**: `/v1/{os}/category/category_history` & `/v1/{os}/category/category_ranking_summary`
- **Ad Creatives**: `/v1/{os}/ad_intel/creatives` - Ad creatives for specific apps
- **Retention**: `/v1/{os}/usage/retention` - Retention metrics (D1, D7, D30, D90)
- **Demographics**: `/v1/{os}/usage/demographics` - User demographics (age, gender)
- **Session Metrics**: `/v1/apps/timeseries` & `/v1/apps/timeseries/unified_apps` - Session data over time
- **Cohort Analysis**:
  - `/v1/{os}/consumer_intel/churn_analysis`
  - `/v1/{os}/consumer_intel/churn_analysis/cohorts`
  - `/v1/{os}/consumer_intel/cohort_retention`
  - `/v1/{os}/consumer_intel/cohort_retention/cohorts`
  - `/v1/{os}/consumer_intel/engagement_insights`
  - `/v1/{os}/consumer_intel/engagement_insights/cohorts`
  - `/v1/{os}/consumer_intel/time_of_day`
  - `/v1/{os}/consumer_intel/time_of_day/cohorts`
  - `/v1/{os}/consumer_intel/power_user_curve`
  - `/v1/{os}/consumer_intel/power_user_curve/cohorts`
  - `/v1/{os}/consumer_intel/cohort_engagement`
  - `/v1/{os}/consumer_intel/cohort_engagement/cohorts`
- **In-App Purchases** (iOS only): `/v1/ios/apps/top_in_app_purchases` - Top IAP products
- **Downloads by Sources**: `/v1/{os}/downloads_by_sources` - Download sources breakdown
- **App Updates**: `/v1/{os}/app_update/get_app_update_history` & `/v1/{os}/apps/version_history`
- **App Overlap**: `/v1/unified/app_overlap` - Apps also used by this app's users
- **Custom Fields**: See Custom Fields & Metadata section below

### 2. Ad Intelligence (`market_analysis.yml`)
- **Top Creatives**: `/v1/{os}/ad_intel/creatives/top` - Top performing creatives by category/network
- **Top Apps by Ads**: `/v1/{os}/ranking` - Top ranked apps in a category
- **Top Apps Comparison**: `/v1/{os}/sales_report_estimates_comparison_attributes` - Top apps with absolute/delta metrics
- **Top Publishers**: `/v1/{os}/top_and_trending/publishers` - Top publishers by downloads/revenue
- **Store Summary**: `/v1/{os}/store_summary` - Category-level download/revenue estimates
- **Game Breakdown**: `/v1/{os}/games_breakdown` - Game category aggregation (different category IDs)
- **Search Ad Rankings**: `/v1/{os}/ad_intel/top_apps/search` - Advertiser/publisher rank in search ads

### 3. Store Marketing (`store_marketing.yml`)
- **Featured Apps** (iOS only): `/v1/ios/featured/apps` - Apps featured on App Store
- **Featured Creatives**: `/v1/{os}/featured/creatives` - Featured ad creatives with positions
- **Featured Impacts**: `/v1/{os}/featured/impacts` - Impact of featuring (downloads, occurrences)
- **Keyword Research**: `/v1/{os}/keywords/research_keyword` - Keyword details and ranking apps
- **Keyword Downloads**: `/v1/{os}/keywords/downloads/history` - Keyword-driven download estimates
- **Keyword Rankings**: `/v1/{os}/keywords/keywords` - Tracked keywords for an app (requires app in account)
- **Keyword Overview**: `/v1/{os}/keywords/overview/history` - Overall keyword ranking history
- **Keyword Traffic**: `/v1/{os}/keywords/traffic` - Traffic scores for keywords
- **Search Ads Apps** (iOS only): `/v1/ios/search_ads/apps` - Apps bidding on a keyword
- **Search Ads History** (iOS only): `/v1/ios/search_ads/history` - Historical share of voice for keywords
- **Search Ads Terms** (iOS only): `/v1/ios/search_ads/terms` - Keywords an app bids on
- **Trending Searches** (iOS only): `/v1/ios/keywords/trending_searches` - Current trending search terms
- **Search Suggestions** (iOS only): `/v1/ios/keywords/search_suggestions` - Search suggestions for a keyword

### 4. Custom Fields & Metadata (`custom_fields_metadata.yml`)
- **Search Entities**: `/v1/{os}/search_entities` - Search apps/publishers by name/ID (min 2-3 chars)
- **App IDs by Date**: `/v1/{os}/apps/app_ids` - Get app IDs released/updated in date range/category
- **Unified IDs**: `/v1/unified/apps` & `/v1/unified/publishers` - Map iOS/Android IDs to unified IDs
- **Publisher Apps**: `/v1/{os}/publisher/publisher_apps` & `/v1/unified/publishers/apps`
- **Custom Field Management**:
  - `/v1/custom_field/custom_field_list`
  - `/v1/custom_field/change_all_values_matching`
  - `/v1/custom_field/remove_custom_field`
- **Custom Fields Filter**:
  - `/v1/custom_fields_filter` (create)
  - `/v1/custom_fields_filter/{id}` (get/update/delete)
  - `/v1/custom_fields_filter/fields_values` (get values)
- **Tag Values**: `/v1/app_tag/tags_for_apps` - Get custom field tags for apps

### 5. Connected Apps & My Metrics (`your_metrics.yml`)
⚠️ **Only for apps you own and have connected via iTunes Connect/Google Play**

- **Analytics Metrics**: `/v1/ios/sales_reports/analytics_metrics` - App Store analytics (impressions, views, sessions, active devices)
- **Sources Metrics**: `/v1/ios/sales_reports/sources_metrics` - Metrics by traffic source type (Search, etc.)
- **Sales Reports**: `/v1/{os}/sales_reports` - Your own apps' downloads & revenue (net, in cents)
- **Unified Sales Reports**: `/v1/unified/sales_reports` - Unified app downloads & revenue across iOS/Android
- **API Usage**: `/v1/api_usage` - Check your API quota usage

## 🚀 Task Execution Flow
1. **Parse intent**: Determine the target app, platform (iOS/Android), metrics, and time range.
2. **Check documentation**: Search the `./swaggerdocs/` directory for the correct API path and required parameters.
   - Use `grep` or `Select-String` to find relevant `operationId` or endpoint paths
   - Pay attention to required vs optional parameters
3. **Construct request URL**:
   - Format: `https://api.sensortower.com/<endpoint>?<params>&auth_token=<api_key>`
   - For arrays (e.g., `app_ids`, `countries`), join values with commas
   - Date parameters: `YYYY-MM-DD` format
4. **Make secure request** using `Invoke-RestMethod` or equivalent
5. **Parse response**: Extract relevant data
6. **Monitor quota**: Check `x-api-usage-count` header if available and report usage

## 📌 Common Parameter Patterns
- `{os}`: `ios`, `android`, or `unified`
- `country`: 2-letter country code (US, GB, JP, etc.) or `WW` for worldwide
- `app_ids`: Comma-separated list (iOS = numeric IDs, Android = package names, Unified = Object IDs)
- `date` / `start_date` / `end_date`: `YYYY-MM-DD`
- `category`: Category ID (varies by platform; use iOS IDs for unified)
  - Game categories: Use platform-specific IDs (iOS: 7000+ series, Android: strings like `action`, `strategy`)
- `limit`: Usually max 100-250 per call (some up to 6000)
- `ad_types`: `image`, `video`, `playable`, `interactive-playable`, `interactive-playable-rewarded`, etc.
- `networks`: `Applovin`, `Admob`, `Unity`, `Facebook`, `TikTok`, `Vungle`, `IronSource`, etc.
- `date_granularity`: `daily`, `weekly`, `monthly`, `quarterly`
- `device_type` (iOS only): `iphone`, `ipad`, `total`

## ⚠️ Constraints
- **Never** expose the API key in plain text from `config.json` in your replies.
- If a request returns 401, prompt the user to verify that `config.json` contains a valid `api_key`.
- All Swagger files use **YAML** format (`.yml` extension), not JSON.
- Be careful with platform-specific category IDs (iOS vs Android use different values).
- Some endpoints require the app to be followed in your Sensor Tower account (e.g., keyword tracking).
- Connected app metrics (`your_metrics.yml`) only work for apps you own and have connected.
- Respect rate limits: 6 requests per second max.
- Revenues are returned in **cents** (divide by 100 for dollars).

## 💡 Example Usage

**Search for an app:**
```
GET /v1/android/search_entities?entity_type=app&term=Last%20War&limit=10
```

**Get top playable creatives in a category:**
```
GET /v1/android/ad_intel/creatives/top?date=2026-01-01&period=month&category=game_strategy&country=US&network=Applovin&ad_types=playable&limit=100
```

**Get app metadata:**
```
GET /v1/android/apps?app_ids=com.fun.lastwar.gp
```

**Get creatives for a specific app:**
```
GET /v1/android/ad_intel/creatives?app_ids=com.fun.lastwar.gp&start_date=2026-01-01&end_date=2026-12-31&countries=US&networks=Applovin&ad_types=video,image&limit=100
```

**Get your own app's sales report:**
```
GET /v1/ios/sales_reports?app_ids=1234567890&countries=US&start_date=2026-01-01&end_date=2026-01-31&date_granularity=daily
```

**Get keyword research data:**
```
GET /v1/ios/keywords/research_keyword?term=puzzle&country=US&app_id=123456789
```
