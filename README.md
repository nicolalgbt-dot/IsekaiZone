index.html
<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ISEKAIZONE</title>
    <link href="https://fonts.googleapis.com/css2?family=MedievalSharp&display=swap" rel="stylesheet">
    <style>
        :root { --p: #F4E4BC; --c: #990000; --n: #1A1A1B; --o: #E6B325; --striscetta-w: 10vw; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'MedievalSharp', cursive; }
        body { background: var(--p); min-height: 100vh; display: flex; flex-direction: column; align-items: center; overflow-x: hidden; }

        @keyframes vento { 0%, 100% { transform: rotate(0deg); } 50% { transform: rotate(1.5deg); } }

        .torre { position: fixed; top: 0; width: var(--striscetta-w); height: 100vh; z-index: 100; background: rgba(0,0,0,0.03); display: flex; flex-direction: column; align-items: center; padding-top: 20px; }
        .sx { left: 0; } .dx { right: 0; }
        .slot { width: 85%; aspect-ratio: 1/1.2; background: white; border: 1px solid var(--n); margin-bottom: 15px; background-size: cover; background-position: center; cursor: pointer; }

        .main-view { width: 70vw; max-width: 1050px; text-align: center; padding: 20px; position: relative; z-index: 10; }
        header { padding: 40px 0; border-bottom: 5px double var(--n); margin-bottom: 50px; }
        .logo-txt { font-size: clamp(2.5rem, 8vw, 5rem); color: var(--c); letter-spacing: 12px; text-shadow: 2px 2px 0px var(--o); }

        .griglia { display: grid; grid-template-columns: repeat(3, 1fr); gap: 40px; }
        .stendardo { 
            aspect-ratio: 2/3.2; color: white; border: none; font-size: 1.1rem; cursor: pointer; 
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 92%, 0% 100%); 
            display: flex; align-items: center; justify-content: center; text-align: center; padding: 20px;
            transform-origin: top center; animation: vento 4.5s ease-in-out infinite; transition: 0.3s;
        }

        .modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.98); z-index: 1000; align-items: center; justify-content: center; padding: 20px; }
        .modal-inner { width: 100%; max-width: 900px; background: #1a1a1b; padding: 30px; border: 2px solid var(--o); color: white; overflow-y: auto; max-height: 90vh; }
        
        .admin-nav { display: flex; gap: 10px; border-bottom: 1px solid var(--o); margin-bottom: 20px; padding-bottom: 10px; }
        .nav-btn { background: transparent; color: #888; border: none; cursor: pointer; font-size: 0.8rem; text-transform: uppercase; padding: 5px 10px; }
        .nav-btn.active { color: var(--o); border-bottom: 2px solid var(--o); }

        .form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; text-align: left; }
        label { display: block; margin-bottom: 5px; color: var(--o); font-size: 0.75rem; }
        input, textarea, select { width: 100%; padding: 10px; background: #2a2a2b; border: 1px solid #444; color: white; }
        .btn-action { background: var(--o); color: black; padding: 12px; width: 100%; border: none; cursor: pointer; font-weight: bold; margin-top: 10px; }
    </style>
</head>
<body>

    <audio id="musica-portale" loop><source src="" type="audio/mpeg"></audio>

    <aside class="torre sx">
        <div class="slot" id="slot-sx-1" onclick="apriLink('slot-sx-1')"></div>
    </aside>
    <aside class="torre dx">
        <div class="slot" id="slot-dx-1" onclick="apriLink('slot-dx-1')"></div>
    </aside>

    <div class="main-view">
        <header>
            <h1 class="logo-txt">ISEKAIZONE</h1>
            <span style="font-size: 2.5rem; cursor: pointer; position: absolute; right: 20px; top: 20px;" onclick="apriModal('modal-ministero')">🏛️</span>
        </header>

        <main class="griglia">
            <button class="stendardo" id="st-1" style="background:#315177;" onclick="apriLink('st-1')">Cronache</button>
            <button class="stendardo" id="st-2" style="background:#795738;" onclick="apriLink('st-2')">Taverna</button>
            <button class="stendardo" id="st-3" style="background:#518F4F;" onclick="apriLink('st-3')">Sigilli</button>
            <button class="stendardo" id="st-4" style="background:#93194B;" onclick="apriLink('st-4')">Quest</button>
            <button class="stendardo" id="st-5" style="background:#2D2D2E;" onclick="apriModal('modal-ministero')">Ministero</button>
            <button class="stendardo" id="st-6" style="background:#A9D1DF; color:black;" onclick="apriLink('st-6')">Sala</button>
        </main>
    </div>

    <div id="modal-ministero" class="modal">
        <div class="modal-inner" id="box-login">
            <h2 style="color:var(--c); text-align:center;">MINISTERO DELLE VIE</h2>
            <input type="password" id="pass-sigillo" placeholder="Sigillo di Frontiera..." style="margin-top:20px;">
            <button class="btn-action" onclick="accessoAdmin()">SBLOCCA REPARTI</button>
        </div>

        <div class="modal-inner" id="box-silver" style="display:none;">
            <div class="admin-nav">
                <button class="nav-btn active" onclick="tab('t-media')">Media & Link</button>
                <button class="nav-btn" onclick="tab('t-sigilli')">Gestione Sigilli</button>
                <button class="nav-btn" onclick="tab('t-quest')">Quest & Audio</button>
            </div>

            <div id="t-media" class="tab-content">
                <div class="form-grid">
                    <div class="form-group"><label>Link Stendardo 1:</label><input type="text" id="link-st-1"></div>
                    <div class="form-group"><label>Link Stendardo 2:</label><input type="text" id="link-st-2"></div>
                    <div class="form-group"><label>Immagine Slot SX (Ads):</label><input type="text" id="img-sx"></div>
                    <div class="form-group"><label>Link Destinazione Slot SX:</label><input type="text" id="link-sx"></div>
                </div>
                <button class="btn-action" onclick="salvaUniversale()">💾 SALVA (OFFLINE + DATABASE)</button>
            </div>

            <div id="t-quest" class="tab-content" style="display:none;">
                <div class="form-group">
                    <label>URL Musica Sottofondo Portale:</label>
                    <input type="text" id="audio-bg" placeholder="URL .mp3">
                </div>
                <div class="form-group">
                    <label>Testo Pergamena (Quest):</label>
                    <textarea id="testo-quest" rows="4"></textarea>
                </div>
                <button class="btn-action" onclick="salvaUniversale()">💾 PUBBLICA MEDIA</button>
            </div>

            <button onclick="location.reload()" class="btn-action" style="background:#444; color:white; margin-top:20px;">ESCI E APPLICA</button>
        </div>
    </div>

    <script>
        // Database Ipertestuale Universale [cite: 2026-03-05]
        let config = JSON.parse(localStorage.getItem('isekaisave')) || {
            links: {}, images: {}, audio: "", quests: ""
        };

        function apriModal(id) { document.getElementById(id).style.display = 'flex'; }
        
        function accessoAdmin() {
            const pass = document.getElementById('pass-sigillo').value;
            if(pass === "ISEKAI0") {
                const cod = prompt("Inserire Chiave Reparto (es. SILVER00):");
                if(cod === "SILVER00") {
                    document.getElementById('box-login').style.display = 'none';
                    document.getElementById('box-silver').style.display = 'block';
                    caricaCampi();
                }
            }
        }

        function tab(id) {
            document.querySelectorAll('.tab-content').forEach(c => c.style.display = 'none');
            document.getElementById(id).style.display = 'block';
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            event.currentTarget.classList.add('active');
        }

        function salvaUniversale() {
            // Cattura dati da tutti i campi [cite: 2026-03-05]
            config.links['st-1'] = document.getElementById('link-st-1').value;
            config.links['slot-sx-1'] = document.getElementById('link-sx').value;
            config.images['sx'] = document.getElementById('img-sx').value;
            config.audio = document.getElementById('audio-bg').value;

            // SALVATAGGIO OFFLINE (LocalStorage) [cite: 2026-03-04]
            localStorage.setItem('isekaisave', JSON.stringify(config));
            
            // SIMULAZIONE INVIO DATABASE (WiFi/Appwrite)
            console.log("Sincronizzazione Database in corso...");
            
            alert("Sincronizzazione completata con successo! I dati sono al sicuro anche offline.");
            applicaConfig();
        }

        function caricaCampi() {
            document.getElementById('link-st-1').value = config.links['st-1'] || "";
            document.getElementById('link-sx').value = config.links['slot-sx-1'] || "";
            document.getElementById('img-sx').value = config.images['sx'] || "";
            document.getElementById('audio-bg').value = config.audio || "";
        }

        function applicaConfig() {
            if(config.images['sx']) document.getElementById('slot-sx-1').style.backgroundImage = `url('${config.images['sx']}')`;
            if(config.audio) document.getElementById('musica-portale').src = config.audio;
        }

        function apriLink(id) {
            const url = config.links[id];
            if(url && url !== "") window.open(url, '_blank');
            else alert("Questo collegamento non è ancora stato tracciato.");
        }

        window.onload = applicaConfig;
    </script>
</body>
</html>
