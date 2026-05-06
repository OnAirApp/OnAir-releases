# OnAir Configuration Reference

OnAir reads its settings from `~/Library/Application Support/OnAir/OnAir.cfg`.

Most settings can be edited via **Settings** in the app. Power users can also
edit the `.cfg` file directly. Each line uses the format `key:value`.

After saving the `.cfg`, restart OnAir for changes to take effect.

> ⚠️ **Sensitive credentials are not stored in this file.** Your Anthropic
> API key is stored in the macOS Keychain — the `.cfg` only contains the
> placeholder `@keychain`.

---

## Hardware Device

Settings that control the physical "On Air" sign device. OnAir works fully
without a device, but if you have one, configure it here.

### `device_type`
- **Type**: enum (`esphome`, `none`, or empty)
- **Default**: `esphome`
- **UI**: Settings → Device
- **Description**: Type of hardware device. Use `esphome` for an ESPHome-based
  sign (the supported hardware), or empty / `none` to run without a device.

### `esphome_name`
- **Type**: string (mDNS hostname)
- **Default**: `` (empty)
- **UI**: Settings → Device
- **Description**: mDNS hostname of your ESPHome device, e.g. `onair-c24d79`.
  OnAir resolves this to an IP address automatically.
- **Example**: `onair-a1b2c3`

### `esphome_ip`
- **Type**: string (IP address)
- **Default**: `` (empty)
- **UI**: filled automatically when `esphome_name` resolves
- **Description**: Cached IP address for the ESPHome device. OnAir overwrites
  this when mDNS resolution succeeds. Editing manually is rarely needed.
- **Example**: `192.168.1.42`

### `esphome_entity_id`
- **Type**: string
- **Default**: `onair_switch`
- **UI**: not exposed
- **Description**: Internal entity ID exposed by the ESPHome firmware. Don't
  change this unless you've customized the ESPHome firmware to use a different
  switch entity name.

### `device_skipped`
- **Type**: bool (`0` or `1`)
- **Default**: `0`
- **UI**: set automatically when the user skips the device step in setup
- **Description**: Marks that the user explicitly chose to run without a
  hardware device. When `1`, OnAir won't show device-related warnings.

---

## Service & Hours

Controls when OnAir is active.

### `default_service_state`
- **Type**: bool (`0` or `1`)
- **Default**: `1`
- **UI**: not exposed
- **Description**: Whether the service starts automatically (on) at app launch.
  When `0`, the user must manually start monitoring from the menu bar.

### `business_hours_enabled`
- **Type**: bool (`0` or `1`)
- **Default**: `0`
- **UI**: Settings → Hours
- **Description**: When `1`, OnAir only monitors meetings during business
  hours. Outside the configured window, the device stays off.

### `business_hours_start`
- **Type**: time (`HH:MM`, 24-hour)
- **Default**: `08:00`
- **UI**: Settings → Hours
- **Description**: Start time of the business hours window. Only used when
  `business_hours_enabled` is `1`.

### `business_hours_end`
- **Type**: time (`HH:MM`, 24-hour)
- **Default**: `17:00`
- **UI**: Settings → Hours
- **Description**: End time of the business hours window. Only used when
  `business_hours_enabled` is `1`.

### `business_hours_days`
- **Type**: comma-separated list (`mon`, `tue`, `wed`, `thu`, `fri`, `sat`, `sun`)
- **Default**: `mon,tue,wed,thu,fri`
- **UI**: Settings → Hours
- **Description**: Days of the week when business hours apply. Order does not
  matter. Only used when `business_hours_enabled` is `1`.
- **Example**: `mon,wed,fri`

### `week_start_day`
- **Type**: string (`monday` or `sunday`)
- **Default**: `monday`
- **UI**: Settings → Hours
- **Description**: First day of the week in the Statistics view. Affects how
  weekly data is grouped.

---

## Meeting Detection

Controls which platforms OnAir watches to detect meetings.

### `monitor_teams`
- **Type**: bool (`0` or `1`)
- **Default**: `1`
- **UI**: Settings → Monitoring
- **Description**: Detect Microsoft Teams meetings via the Teams API.

