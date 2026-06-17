# Product Requirements Document (PRD) 
**Project:** SwissPass - Swiss Citizenship Exam Preparation App
**Version:** 3.0 (Standalone & Secure Firebase Architecture)
**Platform:** iOS & Android (React Native / Flutter / Swift etc.)

## 1. Product Vision and Goals
SwissPass is a "Premium" exam preparation app targeting high-income white-collar professionals immigrating to Switzerland.
- **Time Optimization:** Promises maximum success in minimum time by analyzing weaknesses, rather than just solving random questions.
- **Zero Latency:** Designed to be 100% offline-first to work seamlessly during train commutes and in mountainous areas.
- **Trust and Privacy:** The user's exam performance is kept securely and fully encrypted on their own device.

## 2. Architectural Decisions
The app is architected as "Backend-less" to eliminate server costs and ensure maximum speed.

### 2.1. Data Management
- All Federal, Cantonal, and Communal (Gemeinde) questions will be embedded into the app during build time as an `SQLite` database or local `JSON` schema.
- Question updates will be distributed as "binary updates" (app updates) via the App Store / Play Store.

### 2.2. Firebase Security and Infrastructure
Firebase will only be used for analytics, crash tracking, and "Error Reporting". To prevent Hack/Spam risks, the following 3-layer security is MANDATORY:
1. **Firebase App Check:** (Apple DeviceCheck and Google Play Integrity) must be enabled to reject bot requests at the API level.
2. **Security Rules:** Rules for the `error_reports` collection must be exactly as follows:
   ```javascript
   match /error_reports/{reportId} {
     allow read, update, delete: if false; // Reading and modifying is FORBIDDEN
     allow create: if request.resource.data.reason is string && 
                      request.resource.data.reason.size() < 500;
   }
   ```
3. **Rate Limiting:** A maximum limit of 3 error reports per device per day (Client-side logic) will be enforced.

## 3. Revenue and Subscription Model
**RevenueCat** (or Superwall) will be used for payment infrastructure and subscription management.
- **Freemium Hook:** All Federal level questions will be offered to users for FREE.
- **Paywall:** The Paywall will be triggered when the user attempts to access "Cantonal" or "Communal" questions.
- **Packages:** Monthly, Yearly, and **Lifetime**. Considering the target audience (high-income expats), the "Lifetime" package will be the most highlighted option in the UI.

## 4. Core Features (MVP Scope)

### 4.1. Hyper-Localization
The user selects Canton and Gemeinde during onboarding. The app instantly filters questions specific to that region and updates the dashboard.

### 4.2. Spaced Repetition Algorithm
Solved questions and accuracy rates are stored in the device's local memory (AsyncStorage/SQLite).
- Incorrectly answered questions are sent to the "Weaknesses" pool.
- In daily test mode, the algorithm increases the probability of these questions appearing by 40%.

### 4.3. Readiness Score
Based on the user's overall correct/incorrect history, the "Probability of Passing the Exam" is displayed as a percentage chart (e.g., 85%) on the Dashboard.

### 4.4. Local Notifications (Retention)
On-device (Local Notifications) that do not require an internet connection will be set up. E.g., A reminder at 20:00 every evening: "Last 5 minutes to complete your daily goal of 15 questions."

### 4.5. Transparent Paywall and Analytics
Firebase Analytics events are mandatory for tracking paywall performance:
- `paywall_viewed`
- `paywall_converted`

## 5. User Flow
1. **Onboarding:** Splash Screen -> Privacy Consent -> Canton Selection -> Gemeinde Selection.
2. **Dashboard:** "Readiness Score" Chart, "Start Now" button, Weaknesses summary.
3. **Learning Mode:** Questions appear in flashcard or quiz format. Correct/incorrect answers are supported by immediate visual and haptic (vibration) feedback.
4. **Mock Exam:** A practice exam created with real exam time limits and rules (e.g., 40% Federal, 40% Cantonal, 20% Communal questions).

## 6. Data Schema and Scalability
To easily expand to the German or French markets in the future, the data schema must start with the country code in the root directory.

```json
{
  "market_code": "CH", 
  "available_regions": [
    {
      "region_id": "ZH",
      "region_name": "Zürich",
      "sub_regions": ["Winterthur", "Uster"]
    }
  ],
  "questions": [
    {
      "id": "q_101",
      "level": "federal", // or cantonal, communal
      "region_id": "all",
      "text": "How many cantons does Switzerland have?",
      "options": ["20", "23", "26", "28"],
      "correct_answer_index": 2,
      "explanation": "Switzerland consists of 26 cantons (20 full and 6 half cantons)."
    }
  ]
}
```
*Note: When a new country is added, only this JSON file will be changed; the UI and core logic will remain the same.*
