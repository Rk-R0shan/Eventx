🚀 EventX — A Hackathon-Ready Event Management App

EventX is a complete, single-file event management web application built for speed and simplicity. It's a perfect starter kit for a hackathon, a personal project, or a demo, providing a full-featured, two-sided marketplace for event organizers and students.

All code—HTML, CSS, and JavaScript—is contained in one index.html file. It's powered by Tailwind CSS for styling and Firebase (Auth & Firestore) for the backend, all loaded via CDN.


<img width="1851" height="972" alt="Screenshot 2025-10-25 094057" src="https://github.com/user-attachments/assets/33428a96-5ce3-47fd-a6b8-ec637d3b9fc9" />

<!--  -->

✨ Core Features

👨‍🎓 For Students (Attendees)

Browse & Search: Find events by name, category, or venue.

Event Details: View event descriptions, pricing, and available slots.

Registration: A simple, multi-step registration flow.

Mock Payment: A mock payment portal (UPI, Card, NetBanking) to simulate a full transaction.

Student Dashboard: View a list of all registered events in one place.

👩‍💼 For Organisers

Organiser Dashboard: A central hub to manage all created events.

Event CRUD: Create, Read, Update, and Delete events.

Publish Control: Toggle events between "Draft" and "Published" status.

Participant Viewing: View a list of all registered participants for each event.

Export Data: Export participant lists to CSV or Excel (.xlsx).

Announcements: Post announcements that appear on the event details page for students.

Real-time Stats: See registration counts and available slots update in real-time.

🔑 General & Tech Features

Single-File Architecture: All code lives in index.html. Incredibly easy to deploy.

Role-Based Auth: Separate sign-up and dashboards for "Student" and "Organiser" roles using Firebase Auth.

Real-time Database: Built on Firebase Firestore, all data updates in real-time across the app.

Modern UI: A sleek, responsive dark-mode UI built with Tailwind CSS.

Tiny SPA Router: A custom-built, hash-based router handles navigation without page reloads.

💻 Tech Stack

Frontend: HTML5, Tailwind CSS (via CDN), Vanilla JavaScript (ESM), Lucide Icons

Backend: Firebase Authentication, Firebase Firestore (NoSQL DB)

Libraries: xlsx.js (for Excel export)

🚀 Getting Started

To run this project, you need to set up your own Firebase backend.

1. Clone or Download

Download the index.html file from this repository.

2. Create a Firebase Project

Go to the Firebase Console.

Click "Add project" and give it a name (e.g., "my-event-app").

Once created, click the Web icon (</>) to register a new web app.

Copy the firebaseConfig object. You will need this in the next step.

3. Configure Firebase Services

Inside your new Firebase project:

Authentication: Go to the "Authentication" tab, click "Get Started," and enable the Email/Password sign-in provider.

Firestore: Go to the "Firestore Database" tab, click "Create database," and start in Test Mode (you can secure it later).

4. Update the index.html File

Open the index.html file and find the firebaseConfig object (around line 140). Replace the placeholder config with the one you copied from your own Firebase project.

// ...
  <script type="module">
    // Firebase configuration and loading logic
    const firebaseConfig = {
      // 🔽 REPLACE THIS WITH YOUR OWN FIREBASE CONFIG 🔽
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_AUTH_DOMAIN",
      projectId: "YOUR_PROJECT_ID",
      storageBucket: "YOUR_STORAGE_BUCKET",
      messagingSenderId: "YOUR_SENDER_ID",
      appId: "YOUR_APP_ID"
      // 🔼 REPLACE THIS WITH YOUR OWN FIREBASE CONFIG 🔼
    };
// ...


5. Run the Application

You cannot just open the index.html file in your browser due to file:// restrictions on ES Modules. You must serve it from a local web server.

The easiest way is to use npx:

# Make sure you are in the same directory as the index.html file
npx serve


Now you can open http://localhost:3000 (or as indicated by the command) in your browser.

Alternatively, if you use VS Code, you can install the "Live Server" extension, right-click index.html, and choose "Open with Live Server".

6. (Recommended) Secure Your Database

The default Firestore "Test Mode" rules expire after 30 days. For a real project, replace them with secure rules. Go to Firestore > Rules and paste the following:

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Users can be created by anyone (for sign-up)
    // Users can read/update their own profile
    match /users/{userId} {
      allow read, update: if request.auth != null && request.auth.email == resource.data.email;
      allow create: if request.auth != null;
    }
    
    // Events can be read by anyone if published
    // Organisers can create events
    // Organisers can read/write/delete *their own* events
    match /events/{eventId} {
      allow read: if resource.data.is_published == true || (request.auth != null && request.auth.email == resource.data.organiser_email);
      allow create: if request.auth != null; // Further rules can check 'organiser' role
      allow update, delete: if request.auth != null && request.auth.email == resource.data.organiser_email;
    }
  }
}

