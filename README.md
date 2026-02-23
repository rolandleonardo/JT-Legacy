<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    
    <!-- SEO y Web App -->
    <title>JT Legacy: The Ultimate Fan Experience</title>
    <meta name="description" content="Demuestra cuánto sabes sobre Justin Timberlake en esta trivia interactiva.">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="theme-color" content="#000000">

    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        
        :root {
            --jt-gold: #d4af37;
            --app-bg: #000000;
        }

        body {
            background-color: var(--app-bg);
            color: #ffffff;
            font-family: 'Outfit', sans-serif;
            margin: 0; padding: 0;
            overflow-x: hidden;
            -webkit-user-select: none;
            user-select: none;
            touch-action: manipulation;
        }

        /* Contenedor para centrar en Desktop */
        .web-container {
            max-width: 500px;
            margin: 0 auto;
            height: 100vh;
            position: relative;
            background: #000;
            display: flex;
            flex-direction: column;
            border-left: 1px solid rgba(255,255,255,0.05);
            border-right: 1px solid rgba(255,255,255,0.05);
        }

        .safe-area-top { padding-top: env(safe-area-inset-top); }
        .safe-area-bottom { padding-bottom: env(safe-area-inset-bottom); }

        .option-btn {
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.1);
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
            border-radius: 1.25rem;
        }

        @media (hover: hover) {
            .option-btn:hover {
                background: rgba(255, 255, 255, 0.1);
                border-color: rgba(255, 255, 255, 0.3);
            }
        }

        .option-btn:active {
            transform: scale(0.96);
        }

        .option-correct {
            background: var(--jt-gold) !important;
            color: #000 !important;
            border-color: var(--jt-gold) !important;
            box-shadow: 0 0 30px rgba(212, 175, 55, 0.4);
        }

        .option-wrong {
            background: #ff3b30 !important;
            border-color: #ff3b30 !important;
        }

        #timer-progress { transition: width 0.1s linear; }

        .shake {
            animation: shake 0.4s cubic-bezier(.36,.07,.19,.97) both;
        }
        @keyframes shake {
            10%, 90% { transform: translate3d(-2px, 0, 0); }
            20%, 80% { transform: translate3d(4px, 0, 0); }
            30%, 50%, 70% { transform: translate3d(-6px, 0, 0); }
        }

        .modal-screen {
            position: absolute;
            inset: 0;
            background: black;
            z-index: 100;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 2.5rem;
            text-align: center;
        }

        .hidden { display: none !important; }
    </style>
