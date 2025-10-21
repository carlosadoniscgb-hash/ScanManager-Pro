<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Escáner Universal de Códigos QR y de Barras</title>
<style>
    body {
        background: linear-gradient(135deg, #6a11cb, #2575fc);
        font-family: 'Segoe UI', sans-serif;
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 100vh;
        margin: 0;
    }
    .container {
        background: #fff;
        border-radius: 20px;
        padding: 20px;
        max-width: 800px;
        width: 100%;
        box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        text-align: center;
    }
    h1 {
        background: linear-gradient(90deg, #2575fc, #6a11cb);
        -webkit-background-clip: text;
        color: transparent;
        font-size: 1.8rem;
    }
    #camera-preview {
        width: 100%;
        height: 320px;
        background: #000;
        border-radius: 15px;
        overflow: hidden;
        display: none;
        margin-top: 15px;
        position: relative;
    }
    #video {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }
    .btn {
        padding: 12px 25px;
        margin: 10px;
        border: none;
        border-radius: 50px;
        font-weight: bold;
        cursor: pointer;
        color: #fff;
        background: linear-gradient(90deg, #2575fc, #6a11cb);
        transition: 0.3s;
    }
    .btn:hover {
        transform: scale(1.05);
    }
    #result {
        background: #f4f4f4;
        padding: 10px;
        border-radius: 10px;
        min-height: 50px;
        margin-top: 15px;
        word-break: break-all;
        font-family: monospace;
    }
    label {
        display: inline-block;
        padding: 10px 20px;
        background: #f0f0f0;
        border-radius: 25px;
        cursor: pointer;
        color: #333;
        margin-top: 10px;
    }
    input[type="file"] {
        display: none;
    }
</style>
</head>
<body>

<div class="container">
    <h1>📷 Escáner de Códigos Universal</h1>
    <p>Escanea códigos QR o de barras con tu cámara o subiendo una imagen</p>
    
    <button id="start-btn" class="btn">Activar Cámara</button>
    <button id="stop-btn" class="btn" style="display:none;">Detener Cámara</button>
    
    <div id="camera-preview">
        <video id="video" autoplay playsinline></video>
    </div>

    <input type="file" id="file-input" accept="image/*">
    <label for="file-input">📸 Subir Imagen</label>

    <div id="result">Esperando escaneo...</div>
</div>

<!-- Librerías -->
<script src="https://cdn.jsdelivr.net/npm/jsqr@1.4.0/dist/jsQR.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/quagga/0.12.1/quagga.min.js"></script>

<script>
const startBtn = document.getElementById('start-btn');
const stopBtn = document.getElementById('stop-btn');
const resultEl = document.getElementById('result');
const video = document.getElementById('video');
const cameraPreview = document.getElementById('camera-preview');
let stream = null;
let canvas = document.createElement('canvas');
let ctx = canvas.getContext('2d');
let scanInterval = null;

// ======== ESCÁNER CON CÁMARA ========
startBtn.addEventListener('click', async () => {
    try {
        resultEl.textContent = "Iniciando cámara...";
        stream = await navigator.mediaDevices.getUserMedia({ 
            video: { facingMode: "environment" } 
        });
        video.srcObject = stream;
        cameraPreview.style.display = "block";
        startBtn.style.display = "none";
        stopBtn.style.display = "inline-block";
        resultEl.textContent = "Cámara activada. Escaneando...";
        startScanning();
    } catch (err) {
        console.error(err);
        resultEl.textContent = "Error al acceder a la cámara. Usa HTTPS o localhost.";
    }
});

stopBtn.addEventListener('click', stopCamera);

function stopCamera() {
    if (stream) {
        stream.getTracks().forEach(track => track.stop());
    }
    clearInterval(scanInterval);
    video.srcObject = null;
    cameraPreview.style.display = "none";
    startBtn.style.display = "inline-block";
    stopBtn.style.display = "none";
    resultEl.textContent = "Cámara detenida.";
}

// ======== ESCÁNER AUTOMÁTICO ========
function startScanning() {
    scanInterval = setInterval(() => {
        if (video.readyState === video.HAVE_ENOUGH_DATA) {
            canvas.width = video.videoWidth;
            canvas.height = video.videoHeight;
            ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
            const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
            const qrCode = jsQR(imageData.data, imageData.width, imageData.height, { inversionAttempts: "dontInvert" });

            if (qrCode) {
                resultEl.textContent = `✅ Código QR: ${qrCode.data}`;
                //stopCamera(); // opcional detener al detectar
            } else {
                // Intentar detección con QuaggaJS (barras)
                detectBarcodeFromFrame(imageData);
            }
        }
    }, 300);
}

// ======== DETECCIÓN DE BARRAS CON QUAGGA ========
function detectBarcodeFromFrame(imageData) {
    Quagga.decodeSingle({
        src: canvas.toDataURL(),
        numOfWorkers: 0,
        decoder: {
            readers: [
                "code_128_reader",
                "ean_reader",
                "ean_8_reader",
                "upc_reader",
                "code_39_reader",
                "code_93_reader"
            ]
        }
    }, function(result) {
        if (result && result.codeResult) {
            resultEl.textContent = `📦 Código de Barras: ${result.codeResult.code}`;
            //stopCamera(); // opcional detener al detectar
        }
    });
}

// ======== DETECCIÓN DESDE IMAGEN ========
const fileInput = document.getElementById('file-input');
fileInput.addEventListener('change', (e) => {
    const file = e.target.files[0];
    if (!file) return;

    const reader = new FileReader();
    const img = new Image();
    reader.onload = (ev) => {
        img.onload = () => {
            canvas.width = img.width;
            canvas.height = img.height;
            ctx.drawImage(img, 0, 0);
            const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
            const qrCode = jsQR(imageData.data, imageData.width, imageData.height);

            if (qrCode) {
                resultEl.textContent = `✅ Código QR: ${qrCode.data}`;
            } else {
                Quagga.decodeSingle({
                    src: canvas.toDataURL(),
                    numOfWorkers: 0,
                    decoder: {
                        readers: ["code_128_reader", "ean_reader", "upc_reader"]
                    }
                }, function(result) {
                    if (result && result.codeResult) {
                        resultEl.textContent = `📦 Código de Barras: ${result.codeResult.code}`;
                    } else {
                        resultEl.textContent = "No se detectó ningún código. Intenta con otra imagen.";
                    }
                });
            }
        };
        img.src = ev.target.result;
    };
    reader.readAsDataURL(file);
});
</script>
</body>
</html>
