# 🌌 Ultimate Dynamic Wallpaper

A high-performance, minimalist dynamic wallpaper engine designed for **Lively Wallpaper** and standalone web browsers. It features high-resolution imagery from multiple providers, a dual-unit real-time weather system, and a flicker-free transition engine.



## 🚀 Key Features

* **Multi-Source Imagery**: Native support for Wallhaven (4K), Bing Daily, Windows Spotlight, NASA APOD, and Unsplash.
* **Dual-Unit Weather Engine**: Simultaneous Celsius and Fahrenheit display with automated IP-based geolocation.
* **WMO Icon Mapping**: Weather codes are translated into descriptive icons (☀️, ⛈️, 🌫️, etc.) for real-time environmental monitoring.
* **Flicker-Free Engine**: Uses a dual-layer crossfade system (`#wall1` & `#wall2`) with a virtual preloader to ensure images are fully buffered before appearing.
* **Proactive Feedback**: Top-mounted progress bar and bottom-left status alerts provide transparency into API health and fetch latency.
* **Contextual UI**: Settings sidebar intelligently hides/shows fields (like API keys or categories) based on the active provider.
* **Smart Caching**: Configurable polling intervals (1–24 hours) to reduce network overhead and prevent API rate-limiting.

---

## 🛠️ Configuration & Usage

### 1. Lively Wallpaper (Desktop)
1.  Add the folder containing `index.html` and `LivelyProperties.json` to Lively.
2.  Right-click the wallpaper and select **Customize**.
3.  The Web Sidebar will be disabled in this mode to prioritize Lively's native controls.

### 2. Standalone Browser (Web)
1.  Open `index.html` in any modern browser.
2.  **Hover** over the **left edge** of the screen to reveal the Settings Sidebar.
3.  **Click the background** to manually skip to the next image in the current batch.

---

## 📡 API Providers & Logic

| Provider | Data Type | Notes |
| :--- | :--- | :--- |
| **Wallhaven** | Backgrounds | Supports General, Anime, and People categories. |
| **Bing/NASA** | Backgrounds | Fetches the daily "Image of the Day" with localized metadata. |
| **Open-Meteo** | Weather | Fetches `weather_code` and converts `temp_c` to `temp_f`. |
| **IP-API** | Location | Provides city/region data for localized weather accuracy. |

---

## 🔧 Technical Architecture

### Proactive Management Logic
Following proactive SLA strategies, the engine treats API fetches as monitored services:
* **SafeFetch Wrapper**: Prevents script crashes during provider downtime using try-catch blocks.
* **Progressive Loading**: Visualizes "Latency" ($30\% \rightarrow 60\% \rightarrow 100\%$) during the fetch-to-buffer pipeline.
* **Cache Validation**: Uses a "Stale-While-Revalidate" pattern checking timestamps ($CurrentTime - CachedTime > Expiry$) before hitting external APIs.



### Weather Code Mapping
The engine maps **WMO (World Meteorological Organization)** codes to visual assets:
* **Clear/Mainly Clear (0-1)**: ☀️ / 🌤️
* **Cloudy (2-3)**: ⛅ / ☁️
* **Atmospheric (45-48)**: 🌫️ (Fog/Misty)
* **Precipitation (61-95)**: 🌧️ (Rain) / ⛈️ (Storm)

---

## 🛠️ Troubleshooting

### Images are not loading (Black Screen)
* **CORS Restrictions**: Some providers require a proxy. The engine uses `allorigins.win`. If the proxy is down, Bing and Spotlight will fail. Switch to **NASA** or **Unsplash** to verify.
* **API Limits**: NASA and Wallhaven have hourly limits. If "Batch Size" stays at 0, wait 15 minutes or click **"Force Refresh Cache"**.

### Weather is stuck at "--°C"
* **Location Blocking**: Ad-blockers or VPNs may block `ipapi.co`. Click the **"Refresh Weather & Location"** button in the sidebar to re-trigger detection.

### Settings are not visible
* **Theme Conflict**: v2.1.0 includes a high-contrast CSS fix for dropdowns. Ensure you are using the latest `index.html` to avoid "white-on-white" text issues.

---

## 📝 Change Log (v1.0.2)
* Added Dual-Temperature ($C/F$) logic.
* Implemented WMO-based icon and description mapping.
* Refactored Settings Sidebar for contextual visibility.
* Restored top-mounted Progress Bar for fetch observability.