</head>
<body>

    <div class="web-container">
        <!-- Header -->
        <header class="safe-area-top w-full px-6 py-4 flex justify-between items-end bg-black/80 backdrop-blur-lg border-b border-white/5">
            <div class="flex flex-col">
                <p class="text-[9px] font-black tracking-widest text-zinc-500 uppercase">Fans en el Tour</p>
                <div id="lives-container" class="flex gap-1.5 mt-1 text-lg"></div>
            </div>
            <div class="text-right">
                <p class="text-[9px] font-black tracking-widest text-zinc-500 uppercase">Precisión Global</p>
                <p class="text-xl font-black text-white" id="score-display">0/0</p>
            </div>
        </header>

        <!-- Main Content -->
        <main class="flex-1 flex flex-col px-6 justify-center">
            <div class="w-full h-1 bg-zinc-900 rounded-full overflow-hidden mb-10">
                <div id="timer-progress" class="h-full bg-white w-full shadow-[0_0_10px_rgba(255,255,255,0.5)]"></div>
            </div>

            <div id="game-area">
                <div class="flex justify-between items-center mb-6">
                    <span id="category" class="text-[10px] font-black bg-jt-gold text-black px-2 py-0.5 rounded uppercase">JT Trivia</span>
                    <span id="q-left" class="text-[10px] text-zinc-500 font-bold uppercase">Disponibles: 0</span>
                </div>
                
                <h2 id="question-text" class="text-2xl md:text-3xl font-light leading-tight min-h-[140px] flex items-center">
                    Cargando el escenario...
                </h2>

                <div id="options-grid" class="grid grid-cols-1 gap-3 pb-6"></div>
            </div>
        </main>

        <!-- Footer -->
        <footer class="safe-area-bottom px-10 py-5 flex justify-between items-center border-t border-zinc-900 bg-black">
            <button onclick="toggleModal('credits', true)" class="text-[10px] font-black uppercase tracking-widest text-zinc-500 hover:text-white transition-colors">Acerca de</button>
            <div class="h-1 w-8 bg-zinc-800 rounded-full"></div>
            <button onclick="confirmReset()" class="text-[10px] font-black uppercase tracking-widest text-zinc-500 hover:text-white transition-colors">Reiniciar</button>
        </footer>

        <!-- Start Modal -->
        <div id="start-screen" class="modal-screen">
            <div class="w-20 h-20 bg-white rounded-3xl flex items-center justify-center mb-6 shadow-xl">
                <span class="text-black text-4xl font-black italic">JT</span>
            </div>
            <h1 class="text-5xl font-black tracking-tighter mb-1">LEGACY</h1>
            <p class="text-[10px] text-jt-gold tracking-[0.4em] uppercase mb-12 italic">The Web Experience</p>
            <button id="start-btn" class="w-full py-4 bg-white text-black font-black uppercase tracking-widest rounded-xl shadow-2xl active:scale-95 transition-all">
                Entrar al Show
            </button>
            <p class="mt-8 text-[9px] text-zinc-600 uppercase tracking-widest">Optimizado para móviles y escritorio</p>
        </div>

        <!-- Result Modal -->
        <div id="result-screen" class="modal-screen hidden">
            <h3 id="res-title" class="text-4xl font-black mb-4 italic uppercase">GIRA TERMINADA</h3>
            <div class="bg-zinc-900/50 p-8 rounded-3xl mb-10 w-full border border-white/5">
                <p class="text-zinc-500 text-[10px] uppercase font-black mb-2">Tu Record</p>
                <p class="text-5xl font-black text-white" id="res-score">0/0</p>
            </div>
            <button id="retry-btn" class="w-full py-4 border border-white text-white font-black uppercase tracking-widest rounded-xl hover:bg-white hover:text-black transition-all">
                Volver a Intentar
            </button>
        </div>

        <!-- Credits Modal -->
        <div id="credits-screen" class="modal-screen hidden !justify-start pt-16">
            <h2 class="text-3xl font-black mb-8 tracking-tighter">INFORMACIÓN</h2>
            <div class="w-full text-left space-y-6">
                <p class="text-zinc-400 text-sm font-light">Esta es una aplicación web creada para la comunidad de fans de Justin Timberlake. No requiere instalación y guarda tu progreso automáticamente en este navegador.</p>
                <div class="p-4 bg-zinc-900 rounded-2xl">
                    <h4 class="text-jt-gold text-[10px] font-black mb-2 uppercase">Tip de Web App</h4>
                    <p class="text-xs text-zinc-500">En iPhone usa "Compartir > Añadir a pantalla de inicio" para usarla sin las barras del navegador.</p>
                </div>
                <button onclick="hardReset()" class="w-full py-3 rounded-lg border border-red-900/30 text-red-500 text-[10px] font-black uppercase">
                    Borrar Todo el Progreso
                </button>
            </div>
            <button onclick="toggleModal('credits', false)" class="mt-auto mb-8 text-zinc-600 text-[10px] font-black uppercase tracking-widest">Cerrar</button>
        </div>
    </div>

    <script>
        // Sistema de Audio UI
        let audioCtx;
        function initAudio() { if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)(); }
        function playTone(freq, type, duration, volume) {
            initAudio();
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.type = type;
            osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
            gain.gain.setValueAtTime(volume, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.0001, audioCtx.currentTime + duration);
            osc.connect(gain); gain.connect(audioCtx.destination);
            osc.start(); osc.stop(audioCtx.currentTime + duration);
        }

        const SFX = {
            correct: () => { playTone(523, 'sine', 0.4, 0.1); setTimeout(() => playTone(659, 'sine', 0.4, 0.1), 80); },
            wrong: () => { playTone(110, 'sawtooth', 0.5, 0.05); },
            tick: (f) => { playTone(f ? 1000 : 700, 'square', 0.04, 0.005); }
        };

        // Datos del Juego
        const QUESTIONS = [
            { id: 801, category: "INICIOS", q: "¿En qué programa de talentos debutó Justin cantando música country?", options: ["Star Search", "The Mickey Mouse Club", "American Idol", "X-Factor"], correct: 0 },
            { id: 802, category: "MÚSICA", q: "¿Cuál es el nombre del álbum que Justin lanzó en 2024?", options: ["Everything I Thought It Was", "Man of the Woods", "The 20/20 Experience", "FutureSex"], correct: 0 },
            { id: 803, category: "FAMILIA", q: "¿En qué ciudad nació Justin Timberlake?", options: ["Memphis, Tennessee", "Nashville, Tennessee", "Los Ángeles, California", "Chicago, Illinois"], correct: 0 },
            { id: 804, category: "CINE", q: "¿Cómo se llama el personaje de Justin en la película 'In Time'?", options: ["Will Salas", "Sean Parker", "Branch", "Artie"], correct: 0 },
            { id: 805, category: "NSYNC", q: "¿Quién fue el último integrante en unirse a NSYNC?", options: ["Lance Bass", "Justin Timberlake", "JC Chasez", "Joey Fatone"], correct: 0 },
            { id: 806, category: "HITS", q: "¿Cuál fue el sencillo principal de su primer álbum 'Justified'?", options: ["Like I Love You", "Cry Me a River", "Rock Your Body", "Senorita"], correct: 0 },
            { id: 807, category: "PERSONAL", q: "¿Qué deporte practica Justin a nivel casi profesional?", options: ["Golf", "Baloncesto", "Tenis", "Béisbol"], correct: 0 },
            { id: 808, category: "TOUR", q: "¿Qué instrumento toca Justin frecuentemente durante sus shows en vivo?", options: ["Guitarra y Piano", "Batería", "Saxo", "Violín"], correct: 0 },
            { id: 809, category: "HITS", q: "¿Con qué rapero colaboró en el mega éxito 'SexyBack'?", options: ["Timbaland", "Jay-Z", "Snoop Dogg", "Pharrell"], correct: 0 },
            { id: 810, category: "ESTILO", q: "¿Qué marca de ropa propia fundó Justin en 2005?", options: ["William Rast", "JT Collection", "Timberlake Jeans", "Memphis Denim"], correct: 0 }
        ];

        let mastered = JSON.parse(localStorage.getItem('jt_web_mastered')) || [];
        let pool = [];
        let cur = 0, lives = 3, timer = 10, interval;
        let stats = { ok: 0, total: 0 };

        function init() {
            pool = QUESTIONS.filter(q => !mastered.includes(q.id)).sort(() => Math.random() - 0.5);
            document.getElementById('q-left').innerText = `Disponibles: ${pool.length}`;
            updateHeader();
        }

        function loadQuestion() {
            if(cur >= pool.length || lives <= 0) return endGame(lives > 0);
            
            const q = pool[cur];
            document.getElementById('category').innerText = q.category;
            document.getElementById('question-text').innerText = q.q;
            
            const grid = document.getElementById('options-grid');
            grid.innerHTML = '';
            
            q.options.forEach((opt, i) => {
                const btn = document.createElement('button');
                btn.className = 'option-btn p-5 text-left font-bold text-sm md:text-base';
                btn.innerText = opt;
                btn.onclick = () => handleAnswer(i, btn);
                grid.appendChild(btn);
            });

            timer = 10;
            clearInterval(interval);
            interval = setInterval(() => {
                timer -= 0.1;
                document.getElementById('timer-progress').style.width = (timer * 10) + "%";
                if(timer < 3 && Math.abs(timer % 0.5) < 0.05) SFX.tick(true);
                else if(Math.abs(timer % 1) < 0.05) SFX.tick(false);
                if(timer <= 0) handleFail();
            }, 100);
        }

        function handleAnswer(idx, btn) {
            clearInterval(interval);
            stats.total++;
            const correctIdx = pool[cur].correct;

            if(idx === correctIdx) {
                btn.classList.add('option-correct');
                SFX.correct();
                stats.ok++;
                mastered.push(pool[cur].id);
                localStorage.setItem('jt_web_mastered', JSON.stringify(mastered));
                setTimeout(() => { cur++; loadQuestion(); updateHeader(); }, 700);
            } else {
                btn.classList.add('option-wrong');
                document.getElementById('options-grid').children[correctIdx].classList.add('option-correct');
                handleFail();
            }
            updateHeader();
        }

        function handleFail() {
            clearInterval(interval);
            lives--;
            SFX.wrong();
            document.querySelector('.web-container').classList.add('shake');
            setTimeout(() => document.querySelector('.web-container').classList.remove('shake'), 400);
            updateHeader();
            if(lives <= 0) setTimeout(() => endGame(false), 1000);
            else setTimeout(() => { cur++; loadQuestion(); }, 1200);
        }

        function updateHeader() {
            document.getElementById('score-display').innerText = `${stats.ok}/${stats.total}`;
            const container = document.getElementById('lives-container');
            container.innerHTML = Array(3).fill(0).map((_, i) => 
                `<span style="opacity: ${i < lives ? 1 : 0.15}; transition: opacity 0.3s">🎤</span>`
            ).join('');
        }

        function endGame(win) {
            document.getElementById('result-screen').classList.remove('hidden');
            document.getElementById('res-score').innerText = `${stats.ok}/${stats.total}`;
            document.getElementById('res-title').innerText = win ? "PERFECT SHOW" : "SET INTERRUMPIDO";
        }

        function toggleModal(id, show) {
            document.getElementById(`${id}-screen`).classList[show ? 'remove' : 'add']('hidden');
        }

        function confirmReset() { if(confirm("¿Quieres reiniciar esta partida?")) location.reload(); }
        function hardReset() { if(confirm("¿Borrar todo el historial de aciertos?")) { localStorage.clear(); location.reload(); } }

        document.getElementById('start-btn').onclick = () => {
            initAudio();
            document.getElementById('start-screen').classList.add('hidden');
            init(); loadQuestion();
        };

        document.getElementById('retry-btn').onclick = () => {
            lives = 3; cur = 0; stats = { ok: 0, total: 0 };
            document.getElementById('result-screen').classList.add('hidden');
            init(); loadQuestion();
        };

        init();
    </script>
</body>
</html>

