<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Global Academy | Discussion Hub</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f1f5f9; }
        .tab-content { display: none; }
        .active-tab { display: block; animation: fadeIn 0.3s ease; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        
        /* Chat Styling */
        #chatBox { height: 400px; overflow-y: auto; scroll-behavior: smooth; }
        .msg-card { background: white; border-radius: 15px 15px 15px 0; padding: 12px; margin-bottom: 10px; box-shadow: 0 2px 5px rgba(0,0,0,0.05); max-width: 85%; }
        .msg-name { font-size: 10px; font-weight: 900; color: #1e3a8a; text-transform: uppercase; margin-bottom: 4px; display: block; }
        .msg-text { font-size: 14px; color: #334155; }
        
        .level-card { border-radius: 30px; transition: 0.3s; background: white; cursor: pointer; }
        .locked { filter: grayscale(1); opacity: 0.4; pointer-events: none; }
    </style>
</head>
<body class="pb-20">

    <div id="loginScreen" class="fixed inset-0 bg-slate-900 z-[1000] flex items-center justify-center p-6 hidden">
        <div class="bg-white p-10 rounded-[50px] max-w-md w-full text-center shadow-2xl">
            <h2 class="text-4xl font-black text-blue-900 mb-6 italic">PRIME HUB</h2>
            <input id="candidateName" type="text" placeholder="Enter Your Name" class="w-full p-5 bg-gray-100 rounded-2xl mb-4 border-2 border-transparent focus:border-blue-600 outline-none text-center font-bold">
            <button onclick="loginUser()" class="w-full bg-blue-900 text-white py-4 rounded-2xl font-black shadow-lg">JOIN ACADEMY</button>
        </div>
    </div>

    <nav class="bg-white sticky top-0 z-50 p-4 border-b flex justify-between items-center px-6">
        <h1 class="font-black text-xl text-blue-900 italic">GBTS PRO</h1>
        <div class="flex gap-4">
            <button onclick="showTab('levelsTab')" class="text-[10px] font-black uppercase text-blue-900">Training</button>
            <button onclick="showTab('chatTab')" class="text-[10px] font-black uppercase text-gray-500">Discussion</button>
            <button onclick="showTab('bookTab')" class="text-[10px] font-black uppercase text-gray-500">Syllabus</button>
            <button onclick="logout()" class="text-[10px] font-black uppercase text-red-500">Logout</button>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-4 mt-6">
        
        <div id="levelsTab" class="tab-content active-tab">
            <h2 class="text-3xl font-black mb-8 italic uppercase">Mission Levels</h2>
            <div id="levelGrid" class="grid grid-cols-2 md:grid-cols-5 gap-4"></div>
        </div>

        <div id="chatTab" class="tab-content">
            <div class="max-w-3xl mx-auto bg-slate-200 rounded-[40px] p-6 shadow-inner">
                <div class="mb-4 text-center">
                    <h2 class="font-black text-2xl text-slate-800 uppercase">Global Discussion</h2>
                    <p class="text-[10px] font-bold text-slate-500 italic">Live Chat with all Candidates</p>
                </div>
                
                <div id="chatBox" class="p-4 bg-slate-50 rounded-3xl mb-4 border-2 border-white shadow-sm">
                    </div>
                
                <div class="flex gap-2">
                    <input id="chatInput" type="text" placeholder="Apna sawal yahan likhein..." class="flex-1 p-4 rounded-2xl outline-none border-2 border-transparent focus:border-blue-600 font-medium">
                    <button onclick="sendMessage()" class="bg-blue-900 text-white px-8 rounded-2xl font-black shadow-lg hover:scale-95 transition">SEND</button>
                </div>
            </div>
        </div>

        <div id="bookTab" class="tab-content">
            <h2 class="text-3xl font-black mb-8 italic uppercase">Full Syllabus Book</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="bg-white p-8 rounded-[40px] border-l-8 border-blue-900 shadow-sm">
                    <h4 class="font-black text-xl mb-4 text-blue-900">English & Grammar</h4>
                    <p class="text-sm text-gray-600 leading-relaxed">Pora paper Prepositions, Antonyms, aur Active/Passive voice par mushtamil hota hai. "Die of a disease" aur "Junior to me" jaise rules ko ghol kar pi jayein.</p>
                </div>
                <div class="bg-white p-8 rounded-[40px] border-l-8 border-green-600 shadow-sm">
                    <h4 class="font-black text-xl mb-4 text-green-700">General Knowledge</h4>
                    <p class="text-sm text-gray-600 leading-relaxed">GB ke 14 districts ke naam, Nanga Parbat ki unchai (8126m), aur Karakoram Highway ki details tayar karein.</p>
                </div>
            </div>
        </div>

        <div id="godPanel" class="tab-content mt-10 p-10 bg-black text-white rounded-[50px] border-4 border-blue-600">
            <h2 class="text-3xl font-black text-blue-400 mb-6">GOD MODE CONTROLS</h2>
            <div id="adminArea" class="space-y-4">
                <p class="text-slate-500 italic">Checking for student requests...</p>
            </div>
            <button onclick="localStorage.clear(); location.reload();" class="mt-10 text-red-500 font-bold text-xs underline">WIPE SYSTEM DATA</button>
        </div>

    </main>

    <footer class="fixed bottom-0 left-0 right-0 p-4 text-center opacity-10">
        <span onclick="triggerGod()" class="text-[8px] cursor-default">GOD_MODE_PRIME_2026</span>
    </footer>

    <script>
        // CONFIG
        const firebaseConfig = {
          apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
          authDomain: "dark-web-9.firebaseapp.com",
          projectId: "dark-web-9",
          storageBucket: "dark-web-9.firebasestorage.app",
          databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com/", // MAKE SURE THIS IS CORRECT
          messagingSenderId: "564328425161",
          appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.database();

        let user = { name: "", level: 1, status: "ready" };
        let godTaps = 0;

        window.onload = () => {
            const saved = localStorage.getItem('elite_user');
            if(saved) {
                user.name = saved;
                user.level = parseInt(localStorage.getItem('elite_lvl_' + saved)) || 1;
                user.status = localStorage.getItem('elite_status_' + saved) || "ready";
                renderLevels();
                listenToChat();
            } else {
                document.getElementById('loginScreen').classList.remove('hidden');
            }
        };

        function loginUser() {
            const n = document.getElementById('candidateName').value.trim();
            if(!n) return alert("Enter Name!");
            localStorage.setItem('elite_user', n);
            location.reload();
        }

        function logout() { localStorage.clear(); location.reload(); }

        // --- GLOBAL CHAT SYSTEM ---
        function sendMessage() {
            const input = document.getElementById('chatInput');
            const msg = input.value.trim();
            if(!msg) return;

            db.ref('global_chat').push({
                name: user.name,
                text: msg,
                time: Date.now()
            });
            input.value = '';
        }

        function listenToChat() {
            const box = document.getElementById('chatBox');
            db.ref('global_chat').limitToLast(20).on('value', (snap) => {
                box.innerHTML = '';
                snap.forEach((child) => {
                    const data = child.val();
                    box.innerHTML += `
                        <div class="msg-card">
                            <span class="msg-name">${data.name}</span>
                            <p class="msg-text">${data.text}</p>
                        </div>
                    `;
                });
                box.scrollTop = box.scrollHeight;
            });
        }

        // --- LEVELS & ADMIN ---
        function renderLevels() {
            const grid = document.getElementById('levelGrid');
            grid.innerHTML = '';
            for(let i=1; i<=10; i++) {
                let locked = i > user.level;
                grid.innerHTML += `
                    <div class="level-card p-8 text-center shadow-md border-2 ${locked ? 'locked' : 'border-white'}">
                        <div class="text-4xl mb-2">${locked ? '🔒' : '🎖️'}</div>
                        <h4 class="font-black text-sm">LEVEL ${i}</h4>
                    </div>
                `;
            }
            updateAdmin();
        }

        function triggerGod() {
            godTaps++;
            if(godTaps === 4) {
                const p = prompt("ENTER GOD MODE KEY:");
                if(p === "prime786") { showTab('godPanel'); alert("GOD MODE ACTIVE"); }
                godTaps = 0;
            }
        }

        function updateAdmin() {
            const area = document.getElementById('adminArea');
            if(user.status === "pending") {
                area.innerHTML = `
                    <div class="bg-white p-4 rounded-2xl flex justify-between items-center text-black">
                        <span class="font-black">${user.name} (Level ${user.level})</span>
                        <button onclick="godApprove()" class="bg-green-600 text-white px-4 py-1 rounded-xl font-bold">Approve</button>
                    </div>
                `;
            }
        }

        function godApprove() {
            localStorage.setItem('elite_lvl_' + user.name, user.level + 1);
            localStorage.setItem('elite_status_' + user.name, "ready");
            location.reload();
        }

        function showTab(id) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab'));
            document.getElementById(id).classList.add('active-tab');
        }
    </script>
</body>
</html>
