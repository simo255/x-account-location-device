# X Account Location & Device Info

A Browser Extension and Userscript that displays country flags and device/platform information next to X (~~Twitter~~) usernames. Shows where users are located and what device they're using, all in a clean, unobtrusive way.

## 📰 Background

This script leverages X's official "Country Labels" feature announced on November 16, 2025. X now displays account locations derived from signals like IP addresses, app store regions, and posting behavior. This feature helps identify authentic accounts and combat foreign interference, though it raises privacy concerns for users in repressive regions.

The script extracts this official location and device data from X's GraphQL API, presenting it in a user-friendly format with flags and device emojis.

## ✨ Features

- **Country Flags**: Displays flag emojis next to usernames based on account location from X's API
- **Device Indicators**: Shows device/platform emojis (📱💻🖥️🌐) indicating how users are connected
- **Country Blocking**: NEW! Block tweets from specific countries with an intuitive modal interface
- **Real API Data**: Extracts actual location and device info directly from X's GraphQL API
- **Hover Tooltips**: Detailed information on hover (e.g., "Connected via: Android App")
- **Smart Caching**: 24-hour cache optimized for X's rate limits with manual clearing options
- **Cross-Platform**: Works on Firefox, Chrome, Edge, and other browsers with Tampermonkey
- **Language Agnostic**: Works regardless of your X interface language (English, German, Japanese, etc.)

