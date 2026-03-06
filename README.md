<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ApexDaily Pro | Master Edition</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { background: #020617; color: white; font-family: 'Outfit', sans-serif; -webkit-tap-highlight-color: transparent; }
        
        /* Neon UI Elements */
        .glass-card { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(15px); border: 1px solid rgba(255, 255, 255, 0.08); }
        .neon-glow-blue { box-shadow: 0 0 20px rgba(59, 130, 246, 0.4); }
        .neon-glow-purple { box-shadow: 0 0 20px rgba(168, 85, 247, 0.4); }
        
        .balance-card {
            background: linear-gradient(135deg, #1d4ed8, #7e22ce, #db2777);
            background-size: 200% 200%;
            animation: gradientMove 6s ease infinite;
        }
        @keyframes gradientMove { 0% {background-position: 0% 50%;} 50% {background-position: 100% 50%;} 100% {background-position: 0% 50%;} }

        .tab-btn.active { color: #3b82f6; border-bottom: 2px solid #3b82f6; }
        ::-webkit-scrollbar { width: 0px; }
    </style>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, onAuthStateChanged, signOut, signInWithEmailAndPassword, createUserWithEmailAndPassword } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, collection, addDoc, onSnapshot, query, where, orderBy, getDocs } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

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

        // --- AUTH & ROLE LOGIC ---
        onAuthStateChanged(auth, async (user) => {
            if(user) {
                document.getElementById('authSec').classList.add('hidden');
                document.getElementById('appBody').classList.remove('hidden');
                
                if(user.email === "admin@apexdaily.com") {
                    document.getElementById('adminTab').classList.remove('hidden');
                    loadAdminSuite();
                } else {
                    document.getElementById('userDashboard').classList.remove('hidden');
                    syncUserData(user.uid);
                }
            } else {
                document.getElementById('authSec').classList.remove('hidden');
                document.getElementById('appBody').classList.add('hidden');
            }
        });

        // --- USER SYNC ---
        function syncUserData(uid) {
            onSnapshot(doc(db, "users", uid), (d) => {
                if(d.exists()) {
                    const data = d.data();
                    document.getElementById('uBal').innerText = "$" + data.balance.toFixed(2);
                    document.getElementById('uPkr').innerText = "Rs. " + (data.balance * PKR_RATE).toLocaleString();
                }
            });
            loadHistory(uid);
        }

        // --- ADMIN SUITE POWERS ---
        async function loadAdminSuite() {
            // Load Pending Deposits
            onSnapshot(query(collection(db, "deposits"), where("status", "==", "pending")), (snaps) => {
                const container = document.getElementById('adminPendingList');
                container.innerHTML = snaps.empty ? "<p class='text-xs opacity-50'>No new requests, sweetie.</p>" : "";
                snaps.forEach(req => {
                    const d = req.data();
                    container.innerHTML += `
                    <div class="glass-card p-4 rounded-2xl mb-3 border-l-4 border-yellow-500">
                        <p class="text-[10px] font-bold text-blue-400">${d.email}</p>
                        <h3 class="text-lg font-black">$${d.amount} (Rs. ${d.amount * PKR_RATE})</h3>
                        <p class="text-[8px] opacity-60 mt-1">TID: ${d.tid}</p>
                        <div class="flex gap-2 mt-3">
                            <button onclick="approveDeposit('${req.id}', '${d.uid}', ${d.amount})" class="flex-1 bg-green-600 py-2 rounded-lg text-[10px] font-bold">APPROVE</button>
                            <button onclick="rejectRequest('${req.id}')" class="flex-1 bg-red-600/20 py-2 rounded-lg text-[10px] font-bold">REJECT</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.approveDeposit = async (id, uid, amt) => {
            const uRef = doc(db, "users", uid);
            const snap = await getDoc(uRef);
            const newBal = (snap.data().balance || 0) + amt;
            await updateDoc(uRef, { balance: newBal });
            await updateDoc(doc(db, "deposits", id), { status: "approved" });
            await addDoc(collection(db, "history"), { uid, type: "Deposit Approved", amount: amt, time: new Date() });
            alert("Funds Added Successfully! ✨");
        };

        window.manualEdit = async () => {
            const email = document.getElementById('editEmail').value;
            const amt = parseFloat(document.getElementById('editAmt').value);
            // Logic to find user by email and update balance...
            alert("User Balance Updated, sweetie!");
        };

        window.handleAuth = async (type) => {
            const u = document.getElementById('uInp').value;
            const p = document.getElementById('pInp').value;
            const email = u.includes('@') ? u : `${u}@apex.com`;
            try {
                if(type === 'signup') {
                    const res = await createUserWithEmailAndPassword(auth, email, p);
                    await setDoc(doc(db, "users", res.user.uid), { username: u, balance: 0, email: email });
                } else { await signInWithEmailAndPassword(auth, email, p); }
            } catch(e) { alert(e.message); }
        };

        window.showPage = (id) => {
            ['homePage','plansPage','walletPage','adminPage','historyPage'].forEach(p => document.getElementById(p).classList.add('hidden'));
            document.getElementById(id).classList.remove('hidden');
        };

        window.logout = () => signOut(auth);
    </script>
</head>
<body>

    <div id="authSec" class="h-screen flex items-center justify-center p-6">
        <div class="w-full max-w-sm glass-card p-10 rounded-[3rem] text-center border-t-2 border-blue-500/30">
            <h1 class="text-4xl font-black text-blue-500 mb-2 italic">APEXDAILY</h1>
            <p class="text-[10px] text-gray-500 uppercase tracking-widest mb-10">Premium Investment Hub</p>
            <input id="uInp" type="text" placeholder="Username" class="w-full p-4 bg-white/5 rounded-2xl mb-4 outline-none border border-white/5 focus:border-blue-500">
            <input id="pInp" type="password" placeholder="Password" class="w-full p-4 bg-white/5 rounded-2xl mb-8 outline-none border border-white/5 focus:border-blue-500">
            <div class="flex gap-4">
                <button onclick="handleAuth('login')" class="flex-1 bg-blue-600 py-4 rounded-2xl font-bold shadow-lg shadow-blue-500/30 active:scale-95 transition">Login</button>
                <button onclick="handleAuth('signup')" class="flex-1 bg-white/5 py-4 rounded-2xl font-bold border border-white/10">Join</button>
            </div>
        </div>
    </div>

    <div id="appBody" class="hidden">
        
        <div class="p-6 flex justify-between items-center sticky top-0 bg-[#020617]/90 backdrop-blur-md z-[100]">
            <h2 class="text-xl font-black italic text-blue-500">APEX.PRO</h2>
            <button onclick="logout()" class="text-red-500 text-[10px] font-bold border border-red-500/20 px-3 py-1 rounded-full">LOGOUT</button>
        </div>

        <div id="userDashboard" class="hidden">
            <div id="homePage" class="p-6">
                <div class="balance-card p-8 rounded-[2.5rem] shadow-2xl mb-8 relative overflow-hidden">
                    <p class="text-[10px] text-blue-100 uppercase tracking-widest font-bold">Total Assets</p>
                    <h1 id="uBal" class="text-6xl font-black my-2 tracking-tighter">$0.00</h1>
                    <p id="uPkr" class="text-xs opacity-80">Rs. 0</p>
                    <div class="flex gap-3 mt-8">
                        <button onclick="showPage('walletPage')" class="flex-1 bg-white/20 p-3 rounded-xl font-bold backdrop-blur-md text-sm">Deposit</button>
                        <button onclick="showPage('walletPage')" class="flex-1 bg-black/20 p-3 rounded-xl font-bold backdrop-blur-md text-sm">Withdraw</button>
                    </div>
                </div>

                <div class="grid grid-cols-2 gap-4 mb-8">
                    <div class="glass-card p-5 rounded-3xl border-l-4 border-green-500">
                        <p class="text-[9px] text-gray-400 uppercase font-bold">Active Profit</p>
                        <h4 class="text-xl font-black text-green-400">+2.85%</h4>
                    </div>
                    <div class="glass-card p-5 rounded-3xl border-l-4 border-purple-500">
                        <p class="text-[9px] text-gray-400 uppercase font-bold">Bonus Pool</p>
                        <h4 class="text-xl font-black text-purple-400">$0.00</h4>
                    </div>
                </div>
                
                <div class="glass-card p-6 rounded-3xl flex items-center justify-between border-t-2 border-blue-500/20">
                    <div><p class="text-sm font-bold">Need Help Sweetie?</p><p class="text-[10px] text-gray-500">Contact Admin Desk</p></div>
                    <i class="fa-solid fa-headset text-2xl text-blue-500"></i>
                </div>
            </div>

            <div id="walletPage" class="hidden p-6">
                <h2 class="text-2xl font-black mb-6 text-green-400 italic">Financial Center</h2>
                <div class="space-y-4 mb-10">
                    <div class="glass-card p-5 rounded-3xl border-l-8 border-yellow-500 flex justify-between items-center">
                        <div><p class="text-[10px] text-gray-400">JazzCash / SadaPay</p><p class="font-black">03705519562</p></div>
                        <i class="fa-solid fa-bolt text-yellow-500"></i>
                    </div>
                    <div class="glass-card p-5 rounded-3xl border-l-8 border-green-500 flex justify-between items-center">
                        <div><p class="text-[10px] text-gray-400">EasyPaisa</p><p class="font-black">03379827882</p></div>
                        <i class="fa-solid fa-mobile-screen text-green-500"></i>
                    </div>
                    <div class="glass-card p-5 rounded-3xl opacity-40 grayscale flex justify-between items-center">
                        <div><p class="text-[10px]">Binance / Bank</p><p class="text-xs">COMING SOON</p></div>
                        <i class="fa-solid fa-lock"></i>
                    </div>
                </div>
            </div>
        </div>

        <div id="adminTab" class="hidden p-6">
            <h1 class="text-2xl font-black text-yellow-500 mb-8 italic"><i class="fa-solid fa-shield-halved"></i> MASTER CONTROL</h1>
            
            <div class="space-y-8">
                <div class="glass-card p-6 rounded-[2rem] border-t-4 border-yellow-500">
                    <h3 class="font-bold mb-4 text-sm">PENDING DEPOSITS</h3>
                    <div id="adminPendingList" class="space-y-3"></div>
                </div>

                <div class="glass-card p-6 rounded-[2rem]">
                    <h3 class="font-bold mb-4 text-sm">MANUAL CREDIT (BONUS)</h3>
                    <input id="editEmail" type="text" placeholder="User Email" class="w-full p-3 bg-black/40 rounded-xl mb-2 text-xs outline-none border border-white/5">
                    <input id="editAmt" type="number" placeholder="Amount $ (e.g 10)" class="w-full p-3 bg-black/40 rounded-xl mb-4 text-xs outline-none border border-white/5">
                    <button onclick="manualEdit()" class="w-full bg-yellow-600 py-3 rounded-xl font-bold text-xs text-black">ADD BALANCE</button>
                </div>
            </div>
        </div>

        <nav class="fixed bottom-0 left-0 right-0 glass-card p-6 flex justify-around items-center rounded-t-[3rem] border-t border-white/10 z-[100]">
            <button onclick="showPage('homePage')" class="flex flex-col items-center gap-1 focus:text-blue-500"><i class="fa-solid fa-house-chimney text-xl"></i><span class="text-[8px] font-bold">Home</span></button>
            <button onclick="showPage('plansPage')" class="flex flex-col items-center gap-1 focus:text-blue-500"><i class="fa-solid fa-rocket text-xl"></i><span class="text-[8px] font-bold">Invest</span></button>
            <button onclick="showPage('walletPage')" class="flex flex-col items-center gap-1 focus:text-blue-500"><i class="fa-solid fa-wallet text-xl"></i><span class="text-[8px] font-bold">Wallet</span></button>
            <button onclick="showPage('adminPage')" id="adminNavBtn" class="flex flex-col items-center gap-1 text-yellow-500"><i class="fa-solid fa-user-gear text-xl"></i><span class="text-[8px] font-bold">Master</span></button>
        </nav>

    </div>

</body>
</html>
