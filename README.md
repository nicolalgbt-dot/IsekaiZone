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

        /* LAYOUT ADS [cite: 2026-03-05] */
        .torre { position: fixed; top: 0; width: var(--striscetta-w); height: 100vh; z-index: 100; background: rgba(0,0,0,0.03); display: flex; flex-direction: column; align-items: center; padding-top: 20px; }
        .sx { left: 0; } .dx { right: 0; }
        .slot { width: 85%; aspect-ratio: 1/1.2; background: white; border: 1px solid var(--n); margin-bottom: 15px; background-size: cover; background-position: center; cursor: pointer; }

        /* HEADER & UTENTE */
        .top-bar { width: 70vw; display: flex; justify-content: space-between; align-items: center; padding: 20px 0; }
        .corno { font-size: 2.5rem; cursor: pointer; filter: drop-shadow(2px 2px 2px rgba(0,0,0,0.2)); }
        .user-icon { font-size: 2.5rem; cursor: pointer; color: #3182CE; }

        .main-view { width: 70vw; max-width: 1050px; text-align: center; position: relative; z-index: 10; }
        header { padding: 20px 0 40px 0; border-bottom: 5px double var(--n); margin-bottom: 50px; }
        .logo-txt { font-size: clamp(2.5rem, 8vw, 5rem); color: var(--c); letter-spacing: 12px; text-shadow: 2px 2px 0px var(--o); }

        /* GRIGLIA STENDARDI */
        .griglia { display: grid; grid-template-columns: repeat(3, 1fr); gap: 40px; }
        .stendardo { 
            aspect-ratio: 2/3.2; color: white; border: none; font-size: 1.2rem; cursor: pointer; 
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 92%, 0% 100%); 
            display: flex; align-items: center; justify-content: center; text-align: center; padding: 25px;
            transform-origin: top center; animation: vento 4.5s ease-in-out infinite; transition: 0.3s;
            line-height: 1.4;
        }

        /* MODALI & DASHBOARD SILVER00 [cite: 2026-03-04, 2026-03-05] */
        .modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.98); z-index: 1000; align-items: center; justify-content: center; padding: 20px; }
        .modal-inner { width: 100%; max-width: 900px; background: #1a1a1b; padding: 30px; border: 2px solid var(--o); color: white; overflow-y: auto; max-height: 90vh; }
        
        .form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; text-align: left; margin-top: 20px; }
        label { display: block; margin-bottom: 5px; color: var(--o); font-size: 0.75rem; }
        input, textarea { width: 100%; padding: 10px; background: #2a2a2b; border: 1px solid #444; color: white; }
        .btn-action { background: var(--o); color: black; padding: 12px; width: 100%; border: none; cursor: pointer; font-weight: bold; margin-top: 10px; }
    </style>
</head>
<body>

    <audio id="musica-portale" loop><source src="" type="audio/mpeg"></audio>

    <aside class="torre sx"><div class="slot" id="slot-sx-1" onclick="apriLink('slot-sx-1')"></div></aside>
    <aside class="torre dx"><div class="slot" id="slot-dx-1" onclick="apriLink('slot-dx-1')"></div></aside>

    <div class="main-view">
        <div class="top-bar">
            <div class="corno" onclick="toggleMusica()">📯</div>
            <div class="user-icon" onclick="apriModal('modal-login')">👤</div>
        </div>

        <header>
            <h1 class="logo-txt">ISEKAIZONE</h1>
        </header>

        <main class="griglia">
            <button class="stendardo" id="st-1" style="background:#315177;" onclick="apriLink('st-1')">Cronache della Gilda</button>
            <button class="stendardo" id="st-2" style="background:#795738;" onclick="apriLink('st-2')">Taverna delle ballate</button>
            <button class="stendardo" id="st-3" style="background:#518F4F;" onclick="apriLink('st-3')">Sigilli della Perseveranza</button>
            <button class="stendardo" id="st-4" style="background:#93194B;" onclick="apriLink('st-4')">Albero delle Quest</button>
            <button class="stendardo" id="st-5" style="background:#2D2D2E;" onclick="apriModal('modal-ministero')">Ministero delle Vie</button>
            <button class="stendardo" id="st-6" style="background:#A9D1DF; color:black;" onclick="apriLink('st-6')">Sala dei Giuramenti</button>
        </main>
    </div>

    <div id="modal-login" class="modal">
        <div class="modal-inner" style="max-width:400px; text-align:center;">
            <h2 style="color:var(--o);">ACCESSO VIANDANTE</h2>
            <div class="form-grid" style="grid-template-columns:1fr;">
                <input type="email" placeholder="Email">
                <input type="password" placeholder="Password">
            </div>
            <button class="btn-action">ENTRA</button>
            <p style="margin-top:15px; font-size:0.8rem;">Non hai un sigillo? <span style="color:var(--o); cursor:pointer;">Registrati</span></p>
            <button onclick="chiudiModale('modal-login')" class="btn-action" style="background:none; color:gray;">Chiudi</button>
        </div>
    </div>

    <div id="modal-ministero" class="modal">
        <div class="modal-inner" id="box-login">
            <h2 style="color:var(--c); text-align:center;">ACCESSO MINISTERIALE</h2>
            <input type="password" id="pass-sigillo" placeholder="Sigillo di Frontiera..." style="margin-top:20px; text-align:center;">
            <button class="btn-action" onclick="accessoAdmin()">SBLOCCA</button>
            <button onclick="chiudiModale('modal-ministero')" style="background:none; border:none; color:gray; width:100%; margin-top:10px; cursor:pointer;">Indietro</button>
        </div>

        <div class="modal-inner" id="box-silver" style="display:none;">
            <h2 style="color:var(--o);">EDITING SUPREMO (SILVER00)</h2>
            <div class="form-grid">
                <div class="form-group"><label>Link Cronache:</label><input type="text" id="link-st-1"></div>
                <div class="form-group"><label>URL MP3 Sottofondo:</label><input type="text" id="audio-bg"></div>
                <div class="form-group"><label>Immagine Sponsor SX:</label><input type="text" id="img-sx"></div>
                <div class="form-group"><label>Link Destinazione SX:</label><input type="text" id="link-sx"></div>
                <div class="form-group" style="grid-column: span 2;">
                    <label>Testo Quest (Albero):</label>
                    <textarea id="testo-quest" rows="3"></textarea>
                </div>
            </div>
            <button class="btn-action" onclick="salvaTutto()">💾 SALVA TUTTI I MEDIA E LINK</button>
            <button onclick="location.reload()" class="btn-action" style="background:#444; color:white; margin-top:20px;">ESCI E APPLICA</button>
        </div>
    </div>

    <script>
        let config = JSON.parse(localStorage.getItem('isekaisave')) || { links: {}, images: {}, audio: "" };

        function apriModal(id) { document.getElementById(id).style.display = 'flex'; }
        function chiudiModale(id) { document.getElementById(id).style.display = 'none'; }

        function toggleMusica() {
            const m = document.getElementById('musica-portale');
            return m.paused ? m.play() : m.pause();
        }

        function accessoAdmin() {
            if(document.getElementById('pass-sigillo').value === "ISEKAI0") {
                const cod = prompt("Chiave Reparto (es. SILVER00):");
                if(cod === "SILVER00") {
                    document.getElementById('box-login').style.display = 'none';
                    document.getElementById('box-silver').style.display = 'block';
                    caricaCampi();
                }
            }
        }

        function salvaTutto() {
            config.links['st-1'] = document.getElementById('link-st-1').value;
            config.links['slot-sx-1'] = document.getElementById('link-sx').value;
            config.images['sx'] = document.getElementById('img-sx').value;
            config.audio = document.getElementById('audio-bg').value;
            localStorage.setItem('isekaisave', JSON.stringify(config));
            alert("Sincronizzazione completata offline e pronta per DB!");
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
            if(url) window.open(url, '_blank');
            else alert("Nessun link impostato.");
        }

        window.onload = applicaConfig;
    </script>
</body>
</html>
