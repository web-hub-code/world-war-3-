<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ProfitPro | $ & PKR Dashboard</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        // --- PASTE YOUR FIREBASE CONFIG HERE SWEETIE ---
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
        const PKR_RATE = 285; // 1$ = 285 PKR

        // --- 1. LOGIN / SIGNUP LOGIC ---
        window.handleAuth = async (type) => {
            const user = document.getElementById('username').value;
            const pass = document.getElementById('password').value;
            const email = user + "@profitpro.com"; // Username to Email conversion

            try {
                if (type === 'signup') {
                    const res = await createUserWithEmailAndPassword(auth, email, pass);
                    await setDoc(doc(db, "users", res.user.uid), {
                        username: user,
                        balance: 0,
                        role: "user"
                    });
                    alert("Account Created!");
                } else {
                    await signInWithEmailAndPassword(auth, email, pass);
                }
            } catch (err) { alert(err.message); }
        };

        // --- 2. UPDATE UI BASED ON AUTH ---
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                document.getElementById('authSection').classList.add('hidden');
                document.getElementById('mainDashboard').classList.remove('hidden');
                
                // Fetch Balance from Firestore
                const docSnap = await getDoc(doc(db, "users", user.uid));
                if (docSnap.exists()) {
                    const data = docSnap.data();
                    document.getElementById('userDisplay').innerText = data.username;
                    document.getElementById('balanceID').innerText = "$" + data.balance.toFixed(2);
                    document.getElementById('pkrBalance').innerText = (data.balance * PKR_RATE).toLocaleString() + " PKR";
                }
            } else {
                document.getElementById('authSection').classList.remove('hidden');
                document.getElementById('mainDashboard').classList.add('hidden');
            }
        });

        // --- 3. CONVERSION LOGIC ---
        window.calcDeposit = () => {
            const usd = document.getElementById('depAmount').value;
            document.getElementById('pkrPayable').innerText = (usd * PKR_RATE).toLocaleString() + " PKR";
        };

        window.logout = () => signOut(auth);

    </script>
</head>
<body class="bg-gray-900 text-white font-sans min-h-screen flex items-center justify-center">

    <div id="authSection" class="w-full max-w-md p-8 bg-gray-800 rounded-2xl shadow-xl border border-gray-700">
        <h2 class="text-3xl font-bold text-center text-blue-500 mb-6">ProfitPro Login</h2>
        <input id="username" type="text" placeholder="Username" class="w-full p-3 mb-4 bg-gray-700 rounded-lg outline-none border border-transparent focus:border-blue-500">
        <input id="password" type="password" placeholder="Password" class="w-full p-3 mb-6 bg-gray-700 rounded-lg outline-none border border-transparent focus:border-blue-500">
        <div class="flex gap-4">
            <button onclick="handleAuth('login')" class="flex-1 bg-blue-600 py-3 rounded-lg font-bold">Login</button>
            <button onclick="handleAuth('signup')" class="flex-1 bg-gray-600 py-3 rounded-lg font-bold">Signup</button>
        </div>
    </div>

    <div id="mainDashboard" class="hidden w-full max-w-4xl p-6">
        <div class="flex justify-between items-center mb-10">
            <div>
                <h1 class="text-xl text-gray-400">Welcome, <span id="userDisplay" class="text-blue-400 font-bold">...</span></h1>
                <p class="text-sm text-gray-500 italic">Account Status: Active</p>
            </div>
            <button onclick="logout()" class="text-red-400 text-sm underline">Logout</button>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div class="bg-gray-800 p-8 rounded-3xl border-b-4 border-blue-500 shadow-lg">
                <p class="text-gray-400 uppercase text-xs tracking-widest">Total Balance</p>
                <h2 id="balanceID" class="text-5xl font-extrabold my-2 text-white">$0.00</h2>
                <p id="pkrBalance" class="text-blue-400 font-semibold text-lg">0 PKR</p>
            </div>

            <div class="bg-gray-800 p-8 rounded-3xl border-b-4 border-green-500 shadow-lg">
                <h3 class="text-lg font-bold mb-4">Quick Deposit</h3>
                <input id="depAmount" type="number" oninput="calcDeposit()" placeholder="Enter $ (Min 5)" class="w-full p-3 bg-gray-700 rounded-lg mb-4 outline-none">
                <div class="flex justify-between items-center mb-4">
                    <span class="text-sm text-gray-400 italic">Pay in PKR:</span>
                    <span id="pkrPayable" class="font-bold text-green-400">0 PKR</span>
                </div>
                <button class="w-full bg-green-600 py-2 rounded-lg font-bold hover:bg-green-700">Get Payment Details</button>
            </div>
        </div>
        
        <div class="mt-10 flex justify-center gap-6 opacity-50 grayscale hover:grayscale-0 transition duration-500">
            <span>JazzCash</span> | <span>EasyPaisa</span> | <span>Binance</span> | <span>SadaPay</span>
        </div>
    </div>

</body>
</html>
