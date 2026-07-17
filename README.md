# for-you
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Surat Spesial Untukmu ❤️</title>
    <style>
        :root {
            --bg-color: #ffe5ec;
            --card-bg: #ffffff;
            --primary-color: #ffb3c1;
            --accent-color: #fb6f92;
            --text-color: #4a4a4a;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: var(--bg-color);
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
            perspective: 1000px;
        }

        /* --- KOTAK UTAMA (AMPLOP) --- */
        .envelope-wrapper {
            position: relative;
            width: 300px;
            height: 200px;
            background-color: var(--primary-color);
            border-bottom-left-radius: 10px;
            border-bottom-right-radius: 10px;
            cursor: pointer;
            box-shadow: 0 15px 30px rgba(0,0,0,0.1);
            transition: transform 0.5s ease;
        }

        .envelope-wrapper:hover {
            transform: translateY(-10px);
        }

        /* Penutup Amplop Atas */
        .envelope-flap {
            position: absolute;
            top: 0;
            width: 0;
            height: 0;
            border-left: 150px solid transparent;
            border-right: 150px solid transparent;
            border-top: 100px solid var(--accent-color);
            transform-origin: top;
            transition: transform 0.5s ease 0.3s, z-index 0.2s ease 0.5s;
            z-index: 3;
        }

        /* Bagian Depan Amplop (Kiri & Kanan) */
        .envelope-front {
            position: absolute;
            width: 0;
            height: 0;
            border-top: 100px solid transparent;
            border-bottom: 100px solid transparent;
            border-left: 150px solid var(--primary-color);
            filter: brightness(0.95);
            z-index: 2;
        }
        .envelope-front-right {
            position: absolute;
            width: 0;
            height: 0;
            border-top: 100px solid transparent;
            border-bottom: 100px solid transparent;
            border-right: 150px solid var(--primary-color);
            right: 0;
            filter: brightness(0.9);
            z-index: 2;
        }

        /* Kertas di dalam amplop */
        .letter-preview {
            position: absolute;
            top: 10px;
            left: 15px;
            width: 270px;
            height: 180px;
            background-color: #fff;
            border-radius: 5px;
            z-index: 1;
            transition: transform 0.5s ease;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            padding: 10px;
            text-align: center;
            box-shadow: 0 0 10px rgba(0,0,0,0.05);
        }

        .letter-preview p {
            font-size: 14px;
            color: var(--text-color);
            font-weight: bold;
        }

        .heart-seal {
            position: absolute;
            top: 85px;
            left: 135px;
            width: 30px;
            height: 30px;
            background-color: #ff0a54;
            transform: rotate(45deg);
            z-index: 4;
            transition: transform 0.3s ease, opacity 0.3s ease 0.3s;
            cursor: pointer;
        }
        .heart-seal::before, .heart-seal::after {
            content: '';
            position: absolute;
            width: 30px;
            height: 30px;
            background-color: #ff0a54;
            border-radius: 50%;
        }
        .heart-seal::before { top: -15px; left: 0; }
        .heart-seal::after { left: -15px; top: 0; }

        /* Efek Amplop Terbuka */
        .envelope-wrapper.open .envelope-flap {
            transform: rotateX(180deg);
            z-index: 0;
        }
        .envelope-wrapper.open .heart-seal {
            opacity: 0;
            pointer-events: none;
        }
        .envelope-wrapper.open .letter-preview {
            transform: translateY(-80px);
        }

        /* --- SLIDE SURAT UTAMA --- */
        .slide-container {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) scale(0.9);
            width: 90%;
            max-width: 450px;
            background: var(--card-bg);
            padding: 30px 25px;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.15);
            text-align: center;
            z-index: 10;
            opacity: 0;
            transition: all 0.5s ease;
        }

        .slide-container.active {
            display: block;
            opacity: 1;
            transform: translate(-50%, -50%) scale(1);
        }

        .emoji-holder {
            font-size: 60px;
            margin-bottom: 20px;
            animation: bounce 2s infinite;
        }

        .slide-text {
            font-size: 18px;
            color: var(--text-color);
            line-height: 1.6;
            margin-bottom: 25px;
            font-weight: 500;
        }

        .btn-next {
            background-color: var(--accent-color);
            color: white;
            border: none;
            padding: 12px 30px;
            font-size: 16px;
            border-radius: 25px;
            cursor: pointer;
            font-weight: bold;
            transition: background 0.3s ease, transform 0.2s;
            box-shadow: 0 5px 15px rgba(251, 111, 146, 0.4);
        }

        .btn-next:hover {
            background-color: #ff477e;
            transform: translateY(-2px);
        }

        /* --- AUDIO PLAYER (TERSEMBUNYI ATAU KECIL) --- */
        .music-player {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: rgba(255, 255, 255, 0.8);
            padding: 10px;
            border-radius: 30px;
            display: flex;
            align-items: center;
            gap: 10px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            backdrop-filter: blur(5px);
            z-index: 100;
        }
        .music-player span {
            font-size: 12px;
            font-weight: bold;
            color: var(--text-color);
        }

        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }

        /* Latar belakang kelopak bunga berguguran */
        .heart-fall {
            position: absolute;
            color: rgba(251, 111, 146, 0.6);
            font-size: 20px;
            pointer-events: none;
            animation: fall linear forwards;
            z-index: 0;
        }

        @keyframes fall {
            to {
                transform: translateY(105vh) rotate(360deg);
            }
        }
    </style>
