# ajanta-studio.github.io

This repository hosts the **Google IMA HTML5 Video Ad Player** and authorized digital seller verification records (`ads.txt` and `app-ads.txt`) for the **Ajanta Studio** ecosystem across Desktop (macOS, Windows, Linux) and Mobile/Tablet platforms.

---

## Live Endpoints

Once deployed to GitHub Pages, the following endpoints will be active:

* **Ad Player:** `https://ajanta-studio.github.io/player.html`
* **Custom Ad Tag Testing:** `https://ajanta-studio.github.io/player.html?adTag=<ENCODED_VAST_TAG_URL>`
* **Web ads.txt:** `https://ajanta-studio.github.io/ads.txt`
* **App ads.txt:** `https://ajanta-studio.github.io/app-ads.txt`
* **Landing Page:** `https://ajanta-studio.github.io/`

---

## How to Publish to GitHub Pages

1. **Create Repository:**
   - In GitHub under the **`ajanta-studio`** organization, create a new public repository named:
     ```
     ajanta-studio.github.io
     ```
2. **Push Local Files:**
   ```bash
   cd ajanta-studio.github.io
   git init
   git remote add origin https://github.com/ajanta-studio/ajanta-studio.github.io.git
   git branch -M main
   git add .
   git commit -m "Initial commit of IMA ad player and ads.txt"
   git push -u origin main
   ```
3. **Verify Deployment:**
   - Go to your repository on GitHub $\rightarrow$ **Settings** $\rightarrow$ **Pages**.
   - Verify that **GitHub Pages** is active and serving from `/ (root)` of the `main` branch.
   - HTTPS will automatically activate within a couple of minutes.

---

## Flutter Integration (`flutter_inappwebview`)

The ad player integrates with Flutter across **macOS, Windows, Linux, Android, and iOS** using a single JavaScript handler:

```dart
InAppWebView(
  initialUrlRequest: URLRequest(
    url: WebUri('https://ajanta-studio.github.io/player.html?adTag=${Uri.encodeComponent(adTagUrl)}'),
  ),
  onWebViewCreated: (controller) {
    controller.addJavaScriptHandler(
      handlerName: 'onAdEvent',
      callback: (args) {
        final data = Map<String, dynamic>.from(args[0]);
        final event = data['event'];
        if (event == 'COMPLETED' || event == 'ALL_ADS_COMPLETED') {
          // Unlock feature
        }
      },
    );
  },
)
```

### Event Reference
The player dispatches the following lifecycle events to Flutter:

| Event Name | Description |
| :--- | :--- |
| `STARTED` | Ad playback began. Includes `duration`, `title`, and `isSkippable`. |
| `PAUSED` | Ad playback was paused. |
| `RESUMED` | Ad playback was resumed. |
| `SKIPPED` | User skipped the ad. |
| `COMPLETED` | The current video ad completed playback. |
| `ALL_ADS_COMPLETED` | All ads in the pod / request have completed. |
| `ERROR` | Ad failed to load or play. Includes `error` description. |