### `monitor_zoom`
- **Type**: bool (`0` or `1`)
- **Default**: `1`
- **UI**: Settings → Monitoring
- **Description**: Detect Zoom meetings.

### `monitor_camera`
- **Type**: bool (`0` or `1`)
- **Default**: `1`
- **UI**: Settings → Monitoring
- **Description**: Trigger when any app uses the camera (FaceTime, Meet,
  browser-based calls, etc.).

### `monitor_microphone`
- **Type**: bool (`0` or `1`)
- **Default**: `1`
- **UI**: Settings → Monitoring
- **Description**: Trigger when any app uses the microphone.

### `camera_cooldown`
- **Type**: int (seconds)
- **Default**: `3`
- **UI**: not exposed
- **Description**: Minimum time the camera must be idle before OnAir
  considers the meeting ended. Avoids flicker when apps briefly release
  the camera.

### `microphone_cooldown`
- **Type**: int (seconds)
- **Default**: `3`
- **UI**: not exposed
- **Description**: Minimum time the microphone must be idle before OnAir
  considers the meeting ended.

---

## Meeting Recording (Pro)

These settings only apply to OnAir Pro, which records meetings, transcribes
them, and generates AI summaries.

### `record_meetings`
- **Type**: bool (`0` or `1`)
- **Default**: `0`
- **UI**: Settings → Record meetings
- **Description**: Master switch for recording. When `1`, OnAir captures
  audio for every detected meeting and produces a transcription + AI summary.

### `recording_mic_device`
- **Type**: string (Core Audio device name)
- **Default**: `` (auto-select)
- **UI**: Settings → Record meetings → Microphone
- **Description**: Specific microphone to use for recording your voice.
  When empty, OnAir uses the system default.
- **Example**: `MacBook Pro Microphone`

### `recording_retention_days`
- **Type**: int (days)
- **Default**: `30`
- **UI**: Settings → Record meetings
- **Description**: How long to keep audio + transcription files on disk after
  a meeting ends.
  - `0`: don't keep any audio files — delete immediately after the summary
    is generated. Use this for maximum privacy.
  - `> 0`: keep files for that many days. Files older than this are purged
    automatically when OnAir launches.
- **Note**: The transcription text and AI summary are stored separately in
  the meeting database and are not affected by this setting.

### `min_recording_seconds`
- **Type**: int (seconds)
- **Default**: `30`
- **UI**: not exposed
- **Description**: Minimum meeting duration for OnAir to keep the recording.
  Meetings shorter than this are discarded (avoids saving accidental
  short calls).

### `transcription_segment_minutes`
- **Type**: int (minutes)
- **Default**: `5`
- **UI**: not exposed
- **Description**: Length of audio segments fed to Whisper. Whisper has a
  known issue with long audio (hallucinations), so OnAir splits longer
  recordings. The default of 5 minutes is well-tested.

---

## Summary Generation (Pro)

Configures the AI backend that produces meeting summaries from transcriptions.

### `summary_backend`
- **Type**: enum (`claude`, `ollama`, or empty)
- **Default**: `` (empty — user must choose at first run)
- **UI**: Settings → Record meetings → Backend
- **Description**: Which AI service generates summaries.
  - `claude`: Anthropic Claude API (cloud, requires API key)
  - `ollama`: Local Ollama instance (private, requires Ollama installed)
  - empty: no summaries (transcription only)

### `anthropic_api_key`
- **Type**: string (placeholder `@keychain` or empty)
- **Default**: `@keychain`
- **UI**: Settings → API Key
- **Description**: **The actual key is stored in the macOS Keychain**, not in
  this file. The `@keychain` value tells OnAir to read from the Keychain.
  Only used when `summary_backend` is `claude`.

### `claude_model`
- **Type**: string (Anthropic model identifier)
- **Default**: `claude-haiku-4-5-20251001` (when not set or empty)
- **UI**: not exposed
- **Description**: Which Claude model to use for summaries. Haiku 4.5 is the
  recommended default (good quality, cheap). Sonnet 4.5 produces slightly
  different summaries at ~3x the cost.
