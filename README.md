<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Professional Mining Solutions</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #fbbf24; --bg: #020617; --glass: rgba(255, 255, 255, 0.03); --green: #10b981; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: white; min-height: 100vh; overflow-x: hidden; }
        
        /* Components */
        .glass-card { background: var(--glass); backdrop-filter: blur(20px); border: 1px solid rgba(255,255,255,0.08); border-radius: 24px; padding: 20px; width: 92%; max-width: 450px; margin: 15px auto; }
        .brand-name { font-size: 2.5rem; font-weight: 600; color: var(--gold); text-align: center; margin: 20px 0; user-select: none; }
        input { width: 100%; padding: 14px; background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); border-radius: 14px; color: white; margin-bottom: 12px; outline: none; }
        .btn-main { width: 100%; padding: 15px; background: var(--gold); border: none; border-radius: 14px; color: #000; font-weight: 700; cursor: pointer; text-transform: uppercase; transition: 0.3s; }
        
        /* Layouts */
        #welcomeScreen, #authSection, #dashboardPage, #adminPage, #adminTrigger, #loadingOverlay { display: none; }
        .active { display: block !important; }
        .flex-active { display: flex !important; }

        /* Spinner */
        .spinner { width: 40px; height: 40px; border: 3px solid rgba(251, 191, 36, 0.1); border-top: 3px solid var(--gold); border-radius: 50%; animation: spin 1s linear infinite; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        
        .hide-welcome { opacity: 0 !important; transform: translateY(-100%) !important; transition: 0.8s ease-in-out; }
    </style>
</head>
<body>

    <div id="welcomeScreen" class="flex-active" style="position:fixed; top:0; left:0; width:100%; height:100%; background:#020617; z-index:10000; flex-direction:column; align-items:center; justify-content:center; text-align:center;">
        <div class="brand-name" style="font-size: 3.5rem;">PakGold</div>
        <p style="color: #94a3b8; letter-spacing: 2px; font-size: 0.8rem;">THE FUTURE OF MINING</p>
        <button onclick="enterApp()" class="btn-main" style="margin-top: 40px; width: 200px;">Get Started 🚀</button>
    </div>

    <div id="loadingOverlay" style="position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(2,6,23,0.9); z-index:20000; flex-direction:column; align-items:center; justify-content:center;">
        <div class="spinner"></div>
        <p style="color:var(--gold); margin-top:15px; font-size:0.7rem; letter-spacing:1px;">SECURE CONNECTION...</p>
    </div>

    <div id="authSection">
        <h1 class="brand-name" id="secretTap">PakGold</h1>
        <div class="glass-card">
            <h2 id="authTitle" style="margin-bottom: 20px; font-size: 1.1rem;">Sign In</h2>
            <input type="text" id="username" placeholder="Username">
            <input type="password" id="password" placeholder="Password">
            <div id="signupExtra" style="display:none;"><input type="text" id="refBy" placeholder="Referral Code (Optional)"></div>
            <button id="authBtn" class="btn-main">Continue</button>
            <p id="toggleAuth" style="font-size: 0.8rem; color: #94a3b8; margin-top: 20px; text-align: center; cursor: pointer;">New user? <span style="color: var(--gold)">Create Account</span></p>
            
            <div id="adminTrigger" style="margin-top: 15px;">
                <input type="password" id="adminKey" placeholder="System Access Key" style="border-color: #ef4444;">
                <button onclick="checkAdmin()" class="btn-main" style="background:#ef4444; color:white;">Access Vault</button>
            </div>
        </div>
    </div>

    <div id="dashboardPage">
        <div class="glass-card" style="margin-top:10px; padding:10px; border-color:#4facfe;">
            <marquee id="adminNotice" style="font-size: 0.7rem; color: #cbd5e1;">Welcome to PakGold Mining Platform.</marquee>
        </div>

        <div class="glass-card">
            <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px;">
                <span style="color: var(--gold); font-size: 0.8rem;">User: <b id="displayUser"></b></span>
                <button onclick="logout()" style="background:none; border:none; color:#ef4444; font-size:0.7rem;">Logout</button>
            </div>
            
            <div style="text-align: center; background:rgba(251,191,36,0.05); padding:20px; border-radius:20px;">
                <p style="font-size: 0.7rem; color: #94a3b8;">Total Assets</p>
                <h1 style="color: var(--gold); font-size: 2.2rem;">PKR <span id="bal">0.00</span></h1>
                <p style="font-size: 0.6rem; color: var(--green);">Active Rig: <span id="machine">None</span></p>
            </div>

            <button id="claimBtn" class="btn-main" style="background: var(--green); color: white; margin-top: 15px;">Collect Daily Mining Yield</button>
            
            <button onclick="claimLucky()" id="luckyBtn" style="background:linear-gradient(90deg, #ec4899, #8b5cf6); border:none; border-radius:12px; color:white; padding:10px; width:100%; margin-top:10px; font-size:0.7rem; font-weight:bold;">Daily Reward Box 🎁</button>

            <div style="display:grid; grid-template-columns: 1fr 1fr; gap:10px; margin-top:15px;">
                <div class="glass-card" style="margin:0; padding:10px; font-size:0.6rem;">ID: <b id="myCode"></b><br><button onclick="copyRef()" style="background:var(--gold); border:none; border-radius:4px; padding:2px 5px; margin-top:4px;">Copy Link</button></div>
                <div class="glass-card" style="margin:0; padding:10px; font-size:0.6rem;">Payouts:<div id="payoutScroll" style="color:var(--green); margin-top:4px; overflow:hidden; height:15px;"></div></div>
            </div>

            <h4 style="margin-top: 20px; font-size: 0.8rem;">Mining Store</h4>
            <div id="mGrid" style="display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-top:10px;"></div>
        </div>

        <div onclick="window.open('https://wa.me/9231519562')" style="position:fixed; bottom:20px; right:20px; background:#25d366; width:50px; height:50px; border-radius:50%; display:flex; align-items:center; justify-content:center; cursor:pointer; box-shadow:0 10px 20px rgba(0,0,0,0.3);">
            <img src="https://upload.wikimedia.org/wikipedia/commons/6/6b/WhatsApp.svg" style="width:30px;">
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-app.js";
        import { getFirestore, doc, getDoc, setDoc, updateDoc, onSnapshot, increment } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            projectId: "dark-web-9",
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        let currentUser = localStorage.getItem('pakgold_user');
        let isLogin = true;

        window.enterApp = () => {
            new Audio('https://www.soundjay.com/buttons/button-20.mp3').play().catch(()=>{});
            document.getElementById('welcomeScreen').classList.add('hide-welcome');
            setTimeout(() => {
                document.getElementById('welcomeScreen').style.display = 'none';
                if(!currentUser) document.getElementById('authSection').classList.add('active');
                else showDashboard();
            }, 800);
        };

        document.getElementById('toggleAuth').onclick = () => {
            isLogin = !isLogin;
            document.getElementById('authTitle').innerText = isLogin ? "Sign In" : "Create Account";
            document.getElementById('signupExtra').style.display = isLogin ? 'none' : 'block';
        };

        document.getElementById('authBtn').onclick = async () => {
            const user = document.getElementById('username').value.toLowerCase().trim();
            const pass = document.getElementById('password').value;
            const ref = document.getElementById('refBy').value.toLowerCase().trim();
            if(!user || !pass) return alert("Required fields missing.");

            document.getElementById('loadingOverlay').style.display = 'flex';
            setTimeout(async () => {
                const uRef = doc(db, "users", user);
                const snap = await getDoc(uRef);
                if(isLogin) {
                    if(snap.exists() && snap.data().pass === pass) login(user);
                    else { alert("Access Denied."); document.getElementById('loadingOverlay').style.display = 'none'; }
                } else {
                    if(snap.exists()) { alert("Identity already exists."); document.getElementById('loadingOverlay').style.display = 'none'; return; }
                    await setDoc(uRef, { user, pass, balance: 0, machine: "None", profit: 0, lastClaim: 0, lastLucky: 0, referredBy: ref || null });
                    login(user);
                }
            }, 2000);
        };

        function login(u) { localStorage.setItem('pakgold_user', u); location.reload(); }
        window.logout = () => { localStorage.removeItem('pakgold_user'); location.reload(); }

        function showDashboard() {
            document.getElementById('authSection').classList.remove('active');
            document.getElementById('dashboardPage').classList.add('active');
            document.getElementById('displayUser').innerText = currentUser;
            document.getElementById('myCode').innerText = currentUser;
            onSnapshot(doc(db, "users", currentUser), (d) => {
                document.getElementById('bal').innerText = d.data().balance.toFixed(2);
                document.getElementById('machine').innerText = d.data().machine;
            });
        }

        const rigs = [{n:"Nano",p:200,d:14}, {n:"Pro",p:2000,d:160}, {n:"Quantum 🔥",p:10000,d:1500}];
        rigs.forEach(r => {
            document.getElementById('mGrid').innerHTML += `<div class="glass-card" style="margin:0; text-align:center; padding:10px;"><b>${r.n}</b><br><span style="color:var(--green); font-size:0.6rem;">+PKR ${r.d}/day</span><br><button onclick="buyR('${r.n}',${r.p},${r.d})" style="background:var(--gold); border:none; padding:5px; border-radius:8px; margin-top:5px; width:100%; font-size:0.6rem;">PKR ${r.p}</button></div>`;
        });

        window.buyR = async (n, p, d) => {
            const uRef = doc(db, "users", currentUser);
            const snap = await getDoc(uRef);
            if(snap.data().balance < p) return alert("Insufficient balance.");
            if(snap.data().referredBy) await updateDoc(doc(db, "users", snap.data().referredBy), { balance: increment(50) });
            await updateDoc(uRef, { balance: increment(-p), machine: n, profit: d });
            alert(n + " Activated.");
        };

        document.getElementById('claimBtn').onclick = async () => {
            const uRef = doc(db, "users", currentUser);
            const d = (await getDoc(uRef)).data();
            if(d.profit === 0 || Date.now() - d.lastClaim < 86400000) return alert("Requirement not met.");
            await updateDoc(uRef, { balance: increment(d.profit), lastClaim: Date.now() });
            alert("Yield Dispatched.");
        };

        window.claimLucky = async () => {
            const uRef = doc(db, "users", currentUser);
            const d = (await getDoc(uRef)).data();
            if(Date.now() - d.lastLucky < 86400000) return alert("Next reward in 24h.");
            const p = [2, 5, 10, 20, 0][Math.floor(Math.random()*5)];
            alert(p > 0 ? "Reward: PKR " + p : "Better luck next time.");
            await updateDoc(uRef, { balance: increment(p), lastLucky: Date.now() });
        };

        let taps = 0;
        document.getElementById('secretTap').onclick = () => { taps++; if(taps===4) document.getElementById('adminTrigger').style.display='block'; };
        
        setInterval(() => {
            const n = ["Ali", "Sana", "Zain"][Math.floor(Math.random()*3)];
            document.getElementById('payoutScroll').innerText = `@${n}*** withdrawn PKR ${[200, 500][Math.floor(Math.random()*2)]} ✅`;
        }, 4000);

        if(currentUser) enterApp();
    </script>
</body>
</html>
