# raniabudehn.github.io
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Take a Photo</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
    }
    #startCamera {
      margin: 20px;
      padding: 10px;
      font-size: 16px;
      background-color: #4CAF50;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    #capturePhoto {
      margin: 20px;
      padding: 10px;
      font-size: 16px;
      background-color: #008CBA;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      display: none;
    }
    #video {
      display: block;
      margin: 0 auto;
      margin-top: 20px;
      border: 1px solid black;
    }
    #photoPreview {
      display: block;
      margin: 20px auto;
      max-width: 300px;
    }
  </style>
</head>
<body>
  <h2>Click to Take a Photo</h2>

  <!-- Button to start the camera -->
  <button id="startCamera">Start Camera</button>

  <!-- Video element to show camera feed -->
  <video id="video" width="300" height="200" autoplay></video>

  <!-- Button to capture the photo -->
  <button id="capturePhoto">Capture Photo</button>

  <!-- Canvas to draw the captured image -->
  <canvas id="canvas" width="300" height="200" style="display:none;"></canvas>

  <!-- Preview of the captured photo -->
  <img id="photoPreview" width="300" alt="Preview will appear here">

  <script>
    let videoElement = document.getElementById('video');
    let canvasElement = document.getElementById('canvas');
    let photoPreview = document.getElementById('photoPreview');
    let startCameraButton = document.getElementById('startCamera');
    let capturePhotoButton = document.getElementById('capturePhoto');
    let stream;

    // Start the camera when the "Start Camera" button is clicked
    startCameraButton.addEventListener('click', function() {
      // Request camera access
      navigator.mediaDevices.getUserMedia({ video: true })
        .then(function(mediaStream) {
          stream = mediaStream;
          videoElement.srcObject = mediaStream;
          videoElement.style.display = 'block'; // Show the video element
          capturePhotoButton.style.display = 'inline'; // Show the capture button
        })
        .catch(function(err) {
          console.log("Error accessing the camera: ", err);
          alert("Error accessing the camera: " + err.message);
        });
    });

    // Capture the photo when the "Capture Photo" button is clicked
    capturePhotoButton.addEventListener('click', function() {
      if (stream) {
        // Draw the current frame from the video element onto the canvas
        let context = canvasElement.getContext('2d');
        context.drawImage(videoElement, 0, 0, canvasElement.width, canvasElement.height);

        // Convert the canvas image to a data URL and show the preview
        let photoData = canvasElement.toDataURL('image/png');
        photoPreview.src = photoData;

        // Stop the camera stream after capturing the photo
        stream.getTracks().forEach(track => track.stop());
        videoElement.style.display = 'none'; // Hide the video element
      }
    });
  </script>
</body>
</html>
