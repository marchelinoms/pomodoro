# Pomodoro Timer Web App Architecture

## High-Level Overview

This app will be built using .NET 8 for the backend and HTML/CSS/JavaScript for the frontend. The timer logic and UI will be fully managed client-side using JavaScript, while the backend serves static files and provides a minimal API for logging session data.

---

## System Architecture

```
+---------------------+       HTTP        +------------------------+
|   Browser Client    |<----------------->|   .NET 8 Backend/API   |
|  (HTML/JS/CSS UI)   |                   | (Static + Logging API) |
+---------------------+                   +------------------------+
           |                                       |
           |----------- Timer/Session Logic --------|
           |----------- UI Controls & State -------|
           |------ Local Storage for Settings -----|
           |----- AJAX Log API Call (Session End)--|
```

---

## Backend (.NET 8 ASP.NET Core Minimal API)

- **Serves static files**: index.html, style.css, app.js, and other assets.
- **Session logging API**: Single `POST /api/log-session` endpoint to append session data to a log file (e.g., `pomodoro-log.json` or `.csv`).
- **Settings**: No backend settings persistence; handled locally in-browser unless future requirements change.
- **No timer logic**: All timer and state management handled client-side.

---

## Frontend (HTML/CSS/JavaScript)

### UI Components

1. **Header**
   - Shows Pomodoro Timer title and total session count.

2. **Session Indicator**
   - Shows current session type (Work/Short Rest/Long Rest) and session number.

3. **Central Timer Display**
   - Shows remaining time and current status (running, paused, finished).

4. **Control Buttons**
   - Start, Reset, Skip, Settings.
   - State updates (enabled/disabled) based on session/timer state.

5. **Settings Dialog/Modal**
   - User-configurable: work/break durations, rounds, notifications, etc.
   - Stored locally in browser (LocalStorage).

### Timer Functionality

- Countdown logic using JS `setInterval`/`setTimeout`.
- Session switching via simple state machine for work/rest cycles.
- UI updates for session number, button states.
- UI prompts, transitions, notifications/sounds at session changes.
- Settings read/write via LocalStorage.
- On session completion, log session data to backend API.

---

## Example Folder Structure

```
/pomodoro-app/
├── wwwroot/
│   ├── index.html
│   ├── style.css
│   └── app.js
├── Logs/
│   └── pomodoro-log.json
├── Program.cs
└── ...
```

---

## Key Advantages

- **Separation of concerns**: Timer/state logic on frontend; backend only used for data logging.
- **Scalable/extensible**: Easy to add APIs, settings persistence, user accounts if required later.
- **Modern simplicity**: .NET 8 minimal API for maintainable backend.
- **User-friendly UI**: Modular components, responsive, easy to iterate.

---

## Optional Extensions

- Theme support via CSS variables & LocalStorage.
- API-based settings sync (optional for multi-device use).
- Dashboard UI for log/statistics.

---

## Example UI Skeleton

```html
<header>
  <h1>Pomodoro Timer</h1>
  <div id="session-count">Session 1</div>
</header>
<main>
  <div id="session-indicator">Work</div>
  <div id="timer-display">25:00</div>
  <div id="controls">
    <button id="start-btn">Start</button>
    <button id="reset-btn">Reset</button>
    <button id="skip-btn">Skip</button>
    <button id="settings-btn">Settings</button>
  </div>
</main>
<!-- Settings modal here -->
<script src="app.js"></script>
```

---

## Summary

- Frontend: Rich, interactive timer & UI, session & settings management in-browser.
- Backend: Minimal .NET API for serving static files and logging session events.
- Settings: Local (browser-only) unless otherwise required.

This architecture balances simplicity, effectiveness, and future extensibility for a Pomodoro timer learning project.