<hr>
[x-account-location-device-blocked-country.webm](https://github.com/user-attachments/assets/68ce25bb-8807-43b8-8d72-8e24ae35bc7c)
<hr>

> [!NOTE]
> **Windows Support Added:** Windows 10/11 doesn't natively support flag emojis. This extension now automatically detects Windows and replaces the broken characters with high-quality Twemoji images (the same ones Twitter uses), so flags look perfect on all platforms! 🎨

## 🚀 Installation

### Prerequisites
- [Tampermonkey](https://www.tampermonkey.net/) browser extension
- Modern web browser (Chrome, Brave, Firefox, Edge, etc.)

### Install Steps (Userscript)
1. Install Tampermonkey from the link above
2. Click here to install the script: [X Account Location & Device Info](https://github.com/xaitax/x-account-location-device/raw/main/x-account-location-flag.user.js)
3. Tampermonkey will prompt you to install - click "Install"
4. Visit [x.com](https://x.com) and you'll see flags and device indicators next to usernames!

### Install Steps (Browser Extension)
You can also install this as a standalone extension without Tampermonkey.

**Chrome / Edge / Brave:**
1. Download or clone this repository
2. Go to `chrome://extensions`
3. Enable **Developer mode** (top right)
4. Click **Load unpacked**
5. Select the `extension` folder from this repository

**Firefox:**
1. Download or clone this repository
2. Go to `about:debugging#/runtime/this-firefox`
3. Click **Load Temporary Add-on...**
4. Select the `manifest.json` file inside the `extension` folder

*Note: Temporary add-ons in Firefox are removed when you close the browser.*

### Permanent Firefox Installation
To install permanently on Firefox, you need to sign the extension:
1. Zip the contents of the `extension` folder (or use the provided `extension.zip`).
2. Go to the [Firefox Developer Hub](https://addons.mozilla.org/en-US/developers/addon/submit/distribution).
3. Select **"On your own"** to distribute it yourself.
4. Upload the zip file and wait for the automated review (usually takes a few minutes).
5. Download the signed `.xpi` file.
6. Drag and drop the `.xpi` file into Firefox to install it permanently.

## 📱 Usage

### Basic Usage
Once installed, the script runs automatically on X.com. You'll see:
- `🇺🇸 📱` next to usernames from the United States using mobile devices
- `🇯🇵 💻` next to usernames from Japan using computers
- `🇬🇧 🌐` next to usernames from the UK using web browsers

### Hover for Details
Hover over the device emoji to see detailed information like:
- "Connected via: United States Android App"
- "Connected via: Web"
- "Connected via: iPhone"

### Country Blocking
Click the **"Block Countries"** link in the X sidebar (just above the "More" button) to:
- Select one or multiple countries to block
- Search countries by name
- View all countries with their flags
- See a counter showing how many countries are blocked
- Tweets from blocked countries will be automatically hidden from your feed

Blocked countries are saved in localStorage and persist across browser sessions.

### Cache Management
The script caches data for 24 hours. To manage cache and blocking:

**In Browser Console (F12):**
```javascript
// View cache statistics
XFlagScript.getCacheInfo()

// Clear all cached data
XFlagScript.clearCache()

// Toggle extension on/off
XFlagScript.toggle()

// Open country blocker modal
XFlagScript.openBlocker()

// Get list of blocked countries
XFlagScript.getBlockedCountries()

// Debug info
XFlagScript.debug()
```

## 📜 Changelog

### v1.4.0
- **Country Blocking**: Added new sidebar link and modal to block tweets from specific countries
- **Integrated UI**: Modal matches X's design system perfectly with dark theme, smooth animations, and native styling
- **Multi-Select**: Select multiple countries to block with visual feedback
- **Search**: Search functionality to quickly find countries
- **Persistent Storage**: Blocked countries are saved in localStorage
- **Auto-Hide**: Tweets from blocked countries are automatically hidden from your feed
- **API Extensions**: New console commands [`XFlagScript.openBlocker()`](extension/content.js:874) and [`XFlagScript.getBlockedCountries()`](extension/content.js:877)

### v1.3.0
- **Windows Support**: Added automatic Twemoji image replacement for Windows users, fixing the "missing flag" issue on Chrome/Edge/Brave.
- **Profile Header Support**: Now correctly displays flags in user profile headers (even for unverified accounts).
- **Bug Fixes**: Fixed an issue where flags would only appear on the first tweet of a user and not subsequent ones.
- **Performance**: Optimized DOM scanning to be much lighter on CPU by only processing new nodes.
- **Accuracy**: Removed misleading fallback that showed your own device type when data was missing.

### v1.2.0
- **Dual Mode**: Now available as both a standalone Browser Extension (Chrome/Firefox) and a Userscript.

### v1.1.0
- **Instant Speed**: Rewrote the country lookup engine to be O(1) (instant), removing lag on busy timelines.
- **Language Fix**: Now forces X to return English country names, so flags work even if your interface is in German, French, etc.
- **Firefox & Windows Fix**: Updated font stacks to properly render flag emojis on Windows and Firefox.
- **Smart Fallbacks**: If X's API doesn't return a device, we now intelligently guess based on your browser to show *something* useful.
- **Robustness**: Added a fallback authentication mechanism so the script works even if it misses the initial API handshake.

## 🔧 How It Works

### API Integration
- Intercepts X's own API calls to capture authentication headers
- Queries X's GraphQL API for user profile data
- Extracts `account_based_in` (location) and `source` (device) fields
- Maps locations to country flag emojis
- Maps device info to platform emojis

## 🛠️ Technical Details

### API Endpoints
- Uses X's public GraphQL API
- Only queries public profile information
- Rate limited to ~50 requests per timeframe (automatically handled with queuing and caching)

### Privacy
- No data collection or transmission to third parties
- All API calls go directly to X's servers
- Data is cached locally in your browser only

## 🤝 Contributing

Found a bug or have a feature request? Feel free to:
1. Open an issue on GitHub
2. Submit a pull request
3. Suggest improvements

## 👤 Author

**Alexander Hagenah**

- X/Twitter: [@xaitax](https://x.com/xaitax)
- LinkedIn: [https://www.linkedin.com/in/alexhagenah/](https://www.linkedin.com/in/alexhagenah/)
- Website: [https://primepage.de](https://primepage.de)

---

**Made with ❤️ for the X community**
