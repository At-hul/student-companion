# Student Companion

Student Companion is a student-support app for college students living away from home. It combines anonymous confessions, homesickness support resources, and interest-based student discovery in one simple Flutter experience backed by an Express API and Neon PostgreSQL.

The README explains the project structure, setup steps, API URL decisions, and final submission workflow.

## Features

- Anonymous Confessions Feed: view, create, react/unreact, report, and delete your own confessions.
- Homesickness Support: warm support cards, detailed tips, and a visible safety disclaimer.
- Similar Student Connect: select multiple interests, save them, view recommended students, open profiles, and send connect requests.
- Anonymous local identity: a UUID is stored locally with SharedPreferences so users can interact without account signup.
- Material 3 Flutter UI with Provider state management.

## Tech Stack

- Flutter
- Provider
- SharedPreferences
- Node.js
- Express
- Neon PostgreSQL

## Architecture

```text
Flutter app
  -> services call REST endpoints using ApiConfig.baseUrl
  -> providers manage loading, error, and feature state
  -> screens render feature flows

Express backend
  -> routes expose /api endpoints
  -> controllers validate input and run SQL queries
  -> Neon PostgreSQL stores confessions, reactions, reports, interests, students, connect requests, and support resources
```

## Project Structure

```text
student-companion/
  backend/
    database/        SQL schema and seed data
    scripts/         Database setup helper
    src/
      config/        Database connection
      controllers/   API request handlers
      middleware/    Error handling and rate limiting
      routes/        Express route definitions
      utils/         Shared validators
  mobile/
    lib/
      core/          Config, constants, theme, utilities
      models/        API data models
      providers/     Provider state classes
      screens/       Feature screens
      services/      REST API clients and local preferences
    test/            Flutter widget tests
  screenshots/       Add final app screenshots here
  apk/               Optional copy location for final APK
```

## Backend Setup

1. Create `backend/.env` from `backend/.env.example`.
2. Add your Neon connection string to `DATABASE_URL`.
3. Optionally add `DATABASE_URL_DIRECT` if you want to run schema and seed scripts from the terminal.
4. Install dependencies and start the API:

```bash
cd backend
npm install
npm run db:setup
npm run dev
```

Production-style start:

```bash
cd backend
npm start
```

Health check:

```text
GET http://localhost:4242/api/health
```

## Neon Database Setup

Run the schema and seed scripts after configuring `DATABASE_URL_DIRECT`:

```bash
cd backend
npm run db:setup
```

This creates the tables for confessions, confession reactions, reports, students, interests, anonymous user interests, connect requests, and support resources.

## Flutter Setup

```bash
cd mobile
flutter pub get
flutter run
```

The Flutter API base URL is configured in `mobile/lib/core/config/api_config.dart` and supports `--dart-define=API_BASE_URL=...`.

## API Base URL Notes

Use the backend root URL, not a URL ending in `/api`.

```text
Android emulator: http://10.0.2.2:4242
Chrome/Windows:   http://localhost:4242
Physical phone:   http://<PC_LOCAL_IP>:4242
Hosted backend:   https://<your-deployed-backend-url>
```

Examples:

```bash
cd mobile
flutter run -d chrome --dart-define=API_BASE_URL=http://localhost:4242
flutter run -d emulator-5554 --dart-define=API_BASE_URL=http://10.0.2.2:4242
flutter run --dart-define=API_BASE_URL=http://<PC_LOCAL_IP>:4242
```

For a physical Android phone, keep the phone and PC on the same Wi-Fi network and make sure the backend is listening on port `4242`.

## APK Build

```bash
cd mobile
flutter clean
flutter pub get
flutter analyze
flutter test
flutter build apk --release --dart-define=API_BASE_URL=http://<PC_LOCAL_IP>:4242
```

Expected APK path:

```text
mobile/build/app/outputs/flutter-apk/app-release.apk
```

For a hosted backend later, replace the physical-phone URL with the deployed backend URL:

```bash
flutter build apk --release --dart-define=API_BASE_URL=https://<your-deployed-backend-url>
```

## Screenshots

Add final screenshots in the `screenshots/` folder before submission. Recommended screenshots:

- Confessions feed
- Create confession dialog
- Homesickness support resources
- Support resource detail
- Interest selection
- Recommended students
- Student profile

## Design Decisions

- Anonymous UUIDs avoid login complexity while still supporting ownership, reactions, reports, and interest matching.
- Provider keeps state management simple and readable for a student project.
- Backend validation protects empty confessions, character limits, duplicate reactions, duplicate reports, and duplicate connect requests.
- API URLs are centralized in `ApiConfig` so screens do not hardcode backend addresses.
- The UI stays intentionally simple, readable, and student-friendly rather than over-designed.

## Limitations

- Connect requests are demo-oriented and do not include real chat or notifications.
- Anonymous identity is stored on the device; clearing app data creates a new identity.
- Support content is general wellness guidance and not a replacement for professional counselling or medical care.
- The app currently depends on the backend and Neon database being available during demo.

## Future Scope

- Add authenticated student accounts.
- Add in-app messaging after connect requests are accepted.
- Add moderation dashboard for reported confessions.
- Add campus-specific emergency and counselling contacts.
- Deploy the backend and configure CI checks.

## Demo Video

Use [DEMO_VIDEO_SCRIPT.md](DEMO_VIDEO_SCRIPT.md) for a 3-5 minute walkthrough covering the main features, technical explanation, limitations, and future improvements.

## Security

Never commit:

- `.env`
- Neon database URLs
- API keys
- Passwords
- Tokens
- Build secrets

Only `.env.example` should be public. The `.gitignore` excludes `.env`, `node_modules/`, `build/`, and `.dart_tool/`.

## Final Verification Commands

Backend:

```bash
cd backend
npm test
node --check src/server.js
node --check src/app.js
```

Mobile:

```bash
cd mobile
dart format .
flutter analyze
flutter test
flutter build apk --release --dart-define=API_BASE_URL=http://<PC_LOCAL_IP>:4242
```

## Commit And Push

```bash
git status
git add README.md DEMO_VIDEO_SCRIPT.md backend mobile
git commit -m "Complete Student Companion submission polish"
git push
```
