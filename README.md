# **TikTok Clone Built with React Native and Firebase**

This repository contains the code and setup instructions for building a **TikTok clone** using **React Native** for the frontend and **Firebase** for the backend. The app supports real-time video uploads, social engagement (likes, comments, and follows), and video streaming.

## **Features**

- **User authentication** (Sign in with email, Google)
- **Real-time video uploads** with Firebase Storage
- **Video feed with endless scrolling** (TikTok-style)
- **Like, comment, and follow functionality**
- **Video playback** with React Native Video
- **Push notifications** (using Firebase Cloud Messaging)

## **Table of Contents**

- [**Getting Started**](#getting-started)
- [**Features**](#features)
- [**Prerequisites**](#prerequisites)
- [**Installation**](#installation)
- [**Firebase Setup**](#firebase-setup)
- [**Environment Setup**](#environment-setup)
- [**Running the App**](#running-the-app)
- [**Code Examples**](#code-examples)
- [**Appkodes Pre-Built TikTok Clone**](#appkodes-pre-built-tiktok-clone)
- [**License**](#license)

## **Getting Started**

This project provides a basic framework for building a **TikTok clone app** using **React Native** and **Firebase**. You can follow the steps below to set up the app on your local machine.

## **Prerequisites**

- **Node.js** (v14 or later)
- **React Native CLI** or **Expo CLI**
- A **Firebase account** (with Firestore and Firebase Cloud Storage enabled)
- **Android Studio/Xcode** for testing on emulators

## **Installation**

### **Install dependencies:**

```bash
npm install
```

### **Install pods for iOS (if running on iOS):**

```bash
cd ios
pod install
```

### **Install Firebase dependencies:**

```bash
npm install --save @react-native-firebase/app @react-native-firebase/auth @react-native-firebase/firestore @react-native-firebase/storage
```

## **Firebase Setup**

1. Create a Firebase project at [**Firebase Console**](https://console.firebase.google.com/).
2. Enable **Firebase Authentication** (Google/Email).
3. Enable **Firestore** and **Firebase Cloud Storage**.
4. Download the `google-services.json` file (for Android) and `GoogleService-Info.plist` (for iOS) and place them in the appropriate directories:
   - `android/app/` (for `google-services.json`)
   - `ios/` (for `GoogleService-Info.plist`)

## **Environment Setup**

Create an `.env` file at the root of your project with the following configurations:

```bash
FIREBASE_API_KEY=your_firebase_api_key
FIREBASE_AUTH_DOMAIN=your_project_id.firebaseapp.com
FIREBASE_PROJECT_ID=your_project_id
FIREBASE_STORAGE_BUCKET=your_project_id.appspot.com
FIREBASE_MESSAGING_SENDER_ID=your_messaging_sender_id
FIREBASE_APP_ID=your_app_id
```

## **Running the App**

### **For Android:**

```bash
npx react-native run-android
```

### **For iOS:**

```bash
npx react-native run-ios
```

## **Code Examples**

Here are a few snippets to demonstrate key features in the app:

### **User Authentication (Firebase Auth)**

```javascript
import auth from '@react-native-firebase/auth';

const signInWithEmail = async (email, password) => {
  try {
    const userCredential = await auth().signInWithEmailAndPassword(email, password);
    console.log('User signed in:', userCredential.user);
  } catch (error) {
    console.error('Error signing in:', error);
  }
};
```

### **Upload Video to Firebase Storage**

```javascript
import storage from '@react-native-firebase/storage';
import {launchImageLibrary} from 'react-native-image-picker';

const uploadVideo = async () => {
  const result = await launchImageLibrary({mediaType: 'video'});
  if (!result.didCancel) {
    const videoUri = result.assets[0].uri;
    const reference = storage().ref(`/videos/${Date.now()}.mp4`);
    const task = reference.putFile(videoUri);

    task.on('state_changed', taskSnapshot => {
      console.log(`${taskSnapshot.bytesTransferred} transferred out of ${taskSnapshot.totalBytes}`);
    });

    await task;
    const downloadURL = await reference.getDownloadURL();
    console.log('Download URL:', downloadURL);
  }
};
```

### **Fetch Video Feed from Firestore**

```javascript
import firestore from '@react-native-firebase/firestore';

const fetchVideoFeed = async () => {
  const videos = [];
  const snapshot = await firestore().collection('videos').get();
  snapshot.forEach(doc => videos.push({id: doc.id, ...doc.data()}));
  setVideos(videos); // Assuming setVideos is a state setter for the video feed
};
```

## **Appkodes Pre-Built TikTok Clone**

If you're looking for a quicker solution and want a pre-built **TikTok clone** with advanced features, check out [**Appkodes**](https://www.appkodes.com). Their ready-made solution comes with extensive features like video dubbing, monetization options, and multi-language support, saving you the hassle of building from scratch.

## **License**

This project is licensed under the **MIT License**. Feel free to use it as a base for your own projects.
