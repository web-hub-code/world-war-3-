<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Pro Max | Elite Recruitment Hub</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f1f5f9; color: #0f172a; }
        .tab-content { display: none; }
        .active-tab { display: block; animation: slideUp 0.4s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .level-card { transition: 0.3s; cursor: pointer; border-radius: 35px; }
        .level-card:hover:not(.locked) { transform: translateY(-5px); border-color: #1e3a8a; background: #fff; }
        .locked { opacity: 0.4; cursor: not-allowed; filter: grayscale(1); }
        .quiz-modal { display: none; position: fixed; inset: 0; background: rgba(15, 23, 42, 0.98); z-index: 9999; align-items: center; justify-content: center; padding: 20px; }
    </style>
</head>
<body class="pb-20">

    <div id="quizModal" class="quiz-modal">
        <div class="bg-white p-8 rounded-[40px] max-w-xl w-full shadow-2xl overflow-y-auto max-h-[90vh]">
            <div class="flex justify-between items-center mb-6">
                <h3 id="mTitle" class="text-2xl font-black text-blue-900 italic">TEST MODULE</h3>
                <button onclick="closeQuiz()" class="text-red-500 font-black">CLOSE</button>
            </div>
            <div id="mStudy" class="bg-blue-50 p-6 rounded-3xl mb-6 text-sm text-blue-900 border border-blue-100">
                </div>
            <div id="mQuestion" class="space-y-4">
                </div>
        </div>
    </div>

    <div id="loginScreen" class="fixed inset-0 bg-slate-900 z-[5000] flex items-center justify-center p-6 hidden">
        <div class="bg-white p-12 rounded-[50px] max-w-md w-full text-center shadow-2xl border-t-[10px] border-blue-900">
            <h2 class="text-4xl font-black text-blue-900 mb-2 italic">GBTS LOGIN</h2>
            <p class="text-[10px] font-black text-gray-400 mb-8 tracking-widest uppercase">Candidate Admission System</p>
            <input id="uName" type="text" placeholder="Full Name" class="w-full p-5 bg-gray-50 rounded-3xl mb-4 outline-none border-2 focus:border-blue-600 text-center font-black">
            <button onclick="saveUser()" class="w-full bg-blue-900 text-white py-5 rounded-3xl font-black text-xl hover:bg-blue-800 transition">VERIFY & START</button>
        </div>
    </div>

    <nav class="bg-white/80 backdrop-blur-md sticky top-0 z-50 p-4 flex justify-between items-center px-8 border-b">
        <div class="flex items-center gap-2">
            <div class="w-8 h-8 bg-blue-900 text-white flex items-center justify-center rounded-lg font-black text-xs">G</div>
            <h1 class="font-black text-xl italic text-blue-950">GBTS ELITE</h1>
        </div>
        <div class="flex gap-4">
            <button onclick="showTab('dashTab')" class="text-[10px] font-black uppercase text-blue-900">Portal</button>
            <button onclick="showTab('chatTab')" class="text-[10px] font-black uppercase text-gray-400">Chat</button>
            <button onclick="logout()" class="text-[10px] font-black uppercase text-red-500">Exit</button>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-6 mt-6">
        <div id="dashTab" class="tab-content active-tab">
            <div class="bg-blue-900 p-10 rounded-[50px] text-white shadow-2xl mb-12 relative overflow-hidden">
                <div class="relative z-10">
                    <p class="text-[10px] font-black opacity-50 uppercase mb-4 tracking-widest">Candidate Application Status</p>
                    <h2 id="dispName" class="text-5xl font-black italic mb-2 tracking-tighter uppercase leading-none">---</h2>
                    <div class="flex gap-4 mt-6">
                        <span class="bg-green-500 text-white text-[10px] font-black px-4 py-2 rounded-full">LEVEL <span id="dispLvl">01</span> ACTIVE</span>
                        <span class="bg-white/10 text-white text-[10px] font-black px-4 py-2 rounded-full">RANK: ELITE</span>
                    </div>
                </div>
                <div class="absolute -right-10 -bottom-10 w-64 h-64 bg-white/10 rounded-full blur-3xl"></div>
            </div>

            <h3 class="text-2xl font-black italic uppercase mb-8 tracking-tighter">Mission Modules (1-10)</h3>
            <div id="levelGrid" class="grid grid-cols-2 md:grid-cols-5 gap-6"></div>
        </div>

        <div id="chatTab" class="tab-content">
            <div class="max-w-3xl mx-auto bg-white rounded-[50px] p-8 shadow-2xl border">
                <div id="chatBox" class="h-96 overflow-y-auto mb-6 pr-4 space-y-4"></div>
                <div class="flex gap-2">
                    <input id="chatInp" type="text" placeholder="Discuss with other candidates..." class="flex-1 p-4 bg-gray-100 rounded-3xl outline-none font-bold">
                    <button onclick="sendMsg()" class="bg-blue-900 text-white px-8 rounded-3xl font-black">SEND</button>
                </div>
            </div>
        </div>
    </main>

    <footer class="mt-20 p-10 text-center opacity-30">
        <span onclick="tAdmin()" class="cursor-pointer text-[10px] text-blue-300 font-bold">ROOT_ACCESS_PRIME_2026</span>
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

        // QUIZ DATA 1-10
        const moduleData = {
            1: { 
                study: "<b>English:</b> Synonyms are words with similar meanings. Example: 'Happy' = 'Joyful'.",
                q: "What is the synonym of 'Fast'?", a: "Quick" 
            },
            2: { 
                study: "<b>GK:</b> K2 is the 2nd highest peak in the world, located in the Karakoram range, Gilgit-Baltistan.",
                q: "In which mountain range is K2 located?", a: "Karakoram" 
            },
            3: { 
                study: "<b>Islamiyat:</b> The Battle of Badr took place in 2nd Hijri. 313 Muslims fought.",
                q: "How many Muslims fought in the Battle of Badr?", a: "313" 
            },
            4: { 
                study: "<b>Math:</b> To find 10% of a number, divide it by 10.",
                q: "What is 10% of 500?", a: "50" 
            },
            5: { 
                study: "<b>Current Affairs:</b> The capital of Gilgit-Baltistan is Gilgit.",
                q: "What is the capital of GB?", a: "Gilgit" 
            },
            6: { 
                study: "<b>English:</b> Antonyms are opposites. Example: 'Big' vs 'Small'.",
                q: "Antonym of 'Beautiful'?", a: "Ugly" 
            },
            7: { 
                study: "<b>GK:</b> Pakistan's national flower is Jasmine.",
                q: "What is the national flower of Pakistan?", a: "Jasmine" 
            },
            8: { 
                study: "<b>Math:</b> Average = Sum of values / Number of values.",
                q: "Average of 10, 20, 30?", a: "20" 
            },
            9: { 
                study: "<b>Islamiyat:</b> Prophet Muhammad (PBUH) had 4 daughters.",
                q: "How many daughters did the Prophet (PBUH) have?", a: "4" 
            },
            10: { 
                study: "<b>Final Exam:</b> Combine all knowledge. GB has 14 districts.",
                q: "Total districts in Gilgit-Baltistan?", a: "14" 
            }
        };

        window.onload = () => {
            const saved = localStorage.getItem('gbts_user');
            if(saved) {
                me.name = saved;
                me.level = parseInt(localStorage.getItem('gbts_lvl')) || 1;
                document.getElementById('dispName').innerText = me.name;
                document.getElementById('dispLvl').innerText = me.level;
                renderLevels();
                listenChat();
            } else {
                document.getElementById('loginScreen').classList.remove('hidden');
            }
        };

        function saveUser() {
            const n = document.getElementById('uName').value.trim();
            if(!n) return;
            localStorage.setItem('gbts_user', n);
            localStorage.setItem('gbts_lvl', 1);
            location.reload();
        }

        function renderLevels() {
            const grid = document.getElementById('levelGrid');
            grid.innerHTML = '';
            for(let i=1; i<=10; i++) {
                let lock = i > me.level;
                grid.innerHTML += `
                    <div onclick="openMod(${i}, ${lock})" class="level-card p-10 text-center bg-white border-2 ${lock ? 'locked' : 'border-blue-100 shadow-sm'}">
                        <div class="text-4xl mb-3">${lock ? '🔒' : '🎖️'}</div>
                        <h4 class="font-black text-[10px] uppercase tracking-widest">Level 0${i}</h4>
                    </div>
                `;
            }
        }

        function openMod(num, isLocked) {
            if(isLocked) return alert("Pehly pichla level mukammal karein sweetie!");
            document.getElementById('quizModal').style.display = 'flex';
            const data = moduleData[num];
            document.getElementById('mTitle').innerText = "MODULE 0" + num;
            document.getElementById('mStudy').innerHTML = data.study;
            document.getElementById('mQuestion').innerHTML = `
                <p class="font-bold text-gray-700">${data.q}</p>
                <input id="ans" type="text" placeholder="Apna jawab likhein..." class="w-full p-4 bg-gray-100 rounded-2xl outline-none font-bold border-2 focus:border-blue-900">
                <button onclick="verify(${num}, '${data.a}')" class="w-full bg-blue-900 text-white py-4 rounded-2xl font-black">SUBMIT TEST</button>
            `;
        }

        function verify(lvl, correct) {
            const u = document.getElementById('ans').value.trim().toLowerCase();
            if(u === correct.toLowerCase()) {
                alert("Mubarak ho sweetie! Sahi jawab.");
                if(me.level === lvl) {
                    me.level++;
                    localStorage.setItem('gbts_lvl', me.level);
                }
                closeQuiz();
                location.reload();
            } else {
                alert("Ghalat jawab! Study material dobara parhein.");
            }
        }

        function closeQuiz() { document.getElementById('quizModal').style.display = 'none'; }

        function sendMsg() {
            const t = document.getElementById('chatInp').value.trim();
            if(!t) return;
            db.ref('elite_chat').push({ name: me.name, text: t });
            document.getElementById('chatInp').value = '';
        }

        function listenChat() {
            db.ref('elite_chat').limitToLast(10).on('value', snap => {
                const box = document.getElementById('chatBox');
                box.innerHTML = '';
                snap.forEach(c => {
                    const d = c.val();
                    box.innerHTML += `<div class="p-4 rounded-3xl ${d.name === me.name ? 'bg-blue-900 text-white ml-auto' : 'bg-gray-100 mr-auto'} max-w-[80%]"><p class="text-[8px] font-black uppercase opacity-60 mb-1">${d.name}</p><p class="text-sm font-bold">${d.text}</p></div>`;
                });
                box.scrollTop = box.scrollHeight;
            });
        }

        function tAdmin() {
            taps++;
            if(taps === 5) {
                let p = prompt("Enter Admin Key:");
                if(p === "prime786") alert("Admin Access Granted!");
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
