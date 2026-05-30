<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>平方關係幾何去顫儀 - 醫護數學跨領域教學</title>
    <!-- 使用 Tailwind CSS 打造現代化醫療科技感 UI -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;700;900&display=swap');
        body {
            font-family: 'Noto Sans TC', sans-serif;
            background-color: #09090b;
            touch-action: manipulation;
        }
        /* 科技感外觀 */
        .glass-panel {
            background: rgba(24, 24, 27, 0.9);
            border: 1px solid rgba(63, 63, 70, 0.4);
        }
        .neon-text-green {
            text-shadow: 0 0 10px rgba(16, 185, 129, 0.6);
        }
        /* 搖晃震動 */
        .shake {
            animation: shake-ani 0.15s 2 ease-in-out;
        }
        @keyframes shake-ani {
            0%, 100% { transform: translateX(0); }
            50% { transform: translateX(-8px); }
        }
    </style>
</head>
<body class="text-zinc-100 min-h-screen flex flex-col items-center justify-start py-4 px-3 md:py-8 md:px-6">

    <!-- 遊戲主容器：擴大為 max-w-6xl 給予兩側更多呼吸空間 -->
    <div id="main-container" class="max-w-6xl w-full bg-zinc-900/95 rounded-[2.5rem] p-5 md:p-8 shadow-[0_30px_100px_rgba(0,0,0,0.85)] border border-zinc-800/80 relative">
        
        <!-- 頂部標題列 -->
        <div class="flex flex-col md:flex-row justify-between items-start md:items-center gap-4 mb-6 pb-6 border-b border-zinc-800/80">
            <div>
                <h1 class="text-2xl md:text-3xl font-black bg-gradient-to-r from-emerald-400 to-teal-500 bg-clip-text text-transparent italic tracking-tight">平方關係幾何去顫儀</h1>
                <p class="text-xs text-zinc-400 mt-1">🩺 醫護臨床數學：運用「三角函數平方關係式」補全電擊迴路，搶救病患！</p>
            </div>
            
            <div class="flex gap-2 items-center">
                <div class="bg-zinc-950/80 px-4 py-2 rounded-2xl border border-zinc-800/80 text-center font-mono">
                    <span class="text-zinc-500 block text-[8px] uppercase tracking-wider">成功復律次數</span>
                    <span id="score-val" class="text-emerald-400 font-bold text-sm">0</span>
                </div>
                <button onclick="toggleMute()" id="mute-btn" class="bg-zinc-800 hover:bg-zinc-700 p-2.5 rounded-2xl transition-all border border-zinc-700">
                    <svg id="speaker-icon" xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="text-zinc-300"><path d="M11 5L6 9H2v6h4l5 4V5z"></path><path d="M19.07 4.93a10 10 0 0 1 0 14.14M15.54 8.46a5 5 0 0 1 0 7.07"></path></svg>
                </button>
            </div>
        </div>

        <!-- 雙區塊版面：左側幾何解碼儀，右側去顫搶救室 -->
        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            
            <!-- 左側：幾何單元圓解碼器 (佔 7 格，使其更大、更顯眼) -->
            <div class="lg:col-span-7 flex flex-col gap-4">
                <div class="bg-zinc-950 border border-zinc-800 rounded-3xl p-4 flex flex-col items-center">
                    <span class="text-xs font-bold text-emerald-400 tracking-[0.2em] mb-2">📐 平方關係幾何解碼投影 (終極清晰大畫面版)</span>
                    
                    <!-- 幾何圓 Canvas - 已升級為 900x900 解析度，最大顯示寬度提升至 500px -->
                    <div class="relative w-full aspect-square max-w-[500px] bg-zinc-950 rounded-2xl border border-zinc-900 overflow-hidden flex items-center justify-center shadow-inner">
                        <canvas id="circleCanvas" width="900" height="900" class="w-full h-full"></canvas>
                    </div>

                    <!-- 角度調整滑桿 -->
                    <div class="w-full mt-4 bg-zinc-900/40 p-3 rounded-2xl border border-zinc-800/40 text-left">
                        <div class="flex justify-between text-xs font-bold mb-1">
                            <span class="text-zinc-300">角度控制 (θ)：</span>
                            <span class="text-emerald-400 font-mono" id="lbl-angle">45.0°</span>
                        </div>
                        <input type="range" id="angle-slider" min="5" max="85" step="0.5" value="45" class="w-full h-2 bg-zinc-800 rounded-lg appearance-none cursor-pointer accent-emerald-500" oninput="onAngleChange()">
                    </div>
                </div>

                <!-- 幾何線段對照表 -->
                <div class="bg-zinc-950/60 p-4 rounded-3xl border border-zinc-800/80 text-xs">
                    <span class="text-[10px] font-bold text-zinc-500 tracking-wider block mb-2">🔍 幾何長度對照表</span>
                    <div class="grid grid-cols-2 sm:grid-cols-3 gap-2 font-mono">
                        <div class="bg-zinc-900/50 p-2 rounded-xl border border-zinc-800/30">
                            <span class="text-red-400 block text-[9px]">正弦 sinθ</span>
                            <span id="val-sin" class="text-sm font-bold text-red-400">0.71</span>
                        </div>
                        <div class="bg-zinc-900/50 p-2 rounded-xl border border-zinc-800/30">
                            <span class="text-blue-400 block text-[9px]">餘弦 cosθ</span>
                            <span id="val-cos" class="text-sm font-bold text-blue-400">0.71</span>
                        </div>
                        <div class="bg-zinc-900/50 p-2 rounded-xl border border-zinc-800/30">
                            <span class="text-orange-400 block text-[9px]">正切 tanθ</span>
                            <span id="val-tan" class="text-sm font-bold text-orange-400">1.00</span>
                        </div>
                        <div class="bg-zinc-900/50 p-2 rounded-xl border border-zinc-800/30">
                            <span class="text-cyan-400 block text-[9px]">正割 secθ</span>
                            <span id="val-sec" class="text-sm font-bold text-cyan-400">1.41</span>
                        </div>
                        <div class="bg-zinc-900/50 p-2 rounded-xl border border-zinc-800/30">
                            <span class="text-purple-400 block text-[9px]">餘切 cotθ</span>
                            <span id="val-cot" class="text-sm font-bold text-purple-400">1.00</span>
                        </div>
                        <div class="bg-zinc-900/50 p-2 rounded-xl border border-zinc-800/30">
                            <span class="text-yellow-400 block text-[9px]">餘割 cscθ</span>
                            <span id="val-csc" class="text-sm font-bold text-yellow-400">1.41</span>
                        </div>
                    </div>
                </div>
            </div>

            <!-- 右側：去顫搶救室 (佔 5 格) -->
            <div class="lg:col-span-5 flex flex-col gap-4">
                
                <!-- 患者狀態與去顫監視器 -->
                <div class="bg-zinc-950 border-2 border-red-950/40 rounded-3xl p-4 text-left relative overflow-hidden">
                    <div class="flex justify-between items-center mb-3">
                        <div class="flex items-center gap-3">
                            <span id="patient-avatar" class="text-3xl animate-pulse">🤢</span>
                            <div>
                                <span class="text-[9px] font-bold text-zinc-500 uppercase tracking-widest block">急診 BED 102</span>
                                <span id="patient-status" class="text-xs font-bold text-red-400">🚨 患者處於心室顫動 (V-Fib)！</span>
                            </div>
                        </div>
                        <span id="lbl-power" class="font-mono text-xs font-bold bg-zinc-900 px-3 py-1.5 rounded-xl border border-zinc-800 text-red-400">心率: 180 bpm</span>
                    </div>

                    <!-- 去顫充電量指示條 -->
                    <div class="bg-zinc-900/80 p-3 rounded-2xl border border-zinc-800/60 mb-4">
                        <div class="flex justify-between text-[11px] mb-1">
                            <span class="text-zinc-400">去顫電容充電進度 (能量守恆產出)</span>
                            <span id="charge-percent" class="text-amber-400 font-bold font-mono">0%</span>
                        </div>
                        <div class="w-full h-3 bg-zinc-950 rounded-full overflow-hidden border border-zinc-800">
                            <div id="charge-bar" class="h-full bg-gradient-to-r from-amber-500 to-orange-500 transition-all duration-300" style="width: 0%"></div>
                        </div>
                    </div>

                    <!-- 核心挑戰：平方公式填空 -->
                    <div class="bg-zinc-900/60 p-5 rounded-2xl border border-zinc-800 flex flex-col items-center justify-center min-h-[90px]">
                        <span class="text-[10px] font-bold text-zinc-500 block mb-2 tracking-widest">⚡ 去顫電路公式匹配 ⚡</span>
                        <!-- 帶有空格的公式 -->
                        <div id="equation-display" class="text-xl md:text-2xl font-black font-mono text-center tracking-wide">
                            sin²θ + <span class="px-4 py-1 mx-1 bg-zinc-950 border border-dashed border-red-500 rounded-lg text-red-400">?</span> = 1
                        </div>
                    </div>
                </div>

                <!-- 答題選項卡片 (點擊以配對公式) -->
                <div class="bg-zinc-950/40 p-4 rounded-3xl border border-zinc-800/80 text-left">
                    <span class="text-xs font-bold text-zinc-400 block mb-3">🧩 請選擇正確的函數，完成去顫電路：</span>
                    <div class="grid grid-cols-3 gap-2">
                        <button onclick="selectOption('sin²θ')" class="py-3 bg-zinc-800 hover:bg-zinc-700 rounded-xl font-bold font-mono text-sm transition-all border border-zinc-700 active:scale-95 text-red-400">sin²θ</button>
                        <button onclick="selectOption('cos²θ')" class="py-3 bg-zinc-800 hover:bg-zinc-700 rounded-xl font-bold font-mono text-sm transition-all border border-zinc-700 active:scale-95 text-blue-400">cos²θ</button>
                        <button onclick="selectOption('tan²θ')" class="py-3 bg-zinc-800 hover:bg-zinc-700 rounded-xl font-bold font-mono text-sm transition-all border border-zinc-700 active:scale-95 text-orange-400">tan²θ</button>
                        <button onclick="selectOption('sec²θ')" class="py-3 bg-zinc-800 hover:bg-zinc-700 rounded-xl font-bold font-mono text-sm transition-all border border-zinc-700 active:scale-95 text-cyan-400">sec²θ</button>
                        <button onclick="selectOption('cot²θ')" class="py-3 bg-zinc-800 hover:bg-zinc-700 rounded-xl font-bold font-mono text-sm transition-all border border-zinc-700 active:scale-95 text-purple-400">cot²θ</button>
                        <button onclick="selectOption('csc²θ')" class="py-3 bg-zinc-800 hover:bg-zinc-700 rounded-xl font-bold font-mono text-sm transition-all border border-zinc-700 active:scale-95 text-yellow-400">csc²θ</button>
                    </div>
                </div>

                <!-- 幾何與公式關聯引導 (已完美修正色彩對應) -->
                <div class="bg-emerald-950/20 border border-emerald-900/40 p-4 rounded-3xl text-left text-xs leading-relaxed text-emerald-400">
                    <b class="block mb-1 text-emerald-300">💡 幾何助記直覺 (對比左側投影圓中對應色彩的線條)：</b>
                    拉動左側滑桿，你會發現三個直角三角形的勾股定理（平方和）完美對應三個平方公式：<br>
                    1. <b>紅線² (sin²θ) + 藍線² (cos²θ)</b> 永遠等於斜邊的 <b>綠線² (1²)</b>！<br>
                    2. <b>綠線² (1²) + 橘線² (tan²θ)</b> 永遠等於底邊延伸的 <b>青線² (sec²θ)</b>！<br>
                    3. <b>綠線² (1²) + 紫線² (cot²θ)</b> 永遠等於高延伸的 <b>黃線² (csc²θ)</b>！
                </div>
            </div>
        </div>
    </div>

    <!-- 成功過關彈窗 -->
    <div id="success-modal" class="hidden fixed inset-0 bg-black/85 z-50 flex items-center justify-center p-4">
        <div class="bg-zinc-900 border border-zinc-800 p-6 md:p-8 rounded-[2.5rem] max-w-md w-full shadow-2xl text-center">
            <div class="text-5xl mb-4">💖</div>
            <h2 class="text-2xl font-black text-emerald-400 mb-2">去顫擊發！竇性心律恢復！</h2>
            <div class="text-xs text-zinc-300 leading-relaxed text-left space-y-3 mb-6">
                <p><b>做得好！你用平方關係式成功補全了電阻去顫迴路！</b></p>
                <p>當你去顫配對正確時，機器成功完成了電壓轉換：</p>
                <ul class="list-disc pl-5 space-y-1">
                    <li><b>平方關係一：</b> $\sin^2\theta + \cos^2\theta = 1$</li>
                    <li><b>平方關係二：</b> $1 + \tan^2\theta = \sec^2\theta$</li>
                    <li><b>平方關係三：</b> $1 + \cot^2\theta = \csc^2\theta$</li>
                </ul>
                <p class="border-t border-zinc-800 pt-3 text-[10px] text-zinc-500 leading-normal">
                    這些公式在臨床上，等同於利用特定相位阻抗匹配，完成完美無損電能去顫。你既拯救了生命，又精通了高階幾何三角關係！
                </p>
            </div>
            <button onclick="closeModal()" class="w-full bg-emerald-600 hover:bg-emerald-500 text-white font-bold py-3.5 rounded-xl transition-all">
                搶救下一位患者 (下一關)
            </button>
        </div>
    </div>

    <script>
        var canvas = document.getElementById('circleCanvas');
        var ctx = canvas.getContext('2d');
        var audioCtx = null;
        var isMuted = false;
        var score = 0;

        // 題目與公式庫
        var questions = [
            { formula: "sin²θ + [ ? ] = 1", answer: "cos²θ", display: "sin²θ + <span class='px-4 py-1 mx-1 bg-zinc-950 border border-dashed border-sky-400 rounded-lg text-sky-400 font-bold'>?</span> = 1" },
            { formula: "[ ? ] + cos²θ = 1", answer: "sin²θ", display: "<span class='px-4 py-1 mx-1 bg-zinc-950 border border-dashed border-red-400 rounded-lg text-red-400 font-bold'>?</span> + cos²θ = 1" },
            { formula: "1 + [ ? ] = sec²θ", answer: "tan²θ", display: "1 + <span class='px-4 py-1 mx-1 bg-zinc-950 border border-dashed border-orange-400 rounded-lg text-orange-400 font-bold'>?</span> = sec²θ" },
            { formula: "1 + tan²θ = [ ? ]", answer: "sec²θ", display: "1 + tan²θ = <span class='px-4 py-1 mx-1 bg-zinc-950 border border-dashed border-cyan-400 rounded-lg text-cyan-400 font-bold'>?</span>" },
            { formula: "1 + cot²θ = [ ? ]", answer: "csc²θ", display: "1 + cot²θ = <span class='px-4 py-1 mx-1 bg-zinc-950 border border-dashed border-yellow-400 rounded-lg text-yellow-400 font-bold'>?</span>" },
            { formula: "1 + [ ? ] = csc²θ", answer: "cot²θ", display: "1 + <span class='px-4 py-1 mx-1 bg-zinc-950 border border-dashed border-purple-400 rounded-lg text-purple-400 font-bold'>?</span> = csc²θ" }
        ];

        var currentQuestionIndex = 0;
        var chargeProgress = 0;

        // 初始化 Web Audio API 
        function initAudio() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
            if (audioCtx && audioCtx.state === 'suspended') {
                audioCtx.resume();
            }
        }

        // 解鎖聲音
        document.addEventListener('click', function() { initAudio(); });
        document.addEventListener('touchstart', function() { initAudio(); });

        function playSfx(freq, type, length, delay) {
            if (isMuted) return;
            initAudio();
            if (!audioCtx) return;
            setTimeout(function() {
                try {
                    var osc = audioCtx.createOscillator();
                    var gain = audioCtx.createGain();
                    osc.type = type || "sine";
                    osc.connect(gain);
                    gain.connect(audioCtx.destination);
                    osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
                    gain.gain.setValueAtTime(0.08, audioCtx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + (length || 0.1));
                    osc.start();
                    osc.stop(audioCtx.currentTime + (length || 0.1));
                } catch(e) {}
            }, delay || 0);
        }

        function toggleMute() {
            isMuted = !isMuted;
            var icon = document.getElementById('speaker-icon');
            if (isMuted) {
                icon.innerHTML = '<path d="M11 5L6 9H2v6h4l5 4V5z"></path><line x1="23" y1="9" x2="17" y2="15"></line><line x1="17" y1="9" x2="23" y2="15"></line>';
            } else {
                icon.innerHTML = '<path d="M11 5L6 9H2v6h4l5 4V5z"></path><path d="M19.07 4.93a10 10 0 0 1 0 14.14M15.54 8.46a5 5 0 0 1 0 7.07"></path>';
            }
        }

        // 幾何單元圓繪製主邏輯 - 已升級為極致放大加粗版
        function drawGeometry(deg) {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            var cx = canvas.width / 2; // 450 px
            var cy = canvas.height / 2 + 50; // 500 px, 繪圖中心更寬大
            var r = 280; // 物理半徑重磅放大為 280 (原本 140)，比最初放大整整 4 倍！
            
            var rad = deg * Math.PI / 180;
            var cosV = Math.cos(rad);
            var sinV = Math.sin(rad);
            var tanV = Math.tan(rad);
            var secV = 1 / cosV;
            var cotV = 1 / tanV;
            var cscV = 1 / sinV;

            // 實時數值更新
            document.getElementById('val-sin').innerText = sinV.toFixed(2);
            document.getElementById('val-cos').innerText = cosV.toFixed(2);
            document.getElementById('val-tan').innerText = tanV.toFixed(2);
            document.getElementById('val-sec').innerText = secV.toFixed(2);
            document.getElementById('val-cot').innerText = cotV.toFixed(2);
            document.getElementById('val-csc').innerText = cscV.toFixed(2);

            // 1. 繪製背景網格與坐標軸 (網格更顯著、更清楚)
            ctx.strokeStyle = '#1e1b4b';
            ctx.lineWidth = 3;
            ctx.beginPath();
            ctx.moveTo(0, cy); ctx.lineTo(canvas.width, cy);
            ctx.moveTo(cx, 0); ctx.lineTo(cx, canvas.height);
            ctx.stroke();

            // 2. 繪製標準單元圓 (線條加粗，呼應大畫面)
            ctx.strokeStyle = '#2d2d30';
            ctx.lineWidth = 5;
            ctx.beginPath();
            ctx.arc(cx, cy, r, 0, Math.PI * 2);
            ctx.stroke();

            // 點P的座標
            var px = cx + r * cosV;
            var py = cy - r * sinV;

            // --- 三角形 ①：內置勾股圓 (sin²θ + cos²θ = 1) ---
            // 餘弦 cos (超厚藍線)
            ctx.strokeStyle = '#3b82f6';
            ctx.lineWidth = 14;
            ctx.lineCap = 'round';
            ctx.beginPath();
            ctx.moveTo(cx, cy);
            ctx.lineTo(px, cy);
            ctx.stroke();

            // 正弦 sin (超厚紅線)
            ctx.strokeStyle = '#ef4444';
            ctx.lineWidth = 14;
            ctx.lineCap = 'round';
            ctx.beginPath();
            ctx.moveTo(px, cy);
            ctx.lineTo(px, py);
            ctx.stroke();

            // 斜邊 1 - 亮麗矚目的「翡翠綠 (#22c55e)」加粗綠線，超級顯眼！
            ctx.strokeStyle = '#22c55e';
            ctx.lineWidth = 10;
            ctx.lineCap = 'round';
            ctx.beginPath();
            ctx.moveTo(cx, cy);
            ctx.lineTo(px, py);
            ctx.stroke();

            // --- 三角形 ②：正切與正割段 (1 + tan²θ = sec²θ) ---
            if (deg < 80) { // 限制極端角度防止無限延伸
                var secX = cx + r * secV;
                // 正切 tan (加厚橘線)
                ctx.strokeStyle = '#f97316';
                ctx.lineWidth = 10;
                ctx.lineCap = 'round';
                ctx.beginPath();
                ctx.moveTo(px, py);
                ctx.lineTo(secX, cy);
                ctx.stroke();

                // 正割 sec (加厚青線，自圓心沿X軸至正切線交點)
                ctx.strokeStyle = '#06b6d4';
                ctx.lineWidth = 8;
                ctx.lineCap = 'round';
                ctx.beginPath();
                ctx.moveTo(cx, cy);
                ctx.lineTo(secX, cy);
                ctx.stroke();
            }

            // --- 三角形 ③：餘切與餘割段 (1 + cot²θ = csc²θ) ---
            if (deg > 10) {
                var cscY = cy - r * cscV;
                // 餘切 cot (加厚紫線)
                ctx.strokeStyle = '#a855f7';
                ctx.lineWidth = 10;
                ctx.lineCap = 'round';
                ctx.beginPath();
                ctx.moveTo(px, py);
                ctx.lineTo(cx, cscY);
                ctx.stroke();

                // 餘割 csc (加厚黃線，自圓心沿Y軸至餘切線交點)
                ctx.strokeStyle = '#eab308';
                ctx.lineWidth = 8;
                ctx.lineCap = 'round';
                ctx.beginPath();
                ctx.moveTo(cx, cy);
                ctx.lineTo(cx, cscY);
                ctx.stroke();
            }

            // 繪製圓心與角P點 (相應再放大以配合大比例圓)
            ctx.fillStyle = '#10b981';
            ctx.beginPath(); ctx.arc(cx, cy, 14, 0, Math.PI*2); ctx.fill();
            ctx.fillStyle = '#ffffff';
            ctx.beginPath(); ctx.arc(px, py, 18, 0, Math.PI*2); ctx.fill();

            // 繪製角度 θ 扇形 (半徑加大，看得更清)
            ctx.fillStyle = 'rgba(34, 197, 94, 0.18)';
            ctx.beginPath();
            ctx.moveTo(cx, cy);
            ctx.arc(cx, cy, 90, 0, -rad, true);
            ctx.closePath();
            ctx.fill();

            // 寫上 θ 文字 (字體特大、位移放大)
            ctx.fillStyle = '#34d399';
            ctx.font = 'bold 36px monospace';
            ctx.fillText("θ", cx + 100, cy - 30);
        }

        // 滑桿變動回饋
        function onAngleChange() {
            var deg = parseFloat(document.getElementById('angle-slider').value);
            document.getElementById('lbl-angle').innerText = deg.toFixed(1) + "°";
            drawGeometry(deg);
            playSfx(200 + (deg * 4), "triangle", 0.05); // 動態幾何音效
        }

        // 選擇答案牌卡進行匹配
        function selectOption(ans) {
            initAudio();
            var q = questions[currentQuestionIndex];
            
            if (ans === q.answer) {
                // 配對正確！開始為電容充電
                playSfx(587.33, "sine", 0.08); // 充電遞增音
                chargeProgress += 34; // 答對3題即充滿

                if (chargeProgress >= 100) {
                    chargeProgress = 100;
                    document.getElementById('charge-bar').style.width = "100%";
                    document.getElementById('charge-percent').innerText = "100% (充電飽滿！)";
                    
                    // 觸發擊發復律
                    triggerDefibrillate();
                } else {
                    document.getElementById('charge-bar').style.width = chargeProgress + "%";
                    document.getElementById('charge-percent').innerText = chargeProgress + "%";
                    
                    // 進入下一題
                    nextQuestion();
                }
            } else {
                // 配對出錯！發生皮膚高阻抗警告
                playSfx(150, "sawtooth", 0.3); // 錯誤音
                
                // 螢幕與卡片產生抖動效果
                var container = document.getElementById('main-container');
                container.classList.add('shake');
                setTimeout(function() {
                    container.classList.remove('shake');
                }, 300);

                alert("❌ 配對電路不匹配！這會造成皮膚嚴重電阻燒傷，請重新檢視勾股幾何關係！");
            }
        }

        // 下一題
        function nextQuestion() {
            currentQuestionIndex = (currentQuestionIndex + 1) % questions.length;
            document.getElementById('equation-display').innerHTML = questions[currentQuestionIndex].display;
        }

        // 去顫器電擊救人
        function triggerDefibrillate() {
            playSfx(150, "sawtooth", 0.3); // 充電完成暴鳴
            
            // 模擬電擊震動與心電回復
            setTimeout(function() {
                playSfx(880, "sine", 0.12, 0); // 竇性心律復合音
                playSfx(1100, "sine", 0.12, 80);
                playSfx(1320, "sine", 0.25, 160);

                // 更新狀態為成功
                document.getElementById('patient-avatar').innerText = "🙂";
                document.getElementById('patient-status').innerText = "❇️ 正常竇性心律，生命跡象穩定！";
                document.getElementById('patient-status').className = "text-xs font-bold text-green-400";
                document.getElementById('lbl-power').innerText = "心率: 72 bpm";
                document.getElementById('lbl-power').className = "font-mono text-xs font-bold bg-zinc-900 px-3 py-1.5 rounded-xl border border-zinc-800 text-green-400";

                score++;
                document.getElementById('score-val').innerText = score;

                // 打開過關反思彈窗
                document.getElementById('success-modal').classList.remove('hidden');
            }, 500);
        }

        function closeModal() {
            document.getElementById('success-modal').classList.add('hidden');
            
            // 重置病人與去顫電路
            chargeProgress = 0;
            document.getElementById('charge-bar').style.width = "0%";
            document.getElementById('charge-percent').innerText = "0%";
            document.getElementById('patient-avatar').innerText = "🤢";
            document.getElementById('patient-status').innerText = "🚨 患者處於心室顫動 (V-Fib)！";
            document.getElementById('patient-status').className = "text-xs font-bold text-red-400";
            document.getElementById('lbl-power').innerText = "心率: 180 bpm";
            document.getElementById('lbl-power').className = "font-mono text-xs font-bold bg-zinc-900 px-3 py-1.5 rounded-xl border border-zinc-800 text-red-400";

            nextQuestion();
        }

        window.onload = function() {
            drawGeometry(45);
            document.getElementById('equation-display').innerHTML = questions[0].display;
        };
    </script>
</body>
</html>
