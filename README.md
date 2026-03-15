<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Pro Max | Elite Recruitment Hub</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f4f7fa; color: #0f172a; transition: 0.3s; }
        .tab-content { display: none; }
        .active-tab { display: block; animation: fadeInUp 0.4s ease; }
        @keyframes fadeInUp { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        
        /* Advanced Chat & UI */
        #chatBox, #supportBox { height: 400px; overflow-y: auto; scroll-behavior: smooth; display: flex; flex-direction: column; gap: 8px; }
        .msg-bubble { background: white; padding: 12px 16px; border-radius: 20px 20px 20px 5px; max-width: 85%; box-shadow: 0 4px 10px rgba(0,0,0,0.03); border: 1px solid #e2e8f0; }
        .self-msg { align-self: flex-end; background: #1e3a8a; color: white; border-radius: 20px 20px 5px 20px; }
        .reply-preview { font-size: 10px; opacity: 0.7; border-left: 2px solid #3b82f6; padding-left: 5px; margin-bottom: 5px; }
        
        /* Avatar & Lockdown */
        .avatar { width: 40px; height: 40px; border-radius: 50%; border: 2px solid #1e3a8a; object-fit: cover; }
        #lockOverlay { display: none; position: fixed; inset: 0; background: #000; z-index: 9999; color: white; display: flex; align-items: center; justify-content: center; text-align: center; }
        #banOverlay { display: none; position: fixed; inset: 0; background: white; z-index: 9998; flex-direction: column; align-items: center; justify-content: center; }
    </style>
</head>
<body class="pb-20">

    <div id="lockOverlay" class="hidden">
        <div>
            <h1 class="text-6xl font-black text-red-600 mb-4 animate-pulse">LOCKDOWN</h1>
            <p class="text-xl opacity-60">System is under maintenance by Admin.</p>
        </div>
    </div>

    <div id="banOverlay" class="hidden">
        <h1 class="text-8xl mb-4">🚫</h1>
        <h2 class="text-4xl font-black text-red-600 mb-2">ACCESS DENIED</h2>
        <p class="font-bold text-gray-500 text-center px-6">Your account has been restricted for violating terms.</p>
    </div>

    <div id="loginScreen" class="fixed inset-0 bg-[#0f172a] z-[5000] flex items-center justify-center p-6 hidden">
        <div class="bg-white p-12 rounded-[50px] max-w-md w-full text-center shadow-2xl">
            <h2 class="text-4xl font-black text-blue-900 mb-4 italic">PRIME ACADEMY</h2>
            <input id="uName" type="text" placeholder="Full Name" class="w-full p-5 bg-gray-50 rounded-2xl mb-4 outline-none border-2 focus:border-blue-600 text-center font-bold">
            <p class="text-[10px] font-bold text-gray-400 mb-4 uppercase">Select Avatar</p>
            <div class="flex justify-center gap-4 mb-8">
                <img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Felix" class="avatar cursor-pointer hover:scale-125" onclick="setAvatar(this.src)">
                <img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Aneka" class="avatar cursor-pointer hover:scale-125" onclick="setAvatar(this.src)">
                <img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Jack" class="avatar cursor-pointer hover:scale-125" onclick="setAvatar(this.src)">
            </div>
            <button onclick="saveUser()" class="w-full bg-blue-900 text-white py-5 rounded-2xl font-black text-lg shadow-xl">START TRAINING</button>
        </div>
    </div>

    <nav class="bg-white sticky top-0 z-50 p-4 border-b flex justify-between items-center px-6 shadow-sm">
        <div class="flex items-center gap-2">
            <img id="myAvatar" src="" class="w-8 h-8 rounded-full border border-blue-900">
            <h1 class="font-black text-lg italic text-blue-900">GBTS ELITE</h1>
        </div>
        <div class="flex gap-4 md:gap-6">
            <button onclick="showTab('levelsTab')" class="text-[10px] font-black uppercase text-blue-900">Training</button>
            <button onclick="showTab('chatTab')" class="text-[10px] font-black uppercase text-gray-500">Group Chat</button>
            <button onclick="showTab('supportTab')" class="text-[10px] font-black uppercase text-gray-500">Support</button>
            <button onclick="showTab('godTab')" id="godBtn" class="hidden text-[10px] font-black uppercase text-red-600 border px-2 border-red-600 rounded">God Mode</button>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-4 mt-6">
        
        <div id="levelsTab" class="tab-content active-tab">
            <h2 class="text-3xl font-black mb-8 italic uppercase tracking-tighter">Mission Progress</h2>
            <div id="levelGrid" class="grid grid-cols-2 md:grid-cols-5 gap-6"></div>
        </div>

        <div id="chatTab" class="tab-content">
            <div class="max-w-3xl mx-auto bg-white rounded-[40px] p-6 shadow-xl border border-gray-100">
                <div id="replyArea" class="hidden bg-blue-50 p-3 rounded-xl mb-2 flex justify-between items-center text-xs italic">
                    <span id="replyTxt"></span>
                    <button onclick="cancelRep()" class="text-red-500 font-black">×</button>
                </div>
                <div id="chatBox"></div>
                <div class="flex gap-2 mt-4 bg-gray-50 p-2 rounded-3xl">
                    <input id="chatInp" type="text" placeholder="Ask anything..." class="flex-1 p-4 bg-transparent outline-none font-medium">
                    <button onclick="sendGlobalMsg()" class="bg-blue-900 text-white px-8 rounded-2xl font-black">SEND</button>
                </div>
            </div>
        </div>

        <div id="supportTab" class="tab-content">
            <div class="max-w-3xl mx-auto bg-slate-900 rounded-[40px] p-8 text-white shadow-2xl">
                <h3 class="text-2xl font-black mb-2 text-blue-400 italic">PRIVATE ADMIN SUPPORT</h3>
                <p class="text-[10px] text-slate-400 mb-6 font-bold uppercase tracking-widest">Your conversation is strictly private</p>
                <div id="supportBox" class="mb-4"></div>
                <div class="flex gap-2">
                    <input id="supportInp" type="text" placeholder="Message Admin..." class="flex-1 p-4 rounded-2xl bg-slate-800 text-white outline-none border border-slate-700">
                    <button onclick="sendPrivateMsg()" class="bg-blue-600 px-6 rounded-2xl font-black">CHAT</button>
                </div>
            </div>
        </div>

        <div id="godTab" class="tab-content bg-white p-10 rounded-[50px] shadow-2xl border-t-8 border-blue-900">
            <div class="flex justify-between items-center mb-8">
                <h2 class="text-3xl font-black text-slate-900 italic">SYSTEM OVERRIDE</h2>
                <button onclick="toggleLock()" class="bg-red-600 text-white px-6 py-2 rounded-xl font-bold text-xs">LOCKDOWN SITE</button>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <div class="bg-gray-50 p-6 rounded-3xl">
                    <h4 class="font-black text-xs text-blue-900 uppercase mb-4">Active Approval Requests</h4>
                    <div id="pendingReqs"></div>
                </div>
                <div class="bg-gray-50 p-6 rounded-3xl">
                    <h4 class="font-black text-xs text-red-600 uppercase mb-4">User Ban Control</h4>
                    <div class="flex gap-2">
                        <input id="banUser" type="text" placeholder="User Name" class="flex-1 p-3 rounded-xl border outline-none">
                        <button onclick="banAction(true)" class="bg-red-600 text-white px-4 rounded-xl font-bold">BAN</button>
                        <button onclick="banAction(false)" class="bg-green-600 text-white px-4 rounded-xl font-bold">OK</button>
                    </div>
                </div>
            </div>
        </div>
    </main>

    <footer class="fixed bottom-0 left-0 right-0 p-4 text-center">
        <span onclick="checkGodTaps()" class="text-[8px] text-gray-300 opacity-20 cursor-default">ROOT_ACCESS_PRIME_2026</span>
    </footer>

    <script>
        // CONFIG
        const firebaseConfig = {
          apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
          authDomain: "dark-web-9.firebaseapp.com",
          projectId: "dark-web-9",
          databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com/",
          storageBucket: "dark-web-9.firebasestorage.app",
          messagingSenderId: "564328425161",
          appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.database();

        let user = { name: "", level: 1, status: "ready", avatar: "https://api.dicebear.com/7.x/avataaars/svg?seed=Felix" };
        let currentReply = null;
        let godTaps = 0;

        window.onload = () => {
            const saved = localStorage.getItem('e_user');
            if(saved) {
                user.name = saved;
                user.avatar = localStorage.getItem('e_avatar') || user.avatar;
                user.level = parseInt(localStorage.getItem('e_lvl_'+saved)) || 1;
                user.status = localStorage.getItem('e_status_'+saved) || "ready";
                document.getElementById('myAvatar').src = user.avatar;
                startApp();
            } else {
                document.getElementById('loginScreen').classList.remove('hidden');
            }
        };

        function setAvatar(src) { user.avatar = src; alert("Avatar Selected!"); }

        function saveUser() {
            const n = document.getElementById('uName').value.trim();
            if(!n) return;
            localStorage.setItem('e_user', n);
            localStorage.setItem('e_avatar', user.avatar);
            location.reload();
        }

        function startApp() {
            listenBan();
            listenLock();
            listenGlobalChat();
            listenPrivateChat();
            renderLevels();
        }

        // --- GLOBAL CHAT ---
        function sendGlobalMsg() {
            const t = document.getElementById('chatInp').value.trim();
            if(!t) return;
            db.ref('global_v3').push({ name: user.name, text: t, reply: currentReply, avatar: user.avatar, time: Date.now() });
            cancelRep();
            document.getElementById('chatInp').value = '';
        }

        function listenGlobalChat() {
            db.ref('global_v3').limitToLast(30).on('value', snap => {
                const box = document.getElementById('chatBox');
                box.innerHTML = '';
                snap.forEach(c => {
                    const d = c.val();
                    const key = c.key;
                    box.innerHTML += `
                        <div class="msg-bubble ${d.name === user.name ? 'self-msg' : 'self-start'}">
                            <div class="flex items-center gap-2 mb-1">
                                <img src="${d.avatar}" class="w-4 h-4 rounded-full">
                                <span class="text-[9px] font-black uppercase">${d.name}</span>
                            </div>
                            ${d.reply ? `<div class="reply-preview">Replying to: ${d.reply}</div>` : ''}
                            <p class="text-sm font-medium">${d.text}</p>
                            <div class="mt-2 flex gap-4 text-[10px] font-bold opacity-60">
                                <span class="cursor-pointer" onclick="setRep('${d.name}', '${d.text}')">REPLY</span>
                                <span class="cursor-pointer" onclick="addReact('${key}', '🔥')">🔥</span>
                                <span id="r-${key}"></span>
                            </div>
                        </div>
                    `;
                    db.ref('reacts/'+key).on('value', rs => {
                        const rArea = document.getElementById('r-'+key);
                        if(rArea && rs.val()) rArea.innerText = Object.values(rs.val()).join('');
                    });
                });
                box.scrollTop = box.scrollHeight;
            });
        }

        function setRep(n, t) { currentReply = n + ": " + t; document.getElementById('replyArea').classList.remove('hidden'); document.getElementById('replyTxt').innerText = "Replying to " + n; }
        function cancelRep() { currentReply = null; document.getElementById('replyArea').classList.add('hidden'); }
        function addReact(k, e) { db.ref('reacts/'+k).push(e); }

        // --- PRIVATE SUPPORT ---
        function sendPrivateMsg() {
            const t = document.getElementById('supportInp').value.trim();
            if(!t) return;
            db.ref('support/'+user.name).push({ sender: user.name, text: t, time: Date.now() });
            document.getElementById('supportInp').value = '';
        }

        function listenPrivateChat() {
            db.ref('support/'+user.name).on('value', snap => {
                const box = document.getElementById('supportBox');
                box.innerHTML = '';
                snap.forEach(c => {
                    const d = c.val();
                    box.innerHTML += `<div class="p-3 rounded-2xl ${d.sender === 'Admin' ? 'bg-blue-900/50' : 'bg-slate-800'} text-xs mb-2"><b>${d.sender}:</b> ${d.text}</div>`;
                });
                box.scrollTop = box.scrollHeight;
            });
        }

        // --- ADMIN CONTROLS ---
        function checkGodTaps() {
            godTaps++;
            if(godTaps === 4) {
                const p = prompt("GOD MODE KEY:");
                if(p === "prime786") { document.getElementById('godBtn').classList.remove('hidden'); showTab('godTab'); }
                godTaps = 0;
            }
        }

        function banAction(isBan) {
            const t = document.getElementById('banUser').value.trim();
            if(!t) return;
            db.ref('bans/'+t).set(isBan ? true : null);
            alert(t + (isBan ? " Banned!" : " Unbanned!"));
        }

        function listenBan() { db.ref('bans/'+user.name).on('value', snap => { if(snap.val()) document.getElementById('banOverlay').style.display='flex'; }); }
        function toggleLock() { db.ref('siteLock').once('value', s => db.ref('siteLock').set(!s.val())); }
        function listenLock() { db.ref('siteLock').on('value', s => document.getElementById('lockOverlay').style.display = s.val() ? 'flex' : 'none'); }

        function renderLevels() {
            const grid = document.getElementById('levelGrid');
            grid.innerHTML = '';
            for(let i=1; i<=10; i++) {
                let l = i > user.level;
                grid.innerHTML += `<div class="bg-white p-10 text-center rounded-[40px] shadow-sm ${l ? 'opacity-30' : 'border-2 border-blue-100'}">
                    <div class="text-5xl mb-3">${l ? '🔒' : '🎖️'}</div>
                    <h4 class="font-black text-xs">LEVEL ${i}</h4>
                </div>`;
            }
        }

        function showTab(id) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab'));
            document.getElementById(id).classList.add('active-tab');
        }

        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
