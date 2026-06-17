# Product Requirements Document: "SwissPass" (Citizenship Exam App)

## 1. Product Goals
Deliver a premium, offline-first exam preparation app for high-income expats applying for Swiss citizenship. The app must feel reliable, fast, and hyper-localized to the user's specific Canton and Gemeinde.

## 2. Technical Strategy & Architecture
- **Offline-First:** All questions (Federal, Cantonal, Communal) are fetched as a structured JSON payload and cached locally on the device.
- **Background Sync:** On app launch, the client checks the `latest_version_hash` from the server. If a newer version exists, the client downloads the updated JSON silently in the background. No blocking loaders for the user.
- **Pre-existing Tech:**  Crashlytics, and Notifications are handled by pre-configured Firebase modules. Do not reinvent these.

## 3. Detailed User Flow
- **Onboarding:**
  1. User authenticates.
  2. Selects Target Canton -> Selects Target Gemeinde.
  3. App sets the local context and filters the JSON question bank accordingly.
- **Dashboard:**
  - Displays "Readiness Score" (calculated based on historical correct/incorrect answers).
  - Quick action to "Resume Daily Goal".
- **Quiz Flow:**
  - Displays question, 3-4 options.
  - Upon selection: Immediate visual feedback (Green/Red) + Haptic feedback.
  - Explanation card expands below the question to provide historical/legal context.
  - Swipe or tap to proceed to the next question.

## 4. Logic & Rules (Edge Cases)
- **Spaced Repetition Light:** Questions answered incorrectly should be pushed to a "Review Queue" and prioritized in the next daily session.
- **Paywall Logic:** - Federal questions: Always unlocked (Freemium hook).
  - Cantonal & Gemeinde questions: Locked behind a "Premium" state boolean. If `isPremium == false`, show Paywall when navigating to these categories.
- **State Management:** User progress (correct/incorrect arrays) must be strictly saved to local storage first, then debounced and synced to the user's document in the backend to ensure no data loss during subway commutes.

## 5. Scalability Notes (Write Code With These In Mind)
- Build the data schema so that adding "Germany" or "France" as root-level entities requires zero logic changes, only new JSON files.
- UI components should accept dynamic theme variables, allowing us to easily switch the "Swiss Red" accents to "German Gold" in future multi-app deployments.