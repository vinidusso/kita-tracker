# Kita Finder & Tracker (Neuperlach, München)

Interactive dashboard to track Kindergarten applications for September 2027 (Ü3, age 3).

---

## 🚀 Setting Up Real-Time Cloud Sync (Firebase)

The dashboard works 100% offline with `localStorage`. To enable **live two-way sync** between you and your wife with email login:

### 1. Create a Free Firebase Project (2 mins)
1. Go to [https://console.firebase.google.com/](https://console.firebase.google.com/) and click **"Add Project"** (e.g. `kita-tracker`). You do **not** need Google Analytics.
2. Select the free **Spark Plan** (no credit card required).

### 2. Enable Authentication
1. In the left menu, click **Build > Authentication > Get Started**.
2. Under the **Sign-in method** tab, enable **Email/Password** (save).

### 3. Enable Cloud Firestore
1. In the left menu, click **Build > Firestore Database > Create database**.
2. Choose your location (e.g. `eur3 (europe-west)`).
3. Under the **Rules** tab, paste this secure rule to only allow you and your wife:
   ```javascript
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /families/{familyId} {
         // Allow only your two email addresses
         allow read, write: if request.auth != null && 
           request.auth.token.email in ["YOUR_EMAIL@gmail.com", "WIFES_EMAIL@gmail.com"];
       }
     }
   }
   ```
   *(Replace with your actual email addresses and click **Publish**).*

### 4. Get the Web Config snippet
1. In Project Settings (gear icon ⚙️ at top left) > **General**, scroll down to **"Your apps"**.
2. Click the Web icon `</>` to register an app.
3. Copy the `firebaseConfig` object, for example:
   ```json
   {
     "apiKey": "AIzaSy...",
     "authDomain": "kita-tracker.firebaseapp.com",
     "projectId": "kita-tracker",
     "storageBucket": "kita-tracker.appspot.com",
     "messagingSenderId": "...",
     "appId": "..."
   }
   ```
4. On your tracker web page, click **"⚙️ Connect Firebase"** and paste this JSON.
5. Create your account with your email & password. Have your wife do the same on her device.

---

## 🌐 Deploying to GitHub Pages

1. Push this folder to a GitHub repository:
   ```bash
   git init
   git add .
   git commit -m "feat: initial kita finder tracker"
   gh repo create kita-finder --public --source=. --push
   ```
2. Enable GitHub Pages:
   ```bash
   gh repo edit --enable-pages --pages-branch main
   ```
3. Share the generated `https://<username>.github.io/kita-finder/` link with your wife!
