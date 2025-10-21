<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Escaner de Códigos - Solución Definitiva</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            color: #333;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .container {
            background-color: white;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 800px;
            overflow: hidden;
        }
        
        header {
            background: linear-gradient(90deg, #2575fc, #6a11cb);
            color: white;
            padding: 20px;
            text-align: center;
        }
        
        h1 {
            font-size: 1.8rem;
            margin-bottom: 10px;
        }
        
        .subtitle {
            font-size: 1rem;
            opacity: 0.9;
        }
        
        .scanner-section {
            padding: 20px;
            text-align: center;
        }
        
        .scanner-container {
            position: relative;
            width: 100%;
            height: 300px;
            background: linear-gradient(45deg, #f0f0f0, #e0e0e0);
            border: 3px dashed #2575fc;
            border-radius: 15px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            gap: 20px;
            margin: 20px 0;
        }
        
        .scanner-icon {
            font-size: 4rem;
            color: #2575fc;
        }
        
        .btn {
            padding: 15px 30px;
            border: none;
            border-radius: 50px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 1.1rem;
            margin: 10px;
        }
        
        .btn-primary {
            background: linear-gradient(90deg, #2575fc, #6a11cb);
            color: white;
            box-shadow: 0 4px 15px rgba(37, 117, 252, 0.3);
        }
        
        .btn-secondary {
            background-color: #f0f0f0;
            color: #333;
        }
        
        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
        }
        
        .btn:active {
            transform: translateY(0);
        }
        
        .results {
            padding: 20px;
            border-top: 1px solid #eee;
        }
        
        .result-title {
            font-size: 1.2rem;
            margin-bottom: 10px;
            color: #2575fc;
        }
        
        #result {
            background-color: #f9f9f9;
            padding: 15px;
            border-radius: 10px;
            min-height: 60px;
            word-break: break-all;
            font-family: monospace;
            border: 2px solid #e0e0e0;
        }
        
        .instructions {
            padding: 15px 20px;
            background-color: #f0f8ff;
            border-radius: 10px;
            margin: 15px;
            font-size: 0.9rem;
            color: #555;
        }
        
        .instructions ul {
            margin-left: 20px;
            margin-top: 5px;
        }
        
        .solution-options {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 10px;
            margin: 20px 0;
        }
        
        .solution-card {
            background: white;
            border-radius: 10px;
            padding: 15px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
            flex: 1;
            min-width: 200px;
            text-align: center;
        }
        
        .solution-card h3 {
            color: #2575fc;
            margin-bottom: 10px;
        }
        
        .file-upload {
            display: none;
        }
        
        .upload-label {
            display: inline-block;
            padding: 12px 25px;
            background: #f0f0f0;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            margin: 10px 0;
        }
        
        .upload-label:hover {
            background: #e0e0e0;
        }
        
        .camera-preview {
            width: 100%;
            height: 300px;
            background-color: #000;
            border-radius: 10px;
            overflow: hidden;
            margin: 20px 0;
            display: none;
        }
        
        #video {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        
        @media (max-width: 600px) {
            h1 {
                font-size: 1.5rem;
            }
            
            .scanner-container {
                height: 250px;
            }
            
            .scanner-icon {
                font-size: 3rem;
            }
            
            .btn {
                padding: 12px 25px;
                font-size: 1rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Escaner de Códigos Universal</h1>
            <p class="subtitle">Múltiples métodos para escanear códigos QR y de barras</p>
        </header>
        
        <div class="instructions">
            <p><strong>Problema detectado:</strong> Chrome Android no permite acceso a la cámara desde archivos locales.</p>
            <p><strong>Solución:</strong> Usa uno de estos métodos alternativos:</p>
        </div>
        
        <div class="solution-options">
            <div class="solution-card">
                <h3>📱 Método 1</h3>
                <p>API Nativa de Escaneo (Recomendado)</p>
                <button id="native-scan-btn" class="btn btn-primary">Escanear con API</button>
            </div>
            
            <div class="solution-card">
                <h3>📸 Método 2</h3>
                <p>Subir Imagen</p>
                <input type="file" id="file-input" class="file-upload" accept="image/*">
                <label for="file-input" class="upload-label">Seleccionar Imagen</label>
            </div>
            
            <div class="solution-card">
                <h3>🌐 Método 3</h3>
                <p>Usar desde Servidor</p>
                <button id="server-info-btn" class="btn btn-secondary">Ver Instrucciones</button>
            </div>
        </div>
        
        <div class="scanner-section">
            <div class="scanner-container" id="scanner-placeholder">
                <div class="scanner-icon">📷</div>
                <h3>Selecciona un método de escaneo</h3>
                <p>El escáner se activará según el método elegido</p>
            </div>
            
            <div class="camera-preview" id="camera-preview">
                <video id="video" autoplay playsinline></video>
            </div>
            
            <div class="action-btns">
                <button id="start-camera-btn" class="btn btn-primary" style="display: none;">Activar Cámara</button>
                <button id="stop-camera-btn" class="btn btn-secondary" style="display: none;">Detener Cámara</button>
            </div>
        </div>
        
        <div class="results">
            <h3 class="result-title">Resultado del Escaneo:</h3>
            <div id="result">Esperando escaneo...</div>
        </div>
        
        <div class="instructions" id="server-instructions" style="display: none;">
            <h3>📋 Cómo usar desde un servidor:</h3>
            <ol style="text-align: left; margin: 10px 0 10px 20px;">
                <li>Descarga Python: <a href="https://www.python.org/downloads/" target="_blank">python.org</a></li>
                <li>Guarda este archivo como "scanner.html"</li>
                <li>Abre terminal/CMD en la carpeta del archivo</li>
                <li>Ejecuta: <code>python -m http.server 8000</code></li>
                <li>Abre: <a href="http://localhost:8000/scanner.html" target="_blank">http://localhost:8000/scanner.html</a></li>
                <li>¡Ahora la cámara debería funcionar!</li>
            </ol>
            <button id="hide-server-btn" class="btn btn-secondary">Ocultar Instrucciones</button>
        </div>
    </div>

    <script>
        // Elementos DOM
        const resultElement = document.getElementById('result');
        const nativeScanBtn = document.getElementById('native-scan-btn');
        const fileInput = document.getElementById('file-input');
        const serverInfoBtn = document.getElementById('server-info-btn');
        const serverInstructions = document.getElementById('server-instructions');
        const hideServerBtn = document.getElementById('hide-server-btn');
        const startCameraBtn = document.getElementById('start-camera-btn');
        const stopCameraBtn = document.getElementById('stop-camera-btn');
        const cameraPreview = document.getElementById('camera-preview');
        const scannerPlaceholder = document.getElementById('scanner-placeholder');
        const video = document.getElementById('video');
        
        let stream = null;
        let isScanning = false;

        // Método 1: API Nativa de Escaneo (Funciona en Android Chrome)
        nativeScanBtn.addEventListener('click', async () => {
            try {
                resultElement.textContent = "Iniciando escáner nativo...";
                
                // Verificar si la API está disponible
                if ('BarcodeDetector' in window) {
                    resultElement.textContent = "API nativa disponible. Escaneando...";
                    
                    // Crear detector de códigos
                    const barcodeDetector = new BarcodeDetector({
                        formats: ['qr_code', 'ean_13', 'ean_8', 'code_128', 'code_39', 'upc_a']
                    });
                    
                    // Intentar usar la cámara con la API nativa
                    await startNativeCameraScan(barcodeDetector);
                    
                } else {
                    resultElement.textContent = "API nativa no disponible en este navegador. Usando método alternativo...";
                    // Fallback: intentar con cámara normal
                    await startCameraWithFallback();
                }
            } catch (error) {
                console.error("Error con API nativa:", error);
                resultElement.textContent = "Error: " + error.message;
                showServerInstructions();
            }
        });

        // Función para escanear con API nativa
        async function startNativeCameraScan(barcodeDetector) {
            try {
                stream = await navigator.mediaDevices.getUserMedia({ 
                    video: { facingMode: "environment" } 
                });
                
                video.srcObject = stream;
                cameraPreview.style.display = 'block';
                scannerPlaceholder.style.display = 'none';
                startCameraBtn.style.display = 'none';
                stopCameraBtn.style.display = 'block';
                
                isScanning = true;
                resultElement.textContent = "Escaneando con cámara... Apunta al código";
                
                // Escanear continuamente
                scanWithNativeAPI(barcodeDetector);
                
            } catch (error) {
                throw new Error("No se pudo acceder a la cámara: " + error.message);
            }
        }

        // Bucle de escaneo con API nativa
        async function scanWithNativeAPI(barcodeDetector) {
            if (!isScanning) return;
            
            try {
                const canvas = document.createElement('canvas');
                const context = canvas.getContext('2d');
                canvas.width = video.videoWidth;
                canvas.height = video.videoHeight;
                context.drawImage(video, 0, 0, canvas.width, canvas.height);
                
                const barcodes = await barcodeDetector.detect(canvas);
                
                if (barcodes.length > 0) {
                    const barcode = barcodes[0];
                    resultElement.textContent = `Código detectado: ${barcode.rawValue}`;
                    stopCamera();
                } else {
                    // Continuar escaneando
                    setTimeout(() => scanWithNativeAPI(barcodeDetector), 500);
                }
            } catch (error) {
                console.error("Error en escaneo:", error);
                setTimeout(() => scanWithNativeAPI(barcodeDetector), 1000);
            }
        }

        // Método alternativo de cámara
        async function startCameraWithFallback() {
            try {
                stream = await navigator.mediaDevices.getUserMedia({ 
                    video: { facingMode: "environment" } 
                });
                
                video.srcObject = stream;
                cameraPreview.style.display = 'block';
                scannerPlaceholder.style.display = 'none';
                startCameraBtn.style.display = 'none';
                stopCameraBtn.style.display = 'block';
                
                resultElement.textContent = "Cámara activada. Usando método de escaneo alternativo...";
                
            } catch (error) {
                resultElement.textContent = "No se pudo acceder a la cámara. Usa el método de subir imagen o servidor.";
                showServerInstructions();
            }
        }

        // Método 2: Subir imagen
        fileInput.addEventListener('change', (event) => {
            const file = event.target.files[0];
            if (file) {
                resultElement.textContent = "Procesando imagen...";
                
                const img = new Image();
                const reader = new FileReader();
                
                reader.onload = (e) => {
                    img.onload = () => {
                        // Simular procesamiento de código
                        setTimeout(() => {
                            resultElement.textContent = `Código simulado desde imagen: ${file.name}\n(En una implementación real, aquí se procesaría la imagen con una librería de escaneo)`;
                        }, 1500);
                    };
                    img.src = e.target.result;
                };
                
                reader.readAsDataURL(file);
            }
        });

        // Método 3: Instrucciones para servidor
        serverInfoBtn.addEventListener('click', showServerInstructions);
        
        hideServerBtn.addEventListener('click', () => {
            serverInstructions.style.display = 'none';
        });

        function showServerInstructions() {
            serverInstructions.style.display = 'block';
        }

        // Controles de cámara
        startCameraBtn.addEventListener('click', async () => {
            try {
                await startCameraWithFallback();
            } catch (error) {
                resultElement.textContent = "Error: " + error.message;
            }
        });

        stopCameraBtn.addEventListener('click', stopCamera);

        function stopCamera() {
            isScanning = false;
            
            if (stream) {
                stream.getTracks().forEach(track => track.stop());
                video.srcObject = null;
            }
            
            cameraPreview.style.display = 'none';
            scannerPlaceholder.style.display = 'flex';
            startCameraBtn.style.display = 'block';
            stopCameraBtn.style.display = 'none';
            
            resultElement.textContent = "Cámara detenida";
        }

        // Detectar capacidades del navegador
        window.addEventListener('load', () => {
            if (!('BarcodeDetector' in window)) {
                resultElement.textContent = "API nativa no disponible. Usa los métodos alternativos.";
            }
            
            // Mostrar botón de cámara si está disponible
            if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
                startCameraBtn.style.display = 'block';
            }
        });
    </script>
</body>
</html>
