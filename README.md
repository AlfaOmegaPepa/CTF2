<!DOCTYPE html>
<html lang="cs">
<head>
    <meta charset="UTF-8">
    <title>⚡ CTF OMNIBOT v2.2 - EDITOR EDITION</title>
    <style>
        :root { --neon: #00ff41; --bg: #050505; --kali: #2a8fbd; --warn: #ff003c; --edit: #ffcc00; }
        body { font-family: 'Fira Code', monospace; background: var(--bg); color: #e0e0e0; margin: 0; display: flex; height: 100vh; overflow: hidden; }
        
        /* SIDEBAR */
        .sidebar { width: 280px; background: #000; border-right: 2px solid #222; padding: 15px; font-size: 11px; overflow-y: auto; }
        h2 { color: var(--neon); font-size: 14px; text-transform: uppercase; border-bottom: 1px solid #333; }
        code { display: block; background: #111; padding: 5px; margin: 5px 0; border-left: 2px solid var(--kali); font-size: 10px; color: #f1fa8c; }

        /* HLAVNÍ ČÁST */
        .main { flex: 1; padding: 20px; display: flex; flex-direction: column; gap: 15px; overflow-y: auto; }
        h1 { text-align: center; color: var(--neon); margin: 0; font-size: 22px; text-shadow: 0 0 10px var(--neon); }
        
        textarea#rawInput { width: 100%; height: 150px; background: #000; color: var(--neon); border: 2px solid #333; padding: 10px; border-radius: 5px; font-size: 12px; }

        /* PANELY S VÝSLEDKY */
        .results-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; }
        .panel { background: #0a0a0a; border: 1px solid #333; padding: 15px; border-radius: 8px; position: relative; }
        .panel h3 { margin-top: 0; font-size: 12px; text-transform: uppercase; color: #888; letter-spacing: 1px; }

        /* AUTOMATICKÝ VÝSLEDEK */
        #autoFlag { font-size: 1.5em; font-weight: bold; color: var(--neon); word-break: break-all; margin: 10px 0; min-height: 40px; }

        /* MANUÁLNÍ EDITOR */
        .editor-container { background: #111; border: 2px solid var(--edit); padding: 10px; border-radius: 5px; }
        #manualEdit { width: 100%; background: transparent; border: none; color: white; font-size: 1.8em; font-weight: bold; font-family: 'Fira Code', monospace; outline: none; text-align: center; }
        
        /* SEGMENTY */
        .chip-container { display: flex; flex-wrap: wrap; gap: 5px; margin-top: 10px; }
        .chip { background: #1a1a1a; border: 1px solid #444; padding: 3px 8px; border-radius: 4px; font-size: 11px; cursor: pointer; transition: 0.2s; }
        .chip:hover { border-color: var(--neon); background: #222; }

        .btn { padding: 10px 20px; border: none; border-radius: 4px; font-weight: bold; cursor: pointer; text-transform: uppercase; transition: 0.3s; }
        .btn-green { background: var(--neon); color: black; }
        .btn-yellow { background: var(--edit); color: black; }
        .btn-red { background: var(--warn); color: white; }
        .btn:hover { opacity: 0.8; transform: scale(1.02); }
    </style>
</head>
<body>

    <div class="sidebar">
        <h2>🛠️ QUICK CHEAT</h2>
        <code>Wireshark: Follow TCP Stream</code>
        <code>Filtry: frame contains "pico"</code>
        <code>CyberChef: Magic / From Base64</code>
        <hr>
        <p style="color: #666;">Tip: Kliknutím na "segment" dole ho můžeš ručně přidat do editoru.</p>
    </div>

    <div class="main">
        <h1>🚀 CTF OMNIBOT v2.2 (EDITOR MODE)</h1>

        <!-- VSTUPNÍ DATA -->
        <textarea id="rawInput" oninput="process()" placeholder="Vlož sem chaos z Wiresharku (Ctrl+A, Ctrl+C, Ctrl+V)..."></textarea>

        <div class="results-grid">
            <!-- LEVÝ PANEL: AUTOMATIKA -->
            <div class="panel">
                <h3>🤖 Automatický Odhad</h3>
                <div id="autoFlag">ČEKÁM...</div>
                <button class="btn btn-green" style="width:100%" onclick="copyToEditor()">⚡ POSLAT DO EDITORU ➔</button>
            </div>

            <!-- PRAVÝ PANEL: MANUÁLNÍ LABORKA -->
            <div class="panel" style="border-color: var(--edit);">
                <h3>🧪 Manuální Laborka (Tady to oprav)</h3>
                <div class="editor-container">
                    <input type="text" id="manualEdit" placeholder="Tady piš flag...">
                </div>
                <div style="margin-top:10px; display:flex; gap:5px;">
                    <button class="btn btn-yellow" style="flex:1" onclick="copyFinal()">📋 KOPÍROVAT FINAL</button>
                    <button class="btn btn-red" onclick="document.getElementById('manualEdit').value=''">🗑️</button>
                </div>
            </div>
        </div>

        <!-- NALEZENÉ SEGMENTY -->
        <div class="panel">
            <h3>🧩 Nalezené kousky (Kliknutím přidej do editoru)</h3>
            <div id="chips" class="chip-container"></div>
        </div>
    </div>

<script>
    function process() {
        let input = document.getElementById('rawInput').value;
        let autoOut = document.getElementById('autoFlag');
        let chipsArea = document.getElementById('chips');
        
        if (!input.trim()) {
            autoOut.innerText = "ČEKÁM...";
            chipsArea.innerHTML = "";
            return;
        }

        let segments = [];
        let seen = new Set();
        let cleanText = input.replace(/[^\x20-\x7E]/g, ' ');

        // 1. SCANNER - Hledáme Base64
        let b64Regex = /[A-Za-z0-9+/]{4,}=*/g;
        let matches = cleanText.match(b64Regex) || [];
        matches.forEach(m => {
            try {
                let d = atob(m);
                if (/^[a-zA-Z0-9_{}!-]+$/.test(d) && d.length > 1) {
                    if (!seen.has(d)) {
                        segments.push({ text: d, pos: input.indexOf(m) });
                        seen.add(d);
                    }
                }
            } catch(e) {}
        });

        // 2. SEŘAZENÍ
        segments.sort((a, b) => a.pos - b.pos);

        // 3. VÝPIS ČIPŮ (Segmentů)
        chipsArea.innerHTML = segments.map(s => 
            `<div class="chip" onclick="addToEditor('${s.text}')">${s.text}</div>`
        ).join("");

        // 4. AUTOMATICKÁ KONSTRUKCE
        let prefix = "";
        let body = "";
        segments.forEach(s => {
            if (s.text.toLowerCase().includes("ctf") || s.text.toLowerCase().includes("pico")) {
                prefix = s.text.replace("{", "");
            } else if (s.text !== "{" && s.text !== "}") {
                if (!body.includes(s.text)) body += s.text;
            }
        });

        let finalAuto = prefix ? `${prefix}{${body}}` : body;
        autoOut.innerText = finalAuto || "NIC NENALEZENO";
    }

    function copyToEditor() {
        let autoVal = document.getElementById('autoFlag').innerText;
        if (autoVal !== "ČEKÁM..." && autoVal !== "NIC NENALEZENO") {
            document.getElementById('manualEdit').value = autoVal;
        }
    }

    function addToEditor(val) {
        let editor = document.getElementById('manualEdit');
        // Pokud je editor prázdný, prostě vlož. Pokud ne, přidej na konec.
        editor.value += val;
    }

    function copyFinal() {
        let val = document.getElementById('manualEdit').value;
        if (val) {
            navigator.clipboard.writeText(val);
            alert("Vlajka zkopírována: " + val);
        }
    }
</script>

</body>
</html>
