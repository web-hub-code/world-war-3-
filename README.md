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
        .tab-content { display: none; }
        .active-tab { display: block; animation: slideUp 0.4s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        
        .glass { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(15px); border: 1px solid #e2e8f0; }
        .id-card-gradient { background: linear-gradient(135deg, #1e3a8a 0%, #1e40af 100%); }
        .level-card { transition: 0.3s; cursor: pointer; border-radius: 30px; }
        .level-card:hover { transform: translateY(-5px); border-color: #1e3a8a; }
        .locked { filter: grayscale(1); opacity: 0.4; cursor: not-allowed; }

        /* Scrollbar */
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; }
        ::-webkit-scrollbar-thumb { background: #1e3a8a; border-radius: 10px; }
    </style>
</head>
<body class="pb-24">

    <div id="loginScreen" class="fixed inset-0 bg-slate-900 z-[5000] flex items-center justify-center p-6 hidden">
        <div class="bg-white p-10 rounded-[50px] max-w-md w-full text-center shadow-2xl">
            <h2 class="text-4xl font-black text-blue-900 mb-2 italic uppercase">Admission Desk</h2>
            <p class="text-[10px] font-bold text-gray-400 mb-8 tracking-widest">VERIFY YOUR IDENTITY</p>
            <input id="uName" type="text" placeholder="Candidate Full Name" class="w-full p-5 bg-gray-100 rounded-3xl mb-6 outline-none border-2 border-transparent focus:border-blue-600 text-center font-black">
            <button onclick="saveUser()" class="w-full bg-blue-900 text-white py-5 rounded-3xl font-black text-xl hover:bg-blue-800 transition shadow-xl">ENTER PORTAL</button>
        </div>
    </div>

    <nav class="glass sticky top-0 z-50 p-4 flex justify-between items-center px-8 border-b">
        <div class="flex items-center gap-2">
            <div class="w-8 h-8 bg-blue-900 text-white flex items-center justify-center rounded-lg font-black text-xs">GB</div>
            <h1 class="font-black text-lg italic text-blue-950">GBTS ELITE</h1>
        </div>
        <div class="flex gap-4 md:gap-8 overflow-x-auto">
            <button onclick="showTab('dashTab')" class="text-[10px] font-black uppercase text-blue-900">Dashboard</button>
            <button onclick="showTab('syllabusTab')" class="text-[10px] font-black uppercase text-gray-500">Syllabus</button>
            <button onclick="showTab('chatTab')" class="text-[10px] font-black uppercase text-gray-500">Global Chat</button>
            <button onclick="showTab('supportTab')" class="text-[10px] font-black uppercase text-gray-500">Support</button>
            <button onclick="logout()" class="text-[10px] font-black uppercase text-red-500">Exit</button>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-6">

        <div id="dashTab" class="tab-content active-tab">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8 mb-12">
                <div class="id-card-gradient p-8 rounded-[40px] text-white shadow-2xl relative overflow-hidden">
                    <p class="text-[10px] font-black opacity-60 uppercase mb-6 tracking-widest italic">Recruitment ID 2026</p>
                    <div class="flex items-center gap-4 mb-6">
                        <div class="w-16 h-16 bg-white/20 rounded-2xl flex items-center justify-center text-3xl">🎖️</div>
                        <div>
                            <h3 id="cardName" class="text-3xl font-black uppercase tracking-tighter italic">---</h3>
                            <p class="text-[10px] font-bold text-blue-200">STATUS: VERIFIED CANDIDATE</p>
                        </div>
                    </div>
                    <div class="flex justify-between items-end border-t border-white/10 pt-4">
                        <span id="cardLvl" class="text-xs font-black bg-white/10 px-4 py-2 rounded-full uppercase">Level 01</span>
                        <img src="https://api.qrserver.com/v1/create-qr-code/?size=50x50&data=Verified" class="w-8 h-8 opacity-50">
                    </div>
                </div>
                <div class="bg-white p-8 rounded-[40px] shadow-sm border border-gray-100 grid grid-cols-2 gap-4">
                    <div class="p-6 bg-slate-50 rounded-3xl text-center">
                        <p class="text-[10px] font-black text-gray-400 uppercase">Training %</p>
                        <h4 class="text-3xl font-black text-blue-900">100%</h4>
                    </div>
                    <div class="p-6 bg-slate-50 rounded-3xl text-center">
                        <p class="text-[10px] font-black text-gray-400 uppercase">Daily Rank</p>
                        <h4 class="text-3xl font-black text-blue-900">#01</h4>
                    </div>
                </div>
            </div>

            <h3 class="text-2xl font-black italic uppercase mb-8">Training Modules</h3>
            <div id="levelGrid" class="grid grid-cols-2 md:grid-cols-5 gap-6"></div>
        </div>

        <div id="syllabusTab" class="tab-content">
            <div class="text-center mb-12">
                <h2 class="text-4xl font-black italic text-blue-950 uppercase">Preparation Guide</h2>
                <p class="text-xs font-bold text-gray-400 mt-2">UPDATED FOR 2026 RECRUITMENT</p>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <div class="bg-white p-8 rounded-[40px] border-l-[10px] border-blue-900 shadow-sm">
                    <h3 class="text-xl font-black text-blue-900 mb-4 uppercase">🇬🇧 English Mastery</h3>
                    <ul class="space-y-3 text-sm font-medium text-gray-600">
                        <li><b class="text-blue-950">Grammar:</b> Focus on Direct/Indirect & Active/Passive Voice.</li>
                        <li><b class="text-blue-950">Parts of Speech:</b> Identifying Nouns, Pronouns, and Prepositions in sentences.</li>
                        <li><b class="text-blue-950">Vocabulary:</b> Synonyms/Antonyms are 20% of the test.</li>
                    </ul>
                </div>
                <div class="bg-white p-8 rounded-[40px] border-l-[10px] border-green-600 shadow-sm">
                    <h3 class="text-xl font-black text-green-700 mb-4 uppercase">🌍 General Knowledge</h3>
                    <ul class="space-y-3 text-sm font-medium text-gray-600">
                        <li><b class="text-green-800">GB Districts:</b> Study Gilgit, Skardu, Diamer, and Ghizer (Total 14 districts).</li>
                        <li><b class="text-green-800">Mountains:</b> K2 (8611m), Nanga Parbat (8126m), and Rakaposhi.</li>
                        <li><b class="text-green-800">Geography:</b> Indus River is the backbone of Pakistan's economy.</li>
                    </ul>
                </div>
                <div class="bg-white p-8 rounded-[40px] border-l-[10px] border-orange-500 shadow-sm">
                    <h3 class="text-xl font-black text-orange-600 mb-4 uppercase">🕌 Islamic Studies</h3>
                    <ul class="space-y-3 text-sm font-medium text-gray-600">
                        <li><b class="text-orange-800">History:</b> Battle of Badr (2 AH), Battle of Uhud (3 AH).</li>
                        <li><b class="text-orange-800">Pillars:</b> Details of Zakat (Gold Nisaab: 7.5 Tola).</li>
                        <li><b class="text-orange-800">Holy Quran:</b> Total Surahs: 114, Total Para: 30, Makki/Madni divisions.</li>
                    </ul>
                </div>
                <div class="bg-white p-8 rounded-[40px] border-l-[10px] border-purple-600 shadow-sm">
                    <h3 class="text-xl font-black text-purple-700 mb-4 uppercase">🔢 Basic Mathematics</h3>
                    <ul class="space-y-3 text-sm font-medium text-gray-600">
                        <li><b class="text-purple-800">Arithmetic:</b> Percentages, Ratios, and Proportions.</li>
                        <li><b class="text-purple-800">Logic:</b> Basic Algebra and Word Problems (Age calculations).</li>
                        <li><b class="text-purple-800">Time:</b> Speed, Distance, and Time formula based questions.</li>
                    </ul>
                </div>
            </div>
        </div>

        <div id="chatTab" class="tab-content">
            <div class="max-w-3xl mx-auto bg-white rounded-[50px] p-8 shadow-2xl border">
                <div id="chatBox" class="h-96 overflow-y-auto mb-6 pr-4 space-y-4"></div>
                <div class="flex gap-2">
                    <input id="chatInp" type="text" placeholder="Discuss questions..." class="flex-1 p-4 bg-gray-50 rounded-2xl outline-none font-bold">
                    <button onclick="sendGlobal()" class="bg-blue-900 text-white px-8 rounded-2xl font-black">SEND</button>
                </div>
            </div>
        </div>

        <div id="supportTab" class="tab-content">
            <div class="max-w-2xl mx-auto bg-slate-900 rounded-[50px] p-10 text-white shadow-2xl">
                <h3 class="text-2xl font-black text-blue-400 italic mb-2 uppercase">Admin Support</h3>
                <p class="text-[10px] text-slate-500 mb-6 font-bold uppercase tracking-widest italic">Secure Encrypted Private Channel</p>
                <div id="supportBox" class="h-64 overflow-y-auto mb-4 space-y-3 pr-2 border-b border-white/5"></div>
                <div class="flex gap-2">
                    <input id="supportInp" type="text" placeholder="Type your message..." class="flex-1 p-4 bg-slate-800 rounded-2xl outline-none text-white border border-slate-700">
                    <button onclick="sendSupport()" class="bg-blue-600 px-6 rounded-2xl font-black">SEND</button>
                </div>
            </div>
        </div>

        <div id="godTab" class="tab-content bg-white p-10 rounded-[50px] shadow-2xl border-t-[12px] border-red-600 mt-8">
            <div class="flex justify-between items-center mb-8 border-b pb-6">
                <h2 class="text-4xl font-black text-slate-900 italic uppercase">System Override</h2>
                <div class="bg-red-100 text-red-600 px-4 py-2 rounded-xl text-xs font-black">GOD MODE ACTIVE</div>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <div class="bg-gray-50 p-8 rounded-3xl border">
                    <h4 class="text-xs font-black text-blue-900 uppercase mb-4">User Status & Ban Control</h4>
                    <input id="banName" type="text" placeholder="Candidate Name" class="w-full p-4 rounded-2xl border mb-4">
                    <div class="flex gap-2">
                        <button onclick="alert('User Restricted')" class="bg-red-600 text-white px-6 py-2 rounded-xl font-bold">BAN USER</button>
                        <button onclick="alert('User Restored')" class="bg-green-600 text-white px-6 py-2 rounded-xl font-bold">UNBAN USER</button>
                    </div>
                </div>
                <div class="bg-gray-50 p-8 rounded-3xl border">
                    <h4 class="text-xs font-black text-blue-900 uppercase mb-4">System Lockdown</h4>
                    <button onclick="toggleLock()" class="w-full bg-slate-900 text-white py-4 rounded-2xl font-black mb-2">FREEZE ALL ACCOUNTS</button>
                    <p class="text-[9px] text-gray-400 text-center uppercase font-black">Use only during maintenance</p>
                </div>
            </div>
        </div>

    </main>

    <footer class="mt-20 p-10 text-center opacity-30">
        <p class="text-[10px] font-black uppercase text-gray-500 mb-2">Privacy Protected by Prime Solutions</p>
        <span onclick="checkAdmin()" class="cursor-pointer text-[10px] text-blue-300 font-bold">ROOT_ACCESS_PRIME_2026</span>
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

        let me = { name: "", level: 1 };
        let taps = 0;

        window.onload = () => {
            const saved = localStorage.getItem('hub_user');
            if(saved) {
                me.name = saved;
                me.level = parseInt(localStorage.getItem('hub_lvl')) || 1;
                document.getElementById('cardName').innerText = me.name;
                document.getElementById('cardLvl').innerText = "Level 0" + me.level;
                renderLevels();
                listenChat();
                listenSupport();
            } else {
                document.getElementById('loginScreen').classList.remove('hidden');
            }
        };

        function saveUser() {
            const n = document.getElementById('uName').value.trim();
            if(!n) return;
            localStorage.setItem('hub_user', n);
            localStorage.setItem('hub_lvl', 1);
            location.reload();
        }

        function renderLevels() {
            const grid = document.getElementById('levelGrid');
            grid.innerHTML = '';
            for(let i=1; i<=10; i++) {
                let lock = i > me.level;
                grid.innerHTML += `
                    <div class="level-card p-10 text-center shadow-sm bg-white border-2 ${lock ? 'locked' : 'border-blue-100'}">
                        <div class="text-4xl mb-4">${lock ? '🔒' : '🎖️'}</div>
                        <h4 class="font-black text-[10px] uppercase">Level 0${i}</h4>
                    </div>
                `;
            }
        }

        // GLOBAL CHAT
        function sendGlobal() {
            const t = document.getElementById('chatInp').value.trim();
            if(!t) return;
            db.ref('global_chat_v4').push({ name: me.name, text: t, time: Date.now() });
            document.getElementById('chatInp').value = '';
        }

        function listenChat() {
            db.ref('global_chat_v4').limitToLast(15).on('value', snap => {
                const box = document.getElementById('chatBox');
                box.innerHTML = '';
                snap.forEach(c => {
                    const d = c.val();
                    box.innerHTML += `
                        <div class="p-4 rounded-3xl ${d.name === me.name ? 'bg-blue-900 text-white ml-auto' : 'bg-gray-100 mr-auto'} max-w-[80%]">
                            <span class="text-[8px] font-black uppercase mb-1 block opacity-60">${d.name}</span>
                            <p class="text-sm font-bold">${d.text}</p>
                        </div>
                    `;
                });
                box.scrollTop = box.scrollHeight;
            });
        }

        // PRIVATE SUPPORT
        function sendSupport() {
            const t = document.getElementById('supportInp').value.trim();
            if(!t) return;
            db.ref('support/'+me.name).push({ sender: me.name, text: t });
            document.getElementById('supportInp').value = '';
        }

        function listenSupport() {
            db.ref('support/'+me.name).on('value', snap => {
                const box = document.getElementById('supportBox');
                box.innerHTML = '';
                snap.forEach(c => {
                    const d = c.val();
                    box.innerHTML += `<div class="p-3 bg-slate-800 rounded-2xl text-[11px] mb-2 font-medium"><b>${d.sender}:</b> ${d.text}</div>`;
                });
                box.scrollTop = box.scrollHeight;
            });
        }

        // ADMIN TRIGGER
        function checkAdmin() {
            taps++;
            if(taps === 5) {
                const k = prompt("ENTER SYSTEM KEY:");
                if(k === "prime786") {
                    document.getElementById('godTab').classList.remove('hidden');
                    showTab('godTab');
                    alert("GOD MODE AUTHORIZED");
                }
                taps = 0;
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