</head>
<body>

    <!-- PEMUTAR MUSIK -->
    <!-- Tips: Ganti link src di bawah dengan link lagu romantis format .mp3 favorit kalian berdua -->
    <div class="music-player">
        <span>🎵 Playback</span>
        <audio id="romantic-song" loop>
            <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" type="audio/mpeg">
            Browser kamu tidak mendukung pemutar musik.
        </audio>
    </div>

    <!-- TAMPILAN AMPLOP UTAMA -->
    <div class="envelope-wrapper" id="envelope" onclick="openLetter()">
        <div class="envelope-flap"></div>
        <div class="envelope-front"></div>
        <div class="envelope-front-right"></div>
        <div class="heart-seal"></div>
        <div class="letter-preview">
            <p>Ada surat spesial untukmu...</p>
            <p style="font-size: 11px; margin-top: 5px; color: #aaa;">(Klik untuk buka 💌)</p>
        </div>
    </div>

    <!-- SLIDE 1 -->
    <div class="slide-container" id="slide1">
        <div class="emoji-holder">✨🥰✨</div>
        <p class="slide-text">Hai Sayang! Aku tahu belakangan ini hari-harimu mungkin terasa melelahkan dan padat banget...</p>
        <button class="btn-next" onclick="nextSlide(2)">Lanjut ❤️</button>
    </div>

    <!-- SLIDE 2 -->
    <div class="slide-container" id="slide2">
        <div class="emoji-holder">💪🌟🏆</div>
        <p class="slide-text">Tapi aku cuma mau ingetin, kamu itu orang yang hebat banget. Aku bangga sekali punya kamu di hidupku!</p>
        <button class="btn-next" onclick="nextSlide(3)">Lanjut 🥰</button>
    </div>

    <!-- SLIDE 3 -->
    <div class="slide-container" id="slide3">
        <div class="emoji-holder">🥺🤲💖</div>
        <p class="slide-text">Apapun yang lagi kamu perjuangkan sekarang, jangan menyerah ya. Lelah itu wajar, tapi setelah ini kita istirahat bareng-bareng.</p>
        <button class="btn-next" onclick="nextSlide(4)">Lanjut 🕊️</button>
    </div>

    <!-- SLIDE 4 (AKHIR) -->
    <div class="slide-container" id="slide4">
        <div class="emoji-holder">👩‍❤️‍👨🌻🌹</div>
        <p class="slide-text">I love you so much! Aku selalu ada di sini buat dukung kamu dalam keadaan apapun. Semangat terus ya, Sayangku! 😘</p>
        <button class="btn-next" onclick="resetAll()">Peluk Lagi 🤗</button>
    </div>

    <script>
        const envelope = document.getElementById('envelope');
        const song = document.getElementById('romantic-song');

        // Fungsi Membuka Amplop & Mulai Lagu
        function openLetter() {
            envelope.classList.add('open');
            
            // Coba putar lagu (beberapa browser memblokir autoplay sebelum interaksi user, jadi ini waktu yang pas!)
            song.play().catch(error => {
                console.log("Autoplay dicegah oleh browser, lagu akan dimainkan setelah interaksi selanjutnya.");
            });

            // Beri efek transisi sebelum menampilkan slide pertama
            setTimeout(() => {
                envelope.style.display = 'none';
                document.getElementById('slide1').classList.add('active');
            }, 1200);

            // Mulai efek hujan hati/bintang
            setInterval(createHeart, 300);
        }

        // Fungsi Pindah Slide
        function nextSlide(slideNumber) {
            // Sembunyikan semua slide
            const slides = document.querySelectorAll('.slide-container');
            slides.forEach(slide => slide.classList.remove('active'));

            // Tampilkan slide tujuan
            const activeSlide = document.getElementById('slide' + slideNumber);
            if (activeSlide) {
                activeSlide.classList.add('active');
            }
        }

        // Kembali ke Amplop jika ingin diulang
        function resetAll() {
            const slides = document.querySelectorAll('.slide-container');
            slides.forEach(slide => slide.classList.remove('active'));
            envelope.classList.remove('open');
            envelope.style.display = 'block';
        }

        // Efek Dekorasi Hati Berguguran
        function createHeart() {
            const heart = document.createElement('div');
            heart.classList.add('heart-fall');
            heart.innerText = ['❤️', '💖', '✨', '🌸', '🧸'][Math.floor(Math.random() * 5)];
            heart.style.left = Math.random() * 100 + 'vw';
            heart.style.top = '-5vh';
            heart.style.animationDuration = Math.random() * 3 + 2 + 's';
            document.body.appendChild(heart);

            setTimeout(() => {
                heart.remove();
            }, 5000);
        }
    </script>
</body>
</html>
