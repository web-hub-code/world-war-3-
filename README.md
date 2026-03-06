<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ApexDaily | Pro Neon Trading</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { background: #020617; color: #f1f5f9; font-family: 'Outfit', sans-serif; overflow-x: hidden; }

        /* Neon & Glass Effects */
        .neon-card {
            background: linear-gradient(135deg, #3b82f6 0%, #8b5cf6 50%, #ec4899 100%);
            box-shadow: 0 15px 60px rgba(139, 92, 246, 0.4);
            border: 1px solid rgba(255, 255, 255, 0.2);
            animation: pulse-glow 6s infinite alternate;
        }
        @keyframes pulse-glow { from { filter: brightness(1); } to { filter: brightness(1.2); } }

        .glass-panel { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(20px); border: 1px solid rgba(255, 255, 255, 0.08); }
        .tab-active { color: #3b82f6; text-shadow: 0 0 10px #3b82f6; border-top: 2px solid #3b82f6; }

        /* Ticker Animation */
        .ticker-wrap { background: rgba(0,0,0,0.5); overflow: hidden; white-space: nowrap; }
        .ticker-move { display: inline-block; animation: ticker 20s linear infinite; }
        @keyframes ticker { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

        .btn-action { background: linear-gradient(90deg, #3b82f6, #ec4899); transition: 0.3s; box-shadow: 0 10px 20px rgba(59, 130, 246, 0.3); }
        .btn-action:active { transform: scale(0.95); }
    </style>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, onAuthStateChanged, signOut, signInWithEmailAndPassword, createUserWithEmailAndPassword } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, onSnapshot, collection, addDoc, query, where, updateDoc, getDoc, orderBy } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg",
            authDomain: "investment-84f4e.firebaseapp.com",
            projectId: "investment-84f4e",
            storageBucket: "investment-84f4e.firebasestorage.app",
            messagingSenderId: "975293889308",
            appId: "1:975293889308:web:6d034a99cc966c75ff58d9"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const PKR_RATE = 280;

        onAuthStateChanged(auth, (user) => {
            if(user) {
                document.getElementById('authScreen').classList.add('hidden');
                document.getElementById('appMain').classList.remove('hidden');
                
                if(user.email === "admin@apexdaily.com") {
                    document.getElementById('masterLink').classList.remove('hidden');
                    loadAdminData();
                }
                syncUser(user.uid);
            } else {
                document.getElementById('authScreen').classList.remove('hidden');
                document.getElementById('appMain').classList.add('hidden');
            }
        });

        function syncUser(uid) {
            onSnapshot(doc(db, "users", uid), (d) => {
                if(d.exists()) {
                    const b = d.data().balance || 0;
                    document.getElementById('dispBal').innerText = "$" + b.toFixed(2);
                    document.getElementById('dispPkr').innerText = "≈ Rs. " + (b * PKR_RATE).toLocaleString();
                }
            });
            // Sync Broadcast
            onSnapshot(doc(db, "settings", "news"), (d) => {
                if(d.exists()) document.getElementById('newsText').innerText = d.data().msg;
            });
        }

        window.calcPkr = () => {
            const val = document.getElementById('amtInp').value;
            document.getElementById('pkrLabel').innerText = val ? `Total: Rs. ${val * PKR_RATE}` : "Rs. 0";
        };

        window.showTab = (id) => {
            ['homeT','walletT','adminT','historyT'].forEach(t => document.getElementById(t).classList.add('hidden'));
            document.getElementById(id).classList.remove('hidden');
        };

        window.handleAuth = async (type) => {
            const u = document.getElementById('uI').value;
            const p = document.getElementById('pI').value;
            const email = u.includes('@') ? u : `${u}@apex.com`;
            try {
                if(type === 'signup') {
                    const res = await createUserWithEmailAndPassword(auth, email, p);
                    await setDoc(doc(db, "users", res.user.uid), { username: u, balance: 0, role: 'user' });
                } else { await signInWithEmailAndPassword(auth, email, p); }
            } catch(e) { alert(e.message); }
        };

        window.logout = () => signOut(auth);
    </script>
</head>
<body class="pb-28">

    <div class="ticker-wrap py-2 text-[10px] font-bold text-blue-400 border-b border-white/5">
        <div class="ticker-move">
            BTC $64,231.50 (+1.2%) &nbsp;&nbsp;&nbsp; ETH $3,452.10 (-0.5%) &nbsp;&nbsp;&nbsp; SOL $145.20 (+4.8%) &nbsp;&nbsp;&nbsp; APEX PROFIT TODAY: 2.85% FIXED 🔥
        </div>
    </div>

    <div id="authScreen" class="h-screen flex items-center justify-center p-6">
        <div class="w-full max-w-sm glass-panel p-10 rounded-[3rem] text-center border-t-2 border-blue-500/30">
            <h1 class="text-4xl font-black text-blue-500 mb-2 italic">APEXDAILY</h1>
            <p class="text-[10px] text-gray-500 uppercase tracking-widest mb-10">Future of Finance</p>
            <input id="uI" type="text" placeholder="Username" class="w-full p-4 bg-white/5 rounded-2xl mb-4 outline-none border border-white/10 focus:border-blue-500">
            <input id="pI" type="password" placeholder="Password" class="w-full p-4 bg-white/5 rounded-2xl mb-8 outline-none border border-white/10 focus:border-blue-500">
            <button onclick="handleAuth('login')" class="w-full btn-action py-4 rounded-2xl font-black shadow-lg">UNLOCK TRADING</button>
            <p onclick="handleAuth('signup')" class="mt-6 text-xs text-gray-500 underline cursor-pointer">Create New Account</p>
        </div>
    </div>

    <div id="appMain" class="hidden">
        <div id="homeT" class="p-6">
            <div class="flex justify-between items-center mb-8">
                <div>
                    <h2 class="text-xl font-black italic">APEX<span class="text-blue-500">PRO</span></h2>
                    <p class="text-[8px] text-green-500 font-bold tracking-widest uppercase">● System Live</p>
                </div>
                <button onclick="logout()" class="w-10 h-10 glass-panel rounded-2xl flex items-center justify-center text-red-500"><i class="fa-solid fa-power-off"></i></button>
            </div>

            <div class="glass-panel p-4 rounded-2xl mb-8 flex items-center gap-3 border-l-4 border-blue-500">
                <i class="fa-solid fa-bullhorn text-blue-500 animate-bounce"></i>
                <marquee id="newsText" class="text-xs font-medium text-gray-300">Welcome to ApexDaily! Your profit is being processed...</marquee>
            </div>

            <div class="neon-card p-8 rounded-[2.5rem] mb-10 text-white relative overflow-hidden">
                <div class="relative z-10">
                    <p class="text-[10px] uppercase font-bold tracking-[3px] opacity-80">Portfolio Balance</p>
                    <h1 id="dispBal" class="text-6xl font-black my-2 tracking-tighter">$0.00</h1>
                    <p id="dispPkr" class="text-xs font-medium opacity-70">Rs. 0</p>
                    
                    <div class="flex gap-4 mt-8">
                        <button onclick="showTab('walletT')" class="flex-1 bg-white text-blue-600 py-4 rounded-2xl font-black text-sm active:scale-95 transition">DEPOSIT</button>
                        <button onclick="showTab('walletT')" class="flex-1 bg-black/30 backdrop-blur-md py-4 rounded-2xl font-black text-sm border border-white/20 active:scale-95 transition">WITHDRAW</button>
                    </div>
                </div>
                <div class="absolute -right-10 -top-10 w-40 h-40 bg-white/10 rounded-full blur-3xl"></div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-10">
                <div class="glass-panel p-5 rounded-3xl border-b-4 border-green-500">
                    <p class="text-[9px] text-gray-400 font-bold mb-1 uppercase">Daily ROI</p>
                    <h4 class="text-xl font-black text-green-400">+2.85%</h4>
                </div>
                <div class="glass-panel p-5 rounded-3xl border-b-4 border-purple-500">
                    <p class="text-[9px] text-gray-400 font-bold mb-1 uppercase">Promo Code</p>
                    <h4 class="text-xl font-black text-purple-400">Apply</h4>
                </div>
            </div>
        </div>

        <div id="walletT" class="hidden p-6">
            <h2 class="text-2xl font-black mb-8 italic text-blue-400">Wallet <span class="text-white">Center</span></h2>
            <div class="space-y-4 mb-10">
                <div class="glass-panel p-5 rounded-3xl border-l-4 border-yellow-500 flex justify-between items-center">
                    <div><p class="text-[10px] text-gray-500">JazzCash / SadaPay</p><p class="font-black">03705519562</p></div>
                    <i class="fa-solid fa-copy text-yellow-500"></i>
                </div>
                <div class="glass-panel p-5 rounded-3xl border-l-4 border-green-500 flex justify-between items-center">
                    <div><p class="text-[10px] text-gray-500">EasyPaisa</p><p class="font-black">03379827882</p></div>
                    <i class="fa-solid fa-copy text-green-500"></i>
                </div>
            </div>
            
            <div class="glass-panel p-8 rounded-[2rem]">
                <input id="amtInp" oninput="calcPkr()" type="number" placeholder="Deposit Amount ($)" class="w-full p-4 bg-white/5 rounded-2xl mb-2 outline-none border border-white/10">
                <p id="pkrLabel" class="text-[10px] text-blue-400 font-bold mb-6 ml-2">Rs. 0</p>
                <input id="tidInp" type="text" placeholder="Transaction ID (TID)" class="w-full p-4 bg-white/5 rounded-2xl mb-8 outline-none border border-white/10">
                <button class="w-full btn-action py-4 rounded-2xl font-black">SUBMIT PAYMENT</button>
            </div>
        </div>

        <div id="adminT" class="hidden p-6">
            <h2 class="text-2xl font-black mb-8 text-yellow-500 italic">MASTER CONTROL</h2>
            <div class="glass-panel p-6 rounded-3xl mb-8">
                <p class="text-xs font-bold mb-4 uppercase text-blue-400">Global Broadcast</p>
                <textarea id="bcI" placeholder="Type news for all users..." class="w-full h-24 bg-black/40 rounded-2xl p-4 text-xs outline-none border border-white/5 mb-4"></textarea>
                <button class="w-full bg-blue-600 py-3 rounded-xl font-bold text-xs uppercase">Update News</button>
            </div>
            <div class="glass-panel p-6 rounded-3xl">
                <p class="text-xs font-bold mb-4 uppercase text-green-400">Manual User Credit</p>
                <input type="text" placeholder="User Email" class="w-full p-4 bg-black/40 rounded-xl mb-4 text-xs outline-none border border-white/5">
                <button class="w-full bg-green-600 py-3 rounded-xl font-bold text-xs uppercase text-black">Add Bonus</button>
            </div>
        </div>

        <nav class="fixed bottom-0 left-0 right-0 p-6 flex justify-around items-center glass-panel rounded-t-[3rem] z-50">
            <button onclick="showTab('homeT')" class="flex flex-col items-center gap-1 focus:text-blue-500">
                <i class="fa-solid fa-house-chimney text-xl"></i>
                <span class="text-[8px] font-bold uppercase tracking-widest">Home</span>
            </button>
            <button onclick="showTab('walletT')" class="flex flex-col items-center gap-1 focus:text-green-500">
                <i class="fa-solid fa-wallet text-xl"></i>
                <span class="text-[8px] font-bold uppercase tracking-widest">Wallet</span>
            </button>
            <button onclick="showTab('historyT')" class="flex flex-col items-center gap-1 focus:text-purple-500">
                <i class="fa-solid fa-receipt text-xl"></i>
                <span class="text-[8px] font-bold uppercase tracking-widest">Log</span>
            </button>
            <button id="masterLink" onclick="showTab('adminT')" class="hidden flex flex-col items-center gap-1 text-yellow-500">
                <i class="fa-solid fa-crown text-xl"></i>
                <span class="text-[8px] font-bold uppercase tracking-widest">Master</span>
            </button>
        </nav>
    </div>

</body>
</html>
