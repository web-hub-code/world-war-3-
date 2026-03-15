<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Ultimate Academy | Prime Solutions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f8fafc; color: #1e293b; overflow-x: hidden; }
        .glass-nav { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(15px); border-bottom: 1px solid #e2e8f0; }
        .tab-content { display: none; }
        .active-tab { display: block; animation: slideUp 0.4s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        
        /* Level Cards */
        .level-card { border-radius: 40px; transition: 0.4s; background: white; border: 2px solid transparent; cursor: pointer; }
        .level-card:hover:not(.locked) { transform: translateY(-10px); border-color: #1e3a8a; box-shadow: 0 20px 40px rgba(30,58,138,0.1); }
        .locked { filter: grayscale(1); opacity: 0.5; cursor: not-allowed; background: #f1f5f9; }
        .pending-glow { border: 2px solid #f59e0b; animation: pulse 2s infinite; background: #fffbeb; }
        @keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.6; } 100% { opacity: 1; } }

        /* Chat System */
        #chatBox { height: 350px; overflow-y: auto; scroll-behavior: smooth; border-radius: 25px; }
        .msg { margin-bottom: 12px; padding: 12px 18px; border-radius: 20px 20px 20px 0; background: white; box-shadow: 0 2px 5px rgba(0,0,0,0.03); max-width: 80%; }
        .msg-sender { font-size: 10px; font-weight: 900; color: #1e3a8a; text-transform: uppercase; margin-bottom: 2px; display: block; }
        
        /* Admin Broadcast Overlay */
        #broadcast { display: none; background: #ef4444; color: white; padding: 10px; text-align: center; font-weight: 900; font-size: 12px; position: fixed; top: 0; left: 0; right: 0; z-index: 1000; animation: blink 1s infinite; }
        @keyframes blink { 0% { opacity: 1; } 50% { opacity: 0.7; } 100% { opacity: 1; } }
    </style>
</head>
<body class="pb-24">

    <div id="broadcast">⚠️ SYSTEM NOTIFICATION: NEW SYLLABUS UPDATED!</div>

    <div id="loginScreen" class="fixed inset-0 bg-[#020617] z-[2000] flex items-center justify-center p-6 hidden">
        <div class="bg-white p-12 rounded-[60px] max-w-md w-full text-center shadow-2xl">
            <h2 class="text-4xl font-black text-slate-900 mb-2 italic">PRIME ELITE</h2>
            <p class="text-[10px] font-bold text-blue-500 mb-10 tracking-[0.3em] uppercase">Examination Hub 2026</p>
            <input id="userName" type="text" placeholder="Enter Full Name" class="w-full p-6 bg-slate-50 rounded-3xl mb-4 border-2 border-transparent focus:border-blue-600 outline-none text-center font-bold">
            <button onclick="login()" class="w-full bg-blue-900 text-white py-5 rounded-3xl font-black text-xl hover:scale-95 transition-all shadow-xl">START MISSION</button>
        </div>
    </div>

    <nav class="glass-nav sticky top-0 z-50 p-5 px-8 flex justify-between items-center">
        <div class="flex items-center gap-2">
            <div class="w-8 h-8 bg-blue-900 rounded-lg flex items-center justify-center text-white font-black text-xs">P</div>
            <h1 class="font-black text-lg italic tracking-tighter text-blue-900">PRIME HUB</h1>
        </div>
        <div class="flex gap-6">
            <button onclick="showTab('levelsTab')" class="text-[10px] font-black uppercase text-blue-900">Levels</button>
            <button onclick="showTab('chatTab')" class="text-[10px] font-black uppercase text-gray-500">Global Discussion</button>
            <button onclick="showTab('bookTab')" class="text-[10px] font-black uppercase text-gray-500">Syllabus</button>
            <button onclick="logout()" class="text-[10px] font-black uppercase text-red-500 underline">Exit</button>
        </div>
    </nav>

    <main class="max-w-7xl mx-auto p-6 mt-6">

        <div id="levelsTab" class="tab-content active-tab">
            <div class="mb-10">
                <h2 class="text-4xl font-black italic uppercase tracking-tighter">Mission Progress</h2>
                <p id="welcome" class="text-blue-600 font-bold">Officer Status: Online</p>
            </div>
            <div id="levelGrid" class="grid grid-cols-2 md:grid-cols-5 gap-6"></div>
        </div>

        <div id="chatTab" class="tab-content">
            <div class="max-w-2xl mx-auto">
                <h2 class="text-3xl font-black mb-8 italic uppercase text-center">Candidate Discussion</h2>
                <div id="chatBox" class="bg-slate-200/50 p-6 mb-4"></div>
                <div class="flex gap-2">
                    <input id="chatMsg" type="text" placeholder="Type a message..." class="flex-1 p-5 rounded-3xl outline-none border-2 focus:border-blue-900 font-medium">
                    <button onclick="sendMsg()" class="bg-blue-900 text-white px-10 rounded-3xl font-black shadow-lg">SEND</button>
                </div>
            </div>
        </div>

        <div id="bookTab" class="tab-content">
            <h2 class="text-4xl font-black mb-10 italic uppercase text-center">Preparation Bible</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="bg-white p-8 rounded-[40px] border-l-[12px] border-blue-900 shadow-sm">
                    <h4 class="font-black text-xl mb-4 text-blue-900">English Comprehensive</h4>
                    <p class="text-sm text-gray-600 leading-relaxed">
                        <b>Grammar:</b> Focus on Tenses, Active/Passive, and Direct/Indirect.<br>
                        <b>Vocabulary:</b> Repeat: <i>Ambiguous, Benevolent, Cease, Dwelling.</i><br>
                        <b>Test Tip:</b> Always read the passage twice before answering MCQs.
                    </p>
                </div>
                <div class="bg-white p-8 rounded-[40px] border-l-[12px] border-green-600 shadow-sm">
                    <h4 class="font-black text-xl mb-4 text-green-700">Islamic Studies Detailed</h4>
                    <p class="text-sm text-gray-600 leading-relaxed">
                        <b>Zakat:</b> Made compulsory in 2 AH. Gold Nisaab: 7.5 Tola.<br>
                        <b>History:</b> First Masjid: Quba. Total Ghazwat: 27.<br>
                        <b>Quran:</b> Surah Naml has two Bismillahs.
                    </p>
                </div>
                <div class="bg-white p-8 rounded-[40px] border-l-[12px] border-orange-500 shadow-sm">
                    <h4 class="font-black text-xl mb-4 text-orange-600">GK & Gilgit Baltistan</h4>
                    <p class="text-sm text-gray-600 leading-relaxed">
                        <b>Rivers:</b> Indus is the longest river. Neelum is in Kashmir.<br>
                        <b>GB Districts:</b> Ghizer, Hunza, Nagar, Skardu, Shigar... (Total 14).<br>
                        <b>Height:</b> Rakaposhi (7788m), Nanga Parbat (8126m).
                    </p>
                </div>
            </div>
        </div>

        <div id="godPanel" class="tab-content mt-12 bg-slate-950 p-10 rounded-[60px] text-white">
            <div class="flex justify-between items-center mb-10">
                <h2 class="text-3xl font-black text-blue-400">GOD MODE DASHBOARD</h2>
                <button onclick="broadcastMsg()" class="bg-yellow-500 text-black px-4 py-2 rounded-full font-black text-[10px]">SEND BROADCAST</button>
            </div>
            <div id="adminRequests" class="grid grid-cols-1 md:grid-cols-2 gap-6"></div>
            <div class="mt-10 pt-6 border-t border-slate-800">
                <button onclick="localStorage.clear(); location.reload();" class="text-red-500 font-bold text-xs">GLOBAL RESET (DELETE EVERYTHING)</button>
            </div>
        </div>

    </main>

    <div id="testBox" class="hidden fixed inset-0 bg-white z-[1500] p-6 overflow-y-auto">
        <div class="max-w-3xl mx-auto">
            <h2 id="lvlTitle" class="text-4xl font-black text-blue-900 mb-12 italic uppercase text-center">EXAM START</h2>
            <div id="quiz" class="space-y-6"></div>
            <button onclick="finishTest()" class="w-full bg-blue-900 text-white py-6 rounded-[40px] mt-12 font-black text-2xl shadow-2xl">SUBMIT PAPER</button>
        </div>
    </div>

    <footer class="fixed bottom-0 left-0 right-0 p-4 text-center">
        <span onclick="triggerGod()" class="text-[8px] text-gray-300 opacity-20 cursor-default">PRIME_SOLUTIONS_ROOT_KEY_2026</span>
    </footer>

    <script>
        // CONFIG
        const firebaseConfig = {
          apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
          authDomain: "dark-web-9.firebaseapp.com",
          projectId: "dark-web-9",
          databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com/",
          storageBucket: "dark-web-9.firebasestorage.app",
          messagingSenderId: "564328425161",
          appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.database();

        let me = { name: "", level: 1, status: "ready" };
        let godTaps = 0;

        window.onload = () => {
            const saved = localStorage.getItem('u_name');
            if(saved) {
                me.name = saved;
                me.level = parseInt(localStorage.getItem('u_lvl_' + saved)) || 1;
                me.status = localStorage.getItem('u_status_' + saved) || "ready";
                document.getElementById('welcome').innerText = "Officer: " + saved.toUpperCase();
                renderLevels();
                listenChat();
                listenBroadcast();
            } else {
                document.getElementById('loginScreen').classList.remove('hidden');
            }
        };

        function login() {
            const n = document.getElementById('userName').value.trim();
            if(!n) return;
            localStorage.setItem('u_name', n);
            location.reload();
        }

        function logout() { localStorage.clear(); location.reload(); }

        function renderLevels() {
            const grid = document.getElementById('levelGrid');
            grid.innerHTML = '';
            for(let i=1; i<=10; i++) {
                let locked = i > me.level;
                let pending = (i === me.level && me.status === "pending");
                grid.innerHTML += `
                    <div onclick="openTest(${i}, ${locked}, ${pending})" class="level-card p-10 text-center shadow-md ${locked ? 'locked' : ''} ${pending ? 'pending-glow' : ''}">
                        <div class="text-5xl mb-4">${locked ? '🔒' : (pending ? '⏳' : '🎖️')}</div>
                        <h4 class="font-black text-sm">LEVEL ${i}</h4>
                        <p class="text-[9px] font-bold mt-2 ${pending ? 'text-orange-500' : 'text-blue-600'}">${pending ? 'PENDING' : (locked ? 'LOCKED' : 'START')}</p>
                    </div>
                `;
            }
            updateGodPanel();
        }

        function openTest(n, l, p) {
            if(l || p) return;
            document.getElementById('testBox').classList.remove('hidden');
            document.getElementById('lvlTitle').innerText = "Level 0" + n + " Professional Exam";
            const quiz = document.getElementById('quiz');
            quiz.innerHTML = '';
            for(let i=1; i<=6; i++) {
                quiz.innerHTML += `
                    <div class="bg-slate-50 p-8 rounded-[40px] border-2 border-slate-100">
                        <p class="font-black text-lg mb-6">Question 0${i}: Advanced GK/Syllabus Topic for Level ${n}?</p>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <label class="p-5 bg-white border-2 rounded-3xl cursor-pointer"><input type="radio" name="q${i}" value="1"> Option Alpha</label>
                            <label class="p-5 bg-white border-2 rounded-3xl cursor-pointer"><input type="radio" name="q${i}" value="0"> Option Beta</label>
                        </div>
                    </div>
                `;
            }
        }

        function finishTest() {
            localStorage.setItem('u_status_' + me.name, "pending");
            alert("Result: 85% PASSED! Request sent to God Mode for Approval.");
            location.reload();
        }

        // CHAT LOGIC
        function sendMsg() {
            const msg = document.getElementById('chatMsg').value.trim();
            if(!msg) return;
            db.ref('global_chat').push({ name: me.name, text: msg, time: Date.now() });
            document.getElementById('chatMsg').value = '';
        }

        function listenChat() {
            const box = document.getElementById('chatBox');
            db.ref('global_chat').limitToLast(20).on('value', snap => {
                box.innerHTML = '';
                snap.forEach(c => {
                    const d = c.val();
                    box.innerHTML += `<div class="msg"><span class="msg-sender">${d.name}</span><p class="text-sm font-medium">${d.text}</p></div>`;
                });
                box.scrollTop = box.scrollHeight;
            });
        }

        // BROADCAST
        function broadcastMsg() {
            const m = prompt("Enter Broadcast Message:");
            if(m) db.ref('broadcast').set(m);
        }

        function listenBroadcast() {
            db.ref('broadcast').on('value', snap => {
                const b = document.getElementById('broadcast');
                if(snap.val()) { b.innerText = "📣 " + snap.val(); b.style.display = 'block'; }
            });
        }

        // GOD MODE
        function triggerGod() {
            godTaps++;
            if(godTaps === 4) {
                const k = prompt("ENTER GOD KEY:");
                if(k === "prime786") showTab('godPanel');
                godTaps = 0;
            }
        }

        function updateGodPanel() {
            const admin = document.getElementById('adminRequests');
            if(me.status === "pending") {
                admin.innerHTML = `
                    <div class="bg-white p-6 rounded-3xl flex justify-between items-center text-slate-900">
                        <div><p class="font-black">${me.name}</p><p class="text-[10px] font-bold">Level ${me.level} Pass</p></div>
                        <button onclick="godApprove()" class="bg-green-500 text-white px-6 py-2 rounded-xl font-bold">Approve</button>
                    </div>
                `;
            } else { admin.innerHTML = "<p class='text-slate-500 italic text-xs'>No requests.</p>"; }
        }

        function godApprove() {
            localStorage.setItem('u_lvl_' + me.name, me.level + 1);
            localStorage.setItem('u_status_' + me.name, "ready");
            location.reload();
        }

        function showTab(id) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab'));
            document.getElementById(id).classList.add('active-tab');
        }
    </script>
</body>
</html>
