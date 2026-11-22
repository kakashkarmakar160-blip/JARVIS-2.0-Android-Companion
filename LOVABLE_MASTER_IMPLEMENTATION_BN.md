# JARVIS 2.0 — LOVABLE MASTER IMPLEMENTATION PROMPT (বাংলা)

তুমি একজন Senior Full-Stack Engineer এবং Android Kotlin Engineer হিসেবে কাজ করবে। এই ZIP-এর বর্তমান JARVIS 2.0 project-কে ভিত্তি হিসেবে নাও। বিদ্যমান কার্যকর UI, CSS, JavaScript, ভিডিও, Assets, Memory, Settings, Chat, Voice, GPS, Worker integration বা অন্য কোনো feature অকারণে মুছবে/নষ্ট করবে না। আগে পুরো project audit করো, তারপর ধাপে ধাপে implementation করো। শুধু mockup বা inactive button বানাবে না। যে feature বাস্তবে implement করা যাচ্ছে না, সেটিকে স্পষ্টভাবে “Requires Android companion / Not available yet” দেখাবে।

## 1. Product Goal
JARVIS 2.0 হবে একটি Bengali + English Personal AI Assistant: Web Dashboard + Native Android Companion App + Secure Bridge. Web app একা Android-এর সব app/system control করতে পারে না। তাই website ও Android companion আলাদা module হিসেবে তৈরি করো এবং authenticated API দিয়ে যুক্ত করো।

## 2. Existing Project Preservation
- প্রথমে existing project structure, index.html, app.js, jarvis-enhancements.js/css, worker.js, Kotlin files, manifest ও docs inspect করো।
- Existing features ও assets preserve করো; নতুন code modular files-এ রাখো।
- Existing Cloudflare Worker endpoint configurable default হিসেবে রাখো: https://little-forest-7b75.kakashkarmakar159.workers.dev
- এটি public Worker URL, API key নয়। Gemini API key কখনো frontend, localStorage, GitHub বা ZIP-এ hardcode করবে না। Worker secret হিসেবে GEMINI_API_KEY ব্যবহার করো।
- Backend URL Settings থেকে পরিবর্তনযোগ্য হবে। Backend health check এবং পরিষ্কার error state থাকবে।

## 3. UI / UX
- Reference: Futuristic Glass Workspace; dark navy glass panels, cyan/blue/purple glow, premium sci-fi look.
- Landscape-first তিনটি panel: LEFT Your World, CENTER JARVIS Core/Visual Hub, RIGHT Chat Board.
- Portrait mode-এ panel গুলো responsive stack/drawer হবে; কোনো horizontal overflow নয়।
- Memory, Skills, Character, Settings button সমান dimensions ও spacing-এ থাকবে।
- সব button-এ real action, loading/disabled/error/success states এবং keyboard/touch support থাকবে।
- Robot logo asset থাকলে app icon, splash, header ও login-এ ব্যবহার করো; প্রয়োজনমতো logo clear-space বজায় রাখো।

## 4. Web Assistant Features
- Bengali/English chat, conversation history, streaming AI responses, retry and cancel.
- Browser speech recognition ও speech synthesis feature detection করে ব্যবহার করো; unsupported হলে manual text input fallback।
- Voice start/stop, mute, interrupt, language, speed, voice selection controls। Wake word browser background-এ সবসময় চলবে বলে দাবি করবে না।
- Notes CRUD, local memory/preferences, export/import backup; cloud sync কেবল opt-in ও authenticated backend দিয়ে।
- Agent workspace: Planner, Researcher, Coder, Builder, Reviewer, File, Memory, Automation. Agent state বাস্তব task lifecycle থেকে আসবে; fake progress animation নয়।
- Visual Hub-এ Mermaid flowchart preview, safe HTML preview in sandboxed iframe, code editor/preview, file picker and file analysis (selected files only)।
- Custom commands exact-match বা explicit prefix matching করবে; সাধারণ বাক্যের ভেতরে accidental trigger নয়। RGB theme command R/G/B এবং custom theme settings।
- GPS browser geolocation only after permission; destination coordinates থেকে external maps navigation URL তৈরি করা যাবে। Route distance/time নিজে বানিয়ে দেখাবে না।

## 5. Android Companion App (Kotlin)
আলাদা, buildable Android Studio project তৈরি করো। WebView-only permission workaround নয়। Companion app-এ পরিষ্কার onboarding, device pairing, bridge URL/token, permission dashboard, connection status, audit log এবং disconnect/revoke controls থাকবে।

