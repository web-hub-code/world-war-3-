<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>ApexDaily | Premium Trading</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { background: #020617; color: white; font-family: 'Outfit', sans-serif; -webkit-tap-highlight-color: transparent; }
        
        .glass { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(15px); border: 1px solid rgba(255, 255, 255, 0.1); }
        .neon-gradient { background: linear-gradient(135deg, #0ea5e9, #a855f7, #ec4899); box-shadow: 0 15px 40px rgba(168, 85, 247, 0.3); }
        .btn-modern { background: linear-gradient(90deg, #3b82f6, #d946ef); transition: 0.3s; }
        .btn-modern:active { transform: scale(0.92); }

        /* Mobile Optimization */
        @media (max-width: 480px) {
            h1 { font-size: 2.5rem !important; }
            .card-p { padding: 1.5rem !important; }
        }

        .tab-content { display: none; animation: slideUp 0.4s ease-out; }
        .tab-content.active { display: block; }
        @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
        
        #secretAdminBox { display: none; }
    </style>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, onSnapshot } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

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

        // --- SECRET CLICK LOGIC ---
        let clickCount = 0;
        window.handleSecretClick = () => {
            clickCount++;
            if(clickCount === 3) {
                document.getElementById('secretAdminBox').style.display = 'block';
                alert("Secret Admin Mode Activated! 😉");
                clickCount = 0;
            }
        };

        onAuthStateChanged(auth, (user) => {
            if(user) {
                document.getElementById('authArea').classList.add('hidden');
                document.getElementById('mainArea').classList.remove('hidden');
                syncData(user.uid);
                // Admin specific check
                if(user.email === "admin@apexdaily.com") {
                    document.getElementById('navAdmin').classList.remove('hidden');
                }
            }
        });

        function syncData(uid) {
            onSnapshot(doc(db, "users", uid), (d) => {
                if(d.exists()) {
                    document.getElementById('displayBal').innerText = "Rs. " + (d.data().balance || 0).toLocaleString();
                    document.getElementById('profileName').innerText = d.data().username || "User";
                }
            });
        }

        window.showTab = (id) => {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
            document.getElementById(id).classList.add('active');
        };

        window.processAuth = async (mode) => {
            const e = document.getElementById('email').value;
            const p = document.getElementById('pass').value;
            const u = document.getElementById('user').value;
            try {
                if(mode === 'signup') {
                    const r = await createUserWithEmailAndPassword(auth, e, p);
                    await setDoc(doc(db, "users", r.user.uid), { username: u, balance: 0, email: e });
                } else { await signInWithEmailAndPassword(auth, e, p); }
            } catch(err) { alert("Error: " + err.message); }
        };

        // Secret Admin Login
        window.adminLogin = () => {
            const key = document.getElementById('adminKey').value;
            if(key === "786786") { // Aapka Secret Code sweetie
                document.getElementById('email').value = "admin@apexdaily.com";
                document.getElementById('pass').value = "admin123"; // Jo aapne set kiya ho
                processAuth('login');
            } else { alert("Wrong Secret Key!"); }
        };
    </script>
</head>
<body class="pb-24 select-none">

    <div id="authArea" class="min-h-screen flex flex-col items-center justify-center p-8 bg-[#020617]">
        <h1 onclick="handleSecretClick()" class="text-5xl font-black italic text-transparent bg-clip-text bg-gradient-to-r from-sky-400 to-fuchsia-500 mb-2">APEXDAILY</h1>
        <p class="text-[10px] text-gray-500 tracking-[0.5em] uppercase mb-12">The Future Is Here</p>

        <div class="w-full max-w-sm glass p-8 rounded-[2.5rem] space-y-4">
            <input id="user" type="text" placeholder="Username" class="w-full bg-white/5 p-4 rounded-2xl outline-none border border-white/10 focus:border-sky-500 transition">
            <input id="email" type="email" placeholder="Email Address" class="w-full bg-white/5 p-4 rounded-2xl outline-none border border-white/10 focus:border-sky-500 transition">
            <input id="pass" type="password" placeholder="Password" class="w-full bg-white/5 p-4 rounded-2xl outline-none border border-white/10 focus:border-sky-500 transition">
            
            <div id="secretAdminBox" class="pt-4 border-t border-white/10">
                <input id="adminKey" type="password" placeholder="Enter Admin Secret Key" class="w-full bg-blue-500/10 p-4 rounded-2xl outline-none border border-blue-500/30 text-blue-400 text-sm mb-4">
                <button onclick="adminLogin()" class="w-full bg-blue-600 py-3 rounded-2xl font-bold text-xs uppercase">Verify Admin Access</button>
            </div>

            <button onclick="processAuth('login')" class="w-full btn-modern py-4 rounded-2xl font-black shadow-lg mt-4">LOG IN</button>
            <button onclick="processAuth('signup')" class="w-full bg-white/5 py-4 rounded-2xl font-bold text-xs border border-white/10">CREATE ACCOUNT</button>
        </div>
    </div>

    <div id="mainArea" class="hidden">
        <div class="p-6 flex justify-between items-center sticky top-0 bg-[#020617]/80 backdrop-blur-lg z-50">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-2xl neon-gradient flex items-center justify-center font-black">A</div>
                <div>
                    <h2 class="text-sm font-black flex items-center tracking-tight">@<span id="profileName">User</span> <i class="fa-solid fa-certificate text-sky-400 ml-1 text-[10px]"></i></h2>
                    <div class="flex items-center gap-1"><span class="w-1.5 h-1.5 bg-green-500 rounded-full animate-pulse"></span><span class="text-[8px] text-gray-400 font-bold uppercase">System Active</span></div>
                </div>
            </div>
            <button onclick="location.reload()" class="w-10 h-10 glass rounded-xl flex items-center justify-center text-red-500"><i class="fa-solid fa-power-off"></i></button>
        </div>

        <div id="homeTab" class="tab-content active p-6 card-p">
            <div class="neon-gradient p-8 rounded-[3rem] relative overflow-hidden mb-8">
                <p class="text-[10px] font-bold opacity-80 uppercase tracking-widest">Main Balance (PKR)</p>
                <h1 id="displayBal" class="text-5xl font-black my-3 tracking-tighter">Rs. 0</h1>
                <div class="flex gap-3 mt-8">
                    <button onclick="showTab('walletTab')" class="flex-1 bg-white text-blue-600 py-3.5 rounded-2xl font-black text-xs">RECHARGE</button>
                    <button onclick="showTab('walletTab')" class="flex-1 bg-black/20 py-3.5 rounded-2xl font-black text-xs border border-white/20">WITHDRAW</button>
                </div>
                <div class="absolute -right-10 -bottom-10 w-32 h-32 bg-white/10 rounded-full blur-3xl"></div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-8">
                <div class="glass p-5 rounded-[2rem] border-l-4 border-green-500">
                    <p class="text-[9px] text-gray-500 font-bold uppercase">Daily Profit</p>
                    <h3 class="text-lg font-black text-green-400">+12.5%</h3>
                </div>
                <div class="glass p-5 rounded-[2rem] border-l-4 border-sky-500">
                    <p class="text-[9px] text-gray-500 font-bold uppercase">Next Pulse</p>
                    <h3 class="text-lg font-black text-sky-400">23:59:01</h3>
                </div>
            </div>

            <h3 class="text-xs font-black text-gray-500 uppercase tracking-widest mb-6 ml-2">Active Plans</h3>
            <div class="space-y-4">
                <div class="glass p-6 rounded-[2.5rem] flex justify-between items-center border border-white/5">
                    <div><h4 class="font-black text-xl">Plan 200</h4><p class="text-[10px] text-sky-400">Daily ROI: Rs. 25.00</p></div>
                    <button class="btn-modern px-6 py-3 rounded-xl text-[10px] font-black">BUY</button>
                </div>
                <div class="glass p-6 rounded-[2.5rem] flex justify-between items-center border border-white/5">
                    <div><h4 class="font-black text-xl">Plan 500</h4><p class="text-[10px] text-sky-400">Daily ROI: Rs. 65.00</p></div>
                    <button class="btn-modern px-6 py-3 rounded-xl text-[10px] font-black">BUY</button>
                </div>
            </div>
        </div>

        <div id="walletTab" class="tab-content p-6">
            <h2 class="text-3xl font-black italic mb-8">Cash <span class="text-sky-500">Center</span></h2>
            <div class="glass p-6 rounded-[2rem] mb-8 border-l-4 border-yellow-500">
                <p class="text-[10px] text-gray-400 font-bold uppercase mb-2">Deposit Account</p>
                <div class="flex justify-between items-center">
                    <span class="font-black text-lg">03705519562</span>
                    <span class="text-[10px] bg-yellow-500/20 text-yellow-500 px-3 py-1 rounded-full font-bold">JazzCash</span>
                </div>
            </div>
            <div class="glass p-8 rounded-[2.5rem]">
                <input type="number" placeholder="Enter PKR Amount" class="w-full bg-white/5 p-5 rounded-2xl outline-none border border-white/10 mb-4 focus:border-sky-500">
                <input type="text" placeholder="Transaction ID (TID)" class="w-full bg-white/5 p-5 rounded-2xl outline-none border border-white/10 mb-8 focus:border-sky-500">
                <button class="w-full btn-modern py-4 rounded-2xl font-black">SUBMIT PAYMENT</button>
            </div>
        </div>

        <div id="adminTab" class="tab-content p-6">
            <h2 class="text-2xl font-black text-yellow-500 mb-8 italic"><i class="fa-solid fa-crown mr-2"></i> MASTER CONTROL</h2>
            <div class="glass p-6 rounded-3xl border-t-4 border-yellow-500 space-y-4">
                <p class="text-xs font-bold text-gray-400 uppercase">Profit Pulse System</p>
                <button class="w-full bg-yellow-600 py-4 rounded-2xl font-black text-black text-xs uppercase shadow-xl">Activate Global ROI</button>
            </div>
        </div>

        <nav class="fixed bottom-0 left-0 right-0 glass p-5 flex justify-around items-center rounded-t-[3rem] z-50 border-t border-white/10">
            <button onclick="showTab('homeTab')" class="flex flex-col items-center gap-1 focus:text-sky-400"><i class="fa-solid fa-house-chimney text-xl"></i><span class="text-[8px] font-bold">HOME</span></button>
            <button onclick="showTab('walletTab')" class="flex flex-col items-center gap-1 focus:text-sky-400"><i class="fa-solid fa-wallet text-xl"></i><span class="text-[8px] font-bold">WALLET</span></button>
            <button class="flex flex-col items-center gap-1 focus:text-sky-400"><i class="fa-solid fa-headset text-xl"></i><span class="text-[8px] font-bold">HELP</span></button>
            <button id="navAdmin" onclick="showTab('adminTab')" class="hidden flex flex-col items-center gap-1 text-yellow-500 animate-pulse"><i class="fa-solid fa-crown text-xl"></i><span class="text-[8px] font-bold">ADMIN</span></button>
        </nav>
    </div>

</body>
</html>
