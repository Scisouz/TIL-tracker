# PostHog TIL — Today I Learned Tracker

A lightweight, single-file learning tracker (`v1.1`) built to explore and demonstrate PostHog's core product engineering features in a zero-backend static architecture. Entries are stored locally in `localStorage` (starting empty with no hardcoded data).

## PostHog Features Instrumented

- **Custom Event Capture & Schema (`snake_case`):**
  - `app_opened` (tracks active `theme_mode`: `standard` or `focus`)
  - `learning_created` (saving a topic to the study queue)
  - `learning_logged` (completing a queued item or logging directly with integer `difficulty` `1–5`, `mood`, `topic`, `source`, `from_queue`, `has_note`, and `has_link`)
  - `filter_applied`, `feedback_opened`, `shortcut_button_chased`, `shortcut_button_caught`
- **User Identification & Switching:** Starts anonymous; entering a user ID and clicking **Set** (or pressing `Enter`) calls `posthog.identify()` with person property `name`. Switching to a different user ID automatically calls `posthog.reset()` first so PostHog tracks each user as a distinct person.
- **Session Replay & Privacy Masking:** Uses the `ph-no-capture` class on the private reflection input (`#private-input`) so sensitive text is never sent in events and is masked in recordings before leaving the client.
- **Feature Flags:** Listens asynchronously via `posthog.onFeatureFlags()` for the `focus-mode` flag to toggle a distraction-free logging layout (hiding stats and the feed).
- **Surveys & Autocapture:** `#share-feedback-btn` captures `feedback_opened` (ready for a PostHog Popover Survey triggered on the `feedback_opened` event, or a Feedback Button survey targeting `#share-feedback-btn`), while the 10-dodge hedgehog chase naturally exercises PostHog's autocapture and rage-click detection.

## Setup

1. Create a free project in [PostHog](https://posthog.com) and copy your **Project API Key** (`phc_...`).
2. Provide your PostHog config using either method:
   - **Runtime injection (recommended for Netlify):** Configure Netlify Snippet Injection to set `window.ENV_POSTHOG_KEY` (and optionally `window.ENV_POSTHOG_HOST`, which defaults to `https://us.i.posthog.com`).
   - **Direct edit (quick local testing):** Replace `'YOUR_POSTHOG_PROJECT_API_KEY'` in `index.html` with your `phc_...` key (avoid committing a live key if you prefer keeping it in Netlify).
3. In PostHog, enable **Surveys** in **Project Settings** and add your live domain under **Authorized URLs** when deploying to Netlify or GitHub Pages.
4. *(Local preview note)* If no key is configured, `posthog.init()` is skipped and `safeCapture()` logs all custom events to the browser DevTools console so you can still test the app locally.
