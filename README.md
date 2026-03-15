<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Official Portal 2026 | Prime Solutions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #fdfdfd; color: #0f172a; }
        .tab-content { display: none; }
        .active-tab { display: block; animation: slideIn 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); }
        @keyframes slideIn { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }
        
        /* Modern Glass UI */
        .glass { background: rgba(255, 255, 255, 0.8); backdrop-filter: blur(12px); border: 1px solid rgba(255, 255, 255, 0.3); }
        .id-card { background: linear-gradient(135deg, #1e3a8a 0%, #1e40af 100%); color: white; border-radius: 30px; }
        
        /* Live Ticker */
        .ticker-wrapper { background: #0f172a; color: #fbbf24; padding: 10px; font-weight: 800; font-size: 11px; overflow: hidden; white-space: nowrap; }
        .ticker-text { display: inline-block; animation: ticker 20s linear infinite; }
        @keyframes ticker { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

        /* Chat & Buttons */
        .msg-bubble { border-radius: 20px 20px 20px 4px; transition: 0.3s; }
        .btn-modern { background: #1e3a8a; transition: 0.4s; border-radius: 20px; color: white; font-weight: 800; }
        .btn-modern:hover { transform: scale(1.05); box-shadow: 0 10px 20px rgba(30,58,138,0.2); }
    </style>
</head>
<body class="pb-24">

    <div class="ticker-wrapper fixed top-0 w-full z-[60]">
        <div class="ticker-text uppercase tracking-widest">
            ⚠️ OFFICIAL NOTICE: REGISTRATION FOR 2026 BATCH IS ACTIVE — PREPARE FOR PHYSICAL TEST IN RAWALPINDI — SYLLABUS UPDATED BY PRIME SOLUTIONS
        </div>
    </div>

    <div id="loginScreen" class="fixed inset-0 bg-white z-[5000] flex items-center justify-center p-6 hidden">
        <div class="max-w-md w-full text-center">
            <div class="w-20 h-20 bg-blue-900 rounded-[25px] mx-auto mb-6 flex items-center justify-center text-white text-4xl font-black">G</div>
            <h2 class="text-4xl font-black text-slate-900 mb-2 italic">ACADEMY LOGIN</h2>
            <p class="text-xs font-bold text-gray-400 mb-8 tracking-[0.2em] uppercase">Join the Elite 2026 Force</p>
            <input id="uName" type="text" placeholder="Candidate Full Name" class="w-full p-6 bg-gray-100 rounded-3xl mb-6 outline-none border-2 border-transparent focus:border-blue-600 text-center font-black">
            <button onclick="saveUser()" class="btn-modern w-full py-6 text-xl">AUTHORIZE ACCESS</button>
        </div>
    </div>

    <nav class="glass sticky top-[40px] z-50 p-5 px-8 flex justify-between items-center mx-4 mt-2 rounded-[30px] shadow-sm">
        <div class="flex items-center gap-3">
            <div class="w-8 h-8 id-card flex items-center justify-center text-[10px] font-black">GB</div>
            <h1 class="font-black text-lg italic text-blue-950">GBTS PORTAL</h1>
        </div>
        <div class="flex gap-4">
            <button onclick="showTab('dashTab')" class="text-[10px] font-black uppercase text-blue-900">Dashboard</button>
            <button onclick="showTab('chatTab')" class="text-[10px] font-black uppercase text-gray-500">Group</button>
            <button onclick="showTab('supportTab')" class="text-[10px] font-black uppercase text-gray-500">Support</button>
            <button onclick="triggerGod()" class="w-2 h-2 bg-gray-200 rounded-full mt-2"></button>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-6 mt-6">
        
        <div id="dashTab" class="tab-content active-tab">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
                <div class="id-card p-8 shadow-2xl relative overflow-hidden">
                    <div class="absolute top-[-20px] right-[-20px] w-40 h-40 bg-white/10 rounded-full"></div>
                    <p class="text-[10px] font-black opacity-60 mb-6 uppercase tracking-widest">Official ID Card</p>
                    <div class="flex items-center gap-4 mb-8">
                        <img src="https://api.dicebear.com/7.x/bottts/svg?seed=Officer" class="w-16 h-16 bg-white/20 rounded-2xl p-2">
                        <div>
                            <h3 id="cardName" class="text-2xl font-black uppercase tracking-tighter">CANDIDATE</h3>
                            <p class="text-[10px] font-bold text-blue-200">ID: GBTS-2026-9912</p>
                        </div>
                    </div>
                    <div class="flex justify-between items-end">
                        <div>
                            <p class="text-[8px] font-bold opacity-50 uppercase">Current Status</p>
                            <span class="text-xs font-black bg-green-500/20 text-green-300 px-3 py-1 rounded-full">ACTIVE</span>
                        </div>
                        <img src="https://api.qrserver.com/v1/create-qr-code/?size=50x50&data=VerifiedCandidate" class="w-10 h-10 opacity-70">
                    </div>
                </div>

                <div class="bg-white p-8 rounded-[40px] shadow-sm border border-gray-100 flex flex-col items-center justify-center text-center">
                    <div class="relative w-24 h-24 mb-4">
                        <svg class="w-full h-full" viewBox="0 0 36 36">
                            <path class="text-gray-200" stroke-width="3" stroke="currentColor" fill="none" d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831" />
                            <path class="text-blue-600" id="progressCircle" stroke-dasharray="30, 100" stroke-width="3" stroke-linecap="round" stroke="currentColor" fill="none" d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831" />
                        </svg>
                        <div class="absolute inset-0 flex items-center justify-center font-black text-xl" id="progressText">30%</div>
                    </div>
                    <h4 class="font-black text-sm uppercase">Recruitment Readiness</h4>
                </div>

                <div class="bg-blue-900 p-8 rounded-[40px] text-white">
                    <h4 class="font-black mb-4 italic uppercase">Quick Prep</h4>
                    <button onclick="showTab('bookTab')" class="w-full bg-white/10 hover:bg-white/20 py-3 rounded-2xl mb-2 text-xs font-bold text-left px-4">📖 Read Daily Syllabus</button>
                    <button class="w-full bg-white/10 hover:bg-white/20 py-3 rounded-2xl text-xs font-bold text-left px-4">⏱️ Take Mock Physical Test</button>
                </div>
            </div>

            <div class="mt-12">
                <h2 class="text-2xl font-black mb-8 italic uppercase tracking-tighter">Level Training Modules</h2>
                <div id="levelGrid" class="grid grid-cols-2 md:grid-cols-5 gap-6"></div>
            </div>
        </div>

        <div id="chatTab" class="tab-content">
            <div class="max-w-3xl mx-auto bg-white rounded-[50px] p-8 shadow-2xl border border-gray-100">
                <div id="chatBox" class="h-[450px] overflow-y-auto mb-6 pr-4 space-y-4"></div>
                <div class="flex gap-3 bg-gray-50 p-2 rounded-3xl border">
                    <input id="chatInp" type="text" placeholder="Discuss questions here..." class="flex-1 p-4 bg-transparent outline-none font-bold text-sm">
                    <button onclick="sendGlobal()" class="btn-modern px-8 py-4">SEND</button>
                </div>
            </div>
        </div>

        <div id="supportTab" class="tab-content">
            <div class="max-w-2xl mx-auto bg-slate-900 p-10 rounded-[50px] text-white shadow-2xl">
                <h2 class="text-3xl font-black mb-2 italic text-blue-400">ADMIN DESK</h2>
                <p class="text-[10px] font-bold text-slate-500 mb-8 tracking-[0.3em]">PRIVATE ENCRYPTED CHANNEL</p>
                <div id="supportBox" class="h-64 overflow-y-auto mb-6 space-y-4"></div>
                <div class="flex gap-2">
                    <input id="supportInp" type="text" placeholder="Message Admin..." class="flex-1 p-4 bg-slate-800 rounded-2xl outline-none border border-slate-700">
                    <button onclick="sendPrivate()" class="bg-blue-600 px-8 rounded-2xl font-black">SEND</button>
                </div>
            </div>
        </div>

        <div id="godTab" class="tab-content bg-white p-10 rounded-[60px] shadow-2xl border-t-[15px] border-red-600">
            <div class="flex justify-between items-center mb-10">
                <h2 class="text-4xl font-black text-slate-900 italic">SYSTEM ROOT</h2>
                <button onclick="alert('Locking System...');" class="bg-red-600 text-white px-8 py-3 rounded-2xl font-black">LOCKDOWN SITE</button>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <div class="bg-gray-100 p-8 rounded-3xl">
                    <h4 class="font-black text-xs uppercase mb-6 text-blue-900">Ban Management</h4>
                    <input id="banUser" type="text" placeholder="Username to Ban" class="w-full p-4 rounded-xl mb-4 border outline-none">
                    <div class="flex gap-2">
                        <button onclick="alert('Banned')" class="bg-red-600 text-white px-6 py-2 rounded-xl font-bold">BAN</button>
                        <button onclick="alert('Unbanned')" class="bg-green-600 text-white px-6 py-2 rounded-xl font-bold">UNBAN</button>
                    </div>
                </div>
                <div class="bg-gray-100 p-8 rounded-3xl">
                    <h4 class="font-black text-xs uppercase mb-6 text-blue-900">Broadcast Voice Alert</h4>
                    <button onclick="window.speechSynthesis.speak(new SpeechSynthesisUtterance('Attention Candidates! New update available.'));" class="w-full bg-blue-900 text-white py-4 rounded-xl font-bold">📣 TEST VOICE ANNOUNCE</button>
                </div>
            </div>
        </div>

    </main>

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

        let me = { name: "", level: 1 };
        let godTaps = 0;

        window.onload = () => {
            const saved = localStorage.getItem('p_user');
            if(saved) {
                me.name = saved;
                me.level = parseInt(localStorage.getItem('p_lvl')) || 1;
                document.getElementById('cardName').innerText = me.name;
                updateProgress();
                renderLevels();
                listenChat();
                listenPrivate();
            } else {
                document.getElementById('loginScreen').classList.remove('hidden');
            }
        };

        function saveUser() {
            const n = document.getElementById('uName').value.trim();
            if(!n) return alert("Enter Full Name");
            localStorage.setItem('p_user', n);
            location.reload();
        }

        function updateProgress() {
            const p = me.level * 10;
            document.getElementById('progressCircle').setAttribute('stroke-dasharray', `${p}, 100`);
            document.getElementById('progressText').innerText = p + "%";
        }

        function renderLevels() {
            const grid = document.getElementById('levelGrid');
            grid.innerHTML = '';
            for(let i=1; i<=10; i++) {
                let lock = i > me.level;
                grid.innerHTML += `
                    <div class="p-8 text-center rounded-[40px] shadow-sm bg-white border-2 ${lock ? 'opacity-30' : 'border-blue-100'}">
                        <div class="text-4xl mb-4">${lock ? '🔒' : '🎖️'}</div>
                        <h4 class="font-black text-[10px] uppercase">Level 0${i}</h4>
                    </div>
                `;
            }
        }

        function sendGlobal() {
            const t = document.getElementById('chatInp').value.trim();
            if(!t) return;
            db.ref('global_final').push({ name: me.name, text: t, time: Date.now() });
            document.getElementById('chatInp').value = '';
        }

        function listenChat() {
            db.ref('global_final').limitToLast(20).on('value', snap => {
                const box = document.getElementById('chatBox');
                box.innerHTML = '';
                snap.forEach(c => {
                    const d = c.val();
                    box.innerHTML += `
                        <div class="msg-bubble ${d.name === me.name ? 'bg-blue-900 text-white ml-auto' : 'bg-gray-100 mr-auto'} p-4 max-w-[80%]">
                            <span class="text-[9px] font-black opacity-60 uppercase mb-1 block">${d.name} ${me.level > 5 ? '✔️' : ''}</span>
                            <p class="text-sm font-bold">${d.text}</p>
                        </div>
                    `;
                });
                box.scrollTop = box.scrollHeight;
            });
        }

        function sendPrivate() {
            const t = document.getElementById('supportInp').value.trim();
            if(!t) return;
            db.ref('private/'+me.name).push({ sender: me.name, text: t });
            document.getElementById('supportInp').value = '';
        }

        function listenPrivate() {
            db.ref('private/'+me.name).on('value', snap => {
                const box = document.getElementById('supportBox');
                box.innerHTML = '';
                snap.forEach(c => {
                    const d = c.val();
                    box.innerHTML += `<div class="bg-slate-800 p-3 rounded-2xl text-xs"><b>${d.sender}:</b> ${d.text}</div>`;
                });
                box.scrollTop = box.scrollHeight;
            });
        }

        function triggerGod() {
            godTaps++;
            if(godTaps === 5) {
                const p = prompt("ENTER SYSTEM KEY:");
                if(p === "prime786") showTab('godTab');
                godTaps = 0;
            }
        }

        function showTab(id) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab'));
            document.getElementById(id).classList.add('active-tab');
        }

        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
