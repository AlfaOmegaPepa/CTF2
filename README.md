<!DOCTYPE html>
<html lang="cs">
<head>
    <meta charset="UTF-8">
    <title>⚡ CTF OMNIBOT v2.6 - SCROLL FIX</title>
    <style>
        :root { --neon: #00ff41; --bg: #050505; --kali: #2a8fbd; --warn: #ff003c; --edit: #ffcc00; --panel: #111; }
        
        /* Celkové nastavení pro scrollování */
        body { 
            font-family: 'Fira Code', 'Consolas', monospace; 
            background: var(--bg); 
            color: #e0e0e0; 
            margin: 0; 
            display: flex; 
            min-height: 100vh;
            overflow-x: hidden; 
            overflow-y: auto; /* TADY JE TA OPRAVA */
        }
        
        /* SIDEBAR - nyní zůstává na místě při skrolování */
        .sidebar { 
            width: 300px; 
            background: #000; 
            border-right: 2px solid #222; 
            padding: 20px; 
            font-size: 11px; 
            height: 100vh; 
            position: sticky; 
            top: 0; 
            overflow-y: auto; 
            flex-shrink: 0;
        }

        h2 { color: var(--neon); font-size: 14px; text-transform: uppercase; border-bottom: 1px solid #333; margin-top: 20px; padding-bottom: 5px; }
        h3 { color: var(--kali); font-size: 12px; margin: 10px 0 5px 0; text-transform: uppercase; }
        code { display: block; background: #111; padding: 5px; margin: 5px 0; border-left: 2px solid var(--kali); font-size: 10px; color: #f1fa8c; word-break: break-all; }

        /* QUICK TOOLS */
        .tool-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 5px; margin-bottom: 15px; }
        .tool-btn { background: #1a1a1a; color: var(--neon); border: 1px solid var(--neon); text-decoration: none; padding: 8px 5px; text-align: center; border-radius: 4px; font-weight: bold; font-size: 10px; transition: 0.2s; }
        .tool-btn:hover { background: var(--neon); color: black; box-shadow: 0 0 10px var(--neon); }

        /* HLAVNÍ ČÁST */
        .main { 
            flex-grow: 1; 
            padding: 20px; 
            display: flex; 
            flex-direction: column; 
            gap: 15px; 
            background: radial-gradient(circle at center, #111 0%, #050505 100%);
        }

        h1 { text-align: center; color: var(--neon); margin: 0; font-size: 24px; text-shadow: 0 0 10px var(--neon); }
        
        textarea#rawInput { width: 100%; height: 120px; background: #000; color: var(--neon); border: 2px solid #333; padding: 10px; border-radius: 5px; font-size: 13px; outline: none; box-sizing: border-box; }

        /* VÝSLEDKY */
        .results-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(350px, 1fr)); gap: 20px; }
        .panel { background: rgba(10, 10, 10, 0.9); border: 1px solid #333; padding: 15px; border-radius: 8px; }
        .panel h4 { margin: 0 0 10px 0; font-size: 11px; text-transform: uppercase; color: #888; }

        #autoFlag { font-size: 1.5em; font-weight: bold; color: var(--neon); word-break: break-all; margin: 10px 0; min-height: 45px; text-align: center; border: 1px dashed #333; padding: 10px; border-radius: 4px; }

        .editor-container { background: #000; border: 2px solid var(--edit); padding: 15px; border-radius: 5px; }
        #manualEdit { width: 100%; background: transparent; border: none; color: white; font-size: 1.8em; font-weight: bold; outline: none; text-align: center; font-family: monospace; }
        
        /* CHIPY - oblast pro kousky flagu */
        .chip-panel { border-left: 3px solid var(--kali); }
        .chip-container { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 5px; min-height: 50px; }
        .chip { background: #1a1a1a; border: 1px solid #444; padding: 8px 12px; border-radius: 4px; font-size: 13px; cursor: pointer; transition: 0.1s; font-weight: bold; color: #fff; }
        .chip:hover { border-color: var(--neon); background: #222; transform: scale(1.05); color: var(--neon); }

        .btn { padding: 12px 20px; border: none; border-radius: 4px; font-weight: bold; cursor: pointer; text-transform: uppercase; font-size: 12px; }
        .btn-green { background: var(--neon); color: black; margin-top: 10px; }
        .btn-yellow { background: var(--edit); color: black; }
        .btn-red { background: var(--warn); color: white; }
        
        .hint-bar { background: #111; padding: 15px; border-radius: 5px; font-size: 12px; color: #777; border-left: 3px solid #333; margin-top: 20px; }
    </style>
</head>
<body>

    <div class="sidebar">
        <h1 style="color:white; font-size: 18px; text-shadow:none;">⚔️ SOLVE TOOLS</h1>
        <div class="tool-grid">
            <a href="https://gchq.github.io/CyberChef/" target="_blank" class="tool-btn">🧪 CyberChef</a>
            <a href="https://aperisolve.fr/" target="_blank" class="tool-btn">🖼️ AperiSolve</a>
            <a href="https://www.dcode.fr/en" target="_blank" class="tool-btn">🔢 dCode.fr</a>
            <a href="https://crackstation.net/" target="_blank" class="tool-btn">🔑 CrackStation</a>
            <a href="https://tineye.com/" target="_blank" class="tool-btn">🔍 TinEye</a>
            <a href="https://cutt.ly/" target="_blank" class="tool-btn">🔗 Zkracovač</a>
        </div>

        <h2>📖 KALI / WIRESHARK</h2>
        <h3>Web Úkoly</h3>
        <code>robots.txt / .git / .env</code>
        <code>F12 -> Console -> 'flag'</code>
        <code>F12 -> Storage -> Cookies</code>
        
        <h3>Wireshark (pcap)</h3>
        <code>Right Click -> Follow -> TCP Stream</code>
        <code>Filtruj: frame contains "pico"</code>
        
        <h3>CLI (Terminál)</h3>
        <code>strings file | grep "flag"</code>
        <code>exiftool soubor.png</code>
        <code>base64 -d << "kód"</code>
    </div>

    <div class="main">
        <h1>🚀 CTF OMNIBOT v2.6</h1>

        <textarea id="rawInput" oninput="process()" placeholder="SEM VLOŽ TEN BORDEL Z NOTEPADU NEBO WIRESHARKU (CTRL+V)..."></textarea>

        <div class="results-grid">
            <div class="panel">
                <h4>🤖 AUTO-SOLVE</h4>
                <div id="autoFlag">ČEKÁM...</div>
                <button class="btn btn-green" style="width:100%" onclick="copyToEditor()">KOPÍROVAT DO LABORKY ➔</button>
            </div>

            <div class="panel" style="border-color: var(--edit);">
                <h4>🧪 RUČNÍ LABORKA</h4>
                <div class="editor-container">
                    <input type="text" id="manualEdit" placeholder="Flag poskládej tady...">
                </div>
                <div style="margin-top:10px; display:flex; gap:5px;">
                    <button class="btn btn-yellow" style="flex:3" onclick="copyFinal()">📋 KOPÍROVAT FINAL</button>
                    <button class="btn btn-red" style="flex:1" onclick="document.getElementById('manualEdit').value=''">🗑️</button>
                </div>
            </div>
        </div>

        <div class="panel chip-panel">
            <h4>🧩 DETEKOVANÉ KOUSKY (KLIKNI PRO SLOŽENÍ)</h4>
            <div id="chips" class="chip-container"></div>
        </div>

        <div class="hint-bar">
            <b>INFO:</b> Stránka je nyní plně skrolovací. Pokud vložíš obrovské množství textu, stačí kolečkem myši sjet dolů. <br>
            <i>Nápověda vlevo zůstává stále na místě pro tvoji potřebu.</i>
        </div>
    </div>

<script>
    function process() {
        let input = document.getElementById('rawInput').value;
        let autoOut = document.getElementById('autoFlag');
        let chipsArea = document.getElementById('chips');
        
        if (!input.trim()) { autoOut.innerText = "ČEKÁM..."; chipsArea.innerHTML = ""; return; }

        let segments = [];
        let seen = new Set();
        let cleanText = input.replace(/[^\x20-\x7E]/g, ' ');

        let b64Regex = /[A-Za-z0-9+/]{4,}=*/g;
        let matches = cleanText.match(b64Regex) || [];
        
        matches.forEach(m => {
            [m, m + "=", m + "=="].forEach(variant => {
                try {
                    let d = atob(variant);
                    if (/^[a-zA-Z0-9_{}!-]+$/.test(d) && d.length > 0) {
                        if (!seen.has(d)) {
                            segments.push({ text: d, pos: input.indexOf(m) });
                            seen.add(d);
                        }
                    }
                } catch(e) {}
            });
        });

        segments.sort((a, b) => a.pos - b.pos);

        if (segments.length === 0) {
            chipsArea.innerHTML = "Nic čitelného...";
        } else {
            chipsArea.innerHTML = segments.map(s => 
                `<div class="chip" onclick="addToEditor('${s.text}')">${s.text}</div>`
            ).join("");
        }

        // AUTO KONSTRUKCE
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
        if(val !== "ČEKÁM...") document.getElementById('manualEdit').value = val;
    }

    function copyFinal() {
        let val = document.getElementById('manualEdit').value;
        if (val) {
            navigator.clipboard.writeText(val);
            alert("Flag zkopírován: " + val);
        }
    }
</script>

</body>
</html>
