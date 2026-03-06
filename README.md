<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ApexDaily | Owner & User System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, collection, addDoc, onSnapshot, getDocs, query, where } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        // --- Firebase Config ---
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
        const PKR_RATE = 285;

        // --- AUTH LOGIC ---
        window.handleAuth = async (type) => {
            let u = document.getElementById('username').value.trim();
            let p = document.getElementById('password').value;
            const email = u.includes('@') ? u : `${u}@profitpro.com`;
            try {
                if(type === 'signup') {
                    const res = await createUserWithEmailAndPassword(auth, email, p);
                    await setDoc(doc(db, "users", res.user.uid), { username: u, balance: 0, role: "user" });
                    alert("Account Created!");
                } else { await signInWithEmailAndPassword(auth, email, p); }
            } catch(e) { alert(e.message); }
        };

        // --- REAL-TIME VIEW SWITCHER ---
        onAuthStateChanged(auth, async (user) => {
            const authSec = document.getElementById('authSection');
            const userDash = document.getElementById('userDash');
            const adminDash = document.getElementById('adminDash');

            if(user) {
                authSec.classList.add('hidden');
                if(user.email === "admin@apexdaily.com") {
                    adminDash.classList.remove('hidden');
                    loadAdminRequests();
                } else {
                    userDash.classList.remove('hidden');
                    onSnapshot(doc(db, "users", user.uid), (d) => {
                        if(d.exists()) {
                            document.getElementById('uName').innerText = d.data().username;
                            document.getElementById('uBal').innerText = "$" + d.data().balance.toFixed(2);
                            document.getElementById('uPkr').innerText = (d.data().balance * PKR_RATE).toLocaleString() + " PKR";
                        }
                    });
                }
            } else {
                authSec.classList.remove('hidden');
                userDash.classList.add('hidden');
                adminDash.classList.add('hidden');
            }
        });

        // --- USER: DEPOSIT REQUEST ---
        window.requestDeposit = async () => {
            const amt = parseFloat(document.getElementById('depAmt').value);
            if(amt < 5) return alert("Min $5 sweetie!");
            await addDoc(collection(db, "deposits"), {
                uid: auth.currentUser.uid,
                email: auth.currentUser.email,
                amount: amt,
                status: "pending",
                time: new Date()
            });
            alert("Request Sent to Admin!");
        };

        // --- ADMIN: PROFIT & APPROVAL ---
        window.sendProfit = async () => {
            const p = parseFloat(document.getElementById('pPercent').value);
            const snaps = await getDocs(collection(db, "users"));
            snaps.forEach(async (uDoc) => {
                if(uDoc.data().role !== "admin") {
                    const newBal = (uDoc.data().balance || 0) * (1 + p/100);
                    await updateDoc(doc(db, "users", uDoc.id), { balance: newBal });
                }
            });
            alert("Profit Distributed!");
        };

        async function loadAdminRequests() {
            const q = query(collection(db, "deposits"), where("status", "==", "pending"));
            onSnapshot(q, (snaps) => {
                const list = document.getElementById('adminList');
                list.innerHTML = "";
                snaps.forEach((d) => {
                    const data = d.data();
                    list.innerHTML += `<div class="p-4 bg-gray-700 rounded-xl flex justify-between">
                        <span>${data.email} - <b>$${data.amount}</b></span>
                        <button onclick="approveDep('${d.id}', '${data.uid}', ${data.amount})" class="bg-green-500 px-3 py-1 rounded">Approve</button>
                    </div>`;
                });
            });
        }

        window.approveDep = async (id, uid, amt) => {
            const uRef = doc(db, "users", uid);
            const uSnap = await getDoc(uRef);
            await updateDoc(uRef, { balance: (uSnap.data().balance || 0) + amt });
            await updateDoc(doc(db, "deposits", id), { status: "approved" });
            alert("Approved!");
        };

        window.logout = () => signOut(auth);
    </script>
</head>
<body class="bg-[#0b0f1a] text-white p-5 font-sans min-h-screen flex items-center justify-center">

    <div id="authSection" class="w-full max-w-md bg-[#161b2c] p-8 rounded-3xl border border-gray-800 shadow-2xl">
        <h1 class="text-3xl font-bold text-center text-blue-500 mb-6 font-serif">ApexDaily</h1>
        <input id="username" type="text" placeholder="Username" class="w-full p-4 mb-4 bg-[#1f263d] rounded-xl outline-none">
        <input id="password" type="password" placeholder="Password" class="w-full p-4 mb-6 bg-[#1f263d] rounded-xl outline-none">
        <div class="flex gap-4">
            <button onclick="handleAuth('login')" class="flex-1 bg-blue-600 p-4 rounded-xl font-bold">Login</button>
            <button onclick="handleAuth('signup')" class="flex-1 bg-gray-700 p-4 rounded-xl font-bold">Signup</button>
        </div>
    </div>

    <div id="userDash" class="hidden w-full max-w-4xl">
        <div class="flex justify-between mb-6">
            <h2 class="text-xl">Hi, <span id="uName" class="text-blue-400 font-bold">...</span></h2>
            <button onclick="logout()" class="text-red-400 underline">Logout</button>
        </div>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div class="bg-gradient-to-r from-blue-600 to-blue-800 p-8 rounded-[2rem] shadow-xl">
                <p class="text-sm opacity-80 uppercase tracking-widest">Balance</p>
                <h1 id="uBal" class="text-5xl font-black my-2">$0.00</h1>
                <p id="uPkr" class="font-bold">0 PKR</p>
            </div>
            <div class="bg-[#161b2c] p-8 rounded-[2rem] border border-gray-800">
                <h3 class="font-bold mb-4">Quick Deposit</h3>
                <input id="depAmt" type="number" placeholder="Enter $ (Min 5)" class="w-full p-3 bg-[#1f263d] rounded-xl mb-4 outline-none">
                <button onclick="requestDeposit()" class="w-full bg-green-600 p-3 rounded-xl font-bold">Send Deposit Request</button>
            </div>
        </div>
    </div>

    <div id="adminDash" class="hidden w-full max-w-4xl bg-[#1c2237] p-8 rounded-[2rem] border-2 border-yellow-500 shadow-2xl">
        <div class="flex justify-between mb-8">
            <h1 class="text-2xl font-black text-yellow-500">APEX OWNER PANEL</h1>
            <button onclick="logout()" class="text-red-500 font-bold">Secure Exit</button>
        </div>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
            <div class="bg-[#252d47] p-6 rounded-2xl">
                <h3 class="font-bold mb-4 text-yellow-500">Distribute Profit %</h3>
                <input id="pPercent" type="number" placeholder="Enter % (e.g. 2)" class="w-full p-3 bg-gray-900 rounded-xl mb-4 outline-none">
                <button onclick="sendProfit()" class="w-full bg-yellow-600 p-3 rounded-xl font-bold">Add Profit to All</button>
            </div>
            <div class="bg-[#252d47] p-6 rounded-2xl">
                <h3 class="font-bold mb-4 text-green-500">Pending Requests</h3>
                <div id="adminList" class="space-y-3 overflow-y-auto max-h-40"></div>
            </div>
        </div>
    </div>

</body>
</html>
