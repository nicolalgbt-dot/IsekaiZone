index.html
<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <title>ISEKAIZONE | Fortezza Monetizzata Totale</title>
    <link href="https://fonts.googleapis.com/css2?family=MedievalSharp&display=swap" rel="stylesheet">
    <style>
        :root { 
            --p: #F4E4BC; --c: #990000; --n: #1A1A1B; --o: #E6B325; 
            --side-w: 200px; /* Larghezza strisce laterali */
        }

        body { 
            background: var(--p); 
            font-family: 'MedievalSharp'; 
            margin: 0; 
            padding: 0 var(--side-w); /* Spazio per le due strisce */
            min-height: 100vh;
        }

        /* --- LE DUE STRISCE PUBBLICITARIE --- */
        .striscia-ads {
            position: fixed; top: 0; width: var(--side-w); height: 100vh;
            background: rgba(0,0,0,0.03); border-left: 3px double var(--n); border-right: 3px double var(--n);
            display: flex; flex-direction: column; align-items: center; 
            padding-top: 15px; overflow-y: auto; z-index: 100;
        }
        .sinistra { left: 0; }
        .destra { right: 0; }

        /* Gli slot (4 per striscia) */
        .slot {
            width: 170px; height: 180px; background: white; border: 2px solid var(--n);
            margin-bottom: 20px; position: relative; display: flex; align-items: center; justify-content: center;
            box-shadow: 4px 4px 0 rgba(0,0,0,0.1);
        }
        .slot::after { 
            content: 'SIGILLO'; position: absolute; bottom: 0; width: 100%; 
            background: var(--n); color: var(--o); font-size: 8px; text-align: center; 
        }

        /* --- CONTENUTO CENTRALE --- */
        header { text-align: center; padding: 50px 20px; border-bottom: 8px double var(--n); }
        .logo { font-size: 4rem; color: var(--c); letter-spacing: 10px; }

        .griglia-vie {
            display: grid; grid-template-columns: 1fr 1fr; gap: 30px; 
            padding: 50px; max-width: 900px; margin: auto;
        }

        .stendardo {
            height: 180px; background: var(--n); color: white;
            display: flex; align-items: center; justify-content: center;
            font-size: 1.8rem; cursor: pointer; transition: 0.3s;
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 90%, 0% 100%);
        }
        .stendardo:hover { transform: translateY(10px); filter: brightness(1.4); }

        /* --- PANNELLO GESTIONE (SILVER00) --- */
        #admin-panel {
            display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.95);
            z-index: 10000; color: white; padding: 30px; overflow-y: auto;
        }
        .config-sezione { 
            background: #222; padding: 15px; margin-bottom: 30px; border: 2px solid var(--o); 
        }
        .config-sezione h3 { color: var(--o); margin-bottom: 10px; border-bottom: 1px solid #444; }
        textarea { 
            width: 100%; height: 60px; background: #000; color: #0f0; 
            font-family: monospace; font-size: 10px; margin-bottom: 10px; 
        }
        .save-btn { 
            background: var(--c); color: white; padding: 25px; width: 100%; 
            border: none; cursor: pointer; font-size: 1.8rem; font-weight: bold; 
        }

        .trigger-admin { 
            position: fixed; bottom: 5px; left: 5px; font-size: 8px; 
            opacity: 0.1; cursor: pointer; z-index: 10001; 
        }
    </style>
</head>
<body>

    <aside class="striscia-ads sinistra">
        <div class="slot" id="slot-sin-1"></div>
        <div class="slot" id="slot-sin-2"></div>
        <div class="slot" id="slot-sin-3"></div>
        <div class="slot" id="slot-sin-4"></div>
    </aside>

    <aside class="striscia-ads destra">
        <div class="slot" id="slot-des-1"></div>
        <div class="slot" id="slot-des-2"></div>
        <div class="slot" id="slot-des-3"></div>
        <div class="slot" id="slot-des-4"></div>
    </aside>

    <header>
        <div class="logo">ISEKAIZONE</div>
    </header>

    <main class="griglia-vie">
        <div class="stendardo" onclick="attivaSezione('home')">HOME</div>
        <div class="stendardo" onclick="attivaSezione('cronache')">CRONACHE</div>
        <div class="stendardo" onclick="attivaSezione('taverna')">TAVERNA</div>
        <div class="stendardo" onclick="attivaSezione('giuramenti')">GIURAMENTI</div>
    </main>

    <div id="admin-panel">
        <h2>Gestore Fortezza Flussi</h2>
        <div id="form-generato"></div>
        <button class="save-btn" onclick="salvaTutto()">SIGILLA E ATTIVA TUTTO</button>
        <button onclick="document.getElementById('admin-panel').style.display='none'" style="width:100%; background:none; color:white; margin-top:15px; cursor:pointer;">Annulla</button>
    </div>

    <div class="trigger-admin" onclick="apriAdmin()">[GESTIONE]</div>

    <script>
        const sezioni = ['home', 'cronache', 'taverna', 'giuramenti'];
        const slots = ['sin-1', 'sin-2', 'sin-3', 'sin-4', 'des-1', 'des-2', 'des-3', 'des-4'];

        // Inizializzazione
        window.onload = () => {
            costruisciFormAdmin();
            attivaSezione('home'); // Pagina iniziale conta!
        };

        function costruisciFormAdmin() {
            const container = document.getElementById('form-generato');
            sezioni.forEach(sez => {
                let html = `<div class="config-sezione"><h3>${sez.toUpperCase()}</h3>`;
                slots.forEach(s => {
                    html += `<label>Slot ${s}:</label><textarea id="in-${sez}-${s}"></textarea>`;
                });
                html += `</div>`;
                container.innerHTML += html;
            });
        }

        function apriAdmin() {
            if (prompt("Codice Management:") === "SILVER00") {
                document.getElementById('admin-panel').style.display = 'block';
                // Carica i dati attuali
                sezioni.forEach(sez => {
                    slots.forEach(s => {
                        document.getElementById(`in-${sez}-${s}`).value = localStorage.getItem(`ads-${sez}-${s}`) || "";
                    });
                });
            }
        }

        function salvaTutto() {
            sezioni.forEach(sez => {
                slots.forEach(s => {
                    const val = document.getElementById(`in-${sez}-${s}`).value;
                    localStorage.setItem(`ads-${sez}-${s}`, val);
                });
            });
            alert("Configurazione Salvata. Il sistema si riavvia.");
            location.reload();
        }

        function attivaSezione(nome) {
            // Logica di massimizzazione: ricarica istantanea di tutti gli 8 slot
            slots.forEach(s => {
                const contenitore = document.getElementById(`slot-${s}`);
                const scriptCodice = localStorage.getItem(`ads-${nome}-${s}`);
                
                contenitore.innerHTML = ""; // Distrugge il vecchio annuncio
                
                if (scriptCodice && scriptCodice.trim() !== "") {
                    // Iniezione "viva" per forzare il pagamento della visualizzazione
                    const range = document.createRange();
                    contenitore.appendChild(range.createContextualFragment(scriptCodice));
                } else {
                    contenitore.innerHTML = "<small style='color:#ddd'>Vuoto</small>";
                }
            });
            console.log("Stirpe: 8 nuovi sigilli attivati per " + nome);
        }
    </script>
</body>
</html>
