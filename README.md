<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Prime Solutions</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #fbbf24; --bg: #020617; --glass: rgba(255, 255, 255, 0.03); --green: #10b981; --red: #ef4444; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: white; min-height: 100vh; overflow-x: hidden; }
        
        .glass-card { background: var(--glass); backdrop-filter: blur(20px); border: 1px solid rgba(255,255,255,0.08); border-radius: 24px; padding: 20px; width: 92%; max-width: 450px; margin: 15px auto; }
        .btn-main { width: 100%; padding: 15px; background: var(--gold); border: none; border-radius: 14px; color: #000; font-weight: 700; cursor: pointer; text-transform: uppercase; transition: 0.3s; }
        input, select { width: 100%; padding: 14px; background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); border-radius: 14px; color: white; margin-bottom: 12px; outline: none; }
        
        #authSection, #dashboardPage, #loadingOverlay { display: none; }
        .active { display: block !important; }
        .flex-active { display: flex !important; }
        
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        .spinner { width: 40px; height: 40px; border: 3px solid rgba(251, 191, 36, 0.1); border-top: 3px solid var(--gold); border-radius: 50%; animation: spin 1s linear infinite; }
    </style>
</head>
<body>

    <div id="loadingOverlay" style="position:fixed; inset:0; background:rgba(2,6,23,0.9); z-index:20000; display:none; flex-direction:column; align-items:center; justify-content:center;">
        <div class="spinner"></div>
    </div>

    <div id="authSection" class="active">
        <h1 style="text-align:center; margin:40px 0; color:var(--gold); font-size: 2.5rem;">PakGold</h1>
        <div class="glass-card">
            <h2 id="authTitle" style="font-size:1.1rem; margin-bottom:20px;">Login to Account</h2>
            <input type="text" id="username" placeholder="Username">
            <input type="password" id="password" placeholder="Password">
            <button id="authBtn" class="btn-main">Continue</button>
            <p id="toggleAuth" style="text-align:center; font-size:0.8rem; margin-top:20px; cursor:pointer; color:#94a3b8;">New user? <span style="color:var(--gold)">Create Account</span></p>
        </div>
    </div>

    <div id="dashboardPage">
        <div class="glass-card" style="border-color:var(--gold);">
            <div style="display:flex; justify-content:space-between; align-items:center;">
                <span style="font-size:0.7rem; color:var(--gold);">Live Mining</span>
                <button onclick="logout()" style="background:none; border:none; color:var(--red); font-size:0.6rem; font-weight:bold; cursor:pointer;">LOGOUT</button>
            </div>
            <div style="text-align:center; margin:30px 0;">
                <p style="font-size:0.6rem; color:#94a3b8;">TOTAL BALANCE</p>
                <h1 style="color:var(--gold); font-size:2.8rem;">PKR <span id="bal">0.00</span></h1>
            </div>
            <div style="font-size: 0.65rem; border-top: 1px solid rgba(255,255,255,0.1); padding-top: 15px;">
                <p><b>EasyPaisa:</b> 03379827882</p>
                <p><b>JazzCash:</b> 03705519562</p>
            </div>
        </div>
        
        <div class="glass-card" style="border-color:#25d366;">
            <p style="font-size:0.7rem; text-align:center; margin-bottom:10px;">webhub262@gmail.com</p>
            <button class="btn-main" style="background:#25d366; color:white;">WhatsApp Group</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-app.js";
        import { getFirestore, doc, getDoc, setDoc } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-firestore.js";

        // Aapki Configuration
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
        const authBtn = document.getElementById('authBtn');
        const toggleAuth = document.getElementById('toggleAuth');
        const currentUser = localStorage.getItem('pakgold_user');

        // Toggle Login/Signup
        toggleAuth.onclick = () => {
            isLoginMode = !isLoginMode;
            document.getElementById('authTitle').innerText = isLoginMode ? "Login to Account" : "Create New Account";
            toggleAuth.innerHTML = isLoginMode ? 'New user? <span style="color:var(--gold)">Create Account</span>' : 'Have an account? <span style="color:var(--gold)">Login</span>';
        };

        // --- AUTH LOGIC (Direct Firestore) ---
        authBtn.onclick = async () => {
            const user = document.getElementById('username').value.toLowerCase().trim();
            const pass = document.getElementById('password').value;

            if(!user || !pass) return alert("Please fill all fields");
            document.getElementById('loadingOverlay').style.display = 'flex';

            try {
                const userRef = doc(db, "users", user);
                const snap = await getDoc(userRef);

                if (isLoginMode) {
                    if (snap.exists() && snap.data().pass === pass) {
                        localStorage.setItem('pakgold_user', user);
                        location.reload();
                    } else {
                        alert("Invalid Username or Password");
                    }
                } else {
                    if (snap.exists()) {
                        alert("Username already exists");
                    } else {
                        await setDoc(userRef, { user, pass, balance: 0, lastBonusDate: "" });
                        localStorage.setItem('pakgold_user', user);
                        location.reload();
                    }
                }
            } catch (e) {
                console.error(e);
                alert("Database Error: Please check your Firestore Rules!");
            }
            document.getElementById('loadingOverlay').style.display = 'none';
        };

        if(currentUser) {
            document.getElementById('authSection').style.display = 'none';
            document.getElementById('dashboardPage').classList.add('active');
            const uRef = doc(db, "users", currentUser);
            getDoc(uRef).then(s => {
                if(s.exists()) document.getElementById('bal').innerText = s.data().balance.toFixed(2);
            });
        }

        window.logout = () => { localStorage.removeItem('pakgold_user'); location.reload(); }
    </script>
</body>
</html>
