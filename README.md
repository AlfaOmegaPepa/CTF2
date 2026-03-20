<!DOCTYPE html>
<html lang="cs">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>⚡ CTF OMNIBOT v2.7 - SCROLL & ZOOM FIX</title>
    <style>
        :root { --neon: #00ff41; --bg: #050505; --kali: #2a8fbd; --warn: #ff003c; --edit: #ffcc00; --panel: #111; }
        
        /* Zajištění, aby celá stránka šla vždy posouvat */
        html, body { 
            background: var(--bg); 
            color: #e0e0e0; 
            margin: 0; 
            padding: 0;
            min-height: 100vh;
            overflow-y: auto; /* Tohle povolí scrollování vždy */
            overflow-x: hidden;
            font-family: 'Consolas', 'Fira Code', monospace;
        }

        .layout {
            display: flex;
            flex-direction: row;
            min-height: 100vh;
        }

        /* SIDEBAR - Teď se přizpůsobí zoomu */
        .sidebar { 
            width: 280px; 
            background: #000; 
            border-right: 2px solid #222; 
            padding: 20px; 
            font-size: 11px;
            flex-shrink: 0; /* Sidebar se nebude mačkat */
        }

        h2 { color: var(--neon); font-size: 14px; text-transform: uppercase; border-bottom: 1px solid #333; margin-top: 20px; padding-bottom: 5px; }
        h3 { color: var(--kali); font-size: 12px; margin: 10px 0 5px 0; text-transform: uppercase; }
        code { display: block; background: #111; padding: 5px; margin: 5px 0; border-left: 2px solid var(--kali); font-size: 10px; color: #f1fa8c; word-break: break-all; }

        /* QUICK TOOLS */
        .tool-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 5px; margin-bottom: 15px; }
        .tool-btn { background: #1a1a1a; color: var(--neon); border: 1px solid var(--neon); text-decoration: none; padding: 8px 5px; text-align: center; border-radius: 4px; font-weight: bold; font-size: 10px; transition: 0.2s; }
        .tool-btn:hover { background: var(--neon); color: black; box-shadow: 0 0 10px var(--neon); }

        /* HLAVNÍ OBSAH */
        .main { 
            flex-grow: 1; 
            padding: 30px; 
            background: radial-gradient(circle at center, #111 0%, #050505 100%);
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        h1 { text-align: center; color: var(--neon); margin: 0; font-size: 28px; text-shadow: 0 0 10px var(--neon); }
        
        textarea#rawInput { width: 100%; min-height: 120px; background: #000; color: var(--neon); border: 2px solid #333; padding: 15px; border-radius: 8px; font-size: 14px; outline: none; box-sizing: border-box; }
        textarea#rawInput:focus { border-color: var(--neon); box-shadow: 0 0 15px rgba(0, 255, 65, 0.1); }

        /* VÝSLEDKY */
        .results-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 20px; }
        .panel { background: rgba(15, 15, 15, 0.9); border: 1px solid #333; padding: 20px; border-radius: 12px; }
        .panel h4 { margin: 0 0 12px 0; font-size: 12px; text-transform: uppercase; color: #888; }

        #autoFlag { font-size: 1.6em; font-weight: bold; color: var(--neon); word-break: break-all; margin: 15px 0; text-align: center; padding: 15px; border: 1px dashed #333; }

        .editor-container { background: #000; border: 2px solid var(--edit); padding: 15px; border-radius: 8px; }
        #manualEdit { width: 100%; background: transparent; border: none; color: white; font-size: 1.8em; font-weight: bold; outline: none; text-align: center; font-family: monospace; }
        
        /* ČIPIY (Fragmenty) */
        .chip-container { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 10px; }
        .chip { background: #222; border: 1px solid #444; padding: 10px 14px; border-radius: 6px; font-size: 13px; cursor: pointer; color: #fff; font-weight: bold; transition: 0.1s; }
        .chip:hover { border-color: var(--neon); background: #333; color: var(--neon); transform: scale(1.05); }

        /* BUTTONS */
        .btn { padding: 14px 20px; border: none; border-radius: 6px; font-weight: bold; cursor: pointer; text-transform: uppercase; width: 100%; font-size: 13px; margin-top: 10px; transition: 0.2s; }
        .btn-green { background: var(--neon); color: #000; }
        .btn-yellow { background: var(--edit); color: #000; }
        .btn-red { background: var(--warn); color: #fff; width: 50px; margin-top: 0; }
        .btn:active { transform: scale(0.98); }

        /* MOBILNÍ / ZOOM ÚPRAVA */
        @media (max-width: 800px) {
            .layout { flex-direction: column; }
            .sidebar { width: 100%; border-right: none; border-bottom: 2px solid #222; box-sizing: border-box; }
        }
    </style>
</head>
<body>

    <div class="layout">
        <!-- 🛠️ TAHAČ A ODKAZY (Zůstává nalevo nebo nahoře) -->
        <div class="sidebar">
            <h1 style="color:white; font-size: 18px; border:none; text-shadow:none;">⚔️ SOLVE TOOLS</h1>
            <div class="tool-grid">
                <a href="https://gchq.github.io/CyberChef/" target="_blank" class="tool-btn">🧪 CyberChef</a>
                <a href="https://aperisolve.fr/" target="_blank" class="tool-btn">🖼️ AperiSolve</a>
                <a href="https://www.dcode.fr/en" target="_blank" class="tool-btn">🔢 dCode.fr</a>
                <a href="https://crackstation.net/" target="_blank" class="tool-btn">🔑 CrackStation</a>
                <a href="https://tineye.com/" target="_blank" class="tool-btn">🔍 TinEye</a>
                <a href="https://cutt.ly/" target="_blank" class="tool-btn">🔗 Zkracovač</a>
            </div>

            <h2>📖 NÁPOVĚDA</h2>
            <h3>Pcap (Wireshark)</h3>
            <code>Pravý klik -> Follow -> TCP Stream</code>
            <code>Filtr: frame contains "pico"</code>
            <h3>Terminál</h3>
            <code>strings soubor | grep "pico"</code>
            <code>exiftool obrazek.png</code>
            <h3>Web</h3>
            <code>/robots.txt / .git / .env</code>
            <code>Cookie: admin=1</code>
        </div>

        <!-- 🚀 HLAVNÍ NÁSTROJ -->
        <div class="main">
            <h1>🚀 CTF OMNIBOT v2.7</h1>

            <textarea id="rawInput" oninput="process()" placeholder="Zde 'vysyp' bordel z Wiresharku nebo zdrojáku (CTRL+V)..."></textarea>

            <div class="results-grid">
                <!-- AUTO -->
                <div class="panel">
                    <h4>🤖 AUTOMATIKA</h4>
                    <div id="autoFlag">ČEKÁM NA VSTUP...</div>
                    <button class="btn btn-green" onclick="copyToEditor()">KOPÍROVAT DO LABORKY ➔</button>
                </div>

                <!-- LABORKA -->
                <div class="panel" style="border-color: var(--edit);">
                    <h4>🧪 RUČNÍ LABORKA</h4>
                    <div class="editor-container">
                        <input type="text" id="manualEdit" placeholder="Poskládej vlajku tady...">
                    </div>
                    <div style="margin-top:12px; display:flex; gap:10px; align-items: center;">
                        <button class="btn btn-yellow" onclick="copyFinal()">📋 KOPÍROVAT FINAL FLAG</button>
                        <button class="btn btn-red" onclick="document.getElementById('manualEdit').value=''">🗑️</button>
                    </div>
                </div>
            </div>

            <!-- KOUSKY -->
            <div class="panel">
                <h4>🧩 DETEKOVANÉ FRAGMENTY (KLIKNI PRO PŘIDÁNÍ)</h4>
                <div id="chips" class="chip-container">Zde se objeví nalezené kousky...</div>
            </div>

            <div style="color: #444; font-size: 11px; text-align: center; margin-top: 10px;">
                Pokud vložíš hodně textu a nic nevidíš, stačí skrolovat dolů jako na běžném webu.
            </div>
        </div>
    </div>

<script>
    let seenTokens = new Set();

    function process() {
        let input = document.getElementById('rawInput').value;
        let autoOut = document.getElementById('autoFlag');
        let chipsArea = document.getElementById('chips');
        
        if (!input.trim()) { 
            autoOut.innerText = "ČEKÁM NA VSTUP..."; 
            chipsArea.innerHTML = "Zde se objeví nalezené kousky..."; 
            return; 
        }

        let segments = [];
        let seen = new Set();
        // Čištění nečitelných binárních znaků
        let cleanText = input.replace(/[^\x20-\x7E\n\r\t]/g, ' ');

        // Hledání Base64 patternů
        let b64Regex = /[A-Za-z0-9+/]{3,}=*/g;
        let matches = cleanText.match(b64Regex) || [];
        
        matches.forEach(m => {
            [m, m + "=", m + "=="].forEach(variant => {
                try {
                    let d = atob(variant);
                    if (/^[a-zA-Z0-9_{}!?-]+$/.test(d) && d.length > 0) {
                        if (!seen.has(d)) {
                            segments.push({ text: d, pos: input.indexOf(m) });
                            seen.add(d);
                        }
                    }
                } catch(e) {}
            });
        });

        // Seřazení chronologicky
        segments.sort((a, b) => a.pos - b.pos);

        if (segments.length === 0) {
            chipsArea.innerHTML = "Hledám dál...";
        } else {
            chipsArea.innerHTML = segments.map(s => 
                `<div class="chip" onclick="addToEditor('${s.text}')">${s.text}</div>`
            ).join("");
        }

        // Auto konstruktor
        let start = segments.find(s => s.text.toLowerCase().includes("ctf")) || null;
        let body = segments.filter(s => s !== start).map(s => s.text).join("");
        
        if (start) {
            let res = start.text + body;
            res = res.replace(/{+/g, '{').replace(/}+/g, '}');
            if (res.includes('{') && !res.includes('}')) res += '}';
            autoOut.innerText = res;
        } else {
            autoOut.innerText = body || "---";
        }
    }

    function addToEditor(val) {
        document.getElementById('manualEdit').value += val;
    }

    function copyToEditor() {
        let val = document.getElementById('autoFlag').innerText;
        if(val !== "ČEKÁM NA VSTUP..." && val !== "---") document.getElementById('manualEdit').value = val;
    }

    function copyFinal() {
        let val = document.getElementById('manualEdit').value;
        if (val) {
            navigator.clipboard.writeText(val);
            alert("Vlajka zkopírována do schránky!");
        }
    }
</script>

</body>
</html>
