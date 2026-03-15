<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GB Police Official Recruitment Portal | 2026</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700;900&display=swap');
        body { font-family: 'Montserrat', sans-serif; background: #f4f7f6; }
        .paper-sheet { background: white; border: 1px solid #d1d5db; box-shadow: 0 10px 30px rgba(0,0,0,0.1); }
        .watermark { position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%) rotate(-45deg); font-size: 8rem; color: rgba(0,0,0,0.03); z-index: -1; pointer-events: none; font-weight: 900; }
        .input-field { border-bottom: 2px solid #1e3a8a; background: transparent; outline: none; padding: 5px; }
        .option-label { display: block; padding: 12px; border: 1px solid #e5e7eb; border-radius: 8px; margin-top: 8px; cursor: pointer; transition: 0.2s; }
        .option-label:hover { background: #f0f7ff; border-color: #1e3a8a; }
        input[type="radio"]:checked + span { color: #1e3a8a; font-weight: 700; }
    </style>
</head>
<body class="p-2 md:p-10">

    <div class="watermark uppercase">GB Police</div>

    <div class="max-w-5xl mx-auto paper-sheet p-6 md:p-12 rounded-lg relative overflow-hidden">
        
        <div class="flex flex-col md:flex-row justify-between items-center border-b-4 border-double border-blue-900 pb-6 mb-8">
            <div class="text-center md:text-left mb-4 md:mb-0">
                <h1 class="text-2xl md:text-4xl font-black text-blue-900 uppercase">Government of Gilgit-Baltistan</h1>
                <h2 class="text-xl font-bold text-gray-700 uppercase">Police Department Recruitment Test - 2026</h2>
                <p class="text-sm font-bold bg-blue-100 text-blue-800 inline-block px-3 py-1 mt-2 rounded">Post: Constable (BPS-07)</p>
            </div>
            <div class="bg-blue-900 text-white p-4 rounded-xl text-center shadow-lg">
                <p class="text-xs uppercase font-bold tracking-widest">Time Remaining</p>
                <div id="clock" class="text-3xl font-mono font-black text-yellow-400">90:00</div>
            </div>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-10 bg-gray-50 p-6 rounded-xl border border-dashed border-gray-300">
            <div>
                <label class="block text-xs font-bold text-blue-900 uppercase">Candidate Name:</label>
                <input type="text" class="w-full input-field font-bold text-lg" placeholder="Enter Full Name">
            </div>
            <div>
                <label class="block text-xs font-bold text-blue-900 uppercase">Roll Number:</label>
                <input type="text" class="w-full input-field font-bold text-lg" placeholder="Enter Roll No.">
            </div>
            <div>
                <label class="block text-xs font-bold text-blue-900 uppercase">CNIC / B-Form:</label>
                <input type="text" class="w-full input-field font-bold text-lg" placeholder="00000-0000000-0">
            </div>
            <div>
                <label class="block text-xs font-bold text-blue-900 uppercase">District:</label>
                <select class="w-full input-field font-bold text-lg">
                    <option>Gilgit</option><option>Skardu</option><option>Diamer</option><option>Ghanche</option><option>Ghizer</option><option>Astore</option><option>Hunza</option><option>Nagar</option><option>Shigar</option><option>Kharmang</option>
                </select>
            </div>
        </div>

        <form id="mainExamForm">
            
            <div class="mb-10 text-sm text-gray-600 bg-yellow-50 p-4 rounded-lg border-l-4 border-yellow-400">
                <strong>Important Instructions:</strong>
                <ul class="list-disc ml-5 mt-2">
                    <li>Read each question carefully before selecting an answer.</li>
                    <li>Each correct answer carries 1 mark. There is no negative marking.</li>
                    <li>System will automatically submit the paper when time expires.</li>
                </ul>
            </div>

            <div class="mb-12">
                <h3 class="bg-blue-900 text-white px-4 py-2 inline-block rounded-r-full font-black uppercase text-sm mb-6">Part I: English Grammar</h3>
                <div class="space-y-8">
                    <div class="question">
                        <p class="font-bold text-gray-800">Q1. Choose the word opposite in meaning to 'ANONYMOUS':</p>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-3 mt-4">
                            <label class="option-label"><input type="radio" name="e1" value="A" class="mr-2"> <span>Hidden</span></label>
                            <label class="option-label"><input type="radio" name="e1" value="B" class="mr-2"> <span>Famous / Known</span></label>
                        </div>
                    </div>
                </div>
            </div>

            <div class="mb-12">
                <h3 class="bg-green-700 text-white px-4 py-2 inline-block rounded-r-full font-black uppercase text-sm mb-6">Part II: General Knowledge (GB Special)</h3>
                <div class="space-y-8">
                    <div class="question">
                        <p class="font-bold text-gray-800">Q2. Which pass connects Gilgit-Baltistan with Xinjiang, China?</p>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-3 mt-4">
                            <label class="option-label"><input type="radio" name="g1" value="A" class="mr-2"> <span>Khyber Pass</span></label>
                            <label class="option-label"><input type="radio" name="g1" value="B" class="mr-2"> <span>Khunjerab Pass</span></label>
                        </div>
                    </div>
                </div>
            </div>

            <div class="border-t-4 border-gray-100 pt-10 text-center">
                <button type="button" onclick="calculateFinalResult()" class="bg-blue-900 hover:bg-black text-white px-12 py-5 rounded-2xl font-black text-2xl shadow-2xl transition transform hover:scale-105 uppercase tracking-tighter">
                    Final Submission
                </button>
            </div>

        </form>
    </div>

    <div id="resultPortal" class="hidden fixed inset-0 bg-blue-900/90 backdrop-blur-2xl z-[100] flex items-center justify-center p-4">
        <div class="bg-white rounded-[2rem] p-8 md:p-12 max-w-lg w-full text-center shadow-inner">
            <h2 class="text-2xl font-black text-gray-900 uppercase">Evaluation Report</h2>
            <div id="finalScore" class="text-8xl font-black text-blue-700 my-8">0%</div>
            <p id="gradeMsg" class="text-xl font-bold mb-10 text-gray-600"></p>
            <div class="flex gap-4">
                <button onclick="location.reload()" class="flex-1 bg-gray-900 text-white py-4 rounded-2xl font-bold">Retake Exam</button>
                <button onclick="window.print()" class="flex-1 bg-blue-600 text-white py-4 rounded-2xl font-bold">Print Result</button>
            </div>
        </div>
    </div>

    <script>
        // Official Answer Database
        const keys = { e1: "B", g1: "B" };

        function calculateFinalResult() {
            let correct = 0;
            const total = Object.keys(keys).length;
            const form = new FormData(document.getElementById('mainExamForm'));

            for(let key in keys) {
                if(form.get(key) === keys[key]) correct++;
            }

            const percentage = Math.round((correct / total) * 100);
            document.getElementById('finalScore').innerText = percentage + "%";
            document.getElementById('gradeMsg').innerText = percentage >= 50 ? "Status: QUALIFIED" : "Status: NOT QUALIFIED";
            document.getElementById('resultPortal').classList.remove('hidden');
        }

        // Exam Timer Logic
        let totalSeconds = 5400;
        setInterval(() => {
            if(totalSeconds <= 0) { calculateFinalResult(); return; }
            totalSeconds--;
            let m = Math.floor(totalSeconds / 60);
            let s = totalSeconds % 60;
            document.getElementById('clock').innerText = `${m}:${s < 10 ? '0' : ''}${s}`;
        }, 1000);
    </script>
</body>
</html>
