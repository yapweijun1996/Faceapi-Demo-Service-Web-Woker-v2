# Face API Demo with Service Worker and Web Worker

This project demonstrates a real-time face recognition application using `face-api.js`. It's designed to be a Progressive Web App (PWA) and leverages both Service Workers and Web Workers to perform heavy computations in the background, ensuring a smooth user experience.

## Features

- **Face Registration:** Register new users by capturing multiple face descriptors.
- **Face Verification:** Verify users against the registered profiles in real-time.
- **Profile Management:** (Stubbed) A page for managing user profiles.
- **PWA Ready:** Includes a `manifest.json` for PWA capabilities.
- **Background Processing:** Uses a Service Worker and a Web Worker to offload the face detection and model loading from the main UI thread. This prevents the UI from freezing during intensive operations.
- **Responsive Design:** The UI is designed to work on both desktop and mobile devices, with specific handling for different screen orientations.

## Project Structure

```
.
├── js/
│   ├── face-api.js
│   ├── face-api.min.js
│   ├── faceapi_warmup.js
│   ├── faceDetectionServiceWorker.js
│   ├── faceDetectionWebWorker.js
│   ├── faceEnvWorkerPatch.js
│   └── modelLoaderWorker.js
├── models/
│   ├── ... (face-api.js models)
├── .gitattributes
├── face_register.html
├── face_verify.html
├── index.html
├── manifest.json
└── profile_management.html
```

### Key Files

- **`index.html`**: The main entry point of the application, providing navigation to the different features.
- **`face_register.html`**: The page for registering new users. It captures video from the user's camera and uses `face-api.js` to create face descriptors.
- **`face_verify.html`**: The page for verifying users. It compares the live video feed with the stored face descriptors.
- **`profile_management.html`**: A placeholder for a profile management page.
- **`js/faceDetectionWebWorker.js`**: A Web Worker that handles face detection in the background.
- **`js/faceDetectionServiceWorker.js`**: A Service Worker that also handles face detection, providing persistence and broadcasting capabilities.
- **`js/faceapi_warmup.js`**: A script to preload and warm up the `face-api.js` models for faster performance.
- **`models/`**: This directory contains the pre-trained models used by `face-api.js`.

## How to Use

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/yapweijun1996/Faceapi-Demo-Service-Web-Woker-v2.git
    ```

2.  **Serve the files:**
    You need to serve the files from a local web server. You can use any simple web server. For example, if you have Python installed, you can run:
    ```bash
    # For Python 3
    python -m http.server
    ```
    Or, if you have Node.js, you can use `http-server`:
    ```bash
    npx http-server
    ```

3.  **Open in your browser:**
    Navigate to the local server's address (e.g., `http://localhost:8000`).

## How It Works

The application uses `face-api.js` for all face detection and recognition tasks. To avoid blocking the main UI thread, the heavy processing is offloaded to a background worker.

1.  **Worker Initialization:** The application first tries to use a Service Worker. If that's not available or fails, it may fall back to a Web Worker.
2.  **Model Loading:** The worker loads the required `face-api.js` models in the background. The main page shows a loading indicator until the models are ready.
3.  **Face Detection:** The main page sends video frames to the worker. The worker runs the face detection and sends the results (detections, landmarks, and descriptors) back to the main page.
4.  **UI Updates:** The main page receives the results from the worker and updates the UI to draw bounding boxes, landmarks, and display other information.

This architecture ensures that the UI remains responsive even when the face detection is running, which is crucial for a good user experience.
