<html lang="en" id="main-html" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <title>MintCrest Gold | Premium Node</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap');
        :root { --bg: #06090f; --card: #0d1117; --accent: #fbbf24; --border: #30363d; }
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: #f0f6fc; padding-bottom: 100px; overflow-x: hidden; }
        .glass { background: rgba(13, 17, 23, 0.8); backdrop-filter: blur(15px); border: 1px solid var(--border); border-radius: 28px; }
        .page { display: none; animation: slideUp 0.4s ease forwards; }
        .active-page { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .btn-premium { background: linear-gradient(135deg, #fbbf24, #d97706); color: #000; font-weight: 800; border-radius: 18px; transition: 0.2s; shadow: 0 10px 20px rgba(217, 119, 6, 0.2); }
        input { background: #161b22; border: 1px solid var(--border); color: white; padding: 14px; border-radius: 16px; width: 100%; outline: none; }
        .nav-active { color: var(--accent) !important; filter: drop-shadow(0 0 8px var(--accent)); opacity: 1 !important; }
        marquee { background: rgba(251, 191, 36, 0.05); color: var(--accent); font-size: 10px; font-weight: 700; padding: 8px 0; border-bottom: 1px solid rgba(251, 191, 36, 0.1); }
    </style>
</head>
<body>

    <marquee id="v-news" class="sticky top-0 z-[6000]">Welcome to MintCrest Gold! Minimum Deposit ₨ 200 | Secure Node Active... 😘</marquee>

    <header class="p-6 flex justify-between items-center sticky top-0 bg-[#06090f]/80 backdrop-blur-md z-[5000]">
        <div onclick="adminTap()">
            <h1 class="text-xl font-black italic text-yellow-500 tracking-tighter uppercase">MINTCREST <span class="text-white">GOLD</span></h1>
            <p id="display-user" class="text-[8px] font-bold opacity-40 uppercase tracking-[2px]">Initialize...</p>
        </div>
        <div class="text-right">
            <p class="text-[8px] font-black opacity-40 uppercase">Yield Assets</p>
            <h2 id="top-bal" class="text-2xl font-black tracking-tighter text-white">₨ 0.00</h2>
        </div>
    </header>

    <main class="p-4 space-y-6">

        <div id="p-home" class="page active-page space-y-6">
            <div class="glass p-6 border-l-4 border-yellow-500 flex justify-between items-center">
                <div>
                    <p class="text-[9px] font-black opacity-50 uppercase tracking-widest">Profit Countdown</p>
                    <h3 id="timer" class="text-2xl font-black text-yellow-500 tracking-tighter">23:59:59</h3>
                </div>
                <div class="w-12 h-12 rounded-full bg-yellow-500/10 flex items-center justify-center animate-pulse text-xl">⚡</div>
            </div>

            <div class="glass p-5 flex gap-2">
                <input type="text" id="promo-input" placeholder="PROMO CODE" class="!p-2 text-center font-bold uppercase">
                <button onclick="applyPromo()" class="btn-premium px-8 text-[10px] uppercase">Claim</button>
            </div>

            <div class="grid grid-cols-2 gap-4">
                <button onclick="showPage('wallet')" class="glass p-5 text-center font-bold text-[11px] uppercase border-b-2 border-green-500">Recharge</button>
                <button onclick="showPage('withdraw')" class="glass p-5 text-center font-bold text-[11px] uppercase border-b-2 border-red-500">Withdraw</button>
            </div>
        </div>

        <div id="p-nodes" class="page space-y-4">
            <div class="flex gap-2 p-1 bg-white/5 rounded-2xl mb-4">
                <button onclick="renderNodes('normal')" id="btn-n" class="flex-1 p-3 rounded-xl bg-yellow-500 text-black text-[9px] font-black uppercase">Standard (20)</button>
                <button onclick="renderNodes('vip')" id="btn-v" class="flex-1 p-3 rounded-xl text-[9px] font-black uppercase opacity-40">VIP Offers (5)</button>
            </div>
            <div id="nodes-grid" class="space-y-4"></div>
        </div>

        <div id="p-wallet" class="page space-y-4">
            <div class="glass p-8">
                <h3 class="text-xs font-black uppercase text-yellow-500 mb-6">Deposit Infrastructure</h3>
                <div class="space-y-3 mb-6">
                    <div class="flex justify-between p-4 bg-white/5 rounded-2xl border-l-4 border-yellow-500">
                        <span class="text-[10px] font-bold opacity-60 uppercase">EasyPaisa/JazzCash</span>
                        <span class="text-sm font-black text-white">03379827882</span>
                    </div>
                </div>
                <input type="number" id="dep-amt" placeholder="Amount (PKR)" class="mb-3">
                <input type="text" id="dep-tid" placeholder="Transaction ID (TID)" class="mb-4">
                <p class="text-[8px] font-black opacity-40 uppercase mb-2 ml-1">Upload Receipt Screenshot:</p>
                <input type="file" id="dep-file" class="mb-6 !p-2 text-[10px]">
                <button onclick="submitDep()" class="w-full p-4 btn-premium uppercase text-[11px] tracking-widest">Verify Deposit ⚡</button>
            </div>
        </div>

        <div id="p-history" class="page space-y-4">
            <div class="flex justify-between items-center px-1">
                <h3 class="text-xs font-black uppercase opacity-50 italic">Activity Logs</h3>
                <button onclick="clearHistory()" class="text-[9px] font-black text-red-500 uppercase tracking-widest">Clear Records</button>
            </div>
            <div id="history-list" class="space-y-3"></div>
        </div>

        <div id="p-menu" class="page space-y-4">
            <div class="glass p-6 text-center">
                <p class="text-[8px] font-black opacity-40 uppercase mb-2">My Referral Link</p>
                <p id="ref-link" class="text-[10px] text-blue-400 break-all p-3 bg-white/5 rounded-2xl border border-dashed border-white/10">Loading...</p>
                <button onclick="copyRef()" class="mt-4 text-[9px] font-black uppercase text-yellow-500">Copy Link</button>
            </div>
            <button class="w-full glass p-5 text-left text-[11px] font-bold uppercase">Company Whitepaper</button>
            <button class="w-full glass p-5 text-left text-[11px] font-bold uppercase">Privacy Protocols</button>
            <button onclick="window.location.href='https://wa.me/923705519562'" class="w-full glass p-5 text-left text-[11px] font-bold uppercase text-green-500">Support Desk (Admin)</button>
            <button onclick="logout()" class="w-full p-4 bg-red-500/10 text-red-500 rounded-2xl font-black uppercase text-[10px] mt-10">System Logout</button>
        </div>

    </main>

    <nav class="fixed bottom-0 w-full h-22 glass rounded-t-[3rem] border-t border-white/5 flex justify-around items-center px-6 z-[10000]">
        <button onclick="showPage('home')" id="n-home" class="nav-active flex flex-col items-center gap-1"><span class="text-xl">🏠</span><span class="text-[7px] font-black uppercase">Home</span></button>
        <button onclick="showPage('nodes')" id="n-nodes" class="opacity-40 flex flex-col items-center gap-1"><span class="text-xl">⚡</span><span class="text-[7px] font-black uppercase">Nodes</span></button>
        <button onclick="showPage('history')" id="n-history" class="opacity-40 flex flex-col items-center gap-1"><span class="text-xl">📜</span><span class="text-[7px] font-black uppercase">Logs</span></button>
        <button onclick="showPage('menu')" id="n-menu" class="opacity-40 flex flex-col items-center gap-1"><span class="text-xl">👤</span><span class="text-[7px] font-black uppercase">Menu</span></button>
    </nav>

    <script>
        // --- FIREBASE INITIALIZE ---
        const firebaseConfig = { apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg", authDomain: "investment-84f4e.firebaseapp.com", projectId: "investment-84f4e", storageBucket: "investment-84f4e.appspot.com", messagingSenderId: "975293889308", appId: "1:975293889308:web:6d034a99cc966c75ff58d9" };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();
        let currentUser = localStorage.getItem('mc_user');

        // --- 25 NODES RENDERER ---
        function renderNodes(type) {
            const grid = document.getElementById('nodes-grid');
            grid.innerHTML = '';
            const btnN = document.getElementById('btn-n'); const btnV = document.getElementById('btn-v');
            
            if(type === 'normal') {
                btnN.className = "flex-1 p-3 rounded-xl bg-yellow-500 text-black text-[9px] font-black uppercase";
                btnV.className = "flex-1 p-3 rounded-xl text-[9px] font-black uppercase opacity-40";
                for(let i=1; i<=20; i++) {
                    let cost = 200 + (i*900);
                    grid.innerHTML += `<div class="glass p-6 flex justify-between items-center border-r-4 border-yellow-500"><div><p class="text-[8px] font-black text-yellow-500 uppercase">S-Node v0${i}</p><h4 class="text-xl font-black italic">₨ ${cost.toLocaleString()}</h4></div><button class="px-6 py-3 btn-premium text-[9px] uppercase">Rent</button></div>`;
                }
            } else {
                btnV.className = "flex-1 p-3 rounded-xl bg-yellow-500 text-black text-[9px] font-black uppercase";
                btnN.className = "flex-1 p-3 rounded-xl text-[9px] font-black uppercase opacity-40";
                const vips = [50000, 100000, 250000, 500000, 1000000];
                vips.forEach((v, idx) => {
                    grid.innerHTML += `<div class="glass p-6 flex justify-between items-center border-r-4 border-green-500"><div><p class="text-[8px] font-black text-green-500 uppercase">VIP Offer 0${idx+1}</p><h4 class="text-xl font-black italic">₨ ${v.toLocaleString()}</h4></div><button class="px-6 py-3 btn-premium text-[9px] uppercase">Claim</button></div>`;
                });
            }
        }

        // --- DEPOSIT WITH PROOF ---
        async function submitDep() {
            const amt = document.getElementById('dep-amt').value;
            const tid = document.getElementById('dep-tid').value;
            const file = document.getElementById('dep-file').files[0];
            if(!amt || !tid || !file) return alert("Fill all details & attach receipt! 😘");
            const reader = new FileReader(); reader.readAsDataURL(file);
            reader.onload = async () => {
                await db.collection("requests").add({ user: currentUser, type: 'deposit', amount: parseFloat(amt), tid, proof: reader.result, status: 'pending', time: Date.now() });
                alert("Request Sent! Admin will verify soon. 😘");
                showPage('home');
            };
        }

        // --- CORE SYNC ---
        function initApp() {
            if(currentUser) {
                document.getElementById('display-user').innerText = "@" + currentUser.toUpperCase();
                document.getElementById('ref-link').innerText = window.location.origin + "?ref=" + currentUser;
                db.collection("users").doc(currentUser).onSnapshot(d => {
                    document.getElementById('top-bal').innerText = "₨ " + (d.data()?.balance || 0).toLocaleString();
                });
                renderNodes('normal');
                startCycleTimer();
                loadHistory();
            } else {
                let n = prompt("Enter Username to Login:");
                if(n) { localStorage.setItem('mc_user', n.toLowerCase()); location.reload(); }
            }
        }

        function startCycleTimer() {
            setInterval(() => {
                let now = new Date();
                let h = 23 - now.getHours(); let m = 59 - now.getMinutes(); let s = 59 - now.getSeconds();
                document.getElementById('timer').innerText = `${h < 10 ? '0'+h : h}:${m < 10 ? '0'+m : m}:${s < 10 ? '0'+s : s}`;
            }, 1000);
        }

        function showPage(p) {
            document.querySelectorAll('.page').forEach(pg => pg.classList.remove('active-page'));
            document.getElementById('p-'+p).classList.add('active-page');
            document.querySelectorAll('nav button').forEach(b => b.classList.add('opacity-40'));
            document.querySelectorAll('nav button').forEach(b => b.classList.remove('nav-active'));
            document.getElementById('n-'+p)?.classList.add('nav-active');
            document.getElementById('n-'+p)?.classList.remove('opacity-40');
        }

        function logout() { localStorage.clear(); location.reload(); }
        window.onload = initApp;
    </script>
</body>
</html>
