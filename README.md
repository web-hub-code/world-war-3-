<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Prime Solutions Official</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #fbbf24; --bg: #020617; --glass: rgba(255, 255, 255, 0.03); --green: #10b981; --red: #ef4444; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: white; min-height: 100vh; overflow-x: hidden; }
        
        /* 🛡️ SCREEN SYSTEM (FIXED) */
        .page { display: none; width: 100%; min-height: 100vh; padding-bottom: 90px; }
        .page.active { display: block !important; }

        .glass-card { background: var(--glass); backdrop-filter: blur(20px); border: 1px solid rgba(255,255,255,0.08); border-radius: 24px; padding: 20px; width: 92%; max-width: 450px; margin: 15px auto; }
        .btn-main { width: 100%; padding: 15px; background: var(--gold); border: none; border-radius: 14px; color: #000; font-weight: 700; cursor: pointer; text-transform: uppercase; transition: 0.3s; }
        input { width: 100%; padding: 14px; background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); border-radius: 14px; color: white; margin-bottom: 12px; outline: none; }
        
        .machine-grid { display: grid; grid-template-columns: 1fr; gap: 10px; margin-top: 15px; }
        .machine-card { background: rgba(255,255,255,0.02); border-left: 4px solid var(--gold); padding: 15px; border-radius: 12px; display: flex; justify-content: space-between; align-items: center; border: 1px solid rgba(255,255,255,0.05); }
        .premium { border-left-color: var(--green); background: rgba(16, 185, 129, 0.05); }

        nav { position: fixed; bottom: 0; width: 100%; background: #0f172a; display: flex; justify-content: space-around; padding: 15px; border-top: 1px solid rgba(255,255,255,0.1); z-index: 1000; }
        nav div { font-size: 0.65rem; color: #94a3b8; cursor: pointer; text-align: center; }
        nav div.active { color: var(--gold); }
    </style>
</head>
<body>

    <div id="authPage" class="page active">
        <h1 style="text-align:center; margin-top:60px; color:var(--gold); font-size: 3rem;">PakGold</h1>
        <div class="glass-card">
            <h2 style="font-size:1rem; margin-bottom:20px; text-align:center; opacity:0.8;">Secure Access</h2>
            <input type="text" id="username" placeholder="Username">
            <input type="password" id="password" placeholder="Password">
            <button id="authBtn" class="btn-main">Get Started 🚀</button>
            <p id="toggleAuth" style="text-align:center; font-size:0.8rem; margin-top:20px; cursor:pointer; color:#94a3b8;">New? <span style="color:var(--gold)">Create Account</span></p>
        </div>
    </div>

    <div id="dashPage" class="page">
        <div class="glass-card" style="border-color:var(--gold); margin-top:20px;">
            <p style="font-size:0.6rem; color:#94a3b8; letter-spacing:1px;">TOTAL EARNINGS</p>
            <h1 style="color:var(--gold); font-size:2.8rem;">PKR <span id="bal">0.00</span></h1>
            <div style="width:100%; height:6px; background:rgba(255,255,255,0.05); border-radius:10px; margin-top:15px; overflow:hidden;">
                <div id="miningBar" style="width:0%; height:100%; background:var(--gold); box-shadow: 0 0 15px var(--gold); transition:1s;"></div>
            </div>
        </div>

        <h3 style="margin:20px 0 10px 20px; font-size:0.9rem;">Mining Machines (15+5)</h3>
        <div id="machineContainer" class="glass-card" style="max-height: 400px; overflow-y: auto;">
            </div>
    </div>

    <div id="infoPage" class="page">
        <div class="glass-card">
            <h3 style="color:var(--gold); margin-bottom:10px;">About PakGold</h3>
            <p style="font-size:0.75rem; color:#94a3b8; line-height:1.6;">PakGold is an official project by **Prime Solutions**, designed for automated digital mining.</p>
            
            <h3 style="color:var(--gold); margin-top:25px; margin-bottom:10px;">Privacy Policy</h3>
            <p style="font-size:0.75rem; color:#94a3b8; line-height:1.6;">All investments are secure. Withdrawals are processed within 24 hours to your linked account.</p>

            <h3 style="color:var(--gold); margin-top:25px; margin-bottom:10px;">Contact Support</h3>
            <p style="font-size:0.75rem; color:#94a3b8;">Email: webhub262@gmail.com<br>WhatsApp Support: Active 24/7</p>
        </div>
    </div>

    <div id="walletPage" class="page">
        <div class="glass-card">
            <h3 style="color:var(--gold); margin-bottom:20px;">Deposit / Withdraw</h3>
            <div style="background:rgba(255,255,255,0.02); padding:15px; border-radius:12px; font-size:0.75rem; margin-bottom:20px;">
                <p><b>EasyPaisa:</b> 03379827882</p>
                <p><b>JazzCash:</b> 03705519562</p>
            </div>
            <input type="number" id="payAmount" placeholder="Enter Amount">
            <input type="text" id="payTid" placeholder="Transaction ID / Account">
            <button class="btn-main">Submit Request</button>
        </div>
    </div>

    <nav id="navbar" style="display:none;">
        <div onclick="switchPage('dashPage')">🏠<br>Home</div>
        <div onclick="switchPage('walletPage')">💰<br>Wallet</div>
        <div onclick="switchPage('infoPage')">📄<br>Company</div>
        <div onclick="logout()">❌<br>Logout</div>
    </nav>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-app.js";
        import { getFirestore, doc, getDoc, setDoc, onSnapshot } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-firestore.js";

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
        const currentUser = localStorage.getItem('pakgold_user');

        // 🛠️ AUTOMATIC MACHINE LIST GENERATION (15+5)
        const machines = [
            { id: "M-01", cost: 200, profit: 12 }, { id: "M-02", cost: 500, profit: 30 },
            { id: "M-03", cost: 1000, profit: 65 }, { id: "M-04", cost: 2000, profit: 140 },
            { id: "M-05", cost: 3500, profit: 250 }, { id: "M-06", cost: 5000, profit: 380 },
            { id: "M-07", cost: 7000, profit: 550 }, { id: "M-08", cost: 9000, profit: 720 },
            { id: "M-09", cost: 11000, profit: 900 }, { id: "M-10", cost: 13000, profit: 1100 },
            { id: "M-11", cost: 15000, profit: 1300 }, { id: "M-12", cost: 17000, profit: 1550 },
            { id: "M-13", cost: 20000, profit: 1900 }, { id: "M-14", cost: 23000, profit: 2200 },
            { id: "M-15", cost: 25000, profit: 2500 },
            // Premium Machines (5 Units)
            { id: "P-01", cost: 40000, profit: 4500, type: 'premium' },
            { id: "P-02", cost: 60000, profit: 7000, type: 'premium' },
            { id: "P-03", cost: 80000, profit: 10000, type: 'premium' },
            { id: "P-04", cost: 100000, profit: 13500, type: 'premium' },
            { id: "P-15", cost: 150000, profit: 22000, type: 'premium' }
        ];

        const container = document.getElementById('machineContainer');
        machines.forEach(m => {
            container.innerHTML += `
                <div class="machine-card ${m.type === 'premium' ? 'premium' : ''}">
                    <div><b>${m.id} Mining</b><br><small>Cost: PKR ${m.cost}</small></div>
                    <div style="text-align:right;"><span style="color:var(--gold)">PKR ${m.profit}</span><br><small>Daily</small></div>
                </div>`;
        });

        // 🔄 NAVIGATION LOGIC
        window.switchPage = (id) => {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
        };

        if(currentUser) {
            switchPage('dashPage');
            document.getElementById('navbar').style.display = 'flex';
            onSnapshot(doc(db, "users", currentUser), (s) => {
                if(s.exists()) document.getElementById('bal').innerText = s.data().balance.toFixed(2);
            });
            let p = 0; setInterval(() => { p = p >= 100 ? 0 : p + 5; document.getElementById('miningBar').style.width = p + "%"; }, 1500);
        }

        // 🔐 LOGIN/SIGNUP LOGIC
        document.getElementById('authBtn').onclick = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("Fill all details");
            const ref = doc(db, "users", u);
            const snap = await getDoc(ref);
            if(snap.exists() && snap.data().pass === p) { localStorage.setItem('pakgold_user', u); location.reload(); }
            else if(!snap.exists()) { await setDoc(ref, { user: u, pass: p, balance: 0 }); localStorage.setItem('pakgold_user', u); location.reload(); }
            else { alert("Failed"); }
        };

        window.logout = () => { localStorage.removeItem('pakgold_user'); location.reload(); }
    </script>
</body>
</html>
