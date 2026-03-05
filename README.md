index.html
<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ISEKAIZONE</title>
    <link href="https://fonts.googleapis.com/css2?family=MedievalSharp&display=swap" rel="stylesheet">
    <style>
        :root { 
            --p: #F4E4BC; --c: #990000; --n: #1A1A1B; --o: #E6B325; 
            --striscetta-w: 10vw; 
        }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'MedievalSharp', cursive; }
        
        body { background: var(--p); min-height: 100vh; display: flex; flex-direction: column; align-items: center; overflow-x: hidden; }

        @keyframes vento { 0%, 100% { transform: rotate(0deg); } 50% { transform: rotate(1.5deg); } }

        /* LAYOUT 1:8:1 - ADS LATERALI */
        .torre { position: fixed; top: 0; width: var(--striscetta-w); height: 100vh; z-index: 100; display: flex; flex-direction: column; align-items: center; padding-top: 20px; background: rgba(0,0,0,0.03); }
        .sx { left: 0; } .dx { right: 0; }
        .slot { width: 80%; aspect-ratio: 1/1.2; background: white; border: 1px solid var(--n); margin-bottom: 15px; }

        /* AREA CENTRALE */
        .main-view { width: 70vw; max-width: 1050px; text-align: center; padding: 20px; flex-grow: 1; position: relative; z-index: 10; }

        header { padding: 40px 0; border-bottom: 5px double var(--n); margin-bottom: 50px; position: relative; }
        .logo-txt { font-size: clamp(2.5rem, 8vw, 5rem); color: var(--c); letter-spacing: 12px; text-shadow: 2px 2px 0px var(--o); }

        /* GRIGLIA STENDARDI */
        .griglia { display: grid; grid-template-columns: repeat(3, 1fr); gap: 40px; }
        .stendardo { 
            aspect-ratio: 2/3.2; color: white; border: none; font-size: 1.1rem; cursor: pointer; 
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 92%, 0% 100%); 
            display: flex; align-items: center; justify-content: center; text-align: center; padding: 20px;
            transform-origin: top center; animation: vento 4.5s ease-in-out infinite;
            box-shadow: 0 10px 15px rgba(0,0,0,0.2); transition: 0.3s;
        }
        .stendardo:hover { filter: brightness(1.2); transform: scale(1.05); }

        /* MODALI */
        .modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.98); z-index: 1000; align-items: center; justify-content: center; padding: 20px; }
        .modal-inner { width: 100%; max-width: 650px; background: #1a1a1b; padding: 35px; border: 2px solid var(--o); color: white; }
        
        .form-group { margin-bottom: 20px; text-align: left; }
        label { display: block; margin-bottom: 8px; color: var(--o); font-size: 0.8rem; letter-spacing: 1px; }
        input { width: 100%; padding: 12px; background: #2a2a2b; border: 1px solid #444; color: white; font-family: sans-serif; }
        
        .btn-action { background: var(--o); color: black; padding: 15px; width: 100%; border: none; cursor: pointer; font-weight: bold; font-size: 1rem; margin-top: 10px; }
        .btn-admin { background: var(--c); color: white; margin-top: 15px; }

        /* CENTRO DI COMANDO - PULITI DAI CODICI */
        .admin-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 30px; }
        .dept-card { background: #1a1a1b; border: 1px solid #444; padding: 25px; cursor: pointer; transition: 0.3s; text-align: center; }
        .dept-card:hover { border-color: var(--o); background: #222; box-shadow: 0 0 15px var(--o); }
        .dept-card b { display: block; margin-bottom: 8px; font-size: 1.1rem; color: var(--o); letter-spacing: 1px; }
        .dept-card small { font-size: 0.75rem; color: #aaa; text-transform: uppercase; }

        .congelata { filter: grayscale(1) blur(2px); pointer-events: none; opacity: 0.5; }
    </style>
</head>
<body>

    <audio id="musica-portale" loop><source src="URL_TUA_MUSICA.mp3" type="audio/mpeg"></audio>

    <aside class="torre sx"><div class="slot"></div><div class="slot"></div><div class="slot"></div></aside>
    <aside class="torre dx"><div class="slot"></div><div class="slot"></div><div class="slot"></div></aside>

    <div class="main-view">
        <header>
            <span style="position: absolute; left: 10px; top: -10px; font-size: 2.5rem; cursor: pointer;" onclick="gestisciMusica()" title="Il Corno">📯</span>
            <h1 class="logo-txt">ISEKAIZONE</h1>
            <span style="position: absolute; right: 10px; top: -10px; font-size: 2.5rem; cursor: pointer;" onclick="apriModal('modal-viandante')" title="Profilo">👤</span>
        </header>

        <main class="griglia">
            <button class="stendardo" style="background:#315177;" onclick="alert('In viaggio verso Cronache...')">Cronache della Gilda</button>
            <button class="stendardo" style="background:#795738;" onclick="alert('In viaggio verso Taverna...')">Taverna delle ballate</button>
            <button class="stendardo" style="background:#518F4F;" onclick="apriModal('modal-sigilli')">Sigilli della Perseveranza</button>
            <button class="stendardo" style="background:#93194B;" onclick="alert('In viaggio verso Quest e Pergamene...')">Albero delle Quest</button>
            <button class="stendardo" style="background:#2D2D2E;" onclick="apriModal('modal-ministero')">Ministero delle Vie</button>
            <button class="stendardo" style="background:#A9D1DF; color:black;" onclick="alert('In viaggio verso Giuramenti...')">Sala dei Giuramenti</button>
        </main>
    </div>

    <div id="modal-viandante" class="modal">
        <div class="modal-inner" id="v-login-box">
            <h2 style="color:var(--o); text-align:center;">ACCESSO VIANDANTE</h2>
            <div class="form-group"><label>EMAIL:</label><input type="email" id="v-mail"></div>
            <div class="form-group"><label>PASSWORD:</label><input type="password" id="v-pass"></div>
            <button class="btn-action" onclick="logicaViandante()">VEDI SCHEDA</button>
            <button onclick="chiudiModale('modal-viandante')" style="background:none; border:none; color:gray; width:100%; margin-top:20px; cursor:pointer;">Annulla</button>
        </div>
        <div class="modal-inner" id="v-scheda-box" style="display:none;">
            <h2 style="color:var(--o); text-align:center;">SCHEDA PERSONALE</h2>
            <div id="scheda-content">
                <div class="form-group"><label>NOME DI ELEZIONE:</label><input type="text" placeholder="Identità scelta"></div>
                <div class="form-group"><label>PRONOMI:</label><input type="text" placeholder="es. lui/lei/loro"></div>
                <div class="form-group"><label>IDENTITÀ DI GENERE:</label><input type="text" placeholder="La tua identità"></div>
            </div>
            <button class="btn-action" onclick="alert('Salvato!'); chiudiModale('modal-viandante')">SALVA</button>
        </div>
    </div>

    <div id="modal-ministero" class="modal">
        <div class="modal-inner" id="m-sigillo-box">
            <h2 style="color:var(--c); text-align:center;">MINISTERO DELLE VIE</h2>
            <div class="form-group"><label>SIGILLO DI FRONTIERA:</label><input type="password" id="m-sigillo"></div>
            <button class="btn-action btn-admin" onclick="logicaMinistero()">SBLOCCA</button>
            <button onclick="chiudiModale('modal-ministero')" style="background:none; border:none; color:gray; width:100%; margin-top:20px; cursor:pointer;">Indietro</button>
        </div>
        <div class="modal-inner" id="m-dashboard-box" style="display:none;">
            <h2 style="color:var(--o); text-align:center;">CENTRO DI COMANDO</h2>
            <div class="admin-grid">
                <div class="dept-card" onclick="reparto('SILVER00')">
                    <b>AMMINISTRAZIONE TOTALE</b><small>EDITING LEVEL 0</small>
                </div>
                <div class="dept-card" onclick="reparto('TITAN01')">
                    <b>EDITING E CONTENUTI</b><small>EDITING LEVEL 1</small>
                </div>
                <div class="dept-card" onclick="reparto('TITAN02')">
                    <b>ARCHIVIO SCHEDE</b><small>VISUALIZZAZIONE E STAMPA</small>
                </div>
                <div class="dept-card" onclick="reparto('TITAN03')">
                    <b>GESTIONE SIGILLI</b><small>RINNOVI E PAGAMENTI</small>
                </div>
            </div>
            <button onclick="chiudiModale('modal-ministero')" style="background:none; border:none; color:gray; width:100%; margin-top:30px; cursor:pointer;">Disconnetti</button>
        </div>
    </div>

    <div id="modal-sigilli" class="modal">
        <div class="modal-inner" style="border-color:#518F4F;">
            <h2 style="color:#518F4F; text-align:center;">SIGILLI DELLA PERSEVERANZA</h2>
            <p style="margin:20px 0; font-size:0.9rem; text-align:center;">Scongela la tua scheda: contributo di 5€ per 30 giorni di energia identitaria.</p>
            <button class="btn-action" style="background:#518F4F; color:white;" onclick="alert('Reindirizzamento al pagamento...')">SCONGELA / RINNOVA</button>
            <button onclick="chiudiModale('modal-sigilli')" style="background:none; border:none; color:gray; width:100%; margin-top:20px; cursor:pointer;">Chiudi</button>
        </div>
    </div>

    <script>
        function gestisciMusica() {
            const a = document.getElementById('musica-portale');
            a.paused ? a.play() : a.pause();
        }

        function apriModal(id) { document.getElementById(id).style.display = 'flex'; }
        function chiudiModale(id) { document.getElementById(id).style.display = 'none'; }

        function logicaViandante() {
            if(document.getElementById('v-mail').value && document.getElementById('v-pass').value) {
                document.getElementById('v-login-box').style.display = 'none';
                document.getElementById('v-scheda-box').style.display = 'block';
            }
        }

        function logicaMinistero() {
            // Sigillo di primo livello per entrare nella dashboard
            if(document.getElementById('m-sigillo').value === "ISEKAI0") {
                document.getElementById('m-sigillo-box').style.display = 'none';
                document.getElementById('m-dashboard-box').style.display = 'block';
            } else { alert("Sigillo non riconosciuto."); }
        }

        function reparto(target) {
            // Il prompt ora è generico per non suggerire i codici
            const p = prompt("Inserire Chiave d'Accesso Reparto:");
            if(p === target) {
                alert("Chiave accettata. Accesso al settore autorizzato.");
                if(target === "SILVER00") {
                    location.href = "https://nicolalgbt-dot.github.io/IsekaiZone/admin-dashboard";
                }
                // Qui si possono aggiungere i link per gli altri reparti (TITAN01-03) [cite: 2026-03-04]
            } else {
                alert("Chiave errata.");
            }
        }
    </script>
</body>
</html>
