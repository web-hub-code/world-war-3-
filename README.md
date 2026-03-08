<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <title>PakGold | SkyNode Professional</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap');
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: #06090f; color: #f0f6fc; padding-bottom: 100px; overflow-x: hidden; }
        .sky-card { background: #0d1117; border: 1px solid #30363d; border-radius: 24px; transition: 0.3s; }
        .page { display: none; animation: fadeIn 0.4s ease; }
        .active-page { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .nav-active { color: #fbbf24 !important; border-top: 2px solid #fbbf24; }
        .btn-modern { background: linear-gradient(135deg, #fbbf24, #d97706); color: #000; font-weight: 800; border-radius: 16px; transition: 0.2s; }
        .btn-modern:active { transform: scale(0.95); }
        ::-webkit-scrollbar { display: none; }
    </style>
</head>
<body>

    <header class="p-6 sticky top-0 bg-[#06090f]/80 backdrop-blur-xl z-[1000]">
        <div class="flex justify-between items-center mb-4">
            <h1 class="text-xl font-black text-yellow-500 italic uppercase">PakGold</h1>
            <div class="flex gap-2">
                <div class="bg-white/5 p-2 rounded-xl text-[10px] font-bold">VIP 1</div>
                <button onclick="logout()" class="bg-red-500/10 text-red-500 p-2 rounded-xl text-[10px] font-bold uppercase">Logout</button>
            </div>
        </div>
        <div class="sky-card p-6 bg-gradient-to-br from-[#1c2128] to-[#0d1117]">
            <p class="text-[9px] font-bold opacity-50 tracking-widest uppercase">Total Assets</p>
            <h2 class="text-3xl font-black mt-1" id="v-bal">₨ 0.00</h2>
            <div class="grid grid-cols-2 gap-4 mt-6 border-t border-white/5 pt-4">
                <button onclick="showPage('recharge')" class="p-3 bg-yellow-500 text-black rounded-xl text-[10px] font-black uppercase">Deposit</button>
                <button onclick="showPage('withdraw')" class="p-3 bg-white/5 rounded-xl text-[10px] font-black uppercase border border-white/10">Withdraw</button>
            </div>
        </div>
    </header>

    <main class="p-4">

        <div id="p-home" class="page active-page space-y-4">
            <div class="flex gap-2 p-1 bg-white/5 rounded-2xl mb-4">
                <button onclick="renderNodes('std')" id="btn-std" class="flex-1 py-3 text-[10px] font-black uppercase rounded-xl bg-yellow-500 text-black">Machines</button>
                <button onclick="renderNodes('vip')" id="btn-vip" class="flex-1 py-3 text-[10px] font-black uppercase rounded-xl opacity-40">VIP Nodes</button>
            </div>
            <div id="machine-grid" class="space-y-4"></div>
        </div>

        <div id="p-history" class="page space-y-4">
            <h3 class="text-xs font-black uppercase text-yellow-500 ml-2">Mining Logs</h3>
            <div class="sky-card p-4 text-[10px] font-bold opacity-60 text-center">No transactions found yet.</div>
        </div>

        <div id="p-recharge" class="page space-y-4">
            <div class="sky-card p-6">
                <h3 class="text-yellow-500 font-black text-xs uppercase mb-4">Fast Deposit</h3>
                <div class="space-y-2 text-[11px] font-bold mb-6">
                    <div class="flex justify-between p-4 bg-white/5 rounded-2xl border-l-4 border-yellow-500"><span>EasyPaisa</span><span>03379827882</span></div>
                    <div class="flex justify-between p-4 bg-white/5 rounded-2xl border-l-4 border-yellow-600"><span>JazzCash</span><span>03705519562</span></div>
                </div>
                <input type="number" id="dep-amt" placeholder="Deposit Amount (₨ 200+)" class="w-full p-4 bg-black border border-[#30363d] rounded-xl mb-3 outline-none">
                <input type="text" id="dep-tid" placeholder="Transaction ID" class="w-full p-4 bg-black border border-[#30363d] rounded-xl mb-4 outline-none">
                <button class="w-full p-4 btn-modern uppercase text-[10px] tracking-widest">Confirm Payment ⚡</button>
            </div>
        </div>

        <div id="p-about" class="page space-y-4">
            <div class="sky-card p-6 space-y-6 text-[11px] leading-relaxed">
                <section>
                    <h4 class="text-yellow-500 font-black uppercase mb-2">Company Details</h4>
                    <p class="opacity-70">PakGold is powered by Prime Solutions. We provide cloud mining infrastructure with high-yield nodes across Asia.</p>
                </section>
                <section>
                    <h4 class="text-yellow-500 font-black uppercase mb-2">Privacy Policy</h4>
                    <p class="opacity-70">Your assets are protected by Firebase encryption. Data is never shared with third parties. Withdrawals are processed 24/7.</p>
                </section>
                <section>
                    <h4 class="text-yellow-500 font-black uppercase mb-2">Contact Us</h4>
                    <p class="opacity-70">Support: webhub262@gmail.com<br>WhatsApp: Join Official Group via Dashboard</p>
                </section>
            </div>
        </div>

    </main>

    <nav class="fixed bottom-0 w-full p-4 flex justify-around items-center bg-[#0d1117]/95 backdrop-blur-xl border-t border-white/5 rounded-t-[2.5rem] z-[5000]">
        <button onclick="showPage('home')" id="n-home" class="flex flex-col items-center gap-1 nav-active pt-2">
            <span class="text-xl">⚡</span><span class="text-[7px] font-black uppercase">Nodes</span>
        </button>
        <button onclick="showPage('history')" id="n-history" class="flex flex-col items-center gap-1 opacity-40 pt-2">
            <span class="text-xl">📊</span><span class="text-[7px] font-black uppercase">History</span>
        </button>
        <button onclick="showPage('recharge')" id="n-recharge" class="flex flex-col items-center gap-1 opacity-40 pt-2">
            <span class="text-xl">📥</span><span class="text-[7px] font-black uppercase">Fund</span>
        </button>
        <button onclick="showPage('about')" id="n-about" class="flex flex-col items-center gap-1 opacity-40 pt-2">
            <span class="text-xl">🏢</span><span class="text-[7px] font-black uppercase">Policy</span>
        </button>
    </nav>

    <script>
        // --- MACHINE LOGIC (200 Start, 15+5) ---
        const STDS = [];
        for(let i=1; i<=15; i++) {
            let cost = 200 + (i-1)*1500;
            STDS.push({ id: `NODE-S${i}`, cost, daily: (cost*0.1).toFixed(0) });
        }
        const VIPS = [
            { id: "VIP-GOLD 1", cost: 50000, daily: 6000 },
            { id: "VIP-DIAMOND 2", cost: 100000, daily: 13500 },
            { id: "VIP-PLATINUM 3", cost: 250000, daily: 35000 },
            { id: "VIP-MASTER 4", cost: 500000, daily: 80000 },
            { id: "VIP-ULTRA 5", cost: 1000000, daily: 180000 }
        ];

        function renderNodes(type) {
            const grid = document.getElementById('machine-grid');
            const data = type === 'std' ? STDS : VIPS;
            grid.innerHTML = '';
            data.forEach(n => {
                grid.innerHTML += `
                <div class="sky-card p-5 border-l-4 ${type==='std'?'border-yellow-500':'border-green-500'}">
                    <div class="flex justify-between items-center mb-4">
                        <span class="text-[9px] font-black uppercase text-yellow-500 tracking-widest">${n.id}</span>
                        <span class="text-[7px] font-bold opacity-40">30 DAYS TERM</span>
                    </div>
                    <div class="flex justify-between items-end">
                        <div><p class="text-[7px] opacity-40 uppercase">Daily Yield</p><p class="text-sm font-black text-green-500">₨ ${n.daily}</p></div>
                        <button class="px-6 py-2 btn-modern text-[9px] uppercase">Rent ₨ ${n.cost.toLocaleString()}</button>
                    </div>
                </div>`;
            });
            // Tab Toggle
            document.getElementById('btn-std').classList.toggle('bg-yellow-500', type==='std');
            document.getElementById('btn-std').classList.toggle('text-black', type==='std');
            document.getElementById('btn-std').classList.toggle('opacity-40', type!=='std');
            document.getElementById('btn-vip').classList.toggle('bg-yellow-500', type==='vip');
            document.getElementById('btn-vip').classList.toggle('text-black', type==='vip');
            document.getElementById('btn-vip').classList.toggle('opacity-40', type!=='vip');
        }

        function showPage(p) {
            document.querySelectorAll('.page').forEach(pg => pg.classList.remove('active-page'));
            document.getElementById('p-'+p).classList.add('active-page');
            document.querySelectorAll('nav button').forEach(b => b.classList.remove('nav-active', 'opacity-100'));
            document.querySelectorAll('nav button').forEach(b => b.classList.add('opacity-40'));
            document.getElementById('n-'+p).classList.add('nav-active', 'opacity-100');
            document.getElementById('n-'+p).classList.remove('opacity-40');
        }

        function logout() { localStorage.clear(); location.reload(); }
        window.onload = () => { renderNodes('std'); }
    </script>
</body>
</html>
