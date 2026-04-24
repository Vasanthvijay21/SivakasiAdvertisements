# Sivakasi Advertisements — Real Estate Website

A complete, single-file real estate listing website for Sivakasi built with HTML, Tailwind CSS, JavaScript, and Firebase Firestore.

---

## Features

- **No OTP / No password login** — login with name + mobile number
- **Property listings** — sale & rent, with categories: land, house, office, factory
- **Owner phone hidden** from public UI — shown only in admin panel
- **"Get Owner Contact"** button → shows admin phone + WhatsApp redirect
- **Buyer/Tenant requests** form
- **Maintenance services** booking (plumbing, electrical, painting, cleaning)
- **Admin panel** with hardcoded login — view all properties (with owner phones), delete listings, view requests & service bookings
- **Search & filter** by location, category, listing type
- **Mobile responsive** with clean, warm amber/gold theme

---

## Setup Instructions

### Step 1: Create Firebase Project

1. Go to [https://console.firebase.google.com](https://console.firebase.google.com)
2. Click **"Add project"** → name it (e.g. `sivakasi-ads`) → Continue
3. Disable Google Analytics (optional) → Create project

### Step 2: Enable Firestore

1. In Firebase console → **Firestore Database** → **Create database**
2. Choose **"Start in test mode"** (for development) → Select region → Done

### Step 3: Register Web App & Get Config

1. In Firebase console → Project settings (⚙️) → **"Add app"** → Web (`</>`)
2. Register app name → Copy the `firebaseConfig` object

### Step 4: Replace Config in index.html

Open `index.html` and replace the placeholder config (~line 22):

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT_ID.appspot.com",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

### Step 5: Set Firestore Security Rules

In Firebase console → Firestore → **Rules** tab, paste:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{doc} {
      allow read, write: if true;
    }
    match /properties/{doc} {
      allow read: if true;
      allow create: if true;
      allow delete, update: if false;
    }
    match /requests/{doc} {
      allow read, write: if true;
    }
    match /services/{doc} {
      allow read, write: if true;
    }
  }
}
```

---

## Admin Login

| Field    | Value           |
|----------|-----------------|
| Username | `admin`         |
| Password | `sivakasi@2024` |

Admin contact phone (shown to buyers): `7904216920`

Change these in `index.html` around line 30.

---

## Firestore Collections

| Collection   | Fields |
|-------------|--------|
| `users`      | name, phone, createdAt |
| `properties` | title, desc, price, location, category, listType, ownerPhone, ownerName, createdAt, active |
| `requests`   | buyerName, buyerPhone, budget, location, propType, purpose, notes, createdAt |
| `services`   | userName, userPhone, serviceType, address, date, notes, createdAt, status |

---

## Deployment — GitHub Pages

1. Create a new GitHub repository
2. Upload `index.html` to the root
3. Settings → Pages → Source: `main` branch, `/ (root)` → Save
4. Live at: `https://yourusername.github.io/repo-name/`

## Deployment — Netlify (Drag & Drop)

1. Go to [https://app.netlify.com/drop](https://app.netlify.com/drop)
2. Drag `index.html` into the drop zone — live in seconds!

---

## Security Notes

- Owner phone numbers are **never rendered** in public property cards
- They are stored in Firestore but only displayed inside the password-protected admin panel
- For production use, implement Firebase Authentication for stronger security
