<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PakGold | Premier Mining</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; background: #f8fafc; overflow-x: hidden; padding-top: 40px; }
        .glass { background: rgba(255, 255, 255, 0.8); backdrop-filter: blur(12px); border: 1px solid rgba(255,255,255,0.3); }
        .page { display: none; padding-bottom: 100px; }
        .page.active { display: block; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

<div class="fixed top-0 left-0 w-full bg-slate-900 py-2 z-[50000] border-b border-yellow-500/30 overflow-hidden shadow-2xl">
    <div class="flex items-center">
        <div class="bg-yellow-400 text-slate-900 text-[7px] font-black px-2 py-0.5 ml-3 rounded animate-pulse uppercase">👑 VIP ALERTS</div>
        <marquee class="text-[9px] font-black text-white/90 uppercase tracking-widest ml-4">
            New VIP Member: <span class="text-yellow-400">Arsalan_Malik</span> Join 5K Club! • 👑 <span class="text-yellow-400">Zeeshan_786</span> Upgrade Success! • Welcome to PakGold Global Network!
        </marquee>
    </div>
</div>

<div id="p-home" class="page active space-y-6">
    <div id="launch-timer-container" class="mx-4 mt-4 p-6 glass bg-slate-900 border-2 border-yellow-500/50 rounded-[2rem] text-center shadow-2xl">
        <span class="text-[8px] font-black text-yellow-500 uppercase tracking-widest">Official Launch In</span>
        <div class="flex justify-center gap-4 mt-2">
            <div class="text-white"><h4 id="t-hours" class="text-2xl font-black">24</h4><p class="text-[6px] opacity-40 uppercase">Hours</p></div>
            <div class="text-white"><h4 id="t-mins" class="text-2xl font-black">00</h4><p class="text-[6px] opacity-40 uppercase">Mins</p></div>
            <div class="text-white"><h4 id="t-secs" class="text-2xl font-black">00</h4><p class="text-[6px] opacity-40 uppercase">Secs</p></div>
        </div>
    </div>

    <div class="px-6 flex justify-between items-center">
        <div>
            <h2 class="text-[10px] font-black opacity-30 uppercase tracking-widest">Welcome Back</h2>
            <div class="flex items-center gap-2">
                <span id="user-display" class="font-black text-slate-900">Muhammad Nazim</span>
                <span class="px-2 py-0.5 bg-gradient-to-r from-yellow-400 to-orange-500 text-[7px] font-black text-white rounded-full animate-pulse">👑 VIP ELITE</span>
            </div>
        </div>
        <div id="rank-badge" class="bg-white p-2 rounded-xl shadow-sm border border-slate-100 flex items-center gap-2">
            <span class="text-lg">🥇</span>
            <span class="text-[8px] font-black text-yellow-600 uppercase">Gold Legend</span>
        </div>
    </div>

    <div class="mx-4 p-6 glass bg-gradient-to-br from-blue-600 to-blue-900 rounded-[2.5rem] text-center relative overflow-hidden">
        <div class="relative z-10">
            <p class="text-[8px] font-black text-white/50 uppercase tracking-widest">New User Gift</p>
            <h3 class="text-2xl font-black text-white italic">₨ 50.00</h3>
            <button onclick="claimBonus()" class="mt-4 px-8 py-2 bg-white text-blue-900 rounded-xl font-black text-[9px] uppercase">Claim Now</button>
        </div>
    </div>

    <div class="mx-4 p-4 glass rounded-2xl flex justify-between items-center">
        <div class="flex items-center gap-3">
            <span class="text-xl">📅</span>
            <div><p class="text-[10px] font-black uppercase">Daily Reward</p><p class="text-[8px] opacity-40">Get ₨ 2.00 Free</p></div>
        </div>
        <button onclick="dailyCheckIn()" class="bg-slate-900 text-white px-4 py-2 rounded-lg text-[9px] font-black uppercase">Check-in</button>
    </div>

    <div class="mx-4 p-5 glass rounded-[2rem]">
        <h4 class="text-[10px] font-black uppercase mb-4 text-center tracking-widest">🏆 Top Earners</h4>
        <div class="space-y-3" id="leaderboard">
            <div class="flex justify-between text-[10px] font-bold border-b border-slate-100 pb-2"><span>🥇 Rana_Saab</span><span class="text-green-600">₨ 84,500</span></div>
            <div class="flex justify-between text-[10px] font-bold border-b border-slate-100 pb-2"><span>🥈 Ali_Expert</span><span class="text-green-600">₨ 62,300</span></div>
        </div>
    </div>
</div>

<a id="wa-btn" href="https://wa.me/923705519562" class="fixed bottom-24 right-6 w-14 h-14 bg-[#25D366] rounded-full flex items-center justify-center shadow-xl z-[60000] transition-all duration-500 translate-x-24 opacity-0">
    <img src="https://upload.wikimedia.org/wikipedia/commons/6/6b/WhatsApp.svg" class="w-8 h-8 invert">
</a>

<div class="fixed bottom-0 left-0 w-full bg-slate-900/90 py-1.5 z-[40000]">
    <marquee class="text-[8px] font-bold text-green-400 uppercase tracking-widest">
        • Ali_786 Just Withdrew ₨ 1,200 via EasyPaisa • Sana_Khan Just Withdrew ₨ 5,000 via JazzCash •
    </marquee>
</div>

<script>
    // --- 🔐 SECURITY PIN LOGIC ---
    function withdrawWithPin() {
        let pin = prompt("Enter 4-Digit Security PIN:");
        if(pin === "1234") { alert("Withdrawal Approved!"); }
        else { alert("❌ Invalid PIN!"); }
    }

    // --- ⏳ COUNTDOWN LOGIC ---
    setInterval(() => {
        // Logic to update T-Hours, T-Mins, T-Secs
    }, 1000);

    // --- 📱 SMART BUTTON LOGIC ---
    window.onscroll = () => {
        const btn = document.getElementById('wa-btn');
        if (window.scrollY > 150) {
            btn.classList.remove('translate-x-24', 'opacity-0');
            btn.classList.add('translate-x-0', 'opacity-100');
        } else {
            btn.classList.add('translate-x-24', 'opacity-0');
        }
    };

    function claimBonus() { alert("🎁 Request Sent to Admin!"); }
    function dailyCheckIn() { alert("💰 ₨ 2 Added to your wallet!"); }
</script>

</body>
</html>
