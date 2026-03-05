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
            --striscetta-w: 10vw; /* Striscette da 10cm ipotetici su 100cm */
        }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'MedievalSharp', cursive; }
        
        body { 
            background: var(--p); 
            min-height: 100vh; 
            display: flex;
            flex-direction: column;
            align-items: center;
            overflow-x: hidden;
        }

        /* STRISCETTE PUBBLICITARIE (10% LARGHEZZA) */
        .torre { 
            position: fixed; top: 0; width: var(--striscetta-w); height: 100vh; 
            background: rgba(0,0,0,0.01); display: flex; flex-direction: column; 
            align-items: center; padding-top: 25px; z-index: 100;
        }
        .sx { left: 0; border-right: 1px solid rgba(0,0,0,0.03); }
        .dx { right: 0; border-left: 1px solid rgba(0,0,0,0.03); }
        .slot { width: 75%; aspect-ratio: 1/1.2; background: white; border: 1px solid var(--n); margin-bottom: 20px; }

        /* AREA CENTRALE (70% PER GARANTIRE SPAZIO TRA STRISCETTE E CONTENUTI) */
        .main-view { 
            width: 70vw; 
            max-width: 1100px;
            margin: 0 auto;
            padding: 20px;
            text-align: center;
            flex-grow: 1;
        }

        header { padding: 30px 0; border-bottom: 5px double var(--n); margin-bottom: 50px; position: relative; }
        .logo-box { position: relative; display: inline-block; }
        .logo-txt { font-size: clamp(3rem, 7vw, 4.5rem); color: var(--c); letter-spacing: 10px; }

        .icon-top { position: absolute; font-size: 2rem; cursor: pointer; top: -35px; transition: 0.2s; }
        .icon-top:hover { transform: scale(1.1); color: var(--c); }
        .corno { left: 10px; } .user { right: 10px; }

        /* EFFETTO VENTO REALISTICO */
        @keyframes vento {
            0% { transform: rotate(0deg); }
            50% { transform: rotate(1.2deg); }
            100% { transform: rotate(0deg); }
        }

        .griglia { 
            display: grid; grid-template-columns: repeat(3, 1fr); 
            gap: 40px; padding: 20px; margin-bottom: 80px; 
        }

        .stendardo { 
            aspect-ratio: 2/3.2; color: white; border: none; font-size: 1.2rem; cursor: pointer; 
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 92%, 0% 100%); 
            display: flex; align-items: center; justify-content: center; text-align: center; 
            padding: 20px; box-shadow: 0 8px 15px rgba(0,0,0,0.1);
            transform-origin: top center;
            animation: vento 4s ease-in-out infinite;
        }
        .stendardo:nth-child(even) { animation-delay: 1s; animation-duration: 5s; }
        .stendardo:nth-child(3n) { animation-delay: 0.5s; animation-duration: 4.5s; }

        /* MODALE SCHEDA PERSONALE / LOGIN */
        .modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.98); z-index: 1000; color: white; padding: 40px; overflow-y: auto; }
        .modal-inner { max-width: 400px; margin: auto; text-align: left; }
        .form-group { margin-bottom: 15px; }
        label { display: block; margin-bottom: 5px; color: var(--o); font-size: 0.9rem; }
        input { width: 100%; padding: 12px; background: #222; border: 1px solid #444; color: white; }
        .btn-gold { background: var(--o); color: black; padding: 15px; width: 100%; border: none; cursor: pointer; font-weight: bold; margin-top: 15px; }

        /* AREA DATI NECESSARI (FOOTER) */
        footer {
            width: 100%; padding: 60px 20px; text-align: center;
            border-top: 1px solid rgba(0,0,0,0.05); color: rgba(0,0,0,0.3); font-size: 0.8rem;
        }
    </style>
</head>
<body>

    <audio id="musica-home" loop><source src="URL_TUA_MUSICA.mp3" type="audio/mpeg"></audio>

    <aside class="torre sx">
        <div class="slot" id="sl-sx-1"></div><div class="slot" id="sl-sx-2"></div><div class="slot" id="sl-sx-3"></div>
    </aside>

    <aside class="torre dx">
        <div class="slot" id="sl-dx-1"></div><div class="slot" id="sl-dx-2"></div><div class="slot" id="sl-dx-3"></div>
    </aside>

    <div class="main-view">
        <header>
            <div class="logo-box">
                <span class="icon-top corno" title="Suona il Corno" onclick="suonaCorno()">📯</span>
                <div class="logo-txt">ISEKAIZONE</div>
                <span class="icon-top user" title="Scheda Personale" onclick="toggleModal('login')">👤</span>
            </div>
        </header>

        <main class="griglia">
            <button class="stendardo" style="background:#315177;" onclick="vai('cronache')">Cronache della Gilda</button>
            <button class="stendardo" style="background:#795738;" onclick="vai('taverna')">Taverna delle ballate</button>
            <button class="stendardo" style="background:#518F4F;" onclick="vai('sigilli')">Sigilli della Perseveranza</button>
            <button class="stendardo" style="background:#93194B;" onclick="vai('quest')">Albero delle Quest</button>
            <button class="stendardo" style="background:#2D2D2E;" onclick="vaiMinistero()">Ministero delle Vie</button>
            <button class="stendardo" style="background:#A9D1DF; color:black;" onclick="vai('giuramenti')">Sala dei Giuramenti</button>
        </main>
    </div>

    <footer>
        [ Qui verranno inseriti i dati necessari e istituzionali ]
    </footer>

    <div id="login" class="modal">
        <div class="modal-inner">
            <h2 style="color:var(--o); margin-bottom:20px; text-align:center;">SCHEDA PERSONALE</h2>
            
            <div class="form-group">
                <label>Nome di Elezione:</label>
                <input type="text" id="nome-elezione" placeholder="Il nome con cui ti identifichi">
            </div>
            
            <div class="form-group">
                <label>Pronomi:</label>
                <input type="text" id="pronomi" placeholder="es. lui/lei/loro">
            </div>
            
            <div class="form-group">
                <label>Identità di Genere:</label>
                <input type="text" id="identita" placeholder="Definisci la tua espressione">
            </div>

            <div class="form-group" style="margin-top:20px; border-top:1px solid #333; padding-top:15px;">
                <label>Email Accesso:</label>
                <input type="email">
            </div>

            <button class="btn-gold" onclick="salvaScheda()">SALVA E AGGIORNA IDENTITÀ</button>
            <button onclick="toggleModal('login')" style="background:none; color:gray; border:none; width:100%; margin-top:20px; cursor:pointer;">Chiudi Finestra</button>
        </div>
    </div>

    <script>
        let viaAttuale = 'home';
        const SLOTS = ['sx-1', 'sx-2', 'sx-3', 'dx-1', 'dx-2', 'dx-3'];

        function suonaCorno() {
            const audio = document.getElementById('musica-home');
            if(viaAttuale === 'home') audio.paused ? audio.play() : audio.pause();
            else alert("Il corno risuona solo nella Homepage.");
        }

        function toggleModal(id) {
            const m = document.getElementById(id);
            m.style.display = (m.style.display === 'block') ? 'none' : 'block';
        }

        function vai(nomeVia) {
            viaAttuale = nomeVia;
            // Simulazione rinfresco 8 slot pubblicitari (meccanismo SILVER00)
            console.log("Stirpe in movimento verso: " + nomeVia);
        }

        function vaiMinistero() {
            if(prompt("Sigillo di Frontiera del Ministero:") === "ISEKAI0") {
                const auth = prompt("Codice Reparto Staff (es. TITAN01 o SILVER00):");
                if(["SILVER00", "TITAN01", "TITAN02", "TITAN03"].includes(auth)) {
                    vai('ministero');
                    alert("Accesso autorizzato al Ministero delle Vie.");
                }
            }
        }

        function salvaScheda() {
            const nome = document.getElementById('nome-elezione').value;
            alert("Identità aggiornata: " + nome + ". I dati saranno visibili nel forum e nella scheda fisica.");
            toggleModal('login');
        }
    </script>
</body>
</html>
