<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Elite | Official Academy Portal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-auth-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #0f172a; color: white; overflow-x: hidden; }
        .premium-card { background: rgba(255, 255, 255, 0.03); backdrop-blur: 20px; border: 1px solid rgba(255,255,255,0.1); border-radius: 50px; }
        .tab-content { display: none; }
        .active-tab { display: block; animation: fadeInUp 0.6s ease both; }
        .level-card { transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); cursor: pointer; border-radius: 40px; background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); }
        .level-card:hover:not(.locked) { transform: translateY(-10px) scale(1.05); border-color: #3b82f6; box-shadow: 0 20px 40px rgba(59,130,246,0.2); }
        .locked { filter: grayscale(1); opacity: 0.3; cursor: not-allowed; }
        .welcome-hero { background: linear-gradient(135deg, #1e3a8a 0%, #3b82f6 100%); border-radius: 60px; position: relative; overflow: hidden; }
        #mainLoader { position: fixed; inset: 0; background: #0f172a; z-index: 100000; display: flex; flex-direction: column; items: center; justify-content: center; }
        @keyframes fadeInUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

    <div id="mainLoader">
        <div class="w-16 h-16 border-4 border-blue-900 border-t-blue-400 rounded-full animate-spin mb-6"></div>
        <h1 class="text-white font-black tracking-[0.4em] italic uppercase animate__animated animate__pulse animate__infinite">GBTS Academy</h1>
    </div>

    <div id="loginOverlay" class="fixed inset-0 bg-slate-950 z-[99999] flex items-center justify-center p-6 hidden">
        <div class="premium-card p-12 max-w-md w-full text-center animate__animated animate__zoomIn">
            <div class="w-24 h-24 bg-blue-600 text-white rounded-[35px] flex items-center justify-center mx-auto mb-8 text-4xl font-black italic shadow-2xl">GB</div>
            <h2 class="text-3xl font-black text-white mb-2 italic">CANDIDATE LOGIN</h2>
            <p class="text-[10px] font-black text-blue-400 mb-10 tracking-[0.4em] uppercase">Session 2026 | Prime Solutions</p>
            <input id="uName" type="text" placeholder="Your Full Name" class="w-full p-6 bg-white/5 rounded-[30px] font-bold text-center outline-none border-2 border-transparent focus:border-blue-500 transition-all mb-8 text-white text-lg">
            <button onclick="handleLogin()" class="w-full bg-blue-600 text-white py-6 rounded-[30px] font-black text-xl shadow-2xl hover:bg-blue-700 active:scale-95 transition-all">START LEARNING</button>
            <p class="mt-8 text-[9px] text-white/30 font-bold uppercase tracking-widest">Secured by Firebase Anonymous Auth</p>
        </div>
    </div>

    <nav class="bg-[#0f172a]/80 backdrop-blur-2xl sticky top-0 z-50 p-6 px-10 flex justify-between items-center border-b border-white/5">
        <div class="flex items-center gap-4">
            <div class="w-12 h-12 bg-blue-600 text-white flex items-center justify-center rounded-2xl font-black italic text-xl shadow-lg">G</div>
            <h1 class="font-black text-sm uppercase italic hidden md:block">GBTS Elite</h1>
        </div>
        <div class="flex gap-6 md:gap-10 font-black text-[10px] uppercase italic">
            <button onclick="showTab('dashTab')" class="text-blue-400">Roadmap</button>
            <button onclick="showTab('rankTab')" class="text-white/40">Leaderboard</button>
            <button onclick="logout()" class="text-red-500">Sign Out</button>
        </div>
    </nav>

    <main class="max-w-7xl mx-auto p-6 md:p-10">
        
        <div id="dashTab" class="tab-content active-tab">
            <div class="welcome-hero p-12 md:p-20 text-white shadow-2xl mb-16 animate__animated animate__fadeIn" onclick="checkAdminTrigger()">
                <div class="relative z-10">
                    <p class="text-[10px] font-black text-blue-200 mb-4 uppercase tracking-widest">Candidate Portfolio</p>
                    <h2 id="dispName" class="text-5xl md:text-7xl font-black italic uppercase tracking-tighter mb-8 leading-none">---</h2>
                    <div class="flex flex-wrap gap-4">
                        <div class="bg-white/10 backdrop-blur-md px-6 py-3 rounded-2xl text-[12px] font-black border border-white/10 uppercase italic">
                            Rank: Level <span id="dispLvl" class="text-blue-300">01</span>
                        </div>
                        <div class="bg-green-500/20 backdrop-blur-md px-6 py-3 rounded-2xl text-[12px] font-black border border-green-500/20 text-green-400 uppercase italic">
                            Status: Verified
                        </div>
                    </div>
                </div>
            </div>

            <h3 class="text-xl font-black uppercase italic mb-10 text-white/50 tracking-widest px-4">Training Roadmap</h3>
            <div id="levelGrid" class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-6 gap-8"></div>
        </div>

        <div id="rankTab" class="tab-content">
            <div class="max-w-2xl mx-auto premium-card p-12 text-center">
                <div class="text-5xl mb-6">🏆</div>
                <h3 class="text-3xl font-black text-white mb-10 italic uppercase tracking-tighter">Hall of Fame</h3>
                <div id="leaderList" class="space-y-4"></div>
            </div>
        </div>

        <div id="adminTab" class="tab-content">
            <div class="premium-card p-10">
                <div class="flex justify-between items-center mb-10 border-b border-white/5 pb-6">
                    <h3 class="text-2xl font-black text-red-500 italic uppercase">System Administration</h3>
                    <button onclick="showTab('dashTab')" class="text-[10px] font-black underline opacity-50">EXIT ADMIN</button>
                </div>
                <div class="overflow-x-auto">
                    <table class="w-full text-left">
                        <thead>
                            <tr class="text-[10px] font-black uppercase text-white/30">
                                <th class="pb-6">Student</th>
                                <th class="pb-6">Progress</th>
                                <th class="pb-6 text-right">Action</th>
                            </tr>
                        </thead>
                        <tbody id="adminUserList" class="text-sm font-bold"></tbody>
                    </table>
                </div>
            </div>
        </div>
    </main>

    <div id="quizModal" class="fixed inset-0 bg-slate-950/98 z-[100000] hidden items-center justify-center p-6 backdrop-blur-xl">
        <div class="premium-card p-12 max-w-lg w-full text-center animate__animated animate__bounceIn">
            <h3 id="mTitle" class="text-3xl font-black text-blue-500 mb-6 italic uppercase">Module Unlock</h3>
            <div id="mStudy" class="bg-white/5 p-8 rounded-[40px] mb-8 text-sm italic text-blue-200 border border-white/10"></div>
            <input id="ansInp" type="text" placeholder="Verification Code" class="w-full p-6 bg-white/5 rounded-[35px] mb-8 text-center font-black border-4 border-transparent focus:border-blue-500 outline-none transition-all text-xl text-white">
            <button id="subBtn" class="w-full bg-blue-600 text-white py-6 rounded-[35px] font-black text-lg shadow-2xl">SUBMIT RESPONSE</button>
            <button onclick="closeModal()" class="mt-8 text-[10px] font-black text-white/30 uppercase tracking-widest">Go Back</button>
        </div>
    </div>

    <script>
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
        const auth = firebase.auth();
        const db = firebase.database();

        let userData = null;
        let adminClicks = 0;

        // --- AUTH & INITIALIZATION ---
        window.onload = () => {
            setTimeout(() => {
                document.getElementById('mainLoader').style.display = 'none';
                const saved = localStorage.getItem('gbts_elite_session_v1');
                if(saved) processLogin(saved);
                else document.getElementById('loginOverlay').style.display = 'flex';
            }, 1800);
        };

        async function handleLogin() {
            const name = document.getElementById('uName').value.trim();
            if(!name || name.length < 3) return alert("Sweetie, apna pura naam likhein!");
            localStorage.setItem('gbts_elite_session_v1', name);
            processLogin(name);
        }

        async function processLogin(name) {
            const userKey = name.toLowerCase().replace(/\s/g, '_').substring(0, 20);
            try {
                const res = await auth.signInAnonymously();
                const userRef = db.ref('gbts_elite_v5/' + userKey);
                const snap = await userRef.get();
                if(!snap.exists()) {
                    await userRef.set({ uid: res.user.uid, name: name, level: 1, key: userKey });
                }
                document.getElementById('loginOverlay').style.display = 'none';
                initPortal(userKey);
            } catch (e) { alert("Database Error! Connection lost sweetie."); }
        }

        function initPortal(key) {
            db.ref('gbts_elite_v5/' + key).on('value', snap => {
                userData = snap.val();
                if(userData) {
                    document.getElementById('dispName').innerText = userData.name;
                    document.getElementById('dispLvl').innerText = userData.level < 10 ? "0"+userData.level : userData.level;
                    renderLevels();
                }
            });
            syncRanks();
            loadAdminList();
        }

        function renderLevels() {
            const grid = document.getElementById('levelGrid'); grid.innerHTML = '';
            for(let i=1; i<=12; i++) {
                let lock = i > userData.level;
                grid.innerHTML += `<div onclick="openTest(${i}, ${lock})" class="level-card p-12 text-center ${lock ? 'locked' : 'shadow-lg animate__animated animate__fadeInUp'}" style="animation-delay: ${i*0.05}s">
                    <div class="text-4xl mb-4">${lock ? '🔒' : '🎖️'}</div>
                    <p class="text-[11px] font-black uppercase text-white/30">Level ${i}</p>
                </div>`;
            }
        }

        function openTest(num, lock) {
            if(lock || num !== userData.level) return;
            document.getElementById('quizModal').style.display = 'flex';
            document.getElementById('mStudy').innerText = "Congratulations " + userData.name + "! To unlock Module " + (num+1) + ", type 'prime' below.";
            document.getElementById('subBtn').onclick = () => {
                if(document.getElementById('ansInp').value.trim().toLowerCase() === 'prime') {
                    confetti({ particleCount: 200, spread: 80, origin: { y: 0.6 } });
                    db.ref('gbts_elite_v5/' + userData.key).update({ level: userData.level + 1 });
                    db.ref('elite_ranks_v5/' + userData.key).set({ name: userData.name, level: userData.level + 1 });
                    closeModal();
                } else { alert("Ghalat code!"); }
            };
        }

        function syncRanks() {
            db.ref('elite_ranks_v5').orderByChild('level').limitToLast(10).on('value', snap => {
                const list = document.getElementById('leaderList'); list.innerHTML = '';
                let ranks = []; snap.forEach(c => ranks.push(c.val()));
                ranks.reverse().forEach((r, i) => {
                    list.innerHTML += `<div class="flex justify-between items-center p-6 bg-white/5 rounded-[35px] border border-white/5 animate__animated animate__fadeInLeft">
                        <span class="font-black text-xs uppercase italic">#${i+1} ${r.name}</span>
                        <span class="bg-blue-600 px-4 py-2 rounded-full text-[10px] font-black italic">LEVEL ${r.level}</span>
                    </div>`;
                });
            });
        }

        // --- ADMIN CONTROLS ---
        function checkAdminTrigger() {
            adminClicks++;
            if(adminClicks >= 5) {
                const pass = prompt("Secret Admin Key:");
                if(pass === "prime786") showTab('adminTab');
                adminClicks = 0;
            }
        }

        function loadAdminList() {
            db.ref('gbts_elite_v5').on('value', snap => {
                const list = document.getElementById('adminUserList'); list.innerHTML = '';
                snap.forEach(c => {
                    const u = c.val();
                    list.innerHTML += `<tr class="border-b border-white/5">
                        <td class="py-6 uppercase">${u.name}</td>
                        <td class="py-6 text-blue-400">Level ${u.level}</td>
                        <td class="py-6 text-right">
                            <button onclick="editLvl('${u.key}', ${u.level})" class="text-blue-500 mr-4">Edit</button>
                            <button onclick="delUser('${u.key}')" class="text-red-500">Delete</button>
                        </td>
                    </tr>`;
                });
            });
        }

        function editLvl(key, l) {
            const nl = prompt("Update Level to:", l);
            if(nl) {
                db.ref('gbts_elite_v5/' + key).update({ level: parseInt(nl) });
                db.ref('elite_ranks_v5/' + key).update({ level: parseInt(nl) });
            }
        }

        function delUser(key) {
            if(confirm("Confirm Delete?")) {
                db.ref('gbts_elite_v5/' + key).remove();
                db.ref('elite_ranks_v5/' + key).remove();
            }
        }

        function showTab(id) { document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab')); document.getElementById(id).classList.add('active-tab'); }
        function closeModal() { document.getElementById('quizModal').style.display = 'none'; document.getElementById('ansInp').value = ''; }
        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
