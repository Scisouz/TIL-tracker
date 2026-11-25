# PostHog TIL — Today I Learned Tracker

A lightweight, single-file learning tracker built to explore and demonstrate PostHog's core product engineering features in a zero-backend static architecture.

## PostHog Features Instrumented

- **Custom Event Capture & Schema (`snake_case`):**
  - `app_opened` (tracks active `theme_mode`)
  - `learning_created` (saving a topic to the study queue)
  - `learning_logged` (completing a queued item or logging directly with integer `difficulty` `1–5`, `mood`, `topic`, and `source`)
  - `filter_applied`, `feedback_opened`, `shortcut_button_chased`, `shortcut_button_caught`
- **User Identification:** Calls `posthog.identify()` with custom person properties (`role: 'tse_candidate'`) to merge anonymous and identified timelines.
- **Session Replay & Privacy Masking:** Uses the `ph-no-capture` class on the private reflection input so sensitive text is masked in recordings before leaving the client.
- **Feature Flags:** Listens asynchronously via `posthog.onFeatureFlags()` for the `focus-mode` flag to toggle a distraction-free logging layout.
- **Surveys & Autocapture:** `#share-feedback-btn` triggers `feedback_opened` (ready for a PostHog Survey selector/event trigger), while the 10-second hedgehog chase naturally exercises PostHog's rage-click detection.

## Setup

1. Create a free project in [PostHog](https://posthog.com) and copy your **Project API Key** (`phc_...`).
2. Replace `YOUR_POSTHOG_PROJECT_API_KEY` in `index.html` with your key.
3. Deploy `index.html` to Netlify or GitHub Pages and add your live domain under **Project Settings → Authorized URLs**.
