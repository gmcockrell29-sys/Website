<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <title>QuickMail | Temp Inbox</title>  
    <script src="https://cdn.tailwindcss.com"></script>  
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">  
    <style>  
        @keyframes pulse-soft { 0%, 100% { opacity: 1; } 50% { opacity: 0.3; } }  
        .syncing { animation: pulse-soft 2s infinite; }  
    </style>  
</head>  
<body class="bg-slate-900 text-white font-sans min-h-screen p-4 md:p-10">  
  
    <div class="max-w-2xl mx-auto">  
        <div class="flex justify-between items-center mb-8">  
            <h1 class="text-3xl font-black text-blue-500 tracking-tighter">QUICKMAIL</h1>  
            <div id="status" class="text-xs font-mono text-slate-500 uppercase tracking-widest bg-slate-800 px-3 py-1 rounded-full">  
                System Standby  
            </div>  
        </div>  
          
        <div class="bg-slate-800 p-6 rounded-2xl border border-slate-700 shadow-2xl mb-6">  
            <label class="block text-slate-500 text-xs font-bold mb-2 uppercase">Your Disposable Address</label>  
            <div class="flex flex-col sm:flex-row gap-3">  
                <input type="text" id="addr" readonly   
                    class="w-full bg-slate-950 border border-slate-700 p-3 rounded-lg text-blue-400 font-mono text-lg outline-none focus:border-blue-500 transition">  
                <button onclick="copyBtn()" class="bg-blue-600 hover:bg-blue-500 px-6 py-3 rounded-lg font-bold transition flex items-center justify-center">  
                    <i class="fas fa-copy mr-2"></i> COPY  
                </button>  
            </div>  
            <button onclick="genEmail()" class="mt-4 text-sm text-slate-500 hover:text-blue-400 transition italic">  
                <i class="fas fa-sync-alt mr-1"></i> Generate New Address  
            </button>  
        </div>  
  
        <div class="bg-slate-800 rounded-2xl border border-slate-700 shadow-2xl overflow-hidden">  
            <div class="p-4 border-b border-slate-700 bg-slate-800/50 flex justify-between items-center">  
                <h2 class="font-bold flex items-center">  
                    <i class="fas fa-inbox mr-2 text-blue-500"></i> INBOX  
                </h2>  
                <span id="loader" class="text-[10px] font-bold text-green-500 bg-green-500/10 px-2 py-0.5 rounded uppercase tracking-tighter">  
                    Waiting  
                </span>  
            </div>  
              
            <div id="emails" class="divide-y divide-slate-700/50 min-h-[300px]">  
                <div class="flex flex-col items-center justify-center py-20 text-slate-600">  
                    <i class="fas fa-envelope-open text-4xl mb-3 opacity-20"></i>  
                    <p class="text-sm">No messages yet...</p>  
                </div>  
            </div>  
        </div>  
          
        <p class="text-center mt-8 text-slate-600 text-[10px] uppercase tracking-widest">Powered by 1SecMail API • Auto-Refreshes every 5s</p>  
    </div>  
  
    <script>  
        let user = "";  
        let dom = "";  
  
        // 1. Setup Email  
        function genEmail() {  
            const chars = 'abcdefghijklmnopqrstuvwxyz0123456789';  
            user = '';  
            for(let i=0; i<10; i++) user += chars[Math.floor(Math.random()*chars.length)];  
            dom = "1secmail.com";  
            document.getElementById('addr').value = `${user}@${dom}`;  
            document.getElementById('emails').innerHTML = `  
                <div class="flex flex-col items-center justify-center py-20 text-slate-500">  
                    <i class="fas fa-spinner fa-spin text-2xl mb-3"></i>  
                    <p class="text-sm">Listening for incoming mail...</p>  
                </div>`;  
            document.getElementById('status').innerText = "Active: " + user;  
        }  
  
        // 2. Check Inbox  
        async function checkMail() {  
            if(!user) return;  
              
            const loader = document.getElementById('loader');  
            loader.innerText = "Syncing...";  
            loader.classList.add('syncing');  
  
            try {  
                // Using HTTPS to prevent mixed-content blocks on GitHub  
                const res = await fetch(`https://www.1secmail.com/api/v1/?action=getMessages&login=${user}&domain=${dom}`);  
                const data = await res.json();  
                  
                setTimeout(() => {  
                    loader.innerText = "Online";  
                    loader.classList.remove('syncing');  
                }, 1000);  
  
                if(data.length > 0) {  
                    let html = "";  
                    data.forEach(mail => {  
                        html += `  
                        <div class="p-5 hover:bg-slate-700/30 cursor-pointer transition border-l-4 border-transparent hover:border-blue-500" onclick="readMail(${mail.id})">  
                            <div class="flex justify-between items-start mb-1">  
                                <span class="text-blue-400 font-bold text-sm truncate pr-4">${mail.from}</span>  
                                <span class="text-[10px] text-slate-500 font-mono">${mail.date.split(' ')[1]}</span>  
                            </div>  
                            <div class="text-slate-200 text-sm font-medium truncate">${mail.subject || '(No Subject)'}</div>  
                        </div>`;  
                    });  
                    document.getElementById('emails').innerHTML = html;  
                }  
            } catch(e) {   
                loader.innerText = "Error";  
                loader.classList.remove('syncing');  
            }  
        }  
  
        // 3. Read specific message  
        async function readMail(id) {  
            const res = await fetch(`https://www.1secmail.com/api/v1/?action=readMessage&login=${user}&domain=${dom}&id=${id}`);  
            const data = await res.json();  
              
            // On iPad, a simple alert is easiest to read long text  
            alert(`FROM: ${data.from}\nDATE: ${data.date}\nSUBJECT: ${data.subject}\n\nMESSAGE:\n${data.textBody || data.body}`);  
        }  
  
        function copyBtn() {  
            const el = document.getElementById('addr');  
            el.select();  
            el.setSelectionRange(0, 99999); // For mobile  
            document.execCommand('copy');  
            alert("Address Copied!");  
        }  
  
        // Boot  
        genEmail();  
        setInterval(checkMail, 5000);  
    </script>  
</body>  
</html>  
