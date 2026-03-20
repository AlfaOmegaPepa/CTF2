<!DOCTYPE html>
<html lang="cs">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>⚡ CTF OMNIBOT v3.1 - BURP EDITION</title>
    <style>
        :root { --neon: #00ff41; --bg: #050505; --kali: #2a8fbd; --warn: #ff003c; --edit: #ffcc00; --box: #121212; --burp: #ff6633; }
        * { box-sizing: border-box; }
        body { background: var(--bg); color: #e0e0e0; margin: 0; padding: 20px; font-family: 'Consolas', 'Courier New', monospace; line-height: 1.5; overflow-x: hidden; overflow-y: auto; }
        .container { max-width: 1000px; margin: 0 auto; }
        h1 { text-align: center; color: var(--neon); text-shadow: 0 0 15px var(--neon); font-size: 2.5rem; margin-bottom: 30px; }
        .section-card { background: var(--box); border: 1px solid #333; border-radius: 12px; padding: 20px; margin-bottom: 25px; width: 100%; }
        h2 { color: var(--neon); border-bottom: 1px solid #333; padding-bottom: 10px; margin-top: 0; font-size: 1.2rem; text-transform: uppercase; }
        h3 { color: var(--kali); font-size: 1rem; margin: 15px 0 5px 0; }
        .tool-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(140px, 1fr)); gap: 10px; }
        .tool-btn { background: #1a1a1a; color: var(--neon); border: 2px solid var(--neon); text-decoration: none; padding: 12px; text-align: center; border-radius: 6px; font-weight: bold; transition: 0.2s; }
        .tool-btn:hover { background: var(--neon); color: #000; box-shadow: 0 0 15px var(--neon); }
        textarea { width: 100%; height: 180px; background: #000; color: var(--neon); border: 2px solid #444; padding: 15px; border-radius: 8px; font-size: 1rem; outline: none; }
        .result-panel { background: #000; border: 2px solid var(--neon); border-radius: 8px; padding: 20px; text-align: center; }
        #autoFlag { font-size: 2rem; font-weight: bold; word-break: break-all; color: var(--neon); margin: 10px 0; min-height: 50px; }
        .edit-container { background: #000; border: 3px solid var(--edit); border-radius: 8px; padding: 20px; margin-top: 10px; }
        #manualEdit { width: 100%; background: transparent; border: none; color: #fff; font-size: 2rem; font-weight: bold; text-align: center; outline: none; }
        .chips-area { display: flex; flex-wrap: wrap; gap: 10px; justify-content: center; }
        .chip { background: #222; border: 1px solid var(--kali); color: #fff; padding: 10px 18px; border-radius: 6px; cursor: pointer; font-weight: bold; transition: 0.1s; }
        .chip:hover { background: var(--kali); }
        .btn-row { display: flex; gap: 10px; margin-top: 15px; }
        .btn { flex: 1; padding: 15px; border: none; border-radius: 6px; font-weight: bold; cursor: pointer; text-transform: uppercase; }
        .btn-green { background: var(--neon); color: #000; }
        .btn-yellow { background: var(--edit); color: #000; }
        .btn-red { background: var(--warn); color: #fff; flex: 0 0 80px; }
        code { display: block; background: #000; padding: 10px; margin: 10px 0; border-left: 3px solid var(--kali); color: #f1fa8c; font-size: 0.9rem; }
    </style>
</head>
<body>

    <div class="container">
        <h1>🚀 CTF OMNIBOT v3.1</h1>

        <div class="section-card">
            <h2>🧪 Nástroje</h2>
            <div class="tool-grid">
                <a href="https://gchq.github.io/CyberChef/" target="_blank" class="tool-btn">CyberChef</a>
                <a href="https://aperisolve.fr/" target="_blank" class="tool-btn">AperiSolve</a>
                <a href="https://crackstation.net/" target="_blank" class="tool-btn">CrackStation</a>
            </div>
        </div>

        <div class="section-card">
            <h2>📥 Vstup</h2>
            <textarea id="rawInput" oninput="hackProcess()" placeholder="Vlož text z Wiresharku nebo Burpu sem..."></textarea>
        </div>

        <div class="section-card">
            <h2>🤖 Automat</h2>
            <div class="result-panel"><div id="autoFlag">ČEKÁM...</div></div>
            <button class="btn btn-green" onclick="pushToLab()">➔ Poslat dolů do Laborky</button>
        </div>

        <div class="section-card" style="border-color: var(--edit);">
            <h2>🧪 Ruční Skládačka (Opravování)</h2>
            <div class="edit-container"><input type="text" id="manualEdit" placeholder="Vlajka se staví tady..."></div>
            <div class="btn-row">
                <button class="btn btn-yellow" onclick="copyFinal()">📋 Kopírovat</button>
                <button class="btn btn-red" onclick="document.getElementById('manualEdit').value=''">🗑️</button>
            </div>
        </div>

        <div class="section-card">
            <h2>🧩 Fragmenty</h2>
            <div id="chipsArea" class="chips-area">Zatím nic...</div>
        </div>

        <!-- 📡 NOVÁ SEKCE: BURP SUITE -->
        <div class="section-card" style="border-color: var(--burp);">
            <h2 style="color: var(--burp); border-bottom-color: var(--burp);">🟠 BURP SUITE (Web Hacking)</h2>
            <p style="font-size: 12px; margin-bottom: 5px;"><i>V Kali: Drak -> Napiš 'burp' -> Temp Project -> Start Burp</i></p>
            
            <h3>1. Proxy & Intercept</h3>
            <code>Proxy tab -> Intercept is ON: Zachytí každé tvé kliknutí na webu. Můžeš v datech změnit admin=0 na admin=1 předtím, než to pošleš na server (tlačítko Forward).</code>
            
            <h3>2. Repeater (Zkušebna)</h3>
            <code>Pravým na požadavek -> Send to Repeater (nebo CTRL+R). Tady si můžeš zkoušet měnit hesla nebo jména a jen klikat na 'Send' a hned uvidíš odpověď (flag).</code>

            <h3>3. Intruder (Automatické louskání)</h3>
            <code>Pravým na požadavek -> Send to Intruder. Nastavíš §symbol§ tam, kde chceš měnit slovo, nahraješ tam slovník (např. jména) a necháš Burp útočit za tebe.</code>
        </div>

        <!-- 📖 KALI & WIRESHARK TAHAČ -->
        <div class="section-card">
            <h2>📖 Tahač (Kali / Wireshark)</h2>
            <h3>Wireshark</h3>
            <code>Pravým -> Follow -> TCP Stream</code>
            <code>frame contains "flag"</code>
            <h3>Terminál</h3>
            <code>strings soubor.pcap | grep -i "pico"</code>
            <code>exiftool photo.png (Metadata fotky)</code>
            <code>grep -rnw . -e "pico" (Hledej flag v celé složce)</code>
        </div>
    </div>

<script>
    function hackProcess() {
        const input = document.getElementById('rawInput').value;
        const autoOut = document.getElementById('autoFlag');
        const chipsArea = document.getElementById('chipsArea');
        if (!input.trim()) { autoOut.innerText = "ČEKÁM..."; chipsArea.innerHTML = "Zatím nic..."; return; }
        const cleaned = input.replace(/[^\x20-\x7E\n\r\t]/g, ' ');
        const b64Regex = /[A-Za-z0-9+/]{3,}=*/g;
        const matches = cleaned.match(b64Regex) || [];
        let validChunks = [];
        let seen = new Set();
        matches.forEach(m => {
            [m, m+"=", m+"=="].forEach(variant => {
                try {
                    let d = atob(variant);
                    if (/^[a-zA-Z0-9_{}!?-]+$/.test(d) && d.length > 0) {
                        if (!seen.has(d)) {
                            validChunks.push({ text: d, pos: input.indexOf(m) });
                            seen.add(d);
                        }
                    }
                } catch(e) {}
            });
        });
        validChunks.sort((a,b) => a.pos - b.pos);
        if (validChunks.length === 0) { autoOut.innerText = "NIC..."; chipsArea.innerHTML = "Nenašel jsem kód..."; return; }
        chipsArea.innerHTML = "";
        validChunks.forEach(chunk => {
            let el = document.createElement('div');
            el.className = 'chip';
            el.innerText = chunk.text;
            el.onclick = () => { document.getElementById('manualEdit').value += chunk.text; };
            chipsArea.appendChild(el);
        });
        let prefix = validChunks.find(c => c.text.toLowerCase().includes("ctf")) || null;
        let body = validChunks.filter(c => c !== prefix).map(c => c.text).join("");
        if (prefix) {
            let res = prefix.text + body;
            res = res.replace(/{+/g, '{').replace(/}+/g, '}');
            if (res.includes('{') && !res.includes('}')) res += '}';
            autoOut.innerText = res;
        } else { autoOut.innerText = body; }
    }
    function pushToLab() {
        let val = document.getElementById('autoFlag').innerText;
        if(val !== "ČEKÁM..." && val !== "NIC...") document.getElementById('manualEdit').value = val;
    }
    function copyFinal() {
        let val = document.getElementById('manualEdit').value;
        if (val) { navigator.clipboard.writeText(val); alert("Zkopírováno!"); }
    }
</script>
</body>
</html>
