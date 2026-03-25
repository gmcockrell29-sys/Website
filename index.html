<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <title>QuickMail | Temp Inbox</title>  
    <script src="https://cdn.tailwindcss.com"></script>  
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">  
    <style>  
        .modal { transition: opacity 0.25s ease; }  
        body.modal-active { overflow: hidden; }  
        .hover-bg:hover { background-color: rgba(30, 41, 59, 0.5); }  
    </style>  
</head>  
<body class="bg-slate-900 text-white font-sans min-h-screen">  
  
    <nav class="p-6 border-b border-slate-800">  
        <div class="container mx-auto flex justify-between items-center">  
            <h1 class="text-2xl font-bold text-blue-500"><i class="fas fa-bolt mr-2"></i>QuickMail</h1>  
            <div id="statusIndicator" class="text-xs bg-slate-800 text-slate-400 px-3 py-1 rounded-full flex items-center">  
                <span class="w-2 h-2 bg-green-500 rounded-full mr-2 animate-pulse"></span> System Live  
            </div>  
        </div>  
    </nav>  
  
    <main class="container mx-auto px-4 py-12 max-w-4xl">  
        <div class="bg-slate-800 p-8 rounded-2xl shadow-2xl border border-slate-700 mb-8">  
            <label class="block text-slate-400 mb-2 text-sm uppercase tracking-widest">Your Temporary Email Address</label>  
            <div class="flex flex-col md:flex-row gap-4">  
                <input type="text" id="emailAddr" readonly value="Generating..."   
                    class="bg-slate-900 border border-slate-600 rounded-lg px-4 py-3 flex-grow text-xl font-mono text-blue-300 outline-none">  
                <button onclick="copyEmail()" class="bg-blue-600 hover:bg-blue-500 transition px-6 py-3 rounded-lg font-bold">  
                    <i class="fas fa-copy mr-2"></i> Copy  
                </button>  
                <button onclick="generateNew()" class="bg-slate-700 hover:bg-slate-600 transition px-6 py-3 rounded-lg font-bold">  
                    <i class="fas fa-sync-alt"></i>  
                </button>  
            </div>  
        </div>  
  
        <div class="bg-slate-800 rounded-2xl shadow-2xl border border-slate-700 overflow-hidden">  
            <div class="p-6 border-b border-slate-700 flex justify-between items-center">  
                <h2 class="text-xl font-semibold">Inbox</h2>  
                <span id="mailCount" class="text-sm text-slate-400 italic">Checking for mail every 5s...</span>  
            </div>  
  
            <div id="inboxList" class="min-h-[300px]">  
                <div class="flex flex-col items-center justify-center py-20 text-slate-500">  
                    <i class="fas fa-spinner fa-spin text-5xl mb-4"></i>  
                    <p>Connecting to mail server...</p>  
                </div>  
            </div>  
        </div>  
    </main>  
  
    <div id="emailModal" class="modal opacity-0 pointer-events-none fixed w-full h-full top-0 left-0 flex items-center justify-center z-50">  
        <div class="modal-overlay absolute w-full h-full bg-black opacity-70"></div>  
        <div class="modal-container bg-slate-800 w-11/12 md:max-w-2xl mx-auto rounded-xl shadow-2xl z-50 overflow-y-auto border border-slate-600">  
            <div class="p-6 border-b border-slate-700 flex justify-between items-center">  
                <h3 id="modalSubject" class="text-xl font-bold text-blue-400">Loading...</h3>  
                <button onclick="toggleModal()" class="text-slate-400 hover:text-white text-2xl">&times;</button>  
            </div>  
            <div class="p-6">  
                <p id="modalFrom" class="text-sm text-slate-400 mb-4 font-mono"></p>  
                <div id="modalBody" class="bg-slate-900 p-4 rounded-lg text-slate-200 whitespace-pre-wrap min-h-[200px] border border-slate-700"></div>  
            </div>  
        </div>  
    </div>  
  
    <script>  
        let currentEmail = "";  
        const domains = ["1secmail.com", "1secmail.org", "1secmail.net"];  
  
        // 1. Generate Email  
        async function generateNew() {  
            const domain = domains[Math.floor(Math.random() * domains.length)];  
            const username = Math.random().toString(36).substring(2, 12);  
            currentEmail = `${username}@${domain}`;  
            document.getElementById('emailAddr').value = currentEmail;  
              
            // Initial empty state  
            document.getElementById('inboxList').innerHTML = `  
                <div class="flex flex-col items-center justify-center py-20 text-slate-500">  
                    <i class="fas fa-inbox text-5xl mb-4"></i>  
                    <p>No messages yet. Send one to see it here!</p>  
                </div>`;  
            checkInbox();  
        }  
  
        // 2. Check for new mail  
        async function checkInbox() {  
            if (!currentEmail) return;  
            const [user, domain] = currentEmail.split('@');  
            const url = `https://www.1secmail.com/api/v1/?action=getMessages&login=${user}&domain=${domain}`;  
              
            try {  
                const response = await fetch(url);  
                const emails = await response.json();  
                if (emails.length > 0) renderInbox(emails, user, domain);  
            } catch (e) {  
                console.error("API Error:", e);  
            }  
        }  
  
        // 3. Render list  
        function renderInbox(emails, user, domain) {  
            const list = document.getElementById('inboxList');  
            list.innerHTML = "";  
            document.getElementById('mailCount').innerText = `${emails.length} message(s) received`;  
  
            emails.forEach(email => {  
                const item = document.createElement('div');  
                item.className = "p-5 border-b border-slate-700 hover-bg cursor-pointer transition-all border-l-4 border-l-transparent hover:border-l-blue-500";  
                item.innerHTML = `  
                    <div class="flex justify-between mb-1">  
                        <span class="font-bold text-blue-300 text-sm">${email.from}</span>  
                        <span class="text-xs text-slate-500">${email.date.split(' ')[1]}</span>  
                    </div>  
                    <div class="text-md font-medium text-slate-200 truncate">${email.subject || '(No Subject)'}</div>  
                `;  
                item.onclick = () => viewMessage(email.id, user, domain);  
                list.appendChild(item);  
            });  
        }  
  
        // 4. Read message  
        async function viewMessage(id, user, domain) {  
            const url = `https://www.1secmail.com/api/v1/?action=readMessage&login=${user}&domain=${domain}&id=${id}`;  
            const response = await fetch(url);  
            const data = await response.json();  
              
            document.getElementById('modalSubject').innerText = data.subject || 'No Subject';  
            document.getElementById('modalFrom').innerText = `From: ${data.from}`;  
            document.getElementById('modalBody').innerText = data.textBody || data.body;  
            toggleModal();  
        }  
  
        // UI Helpers  
        function toggleModal() {  
            const modal = document.getElementById('emailModal');  
            modal.classList.toggle('opacity-0');  
            modal.classList.toggle('pointer-events-none');  
            document.body.classList.toggle('modal-active');  
        }  
  
        function copyEmail() {  
            const emailField = document.getElementById('emailAddr');  
            emailField.select();  
            document.execCommand("copy");  
            const btn = event.currentTarget;  
            const originalText = btn.innerHTML;  
            btn.innerHTML = '<i class="fas fa-check"></i> Done!';  
            setTimeout(() => btn.innerHTML = originalText, 2000);  
        }  
  
        // Run  
        window.onload = () => {  
            generateNew();  
            setInterval(checkInbox, 5000);  
        };  
    </script>  
</body>  
</html>  