### Android capabilities — user consent required
- Launch installed apps: PackageManager + explicit launch intents; Android package visibility rules মানতে হবে।
- Open WhatsApp/Instagram/YouTube target screen via supported deep links/intents. Third-party apps-এর private screen/state সরাসরি read বা arbitrary control করার প্রতিশ্রুতি দেবে না।
- YouTube/Instagram search: official app deep link বা browser search open করবে।
- Scroll/tap automation: AccessibilityService optional, user manually enables it in Android Settings; service-এর visible disclosure, allowlist, stop switch, action confirmation এবং audit log থাকবে। Accessibility দিয়ে password/OTP/secure fields পড়া বা গোপন data scrape করা যাবে না। App policy/Android restrictions মানতে হবে।
- Contacts: READ_CONTACTS permission; search local contacts. Call শুরু করার আগে contact/name/number confirm করো; ACTION_DIAL default, direct ACTION_CALL কেবল explicit permission ও confirmation-এর পর।
- WhatsApp chat: official wa.me / package deep link দিয়ে নির্দিষ্ট chat open; message prefill only. Send করার আগে user confirmation এবং final send user নিজে করবে; supported official API ছাড়া background send নয়।
- Notifications: NotificationListenerService, opt-in; শুধু notification preview metadata প্রয়োজনমতো forward, sensitive notification content default off, local toggle, revoke option।
- Files: Android Storage Access Framework / ACTION_OPEN_DOCUMENT / ACTION_OPEN_DOCUMENT_TREE; user-selected files only. Broad file scraping নয়।
- Flashlight: CameraManager torch API after permission; availability check.
- Battery/network/device info: public Android APIs; Wi-Fi/Bluetooth settings পরিবর্তন করতে হলে Android Settings panel খুলবে, restricted toggles silently বদলাবে না।
- Location: foreground location permission; background location only if core feature truly needs it and with separate explanation. Foreground service notification and stop control.
- Camera: open camera intent or CameraX after permission. No covert capture.
- SMS, call log, microphone background, accessibility and notification access: request only when feature enabled and explain why. Respect Google Play policy and Android version restrictions.

## 6. Secure Bridge
- Pairing flow: generate random one-time pairing code on Android, exchange for short-lived device token over HTTPS, allow revoke/rotate.
- Every bridge request authenticated; validate origin, token, payload schema, timestamp and replay protection. Rate limit commands.
- Never expose unauthenticated endpoint that can execute phone actions.
- Web requests a proposed action; Android displays confirmation for sensitive actions. Android returns actual result; web must not pretend success.
- Use HTTPS in production. Cleartext HTTP only for local development and disabled in release builds.
- Bridge commands should be allowlisted, typed and minimal, e.g. open_app, open_url, search_youtube, open_whatsapp_chat, request_call_confirmation, get_battery, get_notifications, pick_file, set_torch. No arbitrary shell/ADB execution.

## 7. AI Action Planning
AI must return structured action proposals, not execute arbitrary text. Schema: {"reply":"...","actions":[{"type":"...","args":{},"requires_confirmation":true}]}.
Validate action type and args against allowlist on backend and Android. Ask clarification when contact/app/destination is ambiguous. Require confirmation for calls, messages, deleting/sharing files, accessibility automation and any external side effect. If action unsupported, say so honestly and provide safe alternative.

## 8. Privacy & Security
- Secure login should use real backend auth (or a documented auth provider), not a frontend password-only lock.
- Do not store raw API keys in browser storage. Keep secrets server-side.
- Provide per-permission toggles, revoke, data export/delete, local/cloud memory controls, retention settings and activity history.
- Do not upload contacts, notifications, files, location or chat content to AI unless the user explicitly chooses that action.
- Never collect OTPs, passwords, banking credentials or hidden third-party message data.

## 9. Testing / Delivery
- Add setup instructions in Bengali and English.
- Add .env.example containing placeholders only.
- Add unit tests for command validation, bridge auth, permission state and action confirmation.
- Test desktop + Android Chrome responsive layout, landscape/portrait, offline/backend error, permission denied, unsupported API and revoked permissions.
- Build website for Lovable deployment and GitHub Pages where compatible; document differences.
- Build Android APK only after Gradle project compiles. Do not label source ZIP as APK.
- Provide a feature matrix: Working in Web, Requires Android Companion, Requires Backend, Restricted/Unsupported.
- Before final delivery run lint/build/tests and report actual results, known limitations and exact setup steps.

## 10. Priority order
Phase 1: preserve and polish web dashboard, chat, voice, notes, memory, theme, diagram/code preview.
Phase 2: secure Worker AI routing and structured action proposals.
Phase 3: native Android pairing, app launching, contact search + dialer confirmation, file picker, battery, torch, location.
Phase 4: notification listener and optional AccessibilityService with strong user controls.
Phase 5: agent orchestration, sync, advanced automation.

IMPORTANT: Do not claim “full phone control” unless each action is implemented, tested on a real Android device, and permitted by Android and the target app. Prefer official intents, deep links and APIs. Never bypass Android security, user consent, app privacy or platform restrictions.
