<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jurnal Hati - Pertanyaan Singkat 🌸</title>
    <style>
        /* Tampilan Awal yang Kalem & Menipu */
        body {
            font-family: 'Segoe UI', sans-serif;
            background: #f8fafc;
            color: #334155;
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            overflow: hidden;
        }
        .container {
            background: white;
            padding: 35px 25px;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.05);
            text-align: center;
            width: 100%;
            max-width: 360px;
            z-index: 10;
        }
        .btn {
            background: #fbcfe8;
            color: #be185d;
            border: none;
            padding: 14px 20px;
            border-radius: 12px;
            font-size: 15px;
            font-weight: bold;
            cursor: pointer;
            width: 100%;
            margin-top: 15px;
            transition: 0.2s;
        }
        .btn:hover { background: #f9a8d4; }

        /* MODE JUMPSCARE (Darah + Getar Ekstrem) */
        .scare-mode {
            background: #450a0a !important; /* Merah Darah Gelap */
            animation: bloodFlash 0.1s infinite, shakeScreen 0.05s infinite;
        }
        @keyframes bloodFlash {
            0% { background-color: #000000; }
            50% { background-color: #7f1d1d; }
            100% { background-color: #000000; }
        }
        @keyframes shakeScreen {
            0% { transform: translate(5px, 5px) rotate(0deg); }
            50% { transform: translate(-5px, -5px) rotate(1deg); }
            100% { transform: translate(5px, -5px) rotate(-1deg); }
        }

        #jumpscare-overlay {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100vw; height: 100vh;
            z-index: 9999;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background: transparent;
        }
        .skull {
            font-size: 150px;
            filter: drop-shadow(0 0 30px #ef4444);
            animation: skullPulse 0.1s infinite;
        }
        @keyframes skullPulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.3); }
            100% { transform: scale(1); }
        }
        .scare-text {
            color: #ffffff;
            font-size: 35px;
            font-weight: 900;
            letter-spacing: 3px;
            margin-top: 20px;
            text-shadow: 0 0 15px #ff0000;
            font-family: 'Courier New', monospace;
        }
    </style>
</head>
<body>

    <!-- Tampilan Jebakan -->
    <div class="container" id="quiz-box">
        <h3 style="margin-top:0; color:#be185d;">Pertanyaan Penting 🔮</h3>
        <p style="color:#64748b; line-height:1.6; font-size:14px;">Katanya kita berdua kaku banget kayak batu Moai kalau di chat. Menurut kamu, kita sebenarnya cocok gak sih? Klik tombol di bawah buat liat jawaban jujur aku.</p>
        <button class="btn" onclick="startScare()">KLIK UNTUK LIAT JAWABAN 💖</button>
    </div>

    <!-- Konten Jumpscare Tengkorak -->
    <div id="jumpscare-overlay">
        <div class="skull">💀</div>
        <div class="scare-text">MIAW!! 🩸</div>
    </div>

    <script>
        function startScare() {
            // 1. Sembunyikan box jebakan
            document.getElementById('quiz-box').style.display = 'none';
            
            // 2. Aktifkan efek darah dan layar getar parah
            document.body.classList.add('scare-mode');
            
            // 3. Munculkan tengkorak
            document.getElementById('jumpscare-overlay').style.display = 'flex';
            
            // 4. AUDIO JERITAN BRUTAL & DISTORSI MAX (VOLUME FULL)
            try {
                const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                
                // Durasi jeritan 2 detik
                const bufferSize = audioCtx.sampleRate * 2.0;
                const buffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
                const data = buffer.getChannelData(0);
                
                // Racikan white noise kasar untuk efek distorsi pecah
                for (let i = 0; i < bufferSize; i++) {
                    data[i] = Math.random() * 2 - 1;
                }
                
                const noiseSource = audioCtx.createBufferSource();
                noiseSource.buffer = buffer;

                // Tambah Oscillator melengking (Suara alarm/jeritan tinggi)
                const osc = audioCtx.createOscillator();
                osc.type = 'sawtooth'; // Gelombang gergaji (paling kasar & bising)
                osc.frequency.setValueAtTime(800, audioCtx.currentTime); // Frekuensi awal tinggi
                osc.frequency.linearRampToValueAtTime(1500, audioCtx.currentTime + 0.3); // Naik melengking tajam
                osc.frequency.linearRampToValueAtTime(400, audioCtx.currentTime + 1.5);  // Turun berat

                // Filter Highpass biar suaranya menusuk telinga
                const filter = audioCtx.createBiquadFilter();
                filter.type = 'highpass';
                filter.frequency.value = 600;

                // Pengatur Volume (DI-SET OVERDRIVE / SANGAT KERAS)
                const gainNode = audioCtx.createGain();
                gainNode.gain.setValueAtTime(3.0, audioCtx.currentTime); // Booster volume melampaui batas normal
                gainNode.gain.linearRampToValueAtTime(3.0, audioCtx.currentTime + 1.5); // Tahan suara kerasnya
                gainNode.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 2.0); // Baru mati di detik ke 2

                // Hubungkan semua komponen ke speaker utama
                noiseSource.connect(filter);
                osc.connect(filter);
                
                filter.connect(gainNode);
                gainNode.connect(audioCtx.destination);
                
                // Mainkan barengan secara instan!
                noiseSource.start();
                osc.start();
                
                // Stop setelah 2 detik
                noiseSource.stop(audioCtx.currentTime + 2.0);
                osc.stop(audioCtx.currentTime + 2.0);
                
            } catch(e) {
                console.log("Audio terblokir kebijakan browser");
            }
        }
    </script>
</body>
</html>
