# 🌌 Ultimate Dynamic Wallpaper

A high-performance, minimalist dynamic wallpaper engine designed for **Lively Wallpaper** and standalone web browsers. It features high-resolution imagery from multiple providers, real-time weather, and a flicker-free transition system.

## 🚀 Key Features

* **Multi-Source Imagery**: Support for Wallhaven (4K), Bing Daily, Windows Spotlight, NASA APOD, and Unsplash.
* **Flicker-Free Engine**: Uses a dual-layer crossfade system with a virtual preloader to ensure images are fully buffered before appearing.
* **Fetch Progress Bar**: A sleek, top-mounted progress bar providing real-time feedback during API syncs and image buffering.
* **Contextual UI**: Settings sidebar automatically adapts; Wallhaven-specific categories hide themselves when using other providers.
* **Dual Mode**: 
    * **Desktop Mode**: Integrates natively with Lively Wallpaper's property menu.
    * **Web Mode**: Hover settings drawer for manual configuration in browsers.
* **Smart Caching**: Configurable API cache (1–24 hours) to reduce network overhead and prevent API rate-limiting.
* **Bidirectional Controls**: Manually skip forward or go back through the current batch using UI buttons or background clicks.

---

## 🛠️ Configuration

### 1. Lively Wallpaper (Recommended)
1.  Add the folder containing `index.html` and `LivelyProperties.json` to Lively Wallpaper.
2.  Right-click the wallpaper and select **Customize**.
3.  Adjust the **Image Provider**, **Refresh Batch**, and **Cycle Interval** sliders.

### 2. Standalone Browser
1.  Open `index.html` in any modern browser.
2.  **Hover** your mouse over the **left edge** of the screen to reveal the settings drawer.
3.  Click the **background** to manually skip to the next image.

---

## 📁 File Structure

* `index.html`: The core engine, styles, and logic.
* `LivelyProperties.json`: Defines the native UI controls for the Lively Wallpaper application.
* `README.md`: Documentation and troubleshooting.

---

## 📡 API Providers & Logic

| Provider | Resolution | Notes |
| :--- | :--- | :--- |
| **Wallhaven** | 4K / 8K | Supports General, Anime, and People categories. |
| **Bing** | 1080p/4K | Official "Image of the Day" with localized metadata. |
| **NASA** | HD+ | Astronomy Picture of the Day. Supports "HD" URLs where available. |
| **Spotlight** | 4K | Original high-quality images used by Windows Lockscreen. |
| **Unsplash** | Variable | Requires a personal API key for high-volume use. |



---

## 🔧 Technical Details

* **Transitions**: Managed via a `transitionWallpaper()` function that handles cross-fading between two absolute-positioned `<img>` tags.
* **Caching**: State is preserved in `localStorage`. The batch of images is stored as a JSON array, while sync timestamps are stored to manage auto-refreshes.
* **Navigation**: Uses modular arithmetic `(index - 1 + length) % length` to allow infinite looping in both forward and backward directions.
* **Safety**: All cross-origin requests are routed through a CORS proxy where necessary (Bing/Spotlight) to prevent browser security blocks.

---

## 🛠️ Troubleshooting

If the wallpaper is not displaying images or the weather is stuck at `--°C`, check the following:

### 1. Images Not Loading (Black Screen)
* **CORS Proxy Issues**: This engine uses `allorigins.win` to bypass security restrictions for Bing and Spotlight. If their server is down, these sources will fail. 
    * *Fix*: Switch to **NASA APOD** or **Unsplash** (which don't require the proxy).
* **API Rate Limits**: Providers like NASA and Wallhaven have hourly limits. 
    * *Fix*: Click **"Force Refresh Now"** in the settings. If it still fails, wait 15 minutes or switch providers.
* **Broken Cache**: Sometimes the stored batch of URLs contains a "dead" link.
    * *Fix*: Use the **"Force Refresh Now"** button in the sidebar to wipe the local cache and fetch a new batch.

### 2. Weather Not Updating
* **Location Permissions**: The engine uses `ipapi.co` to guess location via IP. VPNs or ad-blockers can sometimes block this.
* **HTTPS requirement**: Browsers may block Geolocation/Weather APIs if served over unencrypted connections.

### 3. Settings Not Saving
* **Local Storage**: Preferences are saved in `localStorage`. Incognito modes or "Clear history on exit" settings will reset your wallpaper.
* **Lively vs. Web Conflict**: If Lively Mode is active, the Web Sidebar is dimmed and unclickable to prevent conflicts. Use the Lively "Customize" menu.

---

## 📝 Commit History Summary
* **Refactor**: Shifted to modular fetch engine with `safeFetch` error handling.
* **UX**: Added top-mounted progress bar for download feedback.
* **Controls**: Added bidirectional navigation (Prev/Next) and contextual settings visibility.
