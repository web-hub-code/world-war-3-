<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Digital Earnings 🇵🇰</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
    
    <style>
        /* --- UI DESIGN --- */
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        body { background: #020617; color: white; min-height: 100vh; overflow-x: hidden; display: flex; flex-direction: column; align-items: center; }

        .blob { position: fixed; width: 400px; height: 400px; background: linear-gradient(135deg, #fbbf24 0%, #d97706 100%); filter: blur(120px); border-radius: 50%; z-index: -1; top: -10%; right: -10%; opacity: 0.2; }
        .glass-card { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(20px); border: 1px solid rgba(255, 255, 255, 0.1); border-radius: 24px; padding: 25px; width: 92%; max-width: 450px; text-align: center; margin: 20px auto; box-shadow: 0 10px 40px rgba(0,0,0,0.6); }

        .brand-name { font-size: 1.8rem; font-weight: 600; color: #fbbf24; text-shadow: 0 0 15px rgba(251, 191, 36, 0.4); }
        .tier-tag { font-size: 0.7rem; background: #fbbf24; color: #000; padding: 4px 12px; border-radius: 20px; font-weight: bold; }

        input, select { width: 100%; padding: 14px; background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); border-radius: 12px; color: white; margin-bottom: 12px; outline: none; }
        .btn-main { width: 100%; padding: 14px; background: linear-gradient(90deg, #fbbf24, #d97706); border: none; border-radius: 12px; color: black; font-weight: bold; cursor: pointer; transition: 0.3s; margin-top: 10px; position: relative; z-index: 10; }
        .btn-main:hover { transform: scale(1.02); box-shadow: 0 0 20px rgba(251, 191, 36, 0.4); }

        #loginPage, #dashboardPage, #adminPage, #termsModal { display: none; }
        .active-block { display: block !important; }
        .active-flex { display: flex !important; }

        .overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.95); z-index: 5000; display: flex; justify-content: center; align-items: center; }
        .history-table { width: 100%; border-collapse: collapse; margin-top: 15px; font-size: 0.75rem; text-align: left; }
        .history-table td, .history-table th { padding: 10px; border-bottom: 1px solid rgba(255,255,255,0.05); }

        #notify { position: fixed; top: -100px; background: #fbbf24; color: black; padding: 15px 30px; border-radius: 12px; font-weight: bold; transition: 0.5s; z-index: 6000; }
    </style>
</head>
<body>

    <div class="blob"></div>
    <div id="notify">Success, sweetie! ✅</div>

    <div id="termsModal" class="overlay">
        <div class="glass-card" style="z-index: 5001;">
            <h2 style="color: #fbbf24; margin-bottom: 15px;">PakGold Rules 🇵🇰</h2>
            <div style="text-align: left; font-size: 0.8rem; margin-bottom: 25px; color: #cbd5e1; line-height: 1.8;">
                <p>• Min. Deposit/Withdraw: <b>PKR 500</b></p>
                <p>• Daily Bonus: Once every 24 Hours.</p>
                <p>• Referral: 10% instant commission.</p>
                <p>• Support: 24/7 active.</p>
            </div>
            <button id="finalAcceptBtn" class="btn-main">Accept & Continue</button>
        </div>
    </div>

    <div id="loginPage" class="glass-card">
        <div class="brand-name">PakGold</div>
        <p style="color: #94a3b8; font-size: 0.8rem; margin-bottom: 30px;">Premium Digital Growth.</p>
        <button id="userLoginBtn" class="btn-main">Get Started</button>
        <button id="adminLoginLink" class="btn-main" style="background:transparent; border:1px solid #4facfe; color:#4facfe; margin-top:15px; font-size:0.7rem;">Admin Access</button>
    </div>

    <div id="dashboardPage" class="glass-card">
        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
            <span class="brand-name" style="font-size: 1.2rem;">PakGold</span>
            <button id="logoutBtn" style="background:none; border:1px solid #ef4444; color:#ef4444; padding:4px 10px; border-radius:8px; font-size: 0.7rem; cursor:pointer;">Logout</button>
        </div>

        <div style="background: rgba(255,255,255,0.03); padding: 20px; border-radius: 20px; margin-bottom: 20px;">
            <span id="userTier" class="tier-tag">Starter Tier</span>
            <p style="font-size: 0.8rem; margin-top: 10px; color: #94a3b8;">Total Balance</p>
            <h1 style="font-size: 2.2rem; color: #fbbf24;">PKR <span id="userBal">0.00</span></h1>
            <button id="claimBtn" class="btn-main" style="background: #10b981; margin-top: 15px;">Claim 24h Bonus 💰</button>
        </div>

        <select id="actionType"><option value="Deposit">Deposit</option><option value="Withdraw">Withdraw</option></select>
        <select id="method"><option value="JazzCash">JazzCash</option><option value="EasyPaisa">EasyPaisa</option><option value="Binance">Binance</option></select>
        <input type="number" id="amount" placeholder="Min Amount 500">
        <button id="submitReqBtn" class="btn-main">Submit Request</button>

        <h4 style="text-align: left; margin-top: 25px; font-size: 0.9rem; color: #fbbf24;">Transaction History</h4>
        <table class="history-table"><thead><tr><th>Type</th><th>Amt</th><th>Status</th></tr></thead><tbody id="txnHistory"></tbody></table>
    </div>

    <div id="adminPage" class="glass-card" style="max-width: 600px;">
        <h2 style="color: #fbbf24; margin-bottom: 20px;">Admin Control 👑</h2>
        <div id="adminReqList"></div>
        <button onclick="location.reload()" class="btn-main" style="background:#ef4444; color:white;">Exit Admin</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-auth.js";
        import { getFirestore, doc, getDoc, setDoc, updateDoc, onSnapshot, collection, addDoc, query, where, increment, serverTimestamp } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            projectId: "dark-web-9",
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        // --- TERMS LOGIC ---
        const termsModal = document.getElementById('termsModal');
        const acceptBtn = document.getElementById('finalAcceptBtn');

        if(!localStorage.getItem('pakgold_terms')) {
            termsModal.style.display = 'flex';
        }

        acceptBtn.addEventListener('click', () => {
            termsModal.style.display = 'none';
            localStorage.setItem('pakgold_terms', 'true');
        });

        // --- AUTH & LOGIN ---
        document.getElementById('userLoginBtn').onclick = () => signInAnonymously(auth);
        document.getElementById('logoutBtn').onclick = () => signOut(auth).then(()=>location.reload());
        document.getElementById('adminLoginLink').onclick = () => {
            if(prompt("Admin Password:") === "admin123") {
                document.getElementById('loginPage').style.display='none';
                document.getElementById('adminPage').classList.add('active-block');
                loadAdmin();
            }
        };

        onAuthStateChanged(auth, async (user) => {
            if(user) {
                document.getElementById('loginPage').style.display = 'none';
                document.getElementById('dashboardPage').classList.add('active-block');
                const userRef = doc(db, "users", user.uid);
                const snap = await getDoc(userRef);
                if(!snap.exists()) await setDoc(userRef, { balance: 0, tier: 'Starter', uid: user.uid, lastClaim: null });

                onSnapshot(userRef, (d) => {
                    document.getElementById('userBal').innerText = d.data().balance.toFixed(2);
                    document.getElementById('userTier').innerText = d.data().tier + " Tier";
                });

                const q = query(collection(db, "transactions"), where("uid", "==", user.uid));
                onSnapshot(q, (s) => {
                    const h = document.getElementById('txnHistory'); h.innerHTML = "";
                    s.forEach(doc => { const d = doc.data(); h.innerHTML += `<tr><td>${d.type}</td><td>${d.amount}</td><td style="color:${d.status=='approved'?'#10b981':'#fbbf24'}">${d.status}</td></tr>`; });
                });
            } else { document.getElementById('loginPage').classList.add('active-block'); }
        });

        // --- ACTIONS ---
        document.getElementById('submitReqBtn').onclick = async () => {
            const amt = Number(document.getElementById('amount').value);
            if(amt < 500) return alert("Min PKR 500, sweetie! 😘");
            await addDoc(collection(db, "transactions"), {
                uid: auth.currentUser.uid, amount: amt, type: document.getElementById('actionType').value,
                method: document.getElementById('method').value, status: "pending", timestamp: serverTimestamp()
            });
            alert("Sent! ✅");
        };

        document.getElementById('claimBtn').onclick = async () => {
            const userRef = doc(db, "users", auth.currentUser.uid);
            const snap = await getDoc(userRef);
            const last = snap.data().lastClaim ? snap.data().lastClaim.toMillis() : 0;
            if(Date.now() - last < 86400000) return alert("Wait 24h sweetie! ⏳");
            await updateDoc(userRef, { balance: increment(50), lastClaim: serverTimestamp() });
            alert("PKR 50 Added! 💰");
        };

        function loadAdmin() {
            onSnapshot(query(collection(db, "transactions"), where("status", "==", "pending")), (s) => {
                const list = document.getElementById('adminReqList'); list.innerHTML = "";
                s.forEach(dSnap => {
                    const d = dSnap.data();
                    list.innerHTML += `<div class="glass-card" style="margin:10px 0; font-size:0.8rem;">
                        ${d.type}: PKR ${d.amount} (${d.method})<br>
                        <button onclick="approve('${dSnap.id}', '${d.uid}', ${d.amount}, '${d.type}')" class="btn-main" style="padding:5px; background:#10b981;">Approve</button>
                    </div>`;
                });
            });
        }

        window.approve = async (id, uid, amt, type) => {
            await updateDoc(doc(db, "transactions", id), { status: "approved" });
            const change = type === "Deposit" ? amt : -amt;
            await updateDoc(doc(db, "users", uid), { balance: increment(change) });
            alert("Approved! ✅");
        };
    </script>
</body>
</html>
