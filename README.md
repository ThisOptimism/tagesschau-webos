
# Tagesschau WebOS App

A lightweight Tagesschau video and livestream viewer for WebOS TVs and projectors.

The app uses the public Tagesschau API (https://www.tagesschau.de/api2u/channels/) to display available channels, on-demand videos, and live streams.
Playback uses an HTML5 video player with full remote control support.

---

## Project Structure

tagesschau-webos-app/
- index.html        → Channel overview with remote navigation
- details.html      → Video and livestream player
- README.md         → Project documentation

---

## Features

- Channel overview that lists all Tagesschau channels from the API.
- Video player that supports both on-demand videos and live HLS streams.
- Full remote control support:
  - Arrow keys: navigate between items
  - OK / Enter: open selected video
  - Play / Pause: control playback
  - Left / Right: seek ±10 seconds
  - Back: return to the previous page
- Automatic livestream detection (channels with `streams.adaptivestreaming` are played directly).
- HTML5 + HLS.js playback (uses native video when possible, falls back to HLS.js).
- Tagesschau-inspired layout and colors for a clean TV-friendly design.

---

## How It Works

1. **Fetch channels**
   The app loads data from:
   `https://www.tagesschau.de/api2u/channels/`

   Each channel item may include:
   - `sophoraId`: used for on-demand videos (e.g., `/api2u/video-1517764.json`)
   - `streams.adaptivestreaming`: direct HLS livestream URL

   Example:
   ```json
   {
     "title": "Im Livestream: tagesschau24",
     "sophoraId": "tagesschau-vierundzwanzig-9622",
     "streams": {
       "adaptivestreaming": "https://tagesschau-live.ard-mcdn.de/tagesschau/live/hls/de/master.m3u8"
     }
   }
   ```

2. **Open the details page**
   - For livestreams:
     `details.html?stream=<HLS_URL>&title=<Title>&poster=<Image>`
   - For videos:
     `details.html?id=<sophoraId>`

3. **Play the video**
   The player:
   - Fetches `/api2u/<id>.json` for on-demand videos
   - Plays the stream directly when `?stream=` is given
   - Uses `<video>` for playback and automatically requests fullscreen

---

## Remote Control Mapping

| Button | Action |
|--------|---------|
| Arrow keys | Move selection on the index screen |
| OK / Enter | Open selected video |
| ▶ Play | Start or resume playback |
| ⏸ Pause | Pause playback |
| ← / → | Seek backward or forward 10 seconds |
| Back | Return to previous screen |

---

## Running the App

### Run locally in a browser

1. Clone the repository:
   ```bash
   git clone https://github.com/<yourusername>/tagesschau-webos-app.git
   cd tagesschau-webos-app
   ```
2. Open `index.html` in your browser.

Note: Some browsers restrict autoplay or cross-origin HLS streams.
If a livestream does not start, check the console or use a different browser.

---

### Run on a WebOS TV or projector

1. Install the WebOS CLI:
   https://webostv.developer.lge.com/sdk/installation

2. Package the app:
   ```bash
   ares-package .
   ```

3. Install to your device:
   ```bash
   ares-install com.tagesschau.app_0.0.1_all.ipk
   ```

4. Launch:
   ```bash
   ares-launch com.tagesschau.app
   ```

---

## Notes and Limitations

- The Tagesschau API is public but undocumented; endpoints may change.
- Some videos may not include poster images.
- Devices without native HLS support will automatically use HLS.js.
- Internet access is required for playback.
- Audio defaults to 100% volume.

---

## Future Improvements

- Add latest news feed (`/api2u/news/`)
- Add favorites or recently watched (using localStorage)
- Implement search and category filters
- Include `appinfo.json` and icons for WebOS packaging
- Add smoother focus animations for TV navigation

---

## Author

Built for the WebOS community.

---

## License

Code licensed under the MIT License.
All content, videos, and branding belong to ARD / Tagesschau and are used only for educational and non-commercial purposes.
