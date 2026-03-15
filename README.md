<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GB Police Academy | Advanced Training Portal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f8fafc; }
        .mode-card { cursor: pointer; transition: 0.4s; border: 2px solid transparent; }
        .mode-card:hover { transform: translateY(-10px); border-color: #1e3a8a; }
        .q-card { background: white; border-radius: 15px; padding: 25px; margin-bottom: 20px; box-shadow: 0 4px 6px rgba(0,0,0,0.02); border-left: 5px solid #1e3a8a; }
        .option-item { display: block; padding: 12px; border: 1.5px solid #e2e8f0; border-radius: 10px; margin-top: 10px; cursor: pointer; transition: 0.2s; }
        .option-item:hover { background: #f0f7ff; }
        .correct { background: #dcfce7 !important; border-color: #22c55e !important; }
        .wrong { background: #fee2e2 !important; border-color: #ef4444 !important; }
    </style>
</head>
<body class="pb-20">

    <div id="welcomeScreen" class="fixed inset-0 bg-blue-900 z-[100] flex items-center justify-center p-6 text-white">
        <div class="max-w-4xl w-full text-center">
            <h1 class="text-4xl md:text-6xl font-black mb-4 italic tracking-tighter">GB POLICE TRAINING HUB</h1>
            <p class="text-blue-300 mb-12 font-bold uppercase tracking-widest text-sm">Select Your Training Module, Sweetie</p>
            
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <div onclick="startModule('exam')" class="mode-card bg-white text-blue-900 p-8 rounded-[30px] shadow-2xl">
                    <div class="text-5xl mb-4">⏱️</div>
                    <h2 class="text-2xl font-black uppercase">Official Exam</h2>
                    <p class="text-gray-500 text-sm mt-2">100 Marks | 90 Mins | Timer Mode</p>
                </div>
                <div onclick="startModule('practice')" class="mode-card bg-blue-800 text-white p-8 rounded-[30px] shadow-2xl border-blue-400 border-2">
                    <div class="text-5xl mb-4">📖</div>
                    <h2 class="text-2xl font-black uppercase">Preparation Mode</h2>
                    <p class="text-blue-200 text-sm mt-2">No Timer | Instant Answers | Unlimited Study</p>
                </div>
            </div>
        </div>
    </div>

    <nav class="bg-white shadow-md p-4 sticky top-0 z-50">
        <div class="max-w-5xl mx-auto flex justify-between items-center">
            <h1 class="font-black text-blue-900 text-xl italic uppercase">GB Police Portal</h1>
            <div id="timerContainer" class="hidden bg-red-600 text-white px-5 py-2 rounded-full font-black font-mono">
                90:00
            </div>
        </div>
    </nav>

    <main class="max-w-4xl mx-auto p-4 mt-8">
        <div id="questionContainer">
            </div>

        <button id="submitBtn" onclick="finishTest()" class="w-full bg-blue-900 text-white py-6 rounded-2xl font-black text-2xl shadow-xl hover:bg-black mt-10 uppercase">
            Complete Training
        </button>
    </main>

    <script>
        const questionBank = [
            { q: "Choose the correct spelling:", a: "Lieutenant", b: "Leftenant", ans: "a", cat: "English" },
            { q: "K2 is also known as:", a: "Everest", b: "Godwin-Austen", ans: "b", cat: "GB GK" },
            { q: "Urdu k pehle shayar kon hain?", a: "Amir Khusro", b: "Mir Taqi Mir", ans: "a", cat: "Urdu" },
            { q: "First Ghazwa of Islam?", a: "Badr", b: "Abwa", ans: "b", cat: "Islamiyat" },
            { q: "Solve: 10 + 20 * 2", a: "60", b: "50", ans: "b", cat: "Math" },
            { q: "Capital of Gilgit Baltistan?", a: "Skardu", b: "Gilgit", ans: "b", cat: "GB GK" },
            { q: "Synonym of 'Fast'?", a: "Quick", b: "Slow", ans: "a", cat: "English" }
            // Yahan aap jitne chahe sawalat add kar sakte hain
        ];

        let currentMode = '';
        let timerInterval;

        function startModule(mode) {
            currentMode = mode;
            document.getElementById('welcomeScreen').style.display = 'none';
            if(mode === 'exam') {
                document.getElementById('timerContainer').classList.remove('hidden');
                startTimer(5400);
            }
            renderQuestions();
        }

        function renderQuestions() {
            // Shuffle/Randomize
            const shuffled = questionBank.sort(() => 0.5 - Math.random());
            const container = document.getElementById('questionContainer');
            container.innerHTML = '';

            shuffled.forEach((item, index) => {
                const div = document.createElement('div');
                div.className = 'q-card';
                div.innerHTML = `
                    <span class="text-[10px] bg-blue-100 text-blue-800 px-3 py-1 rounded-full font-bold uppercase">${item.cat}</span>
                    <p class="text-lg font-bold text-gray-800 mt-3">Q${index+1}. ${item.q}</p>
                    <div class="mt-4 space-y-3">
                        <label class="option-item" id="q${index}a">
                            <input type="radio" name="q${index}" value="a" onchange="checkInstant(this, '${item.ans}', 'q${index}a')"> ${item.a}
                        </label>
                        <label class="option-item" id="q${index}b">
                            <input type="radio" name="q${index}" value="b" onchange="checkInstant(this, '${item.ans}', 'q${index}b')"> ${item.b}
                        </label>
                    </div>
                `;
                container.appendChild(div);
            });
        }

        function checkInstant(input, correctAns, elementId) {
            if(currentMode === 'practice') {
                const parent = document.getElementById(elementId);
                if(input.value === correctAns) {
                    parent.classList.add('correct');
                } else {
                    parent.classList.add('wrong');
                }
            }
        }

        function startTimer(duration) {
            let timer = duration, minutes, seconds;
            timerInterval = setInterval(() => {
                minutes = parseInt(timer / 60, 10);
                seconds = parseInt(timer % 60, 10);
                document.getElementById('timerContainer').innerText = `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
                if (--timer < 0) finishTest();
            }, 1000);
        }

        function finishTest() {
            alert("Training Module Completed! Check your result.");
            location.reload();
        }
    </script>
</body>
</html>
