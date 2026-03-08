<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PakGold | Prime Solutions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root { --primary: #fbbf24; --bg: #f8fafc; --card: #ffffff; --text: #0f172a; }
        .dark { --bg: #0f172a; --card: #1e293b; --text: #f8fafc; }
        body { background: var(--bg); color: var(--text); transition: 0.3s; font-family: 'Inter', sans-serif; padding-bottom: 90px; }
        .glass { background: var(--card); border: 1px solid rgba(255,255,255,0.1); border-radius: 1.5rem; }
        .page { display: none; } .page.active { display: block; }
        .tab-active { color: #fbbf24 !important; border-top: 2px solid #fbbf24; }
    </style>
</head>
<body class="">

    <div id="p-auth" class="page active px-6 pt-20">
        <div class="text-center mb-10">
            <i class="fa-solid fa-coins text-5xl text-yellow-500 mb-4"></i>
            <h1 class="text-3xl font-black italic uppercase">PakGold</h1>
            <p class="text-[10px] font-bold opacity-50 uppercase tracking-widest">Powered by Prime Solutions</p>
        </div>
        
        <div class="glass p-8 space-y-4 shadow-2xl">
            <h2 id="auth-title" class="text-xl font-black uppercase text-center">Login</h2>
            <div class="space-y-3">
                <div class="relative">
                    <i class="fa-solid fa-user absolute left-4 top-4 opacity-30"></i>
                    <input type="text" placeholder="Username" class="w-full bg-slate-100 dark:bg-slate-800 p-4 pl-12 rounded-xl border-none text-xs font-bold outline-none">
                </div>
                <div class="relative">
                    <i class="fa-solid fa-lock absolute left-4 top-4 opacity-30"></i>
                    <input type="password" placeholder="Password" class="w-full bg-slate-100 dark:bg-slate-800 p-4 pl-12 rounded-xl border-none text-xs font-bold outline-none">
                </div>
            </div>
            <button onclick="navigate('p-home')" class="w-full py-4 bg-yellow-500 text-slate-900 rounded-xl font-black uppercase text-[11px] shadow-lg">Enter Dashboard</button>
            <p class="text-center text-[10px] font-bold opacity-40 uppercase cursor-pointer" onclick="toggleAuth()">New User? Create Account</p>
        </div>
    </div>

    <div id="p-home" class="page px-4 space-y-6 pt-12">
        <div class="flex justify-between items-center px-2">
            <div>
                <p class="text-[9px] font-black opacity-40 uppercase">Welcome back,</p>
                <h3 class="font-black">Muhammad Nazim <i class="fa-solid fa-circle-check text-blue-500 text-[10px]"></i></h3>
            </div>
            <button onclick="toggleDarkMode()" class="w-10 h-10 glass flex items-center justify-center shadow-lg text-yellow-500">
                <i class="fa-solid fa-moon"></i>
            </button>
        </div>

        <div class="p-8 glass bg-slate-900 text-white text-center relative overflow-hidden shadow-2xl">
            <div class="absolute top-0 right-0 p-4 opacity-10 text-8xl"><i class="fa-solid fa-vault"></i></div>
            <p class="text-[9px] font-black text-yellow-500 uppercase tracking-[4px]">Available Funds</p>
            <h2 class="text-4xl font-black italic mt-2">₨ <span id="balance">552.00</span></h2>
            <div class="flex justify-center gap-4 mt-6">
                <button onclick="navigate('p-dep')" class="bg-yellow-500 text-slate-900 px-4 py-2 rounded-lg text-[10px] font-black uppercase flex items-center gap-2">
                    <i class="fa-solid fa-plus"></i> Deposit
                </button>
                <button onclick="handleWithdraw()" class="bg-white/10 text-white px-4 py-2 rounded-lg text-[10px] font-black uppercase flex items-center gap-2">
                    <i class="fa-solid fa-wallet"></i> Withdraw
                </button>
            </div>
        </div>

        <div class="space-y-4">
            <div class="flex items-center gap-2 px-2">
                <i class="fa-solid fa-microchip text-yellow-500"></i>
                <h3 class="text-[11px] font-black uppercase tracking-widest">Mining Centers</h3>
            </div>
            <div id="plans-container" class="grid grid-cols-1 gap-4">
                </div>
        </div>
    </div>

    <div id="p-dep" class="page px-4 space-y-6 pt-12">
        <div class="flex items-center gap-4 mb-6">
            <button onclick="navigate('p-home')" class="text-xl"><i class="fa-solid fa-arrow-left"></i></button>
            <h2 class="text-xl font-black uppercase">Add Funds</h2>
        </div>
        
        <div class="glass p-6 space-y-6">
            <div class="grid grid-cols-2 gap-3">
                <button onclick="setBank('EasyPaisa', '03379827882')" class="bg-green-100 p-4 rounded-2xl flex flex-col items-center gap-2 border-2 border-green-500/20">
                    <i class="fa-solid fa-mobile-screen text-green-600"></i><span class="text-[9px] font-black text-green-700 uppercase">EasyPaisa</span>
                </button>
                <button onclick="setBank('JazzCash', '03705519562')" class="bg-red-100 p-4 rounded-2xl flex flex-col items-center gap-2 border-2 border-red-500/20">
                    <i class="fa-solid fa-bolt text-red-600"></i><span class="text-[9px] font-black text-red-700 uppercase">Jazz/Sada</span>
                </button>
            </div>

            <div class="bg-slate-100 dark:bg-slate-800 p-6 rounded-3xl text-center">
                <p id="b-name" class="text-[9px] font-black opacity-40 uppercase">Select Method</p>
                <h4 id="b-acc" class="text-xl font-black mt-1">----</h4>
                <p class="text-[8px] font-bold text-blue-500 mt-1 uppercase italic">Owner: Muhammad Nazim</p>
            </div>

            <input type="number" placeholder="Enter Amount" class="w-full bg-slate-50 dark:bg-slate-800 p-4 rounded-xl text-xs font-bold border-none outline-none">
            
            <label class="block w-full py-4 border-2 border-dashed border-slate-300 rounded-2xl text-center cursor-pointer">
                <i class="fa-solid fa-camera mb-1 opacity-40"></i>
                <p class="text-[9px] font-black uppercase opacity-60">Upload Screenshot</p>
                <input type="file" class="hidden">
            </label>

            <button onclick="alert('Sent!')" class="w-full py-4 bg-slate-900 dark:bg-yellow-500 dark:text-slate-900 text-white rounded-xl font-black uppercase text-[11px]">Submit Payment</button>
        </div>
    </div>

    <div id="bot-nav" class="fixed bottom-0 left-0 w-full bg-white dark:bg-slate-900 border-t dark:border-slate-800 py-3 px-8 flex justify-between items-center z-[5000] hidden">
        <button onclick="navigate('p-home')" class="flex flex-col items-center gap-1 tab-active"><i class="fa-solid fa-house"></i><span class="text-[7px] font-black uppercase">Home</span></button>
        <button onclick="alert('Admin Panel')" class="flex flex-col items-center gap-1 text-slate-400"><i class="fa-solid fa-gear"></i><span class="text-[7px] font-black uppercase">Admin</span></button>
        <button onclick="location.reload()" class="flex flex-col items-center gap-1 text-red-500"><i class="fa-solid fa-power-off"></i><span class="text-[7px] font-black uppercase">Logout</span></button>
    </div>

    <script>
        // 28 Regular + 5 Special Plans Generation
        function loadPlans() {
            const container = document.getElementById('plans-container');
            let html = '';
            // 28 Regular
            for(let i=1; i<=28; i++) {
                html += `
                <div class="glass p-6 flex justify-between items-center shadow-lg">
                    <div>
                        <p class="text-[8px] font-black text-blue-500 uppercase italic">Node R-${i}</p>
                        <h4 class="text-xl font-black">₨ ${200 + (i-1)*400}</h4>
                        <p class="text-[10px] font-bold text-green-500">+ ₨ ${12 + (i-1)*20}/day</p>
                    </div>
                    <button onclick="buyNow()" class="bg-slate-900 dark:bg-slate-800 text-white px-6 py-2 rounded-lg text-[9px] font-black uppercase">Buy Now</button>
                </div>`;
            }
            // 5 Special
            for(let j=1; j<=5; j++) {
                html += `
                <div class="glass p-6 bg-slate-900 text-white border-2 border-yellow-500/50 shadow-2xl">
                    <div class="flex justify-between">
                        <p class="text-[8px] font-black text-yellow-500 uppercase">Special VIP S-0${j}</p>
                        <span class="text-[8px] bg-yellow-500 text-slate-900 px-2 rounded-full font-black animate-pulse">HOT</span>
                    </div>
                    <h4 class="text-2xl font-black italic mt-2">₨ ${5000 * j}</h4>
                    <div class="flex gap-4 mt-2">
                        <span class="text-[9px] font-bold opacity-60 italic">Daily: ₨ ${450 * j}</span>
                        <span class="text-[9px] font-bold opacity-60 italic">Days: ${10 + j}</span>
                    </div>
                    <button onclick="buyNow()" class="w-full mt-4 py-3 bg-yellow-500 text-slate-900 rounded-xl font-black uppercase text-[10px]">Buy Special</button>
                </div>`;
            }
            container.innerHTML = html;
        }

        function navigate(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('bot-nav').classList.toggle('hidden', id === 'p-auth');
            if(id === 'p-home') loadPlans();
        }

        function toggleDarkMode() { document.body.classList.toggle('dark'); }
        function setBank(m, n) { document.getElementById('b-name').innerText = m; document.getElementById('b-acc').innerText = n; }
        function handleWithdraw() { const pin = prompt("Enter 4-Digit Security PIN:"); if(pin === "1234") alert("Requested!"); }
        function buyNow() { alert("Process Starting..."); }
    </script>
</body>
</html>
