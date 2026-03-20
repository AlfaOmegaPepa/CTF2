<!DOCTYPE html>
<html lang="cs">
<head>
    <meta charset="UTF-8">
    <title>⚡ CTF OMNIBOT v2.5 - PRO SPEED EDITION</title>
    <style>
        :root { --neon: #00ff41; --bg: #050505; --kali: #2a8fbd; --warn: #ff003c; --edit: #ffcc00; --panel: #111; }
        body { font-family: 'Fira Code', 'Consolas', monospace; background: var(--bg); color: #e0e0e0; margin: 0; display: flex; height: 100vh; overflow: hidden; }
        
        /* SIDEBAR */
        .sidebar { width: 300px; background: #000; border-right: 2px solid #222; padding: 15px; font-size: 11px; overflow-y: auto; display: flex; flex-direction: column; }
        h2 { color: var(--neon); font-size: 14px; text-transform: uppercase; border-bottom: 1px solid #333; margin-top: 20px; padding-bottom: 5px; }
        h3 { color: var(--kali); font-size: 12px; margin: 10px 0 5px 0; text-transform: uppercase; }
        code { display: block; background: #111; padding: 5px; margin: 5px 0; border-left: 2px solid var(--kali); font-size: 10px; color: #f1fa8c; word-break: break-all; }

        /* QUICK TOOLS BUTTONS */
        .tool-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 5px; margin-bottom: 15px; }
        .tool-btn { background: #1a1a1a; color: var(--neon); border: 1px solid var(--neon); text-decoration: none; padding: 8px 5px; text-align: center; border-radius: 4px; font-weight: bold; font-size: 10px; transition: 0.2s; }
        .tool-btn:hover { background: var(--neon); color: black; box-shadow: 0 0 10px var(--neon); }

        /* HLAVNÍ ČÁST */
        .main { flex: 1; padding: 20px; display: flex; flex-direction: column; gap: 15px; overflow-y: auto; background: radial-gradient(circle at center, #111 0%, #050505 100%); }
        h1 { text-align: center; color: var(--neon); margin: 0; font-size: 24px; text-shadow: 0 0 10px var(--neon); }
        
        textarea#rawInput { width: 100%; height: 130px; background: #000; color: var(--neon); border: 2px solid #333; padding: 10px; border-radius: 5px; font-size: 13px; outline: none; transition: 0.3s; }
        textarea#rawInput:focus { border-color: var(--neon); box-shadow: 0 0 15px rgba(0, 255, 65, 0.1); }

        /* PANELY S VÝSLEDKY */
        .results-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; }
        .panel { background: rgba(10, 10, 10, 0.9); border: 1px solid #333; padding: 15px; border-radius: 8px; position: relative; }
        .panel h4 { margin: 0 0 10px 0; font-size: 11px; text-transform: uppercase; color: #888; letter-spacing: 1px; }

        /* AUTOMATICKÝ VÝSLEDEK */
        #autoFlag { font-size: 1.6em; font-weight: bold; color: var(--neon); word-break: break-all; margin: 10px 0; min-height: 45px; text-align: center; }

        /* MANUÁLNÍ EDITOR */
        .editor-container { background: #000; border: 2px solid var(--edit); padding: 15px; border-radius: 5px; box-shadow: inset 0 0 10px rgba(255, 204, 0, 0.1); }
        #manualEdit { width: 100%; background: transparent; border: none; color: white; font-size: 2em; font-weight: bold; font-family: 'Fira Code', monospace; outline: none; text-align: center; }
        
        /* SEGMENTY */
        .chip-container { display: flex; flex-wrap: wrap; gap: 6px; margin-top: 5px; }
        .chip { background: #1a1a1a; border: 1px solid #444; padding: 5px 10px; border-radius: 4px; font-size: 12px; cursor: pointer; transition: 0.2s; font-weight: bold; }
        .chip:hover { border-color: var(--neon); background: #222; transform: translateY(-2px); color: var(--neon); }

        .btn { padding: 12px 20px; border: none; border-radius: 4px; font-weight: bold; cursor: pointer; text-transform: uppercase; transition: 0.3s; font-size: 12px; }
        .btn-green { background: var(--neon); color: black; }
        .btn-yellow { background: var(--edit); color: black; }
        .btn-red { background: var(--warn); color: white; }
        .btn:hover { opacity: 0.8; transform: scale(1.02); filter: brightness(1.2); }

        /* FOOTER NÁPOVĚDA */
        .hint-bar { background: #111; padding: 10px; border-radius: 5px; font-size: 11px; color: #666; border-left: 3px solid #555; }
    </style>
</head>
<body>

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

        <h2>📖 TAHAČ PRO KALI</h2>
        <h3>Web Exploitation</h3>
        <code>robots.txt / .git / .env</code>
        <code>F12 -> Storage -> Cookies</code>
        <code>' OR 1=1 --</code>
        
        <h3>Wireshark (pcap)</h3>
        <code>Follow -> TCP Stream (ASCII)</code>
        <code>frame contains "flag"</code>
        
        <h3>Forenzika (CLI)</h3>
        <code>strings soubor | grep "pico"</code>
        <code>exiftool soubor</code>
        <code>binwalk -e soubor</code>
        
        <h3>Hashing / Crypto</h3>
        <code>MD5 (32 ch), SHA1 (40 ch)</code>
        <code>Base64 (AZ, 0-9, ==)</code>
        <code>ROT13 (A -> N)</code>
    </div>

    <div class="main">
        <h1>🚀 CTF OMNIBOT v2.5 (AUTO-EXTRACTOR)</h1>

        <!-- VSTUPNÍ DATA -->
        <textarea id="rawInput" oninput="process()" placeholder="Zde 'vysyp' bordel z Wiresharku nebo zdrojového kódu (CTRL+V)..."></textarea>

        <div class="results-grid">
            <!-- LEVÝ PANEL: AUTOMATIKA -->
            <div class="panel">
                <h4>🤖 AUTO-DETECT (Guesstimate)</h4>
                <div id="autoFlag">ČEKÁM NA VSTUP...</div>
                <button class="btn btn-green" style="width:100%" onclick="copyToEditor()">POSLAT DO LABORKY ➔</button>
            </div>

            <!-- PRAVÝ PANEL: MANUÁLNÍ LABORKA -->
            <div class="panel" style="border-color: var(--edit);">
                <h4>🧪 MANUÁLNÍ LABORKA (Editace)</h4>
                <div class="editor-container">
                    <input type="text" id="manualEdit" placeholder="Flag se skládá zde...">
                </div>
                <div style="margin-top:10px; display:flex; gap:5px;">
                    <button class="btn btn-yellow" style="flex:3" onclick="copyFinal()">📋 KOPÍROVAT FINÁLNÍ FLAG</button>
                    <button class="btn btn-red" style="flex:1" onclick="document.getElementById('manualEdit').value=''">🗑️</button>
                </div>
            </div>
        </div>

        <!-- NALEZENÉ SEGMENTY -->
        <div class="panel">
            <h4>🧩 DETEKOVANÉ FRAGMENTY (Klikni pro přidání do laborky)</h4>
            <div id="chips" class="chip-container"></div>
        </div>

        <div class="hint-bar">
            <b>PRO TIP:</b> Pokud máš <code>.pcap</code>, jdi do Wiresharku, pravým na paket -> <b>Follow TCP Stream</b>. Celý text vlož nahoru. Můj algoritmus automaticky vyčistí binární smetí a najde jen čitelné Base64/Hex kousky.
        </div>
    </div>

<script>
    function process() {
        let input = document.getElementById('rawInput').value;
        let autoOut = document.getElementById('autoFlag');
        let chipsArea = document.getElementById('chips');
        
        if (!input.trim()) {
            autoOut.innerText = "ČEKÁM NA VSTUP...";
            chipsArea.innerHTML = "";
            return;
        }

        let segments = [];
        let seen = new Set();
        // Agresivní čištění od neviditelných znaků, které bourají kód
        let cleanText = input.replace(/[^\x20-\x7E]/g, ' ');

        // 1. SCANNER - Hledáme Base64, Hex a slova v závorkách
        // Vylepšený regex: hledá kousky s paddingem (=) i bez něj
        let b64Regex = /[A-Za-z0-9+/]{3,}=*/g;
        let matches = cleanText.match(b64Regex) || [];
        
        matches.forEach(m => {
            // Zkusíme dekódovat raw kousek + všechny paddingy (==)
            let trials = [m, m + "=", m + "=="];
            trials.forEach(trial => {
                try {
                    let d = atob(trial);
                    // Filter: dává výsledek smysl jako CTF string? (ASCII text bez hovadin)
                    if (/^[a-zA-Z0-9_{}!?-]+$/.test(d) && d.length > 1) {
                        if (!seen.has(d)) {
                            segments.push({ text: d, pos: input.indexOf(m) });
                            seen.add(d);
                        }
                    }
                } catch(e) {}
            });
        });

        // 2. SEŘAZENÍ (podle pozice v dumpu, což u pcapů pomáhá udržet logiku)
        segments.sort((a, b) => a.pos - b.pos);

        // 3. VÝPIS ČIPŮ (Segmentů)
        if (segments.length === 0) {
            chipsArea.innerHTML = "<span style='color:#555'>Žádné čitelné segmenty nenalezeny.</span>";
        } else {
            chipsArea.innerHTML = segments.map(s => 
                `<div class="chip" onclick="addToEditor('${s.text}')">${s.text}</div>`
            ).join("");
        }

        // 4. AUTOMATICKÁ KONSTRUKCE
        let prefix = "";
        let bodyParts = [];
        
        segments.forEach(s => {
            let t = s.text;
            if (t.toLowerCase().includes("ctf") || t.toLowerCase().includes("pico")) {
                prefix = t.replace(/{$/, ""); // Oříznout koncovou závorku pokud ji obsahuje
            } else if (t !== "{" && t !== "}") {
                if (!bodyParts.includes(t)) bodyParts.push(t);
            }
        });

        // Poskládej to inteligentně
        let body = bodyParts.join("");
        let finalAuto = "";
        if (prefix) {
            finalAuto = prefix.includes("{") ? prefix + body + "}" : prefix + "{" + body + "}";
        } else {
            finalAuto = body;
        }
        
        // Final sanitace pro auto out
        finalAuto = finalAuto.replace(/{+/g, '{').replace(/}+/g, '}');
        autoOut.innerText = finalAuto || "NIC NENALEZENO";
    }

    function copyToEditor() {
        let autoVal = document.getElementById('autoFlag').innerText;
        if (autoVal !== "ČEKÁM NA VSTUP..." && autoVal !== "NIC NENALEZENO") {
            document.getElementById('manualEdit').value = autoVal;
        }
    }

    function addToEditor(val) {
        let editor = document.getElementById('manualEdit');
        editor.value += val;
        // Malý visual efekt pro feedback
        editor.style.borderColor = "#00ff41";
        setTimeout(() => editor.style.borderColor = "var(--edit)", 200);
    }

    function copyFinal() {
        let val = document.getElementById('manualEdit').value;
        if (val) {
            navigator.clipboard.writeText(val).then(() => {
                alert("🏆 FLAG ZKOPÍROVÁN!\nHodně štěstí na SSPŠ!");
            });
        } else {
            alert("Laborka je prázdná!");
        }
    }
</script>

</body>
</html>
