<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Merry Christmas! My Self Introduction</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Kiwi+Maru:wght@400;500;700&family=Mountains+of+Christmas:wght@700&family=Nunito:wght@400;600;800&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        xmas: {
                            red: '#D62828',
                            darkred: '#9E1B1B',
                            green: '#165B33',
                            lightgreen: '#2B8A4C',
                            gold: '#F4A261',
                            accentgold: '#E9C46A',
                            night: '#0B132B',
                            deepnight: '#1C2541'
                        }
                    },
                    fontFamily: {
                        christmas: ['"Mountains of Christmas"', 'cursive'],
                        japanese: ['"Kiwi Maru"', 'serif'],
                        sans: ['"Nunito"', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Kiwi Maru', 'Nunito', sans-serif;
            background: linear-gradient(180deg, #050B14 0%, #0B132B 50%, #1C2541 100%);
            color: #F8F9FA;
            overflow-x: hidden;
            min-height: 100vh;
        }

        /* 雪のアニメーション用キャンバス */
        #snow-canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 10;
        }

        /* イルミネーションライト */
        .light-string {
            display: flex;
            justify-content: space-between;
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            z-index: 20;
            padding: 0;
            margin: 0;
            list-style: none;
            pointer-events: none;
            overflow: hidden;
        }
        .light-string li {
            position: relative;
            flex: 1;
            text-align: center;
            margin: 0;
            padding: 0;
            list-style: none;
        }
        .light-string li:before {
            content: "";
            position: absolute;
            top: -5px;
            left: 50%;
            width: 10px;
            height: 10px;
            border-radius: 50%;
            transform: translateX(-50%);
        }
        .light-red:before { background: #FF4D4D; box-shadow: 0px 5px 15px #FF4D4D; animation: glow-1 1.5s infinite alternate; }
        .light-green:before { background: #2ECC71; box-shadow: 0px 5px 15px #2ECC71; animation: glow-2 1.2s infinite alternate; }
        .light-gold:before { background: #F1C40F; box-shadow: 0px 5px 15px #F1C40F; animation: glow-3 1.8s infinite alternate; }
        .light-blue:before { background: #3498DB; box-shadow: 0px 5px 15px #3498DB; animation: glow-4 1.4s infinite alternate; }

        @keyframes glow-1 { 0% { opacity: 0.4; } 100% { opacity: 1; filter: drop-shadow(0 0 10px #FF4D4D); } }
        @keyframes glow-2 { 0% { opacity: 0.4; } 100% { opacity: 1; filter: drop-shadow(0 0 10px #2ECC71); } }
        @keyframes glow-3 { 0% { opacity: 0.4; } 100% { opacity: 1; filter: drop-shadow(0 0 10px #F1C40F); } }
        @keyframes glow-4 { 0% { opacity: 0.4; } 100% { opacity: 1; filter: drop-shadow(0 0 10px #3498DB); } }

        /* クリスマスカードの立体エフェクト */
        .glass-card {
            background: rgba(255, 255, 255, 0.07);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.15);
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
        }

        .xmas-border {
            border: 3px dashed #E9C46A;
        }

        /* 揺れるアニメーション */
        @keyframes swing {
            0% { transform: rotate(-5deg); }
            50% { transform: rotate(5deg); }
            100% { transform: rotate(-5deg); }
        }
        .animate-swing {
            animation: swing 3s ease-in-out infinite;
            transform-origin: top center;
        }

        /* プレゼントボックスの跳ねるアニメーション */
        @keyframes bounce-gentle {
            0%, 100% { transform: translateY(0) scale(1); }
            50% { transform: translateY(-8px) scale(1.03); }
        }
        .animate-gift {
            animation: bounce-gentle 2s ease-in-out infinite;
        }

        /* パーティクル用キャンバス */
        #confetti-canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 60;
        }
    </style>
</head>
<body class="relative min-h-screen pb-16">

    <!-- 雪降る背景キャンバス -->
    <canvas id="snow-canvas"></canvas>
    <!-- 開封・祝賀用パーティクルキャンバス -->
    <canvas id="confetti-canvas"></canvas>

    <!-- イルミネーションライトヘッダー -->
    <ul class="light-string" id="light-string"></ul>

    <!-- BGM & 音声トグルボタン (固定右上) -->
    <div class="fixed top-4 right-4 z-50 flex gap-2">
        <button id="sound-btn" onclick="toggleAudio()" class="glass-card hover:bg-white/20 text-white font-bold p-3 rounded-full flex items-center justify-center transition duration-300 shadow-lg border border-amber-300/40" title="BGMの再生/停止">
            <i id="sound-icon" class="fas fa-volume-xmark text-xl text-red-400"></i>
        </button>
    </div>

    <!-- メインコンテンツ領域 -->
    <main class="container mx-auto px-4 pt-12 max-w-4xl relative z-20">

        <!-- ヘッダータイトル -->
        <header class="text-center my-8">
            <div class="inline-block animate-swing mb-2">
                <span class="text-6xl drop-shadow-lg">🎄</span>
            </div>
            <h1 class="font-christmas text-5xl md:text-7xl font-bold tracking-wider text-transparent bg-clip-text bg-gradient-to-r from-red-400 via-amber-200 to-green-400 drop-shadow-md">
                Merry Christmas!
            </h1>
            <p class="text-amber-200/90 text-sm md:text-base mt-2 tracking-widest font-semibold flex items-center justify-center gap-2">
                <span>✨</span> サンタクロースがやってくる季節の特別な自己紹介ページ <span>✨</span>
            </p>
        </header>

        <!-- カウントダウンタイマー -->
        <section class="glass-card rounded-2xl p-4 md:p-6 mb-8 text-center border-t-2 border-amber-400/50 shadow-xl">
            <h2 class="text-lg md:text-xl font-bold text-amber-300 mb-3 flex items-center justify-center gap-2">
                <i class="fas fa-hourglass-half text-amber-400 animate-spin" style="animation-duration: 8s;"></i> クリスマス (12/25) までのカウントダウン
            </h2>
            <div class="grid grid-cols-4 gap-2 md:gap-4 max-w-md mx-auto">
                <div class="bg-xmas-night/80 rounded-xl p-3 border border-white/10 shadow-inner">
                    <span id="cd-days" class="block text-2xl md:text-4xl font-extrabold text-red-400">00</span>
                    <span class="text-xs text-gray-300 font-semibold">日</span>
                </div>
                <div class="bg-xmas-night/80 rounded-xl p-3 border border-white/10 shadow-inner">
                    <span id="cd-hours" class="block text-2xl md:text-4xl font-extrabold text-amber-300">00</span>
                    <span class="text-xs text-gray-300 font-semibold">時間</span>
                </div>
                <div class="bg-xmas-night/80 rounded-xl p-3 border border-white/10 shadow-inner">
                    <span id="cd-minutes" class="block text-2xl md:text-4xl font-extrabold text-green-400">00</span>
                    <span class="text-xs text-gray-300 font-semibold">分</span>
                </div>
                <div class="bg-xmas-night/80 rounded-xl p-3 border border-white/10 shadow-inner">
                    <span id="cd-seconds" class="block text-2xl md:text-4xl font-extrabold text-blue-300">00</span>
                    <span class="text-xs text-gray-300 font-semibold">秒</span>
                </div>
            </div>
        </section>

        <!-- メインプロフィールカード -->
        <section class="glass-card rounded-3xl p-6 md:p-10 mb-10 relative overflow-hidden shadow-2xl border-2 border-amber-300/30">
            <!-- クリスマス装飾コーナーアイコン -->
            <div class="absolute -top-4 -left-4 text-4xl text-amber-400 opacity-80 select-none">🎀</div>
            <div class="absolute -bottom-4 -right-4 text-4xl text-emerald-400 opacity-80 select-none">🎁</div>

            <div class="flex flex-col md:flex-row items-center gap-8">
                <!-- プロフィール画像/アバター -->
                <div class="relative group">
                    <div class="w-40 h-40 md:w-48 md:h-48 rounded-full overflow-hidden border-4 border-amber-300 shadow-2xl relative z-10 bg-gradient-to-b from-red-900 to-green-900 ring-4 ring-amber-400/20">
                        <img id="profile-img" src="https://placehold.co/300x300/165B33/F4A261?text=Merry+Me!" alt="Profile Avatar" class="w-full h-full object-cover transition duration-300 group-hover:scale-105">
                    </div>
                    <!-- サンタ帽デコレーション -->
                    <div class="absolute -top-7 -right-2 text-6xl z-20 transform rotate-12 pointer-events-none drop-shadow-lg">🎅</div>
                </div>

                <!-- プロフィール詳細 -->
                <div class="flex-1 text-center md:text-left">
                    <div class="flex flex-wrap items-center justify-center md:justify-start gap-3 mb-2">
                        <h2 id="profile-name" class="text-3xl md:text-4xl font-bold text-white tracking-wide">あなたの名前</h2>
                        <span id="profile-title" class="px-3 py-1 bg-red-600/80 text-amber-100 text-xs font-bold rounded-full border border-amber-300/40 shadow-sm">
                            🎄 Web Developer & Holiday Enthusiast
                        </span>
                    </div>

                    <p id="profile-bio" class="text-gray-200 text-sm md:text-base leading-relaxed my-4 bg-black/25 p-4 rounded-xl border border-white/10 shadow-inner">
                        こんにちは！クリスマスと暖かいココアが大好きなWebエンジニアです。冬の澄んだ空気がお気に入りで、毎年クリスマスのイルミネーションを見に散歩するのが楽しみです。素敵なホリデーシーズンを過ごしましょう！
                    </p>

                    <!-- クリスマスの好みグリッド -->
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 mt-4 text-left">
                        <div class="bg-white/5 p-3 rounded-xl border border-white/10 flex items-center gap-3 hover:bg-white/10 transition">
                            <span class="text-2xl">🎵</span>
                            <div>
                                <span class="text-xs text-amber-300 font-bold block">好きなクリスマスソング</span>
                                <span id="profile-song" class="text-sm font-semibold text-white">Last Christmas - Wham!</span>
                            </div>
                        </div>
                        <div class="bg-white/5 p-3 rounded-xl border border-white/10 flex items-center gap-3 hover:bg-white/10 transition">
                            <span class="text-2xl">🎁</span>
                            <div>
                                <span class="text-xs text-amber-300 font-bold block">今年欲しいプレゼント</span>
                                <span id="profile-wish" class="text-sm font-semibold text-white">新しいヘッドフォン & 温かい手袋</span>
                            </div>
                        </div>
                        <div class="bg-white/5 p-3 rounded-xl border border-white/10 flex items-center gap-3 hover:bg-white/10 transition">
                            <span class="text-2xl">☕</span>
                            <div>
                                <span class="text-xs text-amber-300 font-bold block">冬のお気に入り</span>
                                <span id="profile-favorite" class="text-sm font-semibold text-white">マシュマロ入りホットチョコレート</span>
                            </div>
                        </div>
                        <div class="bg-white/5 p-3 rounded-xl border border-white/10 flex items-center gap-3 hover:bg-white/10 transition">
                            <span class="text-2xl">❄️</span>
                            <div>
                                <span class="text-xs text-amber-300 font-bold block">冬の趣味</span>
                                <span id="profile-hobby" class="text-sm font-semibold text-white">イルミネーション撮影・映画鑑賞</span>
                            </div>
                        </div>
                    </div>

                    <!-- プロフィール編集ボタン -->
                    <div class="mt-6 text-right">
                        <button onclick="openEditModal()" class="px-4 py-1.5 bg-amber-400/20 hover:bg-amber-400/30 text-amber-200 text-xs font-bold rounded-full border border-amber-300/40 transition flex items-center gap-2 justify-end ml-auto">
                            <i class="fas fa-edit"></i> プロフィールを編集する
                        </button>
                    </div>
                </div>
            </div>
        </section>

        <!-- インタラクティブ機能：お楽しみプレゼントボックス -->
        <section class="glass-card rounded-3xl p-6 md:p-8 mb-10 text-center relative border border-emerald-500/40 shadow-xl">
            <h2 class="text-2xl font-bold text-emerald-300 mb-2 flex items-center justify-center gap-2">
                <i class="fas fa-gift text-red-400"></i> クリスマスのラッキープレゼント
            </h2>
            <p class="text-xs md:text-sm text-gray-300 mb-6">
                ボックスをクリックして、サンタからのメッセージや運勢をチェックしよう！
            </p>

            <div id="gift-box" onclick="openGift()" class="cursor-pointer inline-block animate-gift transition-transform hover:scale-110 active:scale-95">
                <div class="relative w-28 h-28 md:w-36 md:h-36 mx-auto bg-gradient-to-br from-red-600 to-red-800 rounded-2xl shadow-2xl flex items-center justify-center border-2 border-amber-300 group">
                    <!-- リボン -->
                    <div class="absolute inset-y-0 w-6 bg-amber-400 border-x border-amber-500"></div>
                    <div class="absolute inset-x-0 h-6 bg-amber-400 border-y border-amber-500"></div>
                    <!-- リボン結び目 -->
                    <div class="absolute -top-4 text-4xl drop-shadow-md">🎀</div>
                    <i class="fas fa-sparkles text-3xl text-amber-200 z-10 opacity-80 group-hover:rotate-12 transition"></i>
                </div>
            </div>

            <!-- 開封後のメッセージダイアログ -->
            <div id="gift-modal" class="hidden fixed inset-0 bg-black/70 backdrop-blur-md z-50 flex items-center justify-center p-4">
                <div class="glass-card max-w-md w-full p-6 rounded-3xl text-center border-2 border-amber-400 shadow-2xl">
                    <div class="text-6xl mb-3" id="gift-icon">🎉</div>
                    <h3 class="text-xl font-bold text-amber-300 mb-2" id="gift-title">ラッキーカード</h3>
                    <p class="text-white text-base leading-relaxed my-4 bg-white/10 p-4 rounded-xl border border-white/10" id="gift-message">
                        メッセージがここに表示されます。
                    </p>
                    <button onclick="closeGift()" class="px-6 py-2.5 bg-gradient-to-r from-red-600 to-green-600 text-white font-bold rounded-full hover:opacity-90 shadow-lg border border-amber-300/40">
                        閉じる
                    </button>
                </div>
            </div>
        </section>

        <!-- 掲示板：みんなの願い事・メッセージボード -->
        <section class="glass-card rounded-3xl p-6 md:p-8 mb-10 border border-amber-400/30 shadow-xl">
            <h2 class="text-2xl font-bold text-amber-300 mb-2 flex items-center justify-center gap-2">
                <i class="fas fa-star text-amber-400"></i> クリスマスメッセージ & 願い事ボード
            </h2>
            <p class="text-xs md:text-sm text-gray-300 text-center mb-6">
                ツリーに願い事を飾るように、メッセージを残してくださいね！
            </p>

            <!-- メッセージ投稿フォーム -->
            <form id="wish-form" onsubmit="addWish(event)" class="mb-6 space-y-3 max-w-lg mx-auto">
                <div class="flex gap-2">
                    <input type="text" id="wish-name" placeholder="お名前" required maxlength="15"
                           class="w-1/3 p-2.5 rounded-xl bg-black/40 border border-white/20 text-white text-sm focus:outline-none focus:border-amber-400">
                    <input type="text" id="wish-text" placeholder="願い事やメッセージを書く..." required maxlength="60"
                           class="w-2/3 p-2.5 rounded-xl bg-black/40 border border-white/20 text-white text-sm focus:outline-none focus:border-amber-400">
                </div>
                <button type="submit" class="w-full py-2.5 bg-gradient-to-r from-red-600 via-amber-600 to-green-600 text-white font-bold rounded-xl shadow-lg hover:brightness-110 transition flex items-center justify-center gap-2 text-sm">
                    <i class="fas fa-paper-plane"></i> オーナメントを飾る（投稿）
                </button>
            </form>

            <!-- メッセージ一覧グリッド -->
            <div id="wish-list" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-3">
                <!-- JavaScriptで投稿カードを生成 -->
            </div>
        </section>

        <!-- フッター -->
        <footer class="text-center text-xs text-gray-400 py-6">
            <p class="font-christmas text-xl text-amber-200">Wishing you a happy Christmas season!</p>
            <p class="mt-1">© 2026 Interactive Christmas Profile. All Rights Reserved.</p>
        </footer>

    </main>

    <!-- プロフィール編集モーダル -->
    <div id="edit-modal" class="hidden fixed inset-0 bg-black/80 backdrop-blur-md z-50 flex items-center justify-center p-4">
        <div class="glass-card max-w-lg w-full p-6 rounded-3xl border border-amber-400 max-h-[90vh] overflow-y-auto">
            <h3 class="text-xl font-bold text-amber-300 mb-4 flex items-center justify-between">
                <span><i class="fas fa-user-edit"></i> プロフィール情報の編集</span>
                <button onclick="closeEditModal()" class="text-gray-400 hover:text-white"><i class="fas fa-times"></i></button>
            </h3>

            <form onsubmit="saveProfile(event)" class="space-y-3 text-left text-xs md:text-sm">
                <div>
                    <label class="block text-amber-200 mb-1">お名前</label>
                    <input type="text" id="edit-name" class="w-full p-2 bg-black/50 rounded-lg border border-white/20 text-white" required>
                </div>
                <div>
                    <label class="block text-amber-200 mb-1">肩書き/キャッチコピー</label>
                    <input type="text" id="edit-title" class="w-full p-2 bg-black/50 rounded-lg border border-white/20 text-white" required>
                </div>
                <div>
                    <label class="block text-amber-200 mb-1">自己紹介文</label>
                    <textarea id="edit-bio" rows="3" class="w-full p-2 bg-black/50 rounded-lg border border-white/20 text-white" required></textarea>
                </div>
                <div>
                    <label class="block text-amber-200 mb-1">好きなクリスマスソング</label>
                    <input type="text" id="edit-song" class="w-full p-2 bg-black/50 rounded-lg border border-white/20 text-white" required>
                </div>
                <div>
                    <label class="block text-amber-200 mb-1">今年欲しいもの</label>
                    <input type="text" id="edit-wish" class="w-full p-2 bg-black/50 rounded-lg border border-white/20 text-white" required>
                </div>
                <div>
                    <label class="block text-amber-200 mb-1">冬のお気に入り</label>
                    <input type="text" id="edit-favorite" class="w-full p-2 bg-black/50 rounded-lg border border-white/20 text-white" required>
                </div>
                <div>
                    <label class="block text-amber-200 mb-1">冬の趣味</label>
                    <input type="text" id="edit-hobby" class="w-full p-2 bg-black/50 rounded-lg border border-white/20 text-white" required>
                </div>
                <div>
                    <label class="block text-amber-200 mb-1">画像URL (任意)</label>
                    <input type="text" id="edit-img" class="w-full p-2 bg-black/50 rounded-lg border border-white/20 text-white">
                </div>

                <div class="flex gap-2 pt-3">
                    <button type="button" onclick="closeEditModal()" class="w-1/2 py-2 bg-gray-600 text-white rounded-xl font-bold">キャンセル</button>
                    <button type="submit" class="w-1/2 py-2 bg-red-600 text-white rounded-xl font-bold">保存する</button>
                </div>
            </form>
        </div>
    </div>

    <!-- JavaScript ロジック -->
    <script>
        /* ----------------------------------------------------
         * 雪降りキャンバス
         * ---------------------------------------------------- */
        const snowCanvas = document.getElementById('snow-canvas');
        const sCtx = snowCanvas.getContext('2d');
        let sWidth = snowCanvas.width = window.innerWidth;
        let sHeight = snowCanvas.height = window.innerHeight;

        const flakes = Array.from({ length: 80 }, () => ({
            x: Math.random() * sWidth,
            y: Math.random() * sHeight,
            r: Math.random() * 3 + 1,
            d: Math.random() + 0.5
        }));

        function drawSnow() {
            sCtx.clearRect(0, 0, sWidth, sHeight);
            sCtx.fillStyle = 'rgba(255, 255, 255, 0.8)';
            sCtx.beginPath();
            flakes.forEach(f => {
                sCtx.moveTo(f.x, f.y);
                sCtx.arc(f.x, f.y, f.r, 0, Math.PI * 2);
                f.y += f.d;
                f.x += Math.sin(f.y / 20) * 0.5;
                if (f.y > sHeight) {
                    f.y = -10;
                    f.x = Math.random() * sWidth;
                }
            });
            sCtx.fill();
            requestAnimationFrame(drawSnow);
        }
        drawSnow();

        /* ----------------------------------------------------
         * 1. イルミネーションライトの動的生成
         * ---------------------------------------------------- */
        const lightString = document.getElementById('light-string');
        const colors = ['light-red', 'light-green', 'light-gold', 'light-blue'];
        const lightCount = Math.floor(window.innerWidth / 30);
        
        for (let i = 0; i < lightCount; i++) {
            const li = document.createElement('li');
            li.className = colors[i % colors.length];
            lightString.appendChild(li);
        }

        /* ----------------------------------------------------
         * Web Audio API (効果音・BGM)
         * ---------------------------------------------------- */
        let audioCtx = null;
        let isPlaying = false;
        let bgmInterval = null;

        function initAudioContext() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
            if (audioCtx.state === 'suspended') {
                audioCtx.resume();
            }
        }

        // チャイム効果音
        function playChimeEffect() {
            initAudioContext();
            if (!audioCtx) return;

            const notes = [523.25, 659.25, 783.99, 1046.50]; // C5, E5, G5, C6
            notes.forEach((freq, idx) => {
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.type = 'sine';
                osc.frequency.setValueAtTime(freq, audioCtx.currentTime + idx * 0.08);

                gain.gain.setValueAtTime(0.2, audioCtx.currentTime + idx * 0.08);
                gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + idx * 0.08 + 0.6);

                osc.connect(gain);
                gain.connect(audioCtx.destination);
                osc.start(audioCtx.currentTime + idx * 0.08);
                osc.stop(audioCtx.currentTime + idx * 0.08 + 0.6);
            });
        }

        // 簡易ジングルベルBGM
        function toggleAudio() {
            initAudioContext();
            const icon = document.getElementById('sound-icon');
            
            if (isPlaying) {
                isPlaying = false;
                clearInterval(bgmInterval);
                icon.className = 'fas fa-volume-xmark text-xl text-red-400';
            } else {
                isPlaying = true;
                icon.className = 'fas fa-volume-high text-xl text-green-400 animate-pulse';
                playJingleBgm();
            }
        }

        function playJingleBgm() {
            const melody = [
                { f: 659.25, d: 0.25 }, { f: 659.25, d: 0.25 }, { f: 659.25, d: 0.5 },
                { f: 659.25, d: 0.25 }, { f: 659.25, d: 0.25 }, { f: 659.25, d: 0.5 },
                { f: 659.25, d: 0.25 }, { f: 783.99, d: 0.25 }, { f: 523.25, d: 0.35 },
                { f: 587.33, d: 0.15 }, { f: 659.25, d: 0.8 }
            ];
            let noteIdx = 0;

            bgmInterval = setInterval(() => {
                if (!isPlaying) return;
                const note = melody[noteIdx];
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();

                osc.type = 'triangle';
                osc.frequency.setValueAtTime(note.f, audioCtx.currentTime);

                gain.gain.setValueAtTime(0.12, audioCtx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + note.d * 0.8);

                osc.connect(gain);
                gain.connect(audioCtx.destination);

                osc.start(audioCtx.currentTime);
                osc.stop(audioCtx.currentTime + note.d * 0.8);

                noteIdx = (noteIdx + 1) % melody.length;
            }, 350);
        }

        /* ----------------------------------------------------
         * カウントダウンタイマー
         * ---------------------------------------------------- */
        function updateCountdown() {
            const now = new Date();
            let targetYear = now.getFullYear();
            let xmasDate = new Date(targetYear, 11, 25); // 12月25日

            if (now > xmasDate) {
                xmasDate = new Date(targetYear + 1, 11, 25);
            }

            const diff = xmasDate - now;

            const days = Math.floor(diff / (1000 * 60 * 60 * 24));
            const hours = Math.floor((diff / (1000 * 60 * 60)) % 24);
            const minutes = Math.floor((diff / (1000 * 60)) % 60);
            const seconds = Math.floor((diff / 1000) % 60);

            document.getElementById('cd-days').textContent = String(days).padStart(2, '0');
            document.getElementById('cd-hours').textContent = String(hours).padStart(2, '0');
            document.getElementById('cd-minutes').textContent = String(minutes).padStart(2, '0');
            document.getElementById('cd-seconds').textContent = String(seconds).padStart(2, '0');
        }
        setInterval(updateCountdown, 1000);
        updateCountdown();

        /* ----------------------------------------------------
         * パーティクル（クラッカー）エフェクト
         * ---------------------------------------------------- */
        const confettiCanvas = document.getElementById('confetti-canvas');
        const cCtx = confettiCanvas.getContext('2d');
        let cWidth = confettiCanvas.width = window.innerWidth;
        let cHeight = confettiCanvas.height = window.innerHeight;

        window.addEventListener('resize', () => {
            cWidth = confettiCanvas.width = window.innerWidth;
            cHeight = confettiCanvas.height = window.innerHeight;
            sWidth = snowCanvas.width = window.innerWidth;
            sHeight = snowCanvas.height = window.innerHeight;
        });

        class Confetti {
            constructor(x, y) {
                this.x = x;
                this.y = y;
                this.size = Math.random() * 8 + 4;
                this.speedX = (Math.random() - 0.5) * 12;
                this.speedY = (Math.random() - 0.7) * 12;
                this.gravity = 0.3;
                this.color = ['#FF4D4D', '#2ECC71', '#F1C40F', '#E9C46A', '#3498DB', '#FFFFFF'][Math.floor(Math.random() * 6)];
                this.rotation = Math.random() * 360;
                this.rotationSpeed = (Math.random() - 0.5) * 10;
                this.opacity = 1;
            }

            update() {
                this.x += this.speedX;
                this.y += this.speedY;
                this.speedY += this.gravity;
                this.rotation += this.rotationSpeed;
                this.opacity -= 0.015;
            }

            draw() {
                cCtx.save();
                cCtx.translate(this.x, this.y);
                cCtx.rotate((this.rotation * Math.PI) / 180);
                cCtx.fillStyle = this.color;
                cCtx.globalAlpha = Math.max(0, this.opacity);
                cCtx.fillRect(-this.size / 2, -this.size / 2, this.size, this.size);
                cCtx.restore();
            }
        }

        let particles = [];

        function triggerConfetti() {
            const originX = cWidth / 2;
            const originY = cHeight / 2;
            for (let i = 0; i < 70; i++) {
                particles.push(new Confetti(originX, originY));
            }
        }

        function animateConfetti() {
            cCtx.clearRect(0, 0, cWidth, cHeight);
            particles = particles.filter(p => p.opacity > 0);
            particles.forEach(p => {
                p.update();
                p.draw();
            });
            requestAnimationFrame(animateConfetti);
        }
        animateConfetti();

        /* ----------------------------------------------------
         * ギフトボックス開封＆効果音呼び出し
         * ---------------------------------------------------- */
        const giftMessages = [
            { icon: '🎄', title: '大吉クリスマス！', msg: 'サンタさんからの特別なプレゼントが届く予感！最高のクリスマスイブになりそうです。' },
            { icon: '🎂', title: 'スイーツ大吉！', msg: '美味しいブッシュ・ド・ノエルやショートケーキでお腹も心も満たされる最高のホリデーになります！' },
            { icon: '🌟', title: '星に願いを！', msg: 'あなたが願っていた夢や欲しいものが叶う素敵な1日になります。' },
            { icon: '⛄', title: 'ほっこり幸運！', msg: '暖かいココアと一緒に大切な人と過ごす心地よい時間が訪れます。' },
            { icon: '🎁', title: 'サプライズ運！', msg: '思いがけない人からのメッセージや素敵なサプライズギフトが届くかも！' }
        ];

        function openGift() {
            playChimeEffect();
            triggerConfetti();
            const randomGift = giftMessages[Math.floor(Math.random() * giftMessages.length)];
            
            document.getElementById('gift-icon').textContent = randomGift.icon;
            document.getElementById('gift-title').textContent = randomGift.title;
            document.getElementById('gift-message').textContent = randomGift.msg;

            document.getElementById('gift-modal').classList.remove('hidden');
        }

        function closeGift() {
            document.getElementById('gift-modal').classList.add('hidden');
        }

        /* ----------------------------------------------------
         * メッセージ掲示板ロジック
         * ---------------------------------------------------- */
        let wishes = [
            { name: 'サンタクロース', text: 'みんなに素敵な笑顔とプレゼントが届きますように！🎅' },
            { name: 'トナカイ', text: '美味しいニンジンと綺麗な夜空が楽しみです！✨' }
        ];

        function loadWishes() {
            try {
                const stored = localStorage.getItem('xmas_wishes');
                if (stored) {
                    wishes = JSON.parse(stored);
                }
            } catch(e) {}
            renderWishes();
        }

        function renderWishes() {
            const list = document.getElementById('wish-list');
            list.innerHTML = '';

            wishes.forEach((w) => {
                const card = document.createElement('div');
                card.className = 'bg-white/10 p-3 rounded-2xl border border-white/10 shadow hover:bg-white/15 transition relative text-left';
                card.innerHTML = `
                    <div class="flex items-center gap-2 mb-1">
                        <span class="text-amber-400 font-bold text-xs"><i class="fas fa-snowflake"></i> ${escapeHTML(w.name)}</span>
                    </div>
                    <p class="text-white text-xs leading-relaxed">${escapeHTML(w.text)}</p>
                `;
                list.appendChild(card);
            });
        }

        function addWish(event) {
            event.preventDefault();
            const nameInput = document.getElementById('wish-name');
            const textInput = document.getElementById('wish-text');

            const newWish = {
                name: nameInput.value.trim(),
                text: textInput.value.trim()
            };

            wishes.unshift(newWish);
            try {
                localStorage.setItem('xmas_wishes', JSON.stringify(wishes));
            } catch(e) {}

            renderWishes();
            playChimeEffect();
            triggerConfetti();

            nameInput.value = '';
            textInput.value = '';
        }

        function escapeHTML(str) {
            return str.replace(/[&<>'"]/g, 
                tag => ({ '&': '&amp;', '<': '&lt;', '>': '&gt;', "'": '&#39;', '"': '&quot;' }[tag] || tag)
            );
        }

        /* ----------------------------------------------------
         * プロフィール情報の編集機能
         * ---------------------------------------------------- */
        function openEditModal() {
            document.getElementById('edit-name').value = document.getElementById('profile-name').textContent;
            document.getElementById('edit-title').value = document.getElementById('profile-title').textContent;
            document.getElementById('edit-bio').value = document.getElementById('profile-bio').textContent;
            document.getElementById('edit-song').value = document.getElementById('profile-song').textContent;
            document.getElementById('edit-wish').value = document.getElementById('profile-wish').textContent;
            document.getElementById('edit-favorite').value = document.getElementById('profile-favorite').textContent;
            document.getElementById('edit-hobby').value = document.getElementById('profile-hobby').textContent;
            document.getElementById('edit-img').value = document.getElementById('profile-img').src;

            document.getElementById('edit-modal').classList.remove('hidden');
        }

        function closeEditModal() {
            document.getElementById('edit-modal').classList.add('hidden');
        }

        function saveProfile(event) {
            event.preventDefault();
            document.getElementById('profile-name').textContent = document.getElementById('edit-name').value;
            document.getElementById('profile-title').textContent = document.getElementById('edit-title').value;
            document.getElementById('profile-bio').textContent = document.getElementById('edit-bio').value;
            document.getElementById('profile-song').textContent = document.getElementById('edit-song').value;
            document.getElementById('profile-wish').textContent = document.getElementById('edit-wish').value;
            document.getElementById('profile-favorite').textContent = document.getElementById('edit-favorite').value;
            document.getElementById('profile-hobby').textContent = document.getElementById('edit-hobby').value;

            const imgUrl = document.getElementById('edit-img').value;
            if (imgUrl) {
                document.getElementById('profile-img').src = imgUrl;
            }

            closeEditModal();
        }

        // 初期化処理
        window.onload = function() {
            loadWishes();
        };
    </script>
</body>
</html>