# Student Companion Demo Video Script

Target length: 3-5 minutes.

## 1. Introduction

Hello, this is Student Companion, a Flutter and Node.js project built for students who are living away from home. The goal is to provide a simple, supportive app where students can share anonymous thoughts, find homesickness support resources, and discover similar students through common interests.

The app uses Flutter with Provider on the frontend, Node.js and Express on the backend, and Neon PostgreSQL for data storage.

## 2. Anonymous Confessions

First, I will open the Anonymous Confessions section.

Here students can read anonymous confessions posted by others. The feed loads from the backend, and each confession shows its content, time, and reaction count.

I can tap Share to create a new confession. Empty confessions are blocked, and there is a 300 character limit to keep posts short. After posting, the new confession appears in the feed.

I can react to a confession, remove my reaction, report inappropriate content, and delete only the confessions created from this device.

## 3. Homesickness Support

Next is the Homesickness Support section.

This screen loads support resources from the backend. Each card gives a calm, readable summary. Tapping a card opens detailed guidance with helpful steps.

The safety disclaimer is visible so users understand that these are general wellness suggestions and not a replacement for professional counselling or emergency help.

## 4. Similar Student Connect

Now I will open Similar Student Connect.

Students can select multiple interests and save them. The app sends those interests to the backend using the anonymous device ID.

After saving, the app loads recommended students with shared interests. If no match is found, the app shows an empty state instead of a broken screen.

I can open a student profile, view their bio and interests, and send a connect request. If I already sent a request, the backend handles the duplicate and the app shows a friendly message.

## 5. Technical Explanation

The Flutter app is organized into models, services, providers, and screens.

Services handle API calls, providers manage loading and error states, and screens focus on the UI.

The API base URL is centralized in `api_config.dart` and supports `--dart-define=API_BASE_URL=...`, which makes it work for Chrome, Android emulator, physical phone testing, and later hosted backend deployment.

The Express backend has separate routes and controllers for confessions, support resources, interests, students, and connect requests. Neon PostgreSQL stores the application data.

Secrets such as `.env` and the Neon database URL are ignored by Git. Only `.env.example` is included publicly.

## 6. Future Improvements

Future improvements could include real student accounts, accepted-request messaging, notifications, a moderation dashboard for reported confessions, and campus-specific support contacts.

## 7. Conclusion

Student Companion is designed to be simple, useful, and demo-ready. It shows a complete Flutter and Express flow with real backend data, clean UI states, and a student-friendly purpose.
