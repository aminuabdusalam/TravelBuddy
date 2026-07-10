# Travel Buddy

A virtual travel assistant that speeds up the airport boarding process by taking the guesswork out of "is this a carry-on or a checked bag?" — you snap a photo of your luggage in the app and it tells you.

> 🏆 **Winner — Nvidia Award, BE-Smart Hack** (Fisk University, 2022). Built by a team of 5.

## What it does

1. You open the app before you head to the airport.
2. You snap a photo of your bag (top view and side view).
3. The backend runs computer vision on the photos to estimate the bag's real-world dimensions.
4. It classifies the bag as carry-on or checked, calculates fees if applicable, and generates a QR code you can flash at the gate — no more surprises at check-in.

## Tech stack — and what each piece does

| Layer | Language / Framework | Role in the project |
|---|---|---|
| Mobile app | **React Native + Expo** (JavaScript) | Cross-platform iOS/Android UI. Camera capture (`expo-image-picker`), navigation (`react-navigation`), booking timeline (`react-native-timeline-flatlist`), HTTP to the backend (`axios`). Runs on device via Expo Go without needing native Xcode/Android Studio builds. |
| API server | **Django + Django REST Framework** (Python) | Exposes the REST endpoints the mobile app calls (`/analyzeImage`, `/checkBagsInfo`). Orchestrates the scanner service and returns dimensions + fee info as JSON. |
| Computer vision | **OpenCV** + **imutils** + **SciPy** + **NumPy** (Python) | The bag-size scanner. Takes top-view and side-view photos, applies grayscale → Gaussian blur → Canny edge detection → contour detection, then uses a reference object to convert pixel measurements to real-world inches. Credit to [Practical-CV/Measuring-Size-of-Objects-with-OpenCV](https://github.com/Practical-CV/Measuring-Size-of-Objects-with-OpenCV) for the size-estimation approach we adapted. |
| Boarding pass | **qrcode** (Python) | Generates a QR encoding the bag verdict + booking ID so gate agents can scan it on their side. |

## Repo layout

```
TravelBuddy/
├── backend/                          # Django project
│   ├── manage.py
│   ├── backend/                      # settings, urls, wsgi
│   ├── main/                         # REST API app (views, urls, models)
│   └── services/
│       └── baggage_scanner_service/  # OpenCV bag-dimension scanner
│           ├── bag_sizes_scanner.py
│           └── images/               # sample luggage photos for testing
└── myFrontend/                       # React Native / Expo app
    ├── App.js
    ├── pages/                        # app screens
    ├── components/                   # reusable UI
    └── assets/, images/
```

## Running locally

**Backend** (Python 3.10+):
```bash
cd backend
pip install django djangorestframework opencv-python imutils scipy numpy qrcode
python manage.py runserver
```

**Frontend** (Node 18+):
```bash
cd myFrontend
npm install
npm start          # opens Expo dev tools; scan the QR with Expo Go
```

## Team

Built for the 2022 BE-Smart Hack at Fisk University by a team of 5. My role was full-stack — worked across the React Native front-end, the Django API, and integrating the bag-scanner service.

## Status

Hackathon-era codebase — kept online as a portfolio artifact. Dependencies are pinned to their 2022 versions and haven't been upgraded since the hack.
