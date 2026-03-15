<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Elite | Leaderboard & Training</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f8fafc; color: #0f172a; }
        .tab-content { display: none; }
        .active-tab { display: block; animation: fadeInUp 0.4s ease; }
        @keyframes fadeInUp { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .glass-nav { background: rgba(255, 255, 255, 0.8); backdrop-filter: blur(10px); }
        .level-card { transition: 0.3s; cursor: pointer; border-radius: 30px; }
        .locked { opacity: 0.4; cursor: not-allowed; filter: grayscale(1); }
        .quiz-modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.9); z-index: 9999; align-items: center; justify-content: center; p: 20px; }
        .rank-1 { background: linear-gradient(90deg, #fbbf24, #f59e0b); color: white; }
    </style>
</head>
<body class="pb-24">

    <div id="quizModal" class="quiz-modal">
        <div class="bg-white p-8 rounded-[40px] max-w-lg w-full shadow-2xl relative">
            <div id="timer" class="absolute -top-6 left-1/2 -translate-x-1/2 bg-red-600 text-white px-8 py-2 rounded-full font-black shadow-xl border-4 border-white">
                ⏱️ <span id="timeSec">30</span>s
            </div>
            <h3 id="mTitle" class="text-2xl font-black text-blue-900 mb-4 uppercase italic">Test</h3>
            <div id="mStudy" class="text-sm bg-blue-50 p-4 rounded-2xl mb-6 border border-blue-100 text-blue-800 italic"></div>
            <div id="mQuestion" class="space-y-4"></div>
        </div>
    </div>

    <nav class="glass-nav sticky top-0 z-50 p-4 flex justify-between items-center px-8 border-b">
        <div class="flex items-center gap-2">
            <div class="w-8 h-8 bg-blue-900 text-white flex items-center justify-center rounded-lg font-black text-xs">G</div>
            <h1 class="font-black text-lg italic text-blue-900">GBTS ELITE</h1>
        </div>
        <div class="flex gap-4">
            <button onclick="showTab('dashTab')" class="text-[10px] font-black uppercase text-blue-900">Portal</button>
            <button onclick="showTab('rankTab')" class="text-[10px] font-black uppercase text-gray-500">Leaderboard</button>
            <button onclick="logout()" class="text-[10px] font-black uppercase text-red-500">Exit</button>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-6">
        
        <div id="dashTab" class="tab-content active-tab">
            <div class="bg-blue-900 p-10 rounded-[45px] text-white shadow-2xl mb-12 flex justify-between items-center">
                <div>
                    <p class="text-[10px] font-black opacity-50 uppercase mb-2">Authenticated User</p>
                    <h2 id="dispName" class="text-4xl font-black italic tracking-tighter uppercase">---</h2>
                </div>
                <div class="text-right">
                    <p class="text-[10px] font-black opacity-50">LEVEL</p>
                    <h2 id="dispLvl" class="text-5xl font-black">01</h2>
                </div>
            </div>
            <h3 class="text-2xl font-black italic uppercase mb-8">Mission Training</h3>
            <div id="levelGrid" class="grid grid-cols-2 md:grid-cols-5 gap-6"></div>
        </div>

        <div id="rankTab" class="tab-content">
            <div class="max-w-2xl mx-auto bg-white rounded-[50px] p-8 shadow-sm border border-gray-100">
                <div class="text-center mb-8">
                    <h2 class="text-3xl font-black italic text-blue-950 uppercase">Top 10 Officers</h2>
                    <p class="text-[10px] font-bold text-gray-400">REAL-TIME RANKING SYSTEM</p>
                </div>
                <div id="leaderList" class="space-y-3">
                    </div>
            </div>
        </div>

    </main>

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
        const db = firebase.database();

        let me = { name: "", level: 1 };
        let timer = null;

        const mods = {
            1: { s: "GK: GB has 14 districts.", q: "How many districts in GB?", a: "14" },
            2: { s: "Math: 5x5 is 25.", q: "What is 5 multiplied by 5?", a: "25" },
            3: { s: "Eng: 'Big' synonym is 'Huge'.", q: "Synonym of Big?", a: "Huge" }
        };

        window.onload = () => {
            const saved = localStorage.getItem('officer_n');
            if(saved) {
                me.name = saved;
                me.level = parseInt(localStorage.getItem('officer_l')) || 1;
                document.getElementById('dispName').innerText = me.name;
                document.getElementById('dispLvl').innerText = me.level;
                renderLevels();
                updateLeaderboard();
                // Push or update my data to Firebase for Leaderboard
                syncData();
            } else {
                let n = prompt("Enter Your Full Name for Admission:");
                if(n) {
                    localStorage.setItem('officer_n', n);
                    localStorage.setItem('officer_l', 1);
                    location.reload();
                }
            }
        };

        function syncData() {
            db.ref('ranks/' + me.name).set({ name: me.name, level: me.level });
        }

        function renderLevels() {
            const grid = document.getElementById('levelGrid');
            grid.innerHTML = '';
            for(let i=1; i<=10; i++) {
                let lock = i > me.level;
                grid.innerHTML += `
                    <div onclick="openMod(${i}, ${lock})" class="level-card p-10 text-center bg-white border-2 ${lock ? 'locked' : 'border-blue-100 shadow-sm'}">
                        <div class="text-4xl mb-3">${lock ? '🔒' : '🎖️'}</div>
                        <h4 class="font-black text-[10px] uppercase">Level 0${i}</h4>
                    </div>
                `;
            }
        }

        function openMod(num, lock) {
            if(lock) return alert("Level locked sweetie!");
            const data = mods[num] || { s: "Keep going!", q: "Type 'go' to pass", a: "go" };
            document.getElementById('quizModal').style.display = 'flex';
            document.getElementById('mTitle').innerText = "Level " + num;
            document.getElementById('mStudy').innerHTML = data.s;
            document.getElementById('mQuestion').innerHTML = `
                <p class="font-bold mb-4">${data.q}</p>
                <input id="ansInp" type="text" placeholder="Your Answer..." class="w-full p-4 bg-gray-100 rounded-2xl outline-none font-bold">
                <button onclick="checkAns(${num}, '${data.a}')" class="w-full bg-blue-900 text-white py-4 mt-4 rounded-2xl font-black">SUBMIT</button>
            `;
            startTimer();
        }

        function startTimer() {
            let sec = 30;
            document.getElementById('timeSec').innerText = sec;
            clearInterval(timer);
            timer = setInterval(() => {
                sec--;
                document.getElementById('timeSec').innerText = sec;
                if(sec <= 0) {
                    clearInterval(timer);
                    alert("TIME OUT! Try again sweetie.");
                    location.reload();
                }
            }, 1000);
        }

        function checkAns(num, correct) {
            const val = document.getElementById('ansInp').value.trim().toLowerCase();
            if(val === correct.toLowerCase()) {
                clearInterval(timer);
                alert("CORRECT!");
                if(me.level === num) {
                    me.level++;
                    localStorage.setItem('officer_l', me.level);
                    syncData();
                }
                location.reload();
            } else { alert("Wrong Answer!"); }
        }

        function updateLeaderboard() {
            db.ref('ranks').orderByChild('level').limitToLast(10).on('value', snap => {
                const list = document.getElementById('leaderList');
                list.innerHTML = '';
                let ranks = [];
                snap.forEach(c => ranks.push(c.val()));
                ranks.reverse().forEach((r, index) => {
                    list.innerHTML += `
                        <div class="flex justify-between items-center p-5 rounded-3xl ${index === 0 ? 'rank-1 shadow-lg' : 'bg-gray-50'}">
                            <div class="flex items-center gap-4">
                                <span class="font-black opacity-50">#${index + 1}</span>
                                <span class="font-black uppercase italic text-sm">${r.name}</span>
                            </div>
                            <span class="bg-blue-900 text-white text-[10px] px-4 py-1 rounded-full font-black">LVL ${r.level}</span>
                        </div>
                    `;
                });
            });
        }

        function showTab(id) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab'));
            document.getElementById(id).classList.add('active-tab');
        }

        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
