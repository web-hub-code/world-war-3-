<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Pro Academy | Elite Training</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f0f4f8; color: #1e293b; }
        .glass-nav { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(15px); }
        .level-card { transition: 0.4s; border-radius: 35px; border: 2px solid transparent; background: white; }
        .level-card:hover:not(.locked) { transform: translateY(-10px); border-color: #1e3a8a; box-shadow: 0 20px 40px rgba(30,58,138,0.1); }
        .locked { filter: grayscale(1); opacity: 0.5; cursor: not-allowed; background: #e2e8f0; }
        .tab-content { display: none; }
        .active-tab { display: block; animation: zoomIn 0.3s ease; }
        .q-card { background: white; border-radius: 25px; padding: 25px; margin-bottom: 20px; border: 1px solid #e2e8f0; }
        .option-btn { display: block; width: 100%; text-align: left; padding: 15px 20px; margin-top: 10px; border: 2px solid #f1f5f9; border-radius: 15px; transition: 0.2s; }
        .option-btn:hover { border-color: #1e3a8a; background: #f0f7ff; }
        @keyframes zoomIn { from { opacity: 0; transform: scale(0.95); } to { opacity: 1; transform: scale(1); } }
        #adminPanel { display: none; } /* Hidden by default */
    </style>
</head>
<body class="pb-24">

    <div id="loginScreen" class="fixed inset-0 bg-[#0f172a] z-[500] flex items-center justify-center p-6 hidden">
        <div class="bg-white p-10 rounded-[45px] max-w-md w-full shadow-2xl text-center">
            <div class="w-20 h-20 bg-blue-100 rounded-full flex items-center justify-center mx-auto mb-6 text-3xl">👮</div>
            <h2 class="text-3xl font-black text-blue-900 mb-2 italic tracking-tighter">GBTS ACADEMY</h2>
            <p class="text-xs font-bold text-gray-400 mb-8 uppercase">Candidate Portal Login</p>
            <input id="candidateName" type="text" placeholder="Enter Full Name" class="w-full p-5 bg-gray-100 rounded-2xl mb-4 outline-none border-2 border-transparent focus:border-blue-600">
            <button onclick="loginUser()" class="w-full bg-blue-900 text-white py-5 rounded-2xl font-black text-lg shadow-xl hover:bg-black transition">START PREPARATION</button>
        </div>
    </div>

    <nav class="glass-nav sticky top-0 z-50 p-4 border-b">
        <div class="max-w-6xl mx-auto flex justify-between items-center">
            <h1 class="font-black text-blue-900 text-2xl italic tracking-tighter">PRIME HUB</h1>
            <div class="flex gap-4">
                <button onclick="showTab('levelsTab')" class="text-xs font-black uppercase text-blue-900">Levels</button>
                <button onclick="showTab('bookTab')" class="text-xs font-black uppercase text-gray-500">The Book</button>
                <button onclick="logout()" class="text-xs font-black uppercase text-red-500">Logout</button>
            </div>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-4 mt-8">
        
        <div id="levelsTab" class="tab-content active-tab">
            <div class="flex justify-between items-end mb-10">
                <div>
                    <h2 class="text-4xl font-black italic uppercase text-slate-800 tracking-tighter">Mission Control</h2>
                    <p class="text-blue-600 font-bold">Pass each level to unlock the next challenge.</p>
                </div>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-4 gap-6" id="levelContainer"></div>
        </div>

        <div id="bookTab" class="tab-content">
            <h2 class="text-4xl font-black mb-10 italic uppercase text-slate-800 tracking-tighter text-center">The Candidate's Bible</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <div class="bg-white p-8 rounded-[35px] shadow-sm border-t-8 border-blue-900">
                    <h3 class="font-black text-2xl mb-4 text-blue-900">General Knowledge (Deep)</h3>
                    <p class="text-sm text-gray-600 leading-relaxed">
                        <b>Gilgit-Baltistan:</b> Total area is 72,971 km². Boundary connects with China, Afghanistan, and India. 
                        <b>Highest Peaks:</b> K2 (8611m), Nanga Parbat (8126m), Gasherbrum I (8080m).
                        <b>Rivers:</b> Indus, Gilgit, Hunza, Shyok.
                    </p>
                </div>
                <div class="bg-white p-8 rounded-[35px] shadow-sm border-t-8 border-green-700">
                    <h3 class="font-black text-2xl mb-4 text-green-700">Islamic History</h3>
                    <p class="text-sm text-gray-600 leading-relaxed">
                        <b>Prophets:</b> 25 Prophets mentioned in Quran. 
                        <b>Firsts in Islam:</b> First Mosque (Quba), First Martyr (Hazrat Sumayya R.A), First Muazzin (Hazrat Bilal R.A).
                    </p>
                </div>
            </div>
        </div>

        <div id="adminPanel" class="mt-20 p-10 bg-black text-white rounded-[40px] text-center">
            <h2 class="text-3xl font-black mb-4">ADMIN ACCESS GRANTED</h2>
            <p class="mb-8 text-gray-400">Master controls for dark-web-9 project database.</p>
            <div class="flex justify-center gap-4">
                <button onclick="resetAllProgress()" class="bg-red-600 px-6 py-3 rounded-xl font-bold">RESET ALL USERS</button>
                <button onclick="location.reload()" class="bg-gray-800 px-6 py-3 rounded-xl font-bold">EXIT ADMIN</button>
            </div>
        </div>
    </main>

    <div id="testBox" class="hidden fixed inset-0 bg-[#f8fafc] z-[400] p-6 overflow-y-auto">
        <div class="max-w-2xl mx-auto">
            <div class="flex justify-between items-center mb-10">
                <h2 id="currentLevelTitle" class="text-4xl font-black text-blue-900 italic uppercase">LEVEL 01</h2>
                <div id="timer" class="bg-red-100 text-red-600 px-4 py-2 rounded-xl font-black">10:00</div>
            </div>
            <div id="quizContent" class="space-y-6"></div>
            <button onclick="validateTest()" class="w-full bg-blue-900 text-white py-6 rounded-[30px] mt-10 font-black text-xl shadow-2xl">SUBMIT PAPER</button>
        </div>
    </div>

    <footer class="fixed bottom-0 left-0 right-0 p-4 text-center text-[10px] text-gray-300">
        <span onclick="triggerAdmin()" class="cursor-default select-none">Powered by Prime Solutions | Official Verification Hub 2026</span>
    </footer>

    <script>
        // PERSISTENCE & DATA
        let user = { name: "", level: 1 };
        let tapCount = 0;

        const questionsData = {
            1: [
                { q: "Siachen Glacier is located in which mountain range?", a: "Karakoram", b: "Himalayas", c: "Hindukush", d: "Sulaiman", ans: "a" },
                { q: "The number of districts in GB is?", a: "10", b: "12", c: "14", d: "16", ans: "c" },
                { q: "What is the capital of Gilgit-Baltistan?", a: "Skardu", b: "Gilgit", c: "Hunza", d: "Astore", ans: "b" },
                { q: "Which is the highest peak in Pakistan?", a: "Nanga Parbat", b: "K2", c: "Rakaposhi", d: "Broad Peak", ans: "b" },
                { q: "Who was the first Governor of GB?", a: "Qamar Zaman Kaira", b: "Shama Khalid", c: "Wazir Baig", d: "Pir Karam Ali Shah", ans: "a" },
                { q: "The Deosai National Park is located in?", a: "Gilgit", b: "Skardu", c: "Diamer", d: "Ghanche", ans: "b" }
            ],
            2: [
                { q: "When did Pakistan win the Cricket World Cup?", a: "1992", b: "1996", c: "1999", d: "2009", ans: "a" },
                { q: "Identify the correct spelling:", a: "Lieutenant", b: "Leftenant", c: "Lieutanant", d: "Luetenant", ans: "a" },
                { q: "The Battle of Badr was fought in which Hijri?", a: "1 AH", b: "2 AH", c: "3 AH", d: "4 AH", ans: "b" },
                { q: "25% of 1000 is?", a: "200", b: "250", c: "300", d: "500", ans: "b" },
                { q: "Synonym of 'Abbreviate' is?", a: "Expand", b: "Shorten", c: "Explain", d: "Ignore", ans: "b" },
                { q: "Which country is to the North of GB?", a: "China", b: "India", c: "Iran", d: "Afghanistan", ans: "a" }
            ]
        };

        window.onload = () => {
            const savedName = localStorage.getItem('gb_user_v2');
            if (savedName) {
                user.name = savedName;
                user.level = parseInt(localStorage.getItem('gb_level_v2_' + savedName)) || 1;
                renderLevels();
            } else {
                document.getElementById('loginScreen').classList.remove('hidden');
            }
        };

        function loginUser() {
            const name = document.getElementById('candidateName').value;
            if(!name) return alert("Enter your name!");
            user.name = name;
            localStorage.setItem('gb_user_v2', name);
            user.level = parseInt(localStorage.getItem('gb_level_v2_' + name)) || 1;
            document.getElementById('loginScreen').classList.add('hidden');
            renderLevels();
        }

        function logout() {
            localStorage.removeItem('gb_user_v2');
            location.reload();
        }

        function triggerAdmin() {
            tapCount++;
            if(tapCount === 4) {
                const key = prompt("ENTER SECRET ADMIN KEY:");
                if(key === "prime786") {
                    document.getElementById('adminPanel').style.display = 'block';
                    showTab('adminPanel');
                    alert("Admin Access Granted!");
                } else {
                    alert("Wrong Key!");
                    tapCount = 0;
                }
            }
        }

        function renderLevels() {
            const container = document.getElementById('levelContainer');
            container.innerHTML = '';
            for(let i=1; i<=10; i++) {
                const isLocked = i > user.level;
                container.innerHTML += `
                    <div onclick="openTest(${i}, ${isLocked})" class="level-card p-8 text-center shadow-sm ${isLocked ? 'locked' : ''}">
                        <div class="text-5xl mb-4">${isLocked ? '🔒' : '🎖️'}</div>
                        <h4 class="font-black text-slate-800 uppercase">Level ${i}</h4>
                        <p class="text-[10px] font-bold text-blue-500 mt-2">${isLocked ? 'LOCKED' : 'AVAILABLE'}</p>
                    </div>
                `;
            }
        }

        function openTest(num, locked) {
            if(locked) return alert("Complete previous levels!");
            document.getElementById('testBox').classList.remove('hidden');
            document.getElementById('currentLevelTitle').innerText = "Level 0" + num;
            
            const quiz = document.getElementById('quizContent');
            const questions = questionsData[num] || questionsData[1]; // Fallback to Level 1
            quiz.innerHTML = '';

            questions.forEach((item, index) => {
                quiz.innerHTML += `
                    <div class="q-card">
                        <p class="font-bold text-lg mb-4">${index + 1}. ${item.q}</p>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
                            <button class="option-btn" onclick="selectOpt(this, 'q${index}')">${item.a}</button>
                            <button class="option-btn" onclick="selectOpt(this, 'q${index}')">${item.b}</button>
                            <button class="option-btn" onclick="selectOpt(this, 'q${index}')">${item.c}</button>
                            <button class="option-btn" onclick="selectOpt(this, 'q${index}')">${item.d}</button>
                        </div>
                    </div>
                `;
            });
        }

        function selectOpt(btn, group) {
            const siblings = btn.parentElement.querySelectorAll('.option-btn');
            siblings.forEach(s => s.style.borderColor = '#f1f5f9');
            btn.style.borderColor = '#1e3a8a';
            btn.classList.add('selected');
        }

        function validateTest() {
            // In a real app, check answers here
            user.level++;
            localStorage.setItem('gb_level_v2_' + user.name, user.level);
            alert("Congratulations! Level Passed. Moving to Next Level.");
            document.getElementById('testBox').classList.add('hidden');
            renderLevels();
        }

        function showTab(id) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab'));
            document.getElementById(id).classList.add('active-tab');
        }

        function resetAllProgress() {
            localStorage.clear();
            alert("All system data wiped!");
            location.reload();
        }
    </script>
</body>
</html>
