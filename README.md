<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Convertitore Pixel Art - Riduzione Colori</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Press Start 2P', 'Courier New', monospace;
            background: #1a1a2e;
            color: #eee;
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: #16213e;
            border: 4px solid #0f3460;
            border-radius: 0;
            padding: 30px;
            box-shadow: 0 0 30px rgba(230, 57, 70, 0.3);
        }

        h1 {
            color: #e94560;
            margin-bottom: 10px;
            font-size: 24px;
            text-shadow: 3px 3px 0 #533483;
            text-align: center;
        }

        .subtitle {
            text-align: center;
            color: #a8dadc;
            font-size: 10px;
            margin-bottom: 30px;
            font-family: Arial, sans-serif;
        }

        .upload-area {
            border: 4px dashed #e94560;
            padding: 40px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            margin-bottom: 30px;
            background: #0f3460;
            position: relative;
        }

        .upload-area:hover {
            background: #1a4a6e;
            border-color: #f77f00;
        }

        .upload-area.dragover {
            background: #2a5a8e;
            border-color: #06ffa5;
            transform: scale(1.02);
        }

        #fileInput {
            display: none;
        }

        .upload-icon {
            font-size: 48px;
            margin-bottom: 15px;
            animation: float 3s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }

        .controls-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 30px;
        }

        .control-panel {
            background: #0f3460;
            border: 3px solid #533483;
            padding: 20px;
        }

        .control-panel h3 {
            color: #06ffa5;
            font-size: 12px;
            margin-bottom: 15px;
            text-transform: uppercase;
        }

        .control-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            font-size: 10px;
            margin-bottom: 10px;
            color: #a8dadc;
            font-family: Arial, sans-serif;
        }

        input[type="range"] {
            width: 100%;
            height: 8px;
            background: #1a1a2e;
            outline: none;
            -webkit-appearance: none;
            border: 2px solid #533483;
        }

        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 20px;
            height: 20px;
            background: #e94560;
            cursor: pointer;
            border: 2px solid #fff;
            box-shadow: 0 0 10px rgba(233, 69, 96, 0.8);
        }

        input[type="range"]::-webkit-slider-thumb:hover {
            background: #f77f00;
            transform: scale(1.2);
        }

        .value-display {
            display: inline-block;
            background: #e94560;
            color: #fff;
            padding: 8px 16px;
            font-size: 14px;
            border: 2px solid #fff;
            box-shadow: 3px 3px 0 #533483;
            margin-top: 10px;
        }

        .palette-info {
            background: #1a1a2e;
            border: 2px solid #06ffa5;
            padding: 15px;
            margin-top: 15px;
            font-size: 9px;
            font-family: Arial, sans-serif;
            line-height: 1.6;
        }

        .palette-preview {
            display: flex;
            flex-wrap: wrap;
            gap: 3px;
            margin-top: 10px;
        }

        .color-swatch {
            width: 30px;
            height: 30px;
            border: 2px solid #fff;
            box-shadow: 2px 2px 0 #000;
        }

        .canvas-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 30px;
        }

        .canvas-wrapper {
            background: #0f3460;
            border: 4px solid #533483;
            padding: 20px;
            text-align: center;
        }

        .canvas-wrapper h3 {
            color: #06ffa5;
            font-size: 12px;
            margin-bottom: 15px;
            text-transform: uppercase;
        }

        .canvas-display {
            background: #000;
            border: 3px solid #e94560;
            display: inline-block;
            image-rendering: pixelated;
            image-rendering: -moz-crisp-edges;
            image-rendering: crisp-edges;
        }

        canvas {
            image-rendering: pixelated;
            image-rendering: -moz-crisp-edges;
            image-rendering: crisp-edges;
            display: block;
        }

        .pixel-grid {
            position: relative;
            display: inline-block;
        }

        button {
            background: #e94560;
            color: #fff;
            border: 4px solid #fff;
            padding: 15px 30px;
            font-size: 12px;
            font-family: 'Press Start 2P', monospace;
            cursor: pointer;
            transition: all 0.2s;
            box-shadow: 5px 5px 0 #533483;
            text-transform: uppercase;
        }

        button:hover:not(:disabled) {
            background: #f77f00;
            transform: translate(-2px, -2px);
            box-shadow: 7px 7px 0 #533483;
        }

        button:active:not(:disabled) {
            transform: translate(2px, 2px);
            box-shadow: 2px 2px 0 #533483;
        }

        button:disabled {
            background: #555;
            cursor: not-allowed;
            opacity: 0.5;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 30px;
        }

        .stat-box {
            background: #0f3460;
            border: 3px solid #06ffa5;
            padding: 15px;
            text-align: center;
        }

        .stat-label {
            font-size: 8px;
            color: #a8dadc;
            margin-bottom: 8px;
            font-family: Arial, sans-serif;
        }

        .stat-value {
            font-size: 18px;
            color: #e94560;
            text-shadow: 2px 2px 0 #000;
        }

        .button-group {
            display: flex;
            gap: 15px;
            justify-content: center;
            flex-wrap: wrap;
        }

        select {
            background: #1a1a2e;
            color: #06ffa5;
            border: 3px solid #533483;
            padding: 10px;
            font-family: Arial, sans-serif;
            font-size: 12px;
            cursor: pointer;
        }

        .dither-options {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-top: 10px;
        }

        .dither-option {
            background: #1a1a2e;
            border: 2px solid #533483;
            padding: 10px;
            cursor: pointer;
            transition: all 0.2s;
            text-align: center;
            font-size: 9px;
            font-family: Arial, sans-serif;
        }

        .dither-option:hover {
            border-color: #e94560;
            background: #2a2a4e;
        }

        .dither-option.active {
            border-color: #06ffa5;
            background: #0f3460;
            box-shadow: inset 0 0 10px rgba(6, 255, 165, 0.3);
        }

        @media (max-width: 968px) {
            .canvas-container, .controls-grid {
                grid-template-columns: 1fr;
            }
            
            h1 {
                font-size: 16px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎮 PIXEL ART CONVERTER 🎨</h1>
        <p class="subtitle">Converti immagini in pixel art con palette colori ridotta (4-64 bit)</p>

        <div class="upload-area" id="uploadArea">
            <div class="upload-icon">📸</div>
            <p style="font-size: 12px; margin-bottom: 10px; font-family: Arial;">CARICA IMMAGINE</p>
            <p style="color: #a8dadc; font-size: 10px; font-family: Arial;">JPG, PNG • Drag & Drop o Click</p>
            <input type="file" id="fileInput" accept="image/jpeg,image/jpg,image/png">
        </div>

        <div class="controls-grid">
            <div class="control-panel">
                <h3>⚙️ Palette Colori</h3>
                
                <div class="control-group">
                    <label>Profondità Bit: <span class="value-display" id="bitValue">8</span></label>
                    <input type="range" id="bitDepth" min="4" max="64" value="8" step="1">
                </div>

                <div class="control-group">
                    <label>Dimensione Pixel: <span class="value-display" id="pixelValue">1x</span></label>
                    <input type="range" id="pixelSize" min="1" max="16" value="1" step="1">
                </div>

                <div class="palette-info" id="paletteInfo">
                    <strong style="color: #06ffa5;">256 COLORI</strong><br>
                    Stile: Classic 8-bit / NES / Game Boy Color
                </div>

                <div class="palette-preview" id="palettePreview"></div>
            </div>

            <div class="control-panel">
                <h3>🎲 Dithering</h3>
                
                <div class="control-group">
                    <label>Algoritmo Dithering:</label>
                    <select id="ditherSelect">
                        <option value="none">Nessuno</option>
                        <option value="floyd" selected>Floyd-Steinberg</option>
                        <option value="atkinson">Atkinson</option>
                        <option value="ordered">Ordered (Bayer)</option>
                        <option value="random">Random</option>
                    </select>
                </div>

                <div class="control-group">
                    <label>Intensità Dithering: <span class="value-display" id="ditherValue">100%</span></label>
                    <input type="range" id="ditherStrength" min="0" max="100" value="100" step="10">
                </div>

                <div class="palette-info" style="border-color: #e94560;">
                    <strong style="color: #f77f00;">INFO DITHERING</strong><br>
                    Il dithering simula colori aggiuntivi alternando pixel di colori diversi, creando l'illusione di sfumature nelle palette limitate.
                </div>
            </div>
        </div>

        <div class="canvas-container" id="canvasContainer" style="display: none;">
            <div class="canvas-wrapper">
                <h3>🖼️ Originale</h3>
                <div class="canvas-display">
                    <canvas id="originalCanvas"></canvas>
                </div>
            </div>
            <div class="canvas-wrapper">
                <h3>🎨 Pixel Art</h3>
                <div class="canvas-display">
                    <canvas id="pixelCanvas"></canvas>
                </div>
            </div>
        </div>

        <div id="statsContainer" style="display: none;">
            <div class="stats-grid">
                <div class="stat-box">
                    <div class="stat-label">DIMENSIONE</div>
                    <div class="stat-value" id="dimensions">-</div>
                </div>
                <div class="stat-box">
                    <div class="stat-label">COLORI PALETTE</div>
                    <div class="stat-value" id="colorCount">-</div>
                </div>
                <div class="stat-box">
                    <div class="stat-label">PROFONDITÀ</div>
                    <div class="stat-value" id="currentBit">-</div>
                </div>
                <div class="stat-box">
                    <div class="stat-label">STILE</div>
                    <div class="stat-value" id="styleInfo">-</div>
                </div>
            </div>
        </div>

        <div class="button-group">
            <button id="downloadBtn" disabled>💾 SCARICA PNG</button>
            <button id="downloadScaledBtn" disabled>📦 SCARICA INGRANDITA</button>
        </div>
    </div>

    <script>
        // Variabili globali
        let img = null;
        let originalCtx, pixelCtx;
        let currentPalette = [];

        // Elementi DOM
        const uploadArea = document.getElementById('uploadArea');
        const fileInput = document.getElementById('fileInput');
        const bitDepth = document.getElementById('bitDepth');
        const bitValue = document.getElementById('bitValue');
        const pixelSize = document.getElementById('pixelSize');
        const pixelValue = document.getElementById('pixelValue');
        const ditherSelect = document.getElementById('ditherSelect');
        const ditherStrength = document.getElementById('ditherStrength');
        const ditherValue = document.getElementById('ditherValue');
        const originalCanvas = document.getElementById('originalCanvas');
        const pixelCanvas = document.getElementById('pixelCanvas');
        const canvasContainer = document.getElementById('canvasContainer');
        const statsContainer = document.getElementById('statsContainer');
        const downloadBtn = document.getElementById('downloadBtn');
        const downloadScaledBtn = document.getElementById('downloadScaledBtn');

        originalCtx = originalCanvas.getContext('2d', { willReadFrequently: true });
        pixelCtx = pixelCanvas.getContext('2d', { willReadFrequently: true });

        // Eventi
        uploadArea.addEventListener('click', () => fileInput.click());
        fileInput.addEventListener('change', handleFileSelect);

        uploadArea.addEventListener('dragover', (e) => {
            e.preventDefault();
            uploadArea.classList.add('dragover');
        });

        uploadArea.addEventListener('dragleave', () => {
            uploadArea.classList.remove('dragover');
        });

        uploadArea.addEventListener('drop', (e) => {
            e.preventDefault();
            uploadArea.classList.remove('dragover');
            if (e.dataTransfer.files.length > 0) {
                handleFile(e.dataTransfer.files[0]);
            }
        });

        bitDepth.addEventListener('input', updateAndConvert);
        pixelSize.addEventListener('input', updateAndConvert);
        ditherSelect.addEventListener('change', updateAndConvert);
        ditherStrength.addEventListener('input', updateAndConvert);

        function updateAndConvert() {
            bitValue.textContent = bitDepth.value;
            pixelValue.textContent = pixelSize.value + 'x';
            ditherValue.textContent = ditherStrength.value + '%';
            
            if (img) {
                convertToPixelArt();
            }
        }

        function handleFileSelect(e) {
            if (e.target.files[0]) {
                handleFile(e.target.files[0]);
            }
        }

        function handleFile(file) {
            if (!file.type.match('image.*')) {
                alert('Seleziona un file immagine valido!');
                return;
            }

            const reader = new FileReader();
            reader.onload = (e) => {
                img = new Image();
                img.onload = () => {
                    setupCanvases();
                    drawOriginal();
                    convertToPixelArt();
                    canvasContainer.style.display = 'grid';
                    statsContainer.style.display = 'block';
                    downloadBtn.disabled = false;
                    downloadScaledBtn.disabled = false;
                };
                img.src = e.target.result;
            };
            reader.readAsDataURL(file);
        }

        function setupCanvases() {
            const maxSize = 400;
            let w = img.width;
            let h = img.height;

            if (w > maxSize || h > maxSize) {
                const ratio = Math.min(maxSize / w, maxSize / h);
                w = Math.floor(w * ratio);
                h = Math.floor(h * ratio);
            }

            originalCanvas.width = w;
            originalCanvas.height = h;
            pixelCanvas.width = w;
            pixelCanvas.height = h;
        }

        function drawOriginal() {
            originalCtx.drawImage(img, 0, 0, originalCanvas.width, originalCanvas.height);
        }

        function convertToPixelArt() {
            const bits = parseInt(bitDepth.value);
            const scale = parseInt(pixelSize.value);
            
            // Ridimensiona per effetto pixelato
            const scaledW = Math.floor(originalCanvas.width / scale);
            const scaledH = Math.floor(originalCanvas.height / scale);
            
            const tempCanvas = document.createElement('canvas');
            tempCanvas.width = scaledW;
            tempCanvas.height = scaledH;
            const tempCtx = tempCanvas.getContext('2d', { willReadFrequently: true });
            
            // Disegna ridimensionato
            tempCtx.drawImage(originalCanvas, 0, 0, scaledW, scaledH);
            
            // Ottieni dati
            const imageData = tempCtx.getImageData(0, 0, scaledW, scaledH);
            
            // Genera palette
            currentPalette = generatePalette(bits);
            
            // Applica dithering e riduzione colori
            const ditherType = ditherSelect.value;
            const strength = parseInt(ditherStrength.value) / 100;
            
            applyDithering(imageData, currentPalette, ditherType, strength);
            
            tempCtx.putImageData(imageData, 0, 0);
            
            // Ridimensiona al canvas finale
            pixelCtx.imageSmoothingEnabled = false;
            pixelCtx.drawImage(tempCanvas, 0, 0, scaledW, scaledH, 
                             0, 0, pixelCanvas.width, pixelCanvas.height);
            
            updateStats();
            updatePalettePreview();
        }

        function generatePalette(bits) {
            const numColors = Math.pow(2, Math.min(bits, 24));
            const palette = [];
            
            if (bits <= 8) {
                // Palette uniforme RGB
                const levels = Math.ceil(Math.pow(numColors, 1/3));
                const step = Math.floor(256 / (levels - 1));
                
                for (let r = 0; r < levels; r++) {
                    for (let g = 0; g < levels; g++) {
                        for (let b = 0; b < levels; b++) {
                            if (palette.length < numColors) {
                                palette.push([
                                    Math.min(r * step, 255),
                                    Math.min(g * step, 255),
                                    Math.min(b * step, 255)
                                ]);
                            }
                        }
                    }
                }
            } else {
                // Per bit più alti, usa quantizzazione RGB standard
                const bitsPerChannel = Math.floor(bits / 3);
                const levels = Math.pow(2, bitsPerChannel);
                const step = Math.floor(256 / (levels - 1));
                
                for (let r = 0; r < levels; r++) {
                    for (let g = 0; g < levels; g++) {
                        for (let b = 0; b < levels; b++) {
                            palette.push([
                                Math.min(r * step, 255),
                                Math.min(g * step, 255),
                                Math.min(b * step, 255)
                            ]);
                        }
                    }
                }
            }
            
            return palette.slice(0, numColors);
        }

        function applyDithering(imageData, palette, type, strength) {
            const data = imageData.data;
            const w = imageData.width;
            const h = imageData.height;
            
            for (let y = 0; y < h; y++) {
                for (let x = 0; x < w; x++) {
                    const idx = (y * w + x) * 4;
                    
                    const oldR = data[idx];
                    const oldG = data[idx + 1];
                    const oldB = data[idx + 2];
                    
                    // Trova colore più vicino nella palette
                    const newColor = findClosestColor([oldR, oldG, oldB], palette);
                    
                    data[idx] = newColor[0];
                    data[idx + 1] = newColor[1];
                    data[idx + 2] = newColor[2];
                    
                    // Calcola errore
                    const errR = (oldR - newColor[0]) * strength;
                    const errG = (oldG - newColor[1]) * strength;
                    const errB = (oldB - newColor[2]) * strength;
                    
                    // Distribuisci errore
                    if (type === 'floyd') {
                        distributeError(data, w, h, x, y, errR, errG, errB, [
                            [1, 0, 7/16],
                            [-1, 1, 3/16],
                            [0, 1, 5/16],
                            [1, 1, 1/16]
                        ]);
                    } else if (type === 'atkinson') {
                        distributeError(data, w, h, x, y, errR, errG, errB, [
                            [1, 0, 1/8],
                            [2, 0, 1/8],
                            [-1, 1, 1/8],
                            [0, 1, 1/8],
                            [1, 1, 1/8],
                            [0, 2, 1/8]
                        ]);
                    } else if (type === 'random') {
                        if (Math.random() < 0.5) {
                            distributeError(data, w, h, x, y, errR, errG, errB, [
                                [1, 0, Math.random()],
                                [0, 1, Math.random()]
                            ]);
                        }
                    } else if (type === 'ordered') {
                        // Bayer matrix 2x2
                        const threshold = [
                            [0, 2],
                            [3, 1]
                        ][y % 2][x % 2] / 4 - 0.5;
                        
                        data[idx] = Math.max(0, Math.min(255, data[idx] + errR * threshold));
                        data[idx + 1] = Math.max(0, Math.min(255, data[idx + 1] + errG * threshold));
                        data[idx + 2] = Math.max(0, Math.min(255, data[idx + 2] + errB * threshold));
                    }
                }
            }
        }

        function distributeError(data, w, h, x, y, errR, errG, errB, matrix) {
            for (const [dx, dy, factor] of matrix) {
                const nx = x + dx;
                const ny = y + dy;
                
                if (nx >= 0 && nx < w && ny >= 0 && ny < h) {
                    const idx = (ny * w + nx) * 4;
                    data[idx] = Math.max(0, Math.min(255, data[idx] + errR * factor));
                    data[idx + 1] = Math.max(0, Math.min(255, data[idx + 1] + errG * factor));
                    data[idx + 2] = Math.max(0, Math.min(255, data[idx + 2] + errB * factor));
                }
            }
        }

        function findClosestColor(color, palette) {
            let minDist = Infinity;
            let closest = palette[0];
            
            for (const pColor of palette) {
                const dist = Math.sqrt(
                    Math.pow(color[0] - pColor[0], 2) +
                    Math.pow(color[1] - pColor[1], 2) +
                    Math.pow(color[2] - pColor[2], 2)
                );
                
                if (dist < minDist) {
                    minDist = dist;
                    closest = pColor;
                }
            }
            
            return closest;
        }

        function updateStats() {
            const bits = parseInt(bitDepth.value);
            const colors = currentPalette.length;
            
            document.getElementById('dimensions').textContent = 
                `${pixelCanvas.width}×${pixelCanvas.height}`;
            document.getElementById('colorCount').textContent = colors;
            document.getElementById('currentBit').textContent = `${bits}-BIT`;
            
            const styles = {
                4: '16-COL',
                8: '256-COL',
                16: 'HIGH',
                24: 'TRUE'
            };
            document.getElementById('styleInfo').textContent = 
                styles[bits] || `${bits}-BIT`;
            
            const info = {
                4: '16 COLORI - CGA / Commodore 64',
                8: '256 COLORI - NES / Game Boy Color / DOS',
                16: '65.536 COLORI - SNES / Sega Genesis',
                24: '16.7M COLORI - True Color / Modern',
                32: '16.7M+ COLORI - True Color + Alpha'
            };
            
            document.getElementById('paletteInfo').innerHTML = 
                `<strong style="color: #06ffa5;">${colors} COLORI</strong><br>` +
                (info[bits] || `${colors.toLocaleString()} colori possibili`);
        }

        function updatePalettePreview() {
            const preview = document.getElementById('palettePreview');
            preview.innerHTML = '';
            
            const maxSwatches = 32;
            const step = Math.ceil(currentPalette.length / maxSwatches);
            
            for (let i = 0; i < currentPalette.length; i += step) {
                const color = currentPalette[i];
                const swatch = document.createElement('div');
                swatch.className = 'color-swatch';
                swatch.style.backgroundColor = `rgb(${color[0]}, ${color[1]}, ${color[2]})`;
                preview.appendChild(swatch);
            }
        }

        downloadBtn.addEventListener('click', () => {
            const bits = bitDepth.value;
            pixelCanvas.toBlob((blob) => {
                const url = URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.href = url;
                a.download = `pixelart_${bits}bit.png`;
                a.click();
                URL.revokeObjectURL(url);
            });
        });

        downloadScaledBtn.addEventListener('click', () => {
            const scale = 4; // Ingrandimento 4x
            const scaledCanvas = document.createElement('canvas');
            scaledCanvas.width = pixelCanvas.width * scale;
            scaledCanvas.height = pixelCanvas.height * scale;
            const ctx = scaledCanvas.getContext('2d');
            ctx.imageSmoothingEnabled = false;
            ctx.drawImage(pixelCanvas, 0, 0, scaledCanvas.width, scaledCanvas.height);
            
            const bits = bitDepth.value;
            scaledCanvas.toBlob((blob) => {
                const url = URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.href = url;
                a.download = `pixelart_${bits}bit_4x.png`;
                a.click();
                URL.revokeObjectURL(url);
            });
        });
    </script>
</body>
</html>
