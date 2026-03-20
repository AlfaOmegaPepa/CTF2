<!DOCTYPE html>
<html lang="cs">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>⚡ CTF OMNIBOT v3.0 - UNBREAKABLE</title>
    <style>
        :root { --neon: #00ff41; --bg: #050505; --kali: #2a8fbd; --warn: #ff003c; --edit: #ffcc00; --box: #121212; }
        
        * { box-sizing: border-box; }
        
        body { 
            background: var(--bg); 
            color: #e0e0e0; 
            margin: 0; 
            padding: 20px; 
            font-family: 'Consolas', 'Courier New', monospace; 
            line-height: 1.5;
            overflow-x: hidden;
            overflow-y: auto; /* Povolí skrolování dolů vždy */
        }

        .container { max-width: 1000px; margin: 0 auto; }

        h1 { text-align: center; color: var(--neon); text-shadow: 0 0 15px var(--neon); font-size: 2.5rem; margin-bottom: 30px; }

        /* BLOKY - Teď jdou pod sebe */
        .section-card {
            background: var(--box);
            border: 1px solid #333;
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 25px;
            width: 100%;
        }

        h2 { color: var(--neon); border-bottom: 1px solid #333; padding-bottom: 10px; margin-top: 0; font-size: 1.2rem; text-transform: uppercase; }
        h3 { color: var(--kali); font-size: 1rem; margin: 15px 0 5px 0; }

        /* TLAČÍTKA NA WEBY */
        .tool-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(140px, 1fr)); gap: 10px; margin-bottom: 10px; }
        .tool-btn { 
            background: #1a1a1a; color: var(--neon); border: 2px solid var(--neon); 
            text-decoration: none; padding: 12px; text-align: center; border-radius: 6px; 
            font-weight: bold; transition: 0.2s; 
        }
        .tool-btn:hover { background: var(--neon); color: #000; box-shadow: 0 0 15px var(--neon); }

        /* VSTUPNÍ POLE */
        textarea { 
            width: 100%; height: 180px; background: #000; color: var(--neon); 
            border: 2px solid #444; padding: 15px; border-radius: 8px; 
            font-size: 1rem; outline: none; transition: 0.3s;
        }
        textarea:focus { border-color: var(--neon); }

        /* VÝSLEDKY - ŠIROKÉ PANELY */
        .result-panel { background: #000; border: 2px solid var(--neon); border-radius: 8px; padding: 20px; text-align: center; margin: 15px 0; }
        #autoFlag { font-size: 2rem; font-weight: bold; word-break: break-all; color: var(--neon); margin: 10px 0; min-height: 50px; }

        /* RUČNÍ LABORKA */
        .edit-panel { border-color: var(--edit); border-style: dashed; }
        .edit-container { background: #000; border: 3px solid var(--edit); border-radius: 8px; padding: 20px; margin-top: 10px; }
        #manualEdit { width: 100%; background: transparent; border: none; color: #fff; font-size: 2rem; font-weight: bold; text-align: center; outline: none; }

        /* FRAGMENTY (ČIPY) */
        .chips-area { display: flex; flex-wrap: wrap; gap: 10px; justify-content: center; }
        .chip { background: #222; border: 1px solid var(--kali); color: #fff; padding: 10px 18px; border-radius: 6px; cursor: pointer; font-size: 1.1rem; font-weight: bold; transition: 0.1s; }
        .chip:hover { background: var(--kali); transform: scale(1.05); }

        /* OVLÁDÁNÍ */
        .btn-row { display: flex; gap: 10px; margin-top: 15px; }
        .btn { flex: 1; padding: 15px; border: none; border-radius: 6px; font-weight: bold; cursor: pointer; text-transform: uppercase; }
        .btn-green { background: var(--neon); color: #000; }
        .btn-yellow { background: var(--edit); color: #000; }
        .btn-red { background: var(--warn); color: #fff; flex: 0 0 80px; }

        /* CHEAT LIST */
        code { display: block; background: #000; padding: 10px; margin: 10px 0; border-left: 3px solid var(--kali); color: #f1fa8c; font-size: 0.9rem; }

        @media (max-width: 600px) {
            #autoFlag, #manualEdit { font-size: 1.2rem; }
            h1 { font-size: 1.8rem; }
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>🚀 OMNIBOT v3.0</h1>

        <!-- 🛠️ SEKCE 1: ODKAZY -->
        <div class="section-card">
            <h2>🧪 CTF Nástroje (Rychlé odkazy)</h2>
            <div class="tool-grid">
                <a href="https://gchq.github.io/CyberChef/" target="_blank" class="tool-btn">CyberChef</a>
                <a href="https://aperisolve.fr/" target="_blank" class="tool-btn">AperiSolve</a>
                <a href="https://www.dcode.fr/en" target="_blank" class="tool-btn">dCode</a>
                <a href="https://crackstation.net/" target="_blank" class="tool-btn">CrackStation</a>
            </div>
        </div>

        <!-- 📥 SEKCE 2: VSTUP -->
        <div class="section-card">
            <h2>📥 Sem vlož hnus z Wiresharku / Notepadu</h2>
            <textarea id="rawInput" oninput="hackProcess()" placeholder="Vlož data a já z nich vydlabu flag..."></textarea>
        </div>

        <!-- 🤖 SEKCE 3: AUTO SOLVER -->
        <div class="section-card">
            <h2>🤖 Automatický Detektor</h2>
            <div class="result-panel">
                <div id="autoFlag">ČEKÁM...</div>
            </div>
            <button class="btn btn-green" onclick="pushToLab()">Vložit do laborky níže ➔</button>
        </div>

        <!-- 🧪 SEKCE 4: LABORKA -->
        <div class="section-card edit-panel">
            <h2>🧪 Ruční Skládačka (Tady flag oprav)</h2>
            <div class="edit-container">
                <input type="text" id="manualEdit" placeholder="Vlajka se staví tady...">
            </div>
            <div class="btn-row">
                <button class="btn btn-yellow" onclick="copyFinal()">📋 Kopírovat Hotovou Vlajku</button>
                <button class="btn btn-red" onclick="document.getElementById('manualEdit').value=''">🗑️</button>
            </div>
        </div>

        <!-- 🧩 SEKCE 5: KOUSKY -->
        <div class="section-card">
            <h2>🧩 Detekované kousky (Klikni na ně)</h2>
            <div id="chipsArea" class="chips-area">Zatím jsem nic nenašel...</div>
        </div>

        <!-- 📖 SEKCE 6: TAHAČ -->
        <div class="section-card">
            <h2>📖 Nápověda pro KALI / Wireshark</h2>
            <h3>Wireshark</h3>
            <code>Pravý klik -> Follow -> TCP Stream (Zkopíruj pak celý text sem)</code>
            <code>Filtruj nahoře: frame contains "pico" nebo "flag"</code>
            <h3>Příkazy do Terminálu</h3>
            <code>strings soubor.pcap | grep -i "pico"</code>
            <code>grep -aE "[A-Za-z0-9+/=]{10,}" soubor.pcap</code>
            <h3>Webové Hacky</h3>
            <code>Běž na web/robots.txt</code>
            <code>F12 -> Console -> napiš 'flag'</code>
        </div>
        
        <p style="text-align: center; color: #444; font-size: 10px;">SSPŠ CTF Speed Tool - Posunuj dolů pro další sekce</p>
    </div>

<script>
    function hackProcess() {
        const input = document.getElementById('rawInput').value;
        const autoOut = document.getElementById('autoFlag');
        const chipsArea = document.getElementById('chipsArea');

        if (!input.trim()) {
            autoOut.innerText = "ČEKÁM...";
            chipsArea.innerHTML = "Zatím jsem nic nenašel...";
            return;
        }

        // Sanitace nulových bytů a ne-ASCII bordelu
        const cleaned = input.replace(/[^\x20-\x7E\n\r\t]/g, ' ');
        
        // Hledání Base64 řetězců
        const b64Regex = /[A-Za-z0-9+/]{3,}=*/g;
        const matches = cleaned.match(b64Regex) || [];
        
        let validChunks = [];
        let seen = new Set();

        matches.forEach(m => {
            // Zkoušíme všechny typy paddingu (chybějící rovnítka)
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

        // Seřazení chronologicky (jak jdou po sobě v pcapu)
        validChunks.sort((a,b) => a.pos - b.pos);

        if (validChunks.length === 0) {
            autoOut.innerText = "HLEDÁM DALŠÍ...";
            chipsArea.innerHTML = "Zkouším prosívat text...";
            return;
        }

        // Generování tlačítek (Chips)
        chipsArea.innerHTML = "";
        validChunks.forEach(chunk => {
            let el = document.createElement('div');
            el.className = 'chip';
            el.innerText = chunk.text;
            el.onclick = () => { document.getElementById('manualEdit').value += chunk.text; };
            chipsArea.appendChild(el);
        });

        // Konstrukce automatického flagu
        let prefix = validChunks.find(c => c.text.toLowerCase().includes("ctf")) || null;
        let body = validChunks.filter(c => c !== prefix).map(c => c.text).join("");
        
        if (prefix) {
            let res = prefix.text + body;
            res = res.replace(/{+/g, '{').replace(/}+/g, '}');
            if (res.includes('{') && !res.includes('}')) res += '}';
            autoOut.innerText = res;
        } else {
            autoOut.innerText = body;
        }
    }

    function pushToLab() {
        let val = document.getElementById('autoFlag').innerText;
        if(val !== "ČEKÁM..." && val !== "HLEDÁM DALŠÍ...") {
            document.getElementById('manualEdit').value = val;
        }
    }

    function copyFinal() {
        let val = document.getElementById('manualEdit').value;
        if (val) {
            navigator.clipboard.writeText(val);
            alert("Flag zkopírován do schránky!");
        }
    }
</script>

</body>
</html>