- **Example**: `claude-sonnet-4-5-20250929`

### `ollama_url`
- **Type**: URL
- **Default**: `http://localhost:11434`
- **UI**: Settings → Record meetings → Ollama URL
- **Description**: Base URL of the Ollama server. Use a remote URL if you
  run Ollama on a beefier machine on your network.
- **Examples**:
  - `http://localhost:11434`
  - `http://homeserver.local:11434`
  - `http://192.168.1.50:11434`

### `ollama_model`
- **Type**: string (Ollama model name)
- **Default**: `qwen3:8b`
- **UI**: Settings → Record meetings → Ollama model
- **Description**: Which model Ollama should use. Smaller models (8B) are
  faster but produce less detailed summaries. Larger models (32B+) need
  more RAM.

### `ollama_num_ctx`
- **Type**: int (tokens)
- **Default**: `16384`
- **UI**: not exposed (advanced setting)
- **Description**: Context window size for Ollama. Higher values allow longer
  meetings to fit in a single request, but use more RAM. OnAir's chunking
  feature splits long meetings automatically, so you rarely need to change
  this. Increase only if you have a powerful GPU and want fewer chunks.
- **Recommended**:
  - 8 GB RAM: `4096` or `8192`
  - 16 GB RAM: `8192` or `16384`
  - 32+ GB RAM: `32768`

### `summary_language`
- **Type**: string (`French`, `English`, `Auto`)
- **Default**: `Auto`
- **UI**: Settings → Record meetings → Summary language
- **Description**: Language for the generated summary. `Auto` detects from
  the transcription.

---

## Shortcuts & Focus Mode

Integrates OnAir with macOS Shortcuts and Focus modes.

### `focus_name`
- **Type**: string (Focus mode name)
- **Default**: `` (empty)
- **UI**: Settings → Focus
- **Description**: Name of the macOS Focus mode to enable when a meeting
  starts. Leave empty to disable Focus integration.
- **Example**: `Work`

### `shortcut_on_call_start`
- **Type**: string (Shortcut name)
- **Default**: `` (empty)
- **UI**: Settings → Shortcuts
- **Description**: Name of a Shortcuts.app shortcut to run when a meeting
  starts. Leave empty to skip.

### `shortcut_on_call_start_confirm`
- **Type**: bool (`0` or `1`)
- **Default**: `0`
- **UI**: Settings → Shortcuts
- **Description**: Show a confirmation dialog before running
  `shortcut_on_call_start`.

### `shortcut_on_call_end`
- **Type**: string (Shortcut name)
- **Default**: `` (empty)
- **UI**: Settings → Shortcuts
- **Description**: Name of a Shortcuts.app shortcut to run when a meeting
  ends.

### `shortcut_on_call_end_confirm`
- **Type**: bool (`0` or `1`)
- **Default**: `0`
- **UI**: Settings → Shortcuts
- **Description**: Show a confirmation dialog before running
  `shortcut_on_call_end`.

---

## Webhooks

Send HTTP requests when meetings start/end. Useful for integrating with
Home Assistant, IFTTT, or custom automation.

### `webhook_url`
- **Type**: URL
- **Default**: `` (empty)
- **UI**: Settings → Webhooks
- **Description**: URL to POST to on meeting state changes. Leave empty to
  disable webhooks.

### `webhook_confirm`
- **Type**: bool (`0` or `1`)
- **Default**: `0`
- **UI**: Settings → Webhooks
- **Description**: Show a confirmation dialog before sending the webhook.

---

## Maintenance

OnAir auto-migrates the `.cfg` file at startup:

- **Missing keys** are added with their default values.
- **Legacy keys** (no longer used by the code) are removed silently.

Both events are logged to `~/Library/Logs/OnAir.log`.

If you want to start fresh, quit OnAir and delete `OnAir.cfg`. On the next
launch, OnAir will recreate it with all defaults.
