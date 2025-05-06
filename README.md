<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Capture Auto Caméra</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      font-family: Arial, sans-serif;
      background: black;
      color: white;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }
    video, canvas {
      display: none;
      margin-top: 20px;
      border-radius: 10px;
    }
    #photo {
      margin-top: 20px;
      max-width: 90%;
      border: 3px solid white;
      border-radius: 10px;
    }
  </style>
</head>
<body>

<h2>Chargement de la caméra...</h2>

<video id="video" autoplay></video>
<canvas id="canvas"></canvas>
<img id="photo" alt="Votre photo apparaîtra ici" />

<script>
  const video = document.getElementById('video');
  const canvas = document.getElementById('canvas');
  const photo = document.getElementById('photo');

  // Demander accès caméra
  navigator.mediaDevices.getUserMedia({ video: true })
    .then((stream) => {
      video.srcObject = stream;
      video.play();

      // Attendre 2 secondes puis prendre la photo
      setTimeout(() => {
        takePicture();
      }, 2000);
    })
    .catch((err) => {
      console.error("Erreur caméra: ", err);
      alert("Erreur : impossible d'accéder à la caméra !");
    });

  function takePicture() {
    const width = video.videoWidth;
    const height = video.videoHeight;

    canvas.width = width;
    canvas.height = height;

    const context = canvas.getContext('2d');
    context.drawImage(video, 0, 0, width, height);

    // Arrêter la caméra
    video.srcObject.getTracks().forEach(track => track.stop());

    // Afficher la photo
    const dataURL = canvas.toDataURL('image/png');
    photo.src = dataURL;
    photo.style.display = 'block';
  }
</script>

</body>
</html>
