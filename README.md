<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>ApexDaily | Premium Assets</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap');
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: #020617; color: white; overflow-x: hidden; }
        
        /* Vibrant Colors & Glassmorphism */
        .glass { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(20px); border: 1px solid rgba(255,255,255,0.08); }
        .neon-card { background: linear-gradient(135deg, #0ea5e9, #a855f7, #ec4899); box-shadow: 0 20px 50px rgba(168, 85, 247, 0.3); }
        .btn-vibrant { background: linear-gradient(90deg, #3b82f6, #d946ef); transition: 0.3s; box-shadow: 0 10px 20px rgba(59, 130, 246, 0.2); }
        .active-tab { color: #0ea5e9; transform: translateY(-5px); }
        
        .page { display: none; animation: slideUp 0.4s ease-out; }
        .active-page { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }

        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 0px; }
    </style>
</head>
<body class="h-screen flex flex-col">

    <section id="auth-ui" class="fixed inset-0 z-[1000] bg-[#020617] flex items-center justify-center p-8 text-center">
        <div class="w-full max-w-sm">
            <h1 onclick="adminTap()" class="text-5xl font-black italic tracking-tighter mb-2 cursor-pointer bg-clip-text text-transparent bg-gradient-to-r from-cyan-400 to-fuchsia-500">APEXDAILY</h1>
            <p class="text-gray-500 text-[8px] uppercase tracking-[0.4em] mb-12 font-bold">Your Personal Asset Vault</p>
            
            <div id="phone-box" class="glass p-10 rounded-[3.5rem] border-t-2 border-cyan-500 shadow-2xl space-y-5">
                <input type="text" id="user-name" placeholder="Full Name" class="w-full bg-white/5 p-5 rounded-2xl border border-white/10 outline-none text-center font-bold focus:border-cyan-500 transition-all">
                <div class="flex gap-2">
                    <select id="country-code" class="bg-white/5 border border-white/10 rounded-2xl p-4 text-xs font-bold outline-none">
                        <option value="+92">+92</option>
                        <option value="+971">+971</option>
                    </select>
                    <input type="number" id="phone-num" placeholder="3001234567" class="flex-1 bg-white/5 p-5 rounded-2xl border border-white/10 outline-none text-center font-bold focus:border-cyan-500">
                </div>
                <button onclick="mockLogin()" class="w-full btn-vibrant py-5 rounded-2xl font-black text-[10px] uppercase tracking-widest active:scale-95 transition-all">Request Access</button>
            </div>
        </div>
    </section>

    <main id="app-ui" class="hidden flex-1 overflow-y-auto pb-32">
        
        <div id="p-home" class="page active-page p-6">
            <div class="neon-card p-10 rounded-[3rem] mb-8 relative overflow-hidden">
                <p class="text-[9px] text-white/80 font-extrabold mb-1 uppercase tracking-widest">Available Capital</p>
                <h2 class="text-5xl font-black tracking-tighter" id="v-bal">₨ 0</h2>
                <div id="countdown-display" class="mt-4 text-[9px] font-black text-white/60 uppercase italic">Syncing Profits...</div>
                <div class="mt-6 flex gap-3">
                    <span class="text-[8px] bg-white/20 px-4 py-2 rounded-full font-bold uppercase border border-white/20">Profit: <span id="v-profit">₨ 0</span></span>
                    <span id="tier-tag" class="text-[8px] bg-black/20 px-4 py-2 rounded-full font-bold uppercase italic border border-white/10">No Fleet</span>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-8">
                <div onclick="changePage('wallet')" class="glass p-8 rounded-[2.5rem] text-center border-b-4 border-cyan-500 active:scale-95 transition-transform">
                    <i class="fa-solid fa-circle-arrow-down text-2xl text-cyan-400"></i>
                    <span class="text-[10px] font-black block mt-3 uppercase tracking-widest">Deposit</span>
                </div>
                <div onclick="changePage('withdraw')" class="glass p-8 rounded-[2.5rem] text-center border-b-4 border-fuchsia-500 active:scale-95 transition-transform">
                    <i class="fa-solid fa-circle-arrow-up text-2xl text-fuchsia-400"></i>
                    <span class="text-[10px] font-black block mt-3 uppercase tracking-widest">Withdraw</span>
                </div>
            </div>

            <h3 class="text-[10px] font-black text-gray-500 uppercase tracking-widest mb-4 ml-2">Popular Fleets</h3>
            <div id="quick-plans" class="space-y-3"></div>
        </div>

        <div id="p-invest" class="page p-6">
            <h2 class="text-center font-black italic mb-6 uppercase text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 to-blue-500 text-xl tracking-widest">Trading Fleet</h2>
            <div id="plans-list" class="space-y-3 pb-10"></div>
        </div>

        <div id="p-wallet" class="page p-6">
            <div class="glass p-10 rounded-[3.5rem] text-center border-t-8 border-cyan-500 shadow-2xl">
                <h3 class="font-black text-cyan-400 mb-8 uppercase text-sm italic tracking-widest">Capital Funding</h3>
                <div class="space-y-3 mb-10 text-[10px] font-black">
                    <div class="glass p-5 rounded-2xl flex justify-between bg-cyan-500/10"><span>JAZZCASH/SADAPAY</span><span class="text-cyan-400">03705519562</span></div>
                    <div class="glass p-5 rounded-2xl flex justify-between bg-green-500/10"><span>EASYPAISA</span><span class="text-green-400">03379827882</span></div>
                </div>
                <input type="number" id="dep-amount" placeholder="Amount (PKR)" class="w-full bg-white/5 p-5 rounded-2xl mb-3 text-center font-bold outline-none border border-white/5">
                <input type="text" id="dep-trx" placeholder="Transaction ID (TID)" class="w-full bg-white/5 p-5 rounded-2xl mb-8 text-center font-bold uppercase outline-none border border-white/5">
                <button id="dep-btn" onclick="submitDeposit()" class="w-full btn-vibrant py-6 rounded-3xl font-black text-[10px] uppercase tracking-widest">Verify Funding</button>
            </div>
        </div>

        <div id="p-withdraw" class="page p-6">
            <div class="glass p-10 rounded-[3.5rem] border-t-8 border-fuchsia-600 text-center shadow-2xl">
                <h3 class="font-black text-fuchsia-500 mb-8 uppercase text-sm italic">Request Payout</h3>
                <input type="number" id="wd-amt" placeholder="Amount (PKR)" class="w-full bg-white/5 p-5 rounded-2xl mb-3 text-center font-bold border border-white/5">
                <input type="text" id="wd-acc" placeholder="Account Name & Number" class="w-full bg-white/5 p-5 rounded-2xl mb-8 text-center text-[9px] font-bold border border-white/5">
                <button onclick="submitWithdraw()" class="w-full bg-gradient-to-r from-fuchsia-600 to-pink-600 py-6 rounded-3xl font-black text-[10px] uppercase shadow-lg">Authorize Payout</button>
            </div>
        </div>

        <div id="p-activity" class="page p-6">
            <h2 class="text-center font-black italic mb-8 uppercase text-gray-500 tracking-widest">Security Ledger</h2>
            <div id="user-history" class="space-y-3 pb-10"></div>
        </div>

    </main>

    <nav id="bottom-nav" class="hidden glass border-t border-white/5 p-6 flex justify-around items-center fixed bottom-0 left-0 w-full z-[200] rounded-t-[3.5rem] shadow-2xl">
        <button onclick="changePage('home')" id="n-home" class="flex flex-col items-center gap-1 active-tab"><i class="fa-solid fa-house-chimney text-xl"></i><span class="text-[8px] font-black uppercase mt-1">Vault</span></button>
        <button onclick="changePage('invest')" id="n-invest" class="flex flex-col items-center gap-1 text-gray-500"><i class="fa-solid fa-chart-line text-xl"></i><span class="text-[8px] font-black uppercase mt-1">Fleet</span></button>
        <button onclick="changePage('activity')" id="n-activity" class="flex flex-col items-center gap-1 text-gray-500"><i class="fa-solid fa-receipt text-xl"></i><span class="text-[8px] font-black uppercase mt-1">Ledger</span></button>
        <button onclick="logout()" class="flex flex-col items-center gap-1 text-red-500"><i class="fa-solid fa-power-off text-xl"></i><span class="text-[8px] font-black uppercase mt-1">Exit</span></button>
    </nav>

    <div id="admin-panel" class="fixed inset-0 bg-[#020617] z-[5000] p-6 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-10">
            <h2 class="text-xl font-black text-cyan-400 uppercase italic">Master Console</h2>
            <button onclick="closeAdmin()" class="bg-red-500/20 text-red-500 px-6 py-2 rounded-xl text-[10px] font-black">CLOSE</button>
        </div>
        <div id="adm-sec-requests" class="space-y-3"></div>
    </div>

    <script>
        // Use your specific Firebase Config
        const firebaseConfig = { apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg", authDomain: "investment-84f4e.firebaseapp.com", projectId: "investment-84f4e", storageBucket: "investment-84f4e.firebasestorage.app", messagingSenderId: "975293889308", appId: "1:975293889308:web:6d034a99cc966c75ff58d9" };
        firebase.initializeApp(firebaseConfig); const db = firebase.firestore();
        
        let user = null; let tapCount = 0;
        const plans = [
            { n: "Apex Micro-Starter", p: 200, r: 12.5, d: 30, c: "cyan" }, 
            { n: "Apex Pro-Trader", p: 500, r: 13, d: 30, c: "fuchsia" },
            { n: "Bronze Fleet", p: 1000, r: 15, d: 30, c: "blue" }
        ];

        window.onload = () => {
            const saved = localStorage.getItem('apex_user');
            if(saved) { document.getElementById('user-name').value = saved; mockLogin(); }
        };

        async function mockLogin() {
            const name = document.getElementById('user-name').value.trim();
            const phone = document.getElementById('phone-num').value;
            if(!name) return;
            localStorage.setItem('apex_user', name);
            const ref = db.collection("users").doc(name);
            const doc = await ref.get();
            if(!doc.exists) await ref.set({ name: name, phone: phone, balance: 0, profit: 0, time: Date.now(), activeTier: 0, tierROI: 0, lastReqTime: Date.now(), tierName: "Inactive" });
            
            startSync(name);
            document.getElementById('auth-ui').classList.add('hidden');
            document.getElementById('app-ui').classList.remove('hidden');
            document.getElementById('bottom-nav').classList.remove('hidden');
            renderPlans();
        }

        function startSync(name) {
            db.collection("users").doc(name).onSnapshot(doc => { 
                if(doc.exists) { 
                    user = doc.data(); 
                    document.getElementById('v-bal').innerText = "₨ " + (user.balance || 0).toLocaleString(); 
                    document.getElementById('v-profit').innerText = "₨ " + (user.profit || 0).toLocaleString(); 
                    document.getElementById('tier-tag').innerText = user.tierName; 
                } 
            });
            // Ledger sync
            db.collection("requests").where("user", "==", name).onSnapshot(snap => {
                const list = document.getElementById('user-history'); list.innerHTML = '';
                snap.forEach(doc => {
                    const d = doc.data();
                    list.innerHTML += `<div class="glass p-5 rounded-3xl flex justify-between items-center border-l-4 border-cyan-500 mb-2 text-[9px] font-black uppercase"><div>${d.type}<br><span class="opacity-30">${new Date(d.time).toLocaleDateString()}</span></div><div class="text-right">₨ ${d.amount}<br><span class="text-cyan-400">${d.status}</span></div></div>`;
                });
            });
        }

        function renderPlans() {
            const list = document.getElementById('plans-list');
            plans.forEach(p => {
                list.innerHTML += `<div onclick="buy(${p.p}, ${p.r}, '${p.n}')" class="glass p-6 rounded-[2.5rem] flex justify-between items-center active:scale-95 cursor-pointer border border-white/5 mb-3"><div><h4 class="font-black text-[11px] uppercase text-cyan-400">${p.n}</h4><p class="text-[9px] text-gray-400 font-bold uppercase">Daily ROI: ${p.r}%</p></div><div class="text-right"><p class="font-black text-lg">₨ ${p.p}</p></div></div>`;
            });
        }

        function changePage(p) {
            document.querySelectorAll('.page').forEach(pg=>pg.classList.remove('active-page'));
            document.querySelectorAll('nav button').forEach(b=>b.classList.remove('active-tab'));
            document.getElementById('p-'+p).classList.add('active-page');
            if(p!=='wallet' && p!=='withdraw') document.getElementById('n-'+p).classList.add('active-tab');
        }

        async function submitDeposit() {
            const a = document.getElementById('dep-amount').value;
            const t = document.getElementById('dep-trx').value;
            if(!a || !t) return alert("Fill all fields sweetie!");
            await db.collection("requests").add({ user: user.name, amount: parseInt(a), tid: t, type: "deposit", status: "pending", time: Date.now() });
            alert("Sent to Admin for Verification! 😘");
            changePage('activity');
        }

        function adminTap() {
            tapCount++;
            if(tapCount >= 4) {
                if(prompt("Master Key:") === "apex786") {
                    document.getElementById('admin-panel').classList.remove('hidden');
                    syncAdmin();
                }
                tapCount = 0;
            }
        }

        function closeAdmin() { document.getElementById('admin-panel').classList.add('hidden'); }
        function logout() { localStorage.removeItem('apex_user'); location.reload(); }

        // Minimal Admin Handler for sweetie
        async function syncAdmin() {
            db.collection("requests").where("status", "==", "pending").onSnapshot(snap => {
                const list = document.getElementById('adm-sec-requests'); list.innerHTML = '';
                snap.forEach(doc => {
                    const d = doc.data();
                    list.innerHTML += `<div class="glass p-5 rounded-2xl flex justify-between items-center text-[8px] font-black uppercase"><div>${d.user}<br>Rs ${d.amount} (${d.type})</div><div class="flex gap-2"><button onclick="handle('${doc.id}','${d.user}',${d.amount},'approved')" class="bg-green-600 px-4 py-2 rounded-xl">YES</button></div></div>`;
                });
            });
        }

        async function handle(id, u, amt, act) {
            const ref = db.collection("users").doc(u);
            const d = await ref.get();
            await ref.update({ balance: (d.data().balance || 0) + amt });
            await db.collection("requests").doc(id).update({ status: act });
            alert("Approved! 💖");
        }
    </script>
</body>
</html>
