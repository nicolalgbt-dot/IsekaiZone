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
            --side-w: 180px; /* Strisce pubblicitarie ancora più sottili per dezoomare il centro */
        }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'MedievalSharp', cursive; }
        body { background: var(--p); color: var(--n); padding: 0 var(--side-w); min-height: 100vh; overflow-x: hidden; }

        /* TORRI ADS LATERALI SOTTILI */
        .torre { position: fixed; top: 0; width: var(--side-w); height: 100vh; background: rgba(0,0,0,0.02); border-left: 1px solid var(--n); border-right: 1px solid var(--n); display: flex; flex-direction: column; align-items: center; padding-top: 15px; overflow-y: auto; z-index: 100; }
        .sx { left: 0; } .dx { right: 0; }
        .slot { width: 150px; height: 170px; background: white; border: 1px solid var(--n); margin-bottom: 20px; position: relative; }

        /* HEADER E ICONE */
        header { text-align: center; padding: 40px 20px; border-bottom: 8px double var(--n); position: relative; }
        .logo { font-size: 4.5rem; color: var(--c); letter-spacing: 12px; cursor: pointer; }
        .corner-icon { position: absolute; font-size: 2.2rem; color: var(--n); cursor: pointer; top: 30px; transition: 0.2s; }
        .corner-icon:hover { transform: scale(1.2); color: var(--c); }
        .icon-corno { left: 40px; }
        .icon-user { right: 40px; }

        /* GRIGLIA STENDARDI */
        .griglia { display: grid; grid-template-columns: repeat(3, 1fr); gap: 25px; padding: 40px; max-width: 1000px; margin: auto; }
        .stendardo { height: 200px; color: white; border: none; font-size: 1.6rem; cursor: pointer; clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 92%, 0% 100%); display: flex; align-items: center; justify-content: center; text-align: center; padding: 15px; box-shadow: 0 8px 15px rgba(0,0,0,0.2); transition: 0.3s; }
        .stendardo:hover { transform: translateY(8px); filter: brightness(1.2); }

        /* MODALI LOGIN E GESTIONE */
        #login, #admin { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.98); z-index: 9999; color: white; padding: 30px; overflow-y: auto; }
        .form-box { max-width: 450px; margin: 50px auto; padding: 30px; border: 2px solid var(--o); background: #111; }
        .form-group { margin-bottom: 15px; text-align: left; }
        label { display: block; margin-bottom: 5px; color: var(--o); font-size: 0.9rem; }
        input { width: 100%; padding: 12px; background: #222; border: 1px solid #444; color: white; margin-bottom: 10px; }
        .btn { width: 100%; padding: 15px; border: none; cursor: pointer; font-weight: bold; font-family: 'MedievalSharp'; text-transform: uppercase; }
        .btn-cremisi { background: var(--c); color: white; margin-top: 10px; }
        .btn-chiudi { background: transparent; color: white; border: 1px solid white; margin-bottom: 20px; width: auto; padding: 5px 15px; }

        /* ADMIN AREA (SILVER00) */
        textarea { width: 100%; height: 50px; background: #000; color: #0f0; font-family: monospace; font-size: 10px; margin-top: 5px; }
        .sez-box { background: #1a1a1a; padding: 15px; margin-bottom: 20px; border: 1px solid #333; }
        .trig { position: fixed; bottom: 5px; left: 5px; font-size: 8px; opacity: 0.1; cursor: pointer; z-index: 10000; }
    </style>
</head>
<body>

    <audio id="musica-home" loop>
        <source src="URL_TUA_MUSICA.mp3" type="audio/mpeg">
    </audio>

    <aside class="torre sx">
        <div class="slot" id="sl-sx-1"></div><div class="slot" id="sl-sx-2"></div>
        <div class="slot" id="sl-sx-3"></div><div class="slot" id="sl-sx-4"></div>
    </aside>

    <aside class="torre dx">
        <div class="slot" id="sl-dx-1"></div><div class="slot" id="sl-dx-2"></div>
        <div class="slot" id="sl-dx-3"></div><div class="slot" id="sl-dx-4"></div>
    </aside>

    <header>
        <span class="corner-icon icon-corno" title="Suona il Corno (Musica Home)" onclick="gestisciMusica()">📯</span>
        <div class="logo" onclick="vai('home')">ISEKAIZONE</div>
        <span class="corner-icon icon-user" title="Accedi alla tua Scheda" onclick="apriLogin()">👤</span>
    </header>

    <main class="griglia">
        <button class="stendardo" style="background-color: #315177;" onclick="vai('cronache')">Cronache della Gilda</button>
        <button class="stendardo" style="background-color: #795738;" onclick="vai('taverna')">Taverna delle ballate</button>
        <button class="stendardo" style="background-color: #518F4F;" onclick="vai('sigilli')">Sigilli della Perseveranza</button>
        <button class="stendardo" style="background-color: #93194B;" onclick="vai('quest')">Albero delle Quest</button>
        <button class="stendardo" style="background-color: #2D2D2E;" onclick="vaiMinistero()">Ministero delle Vie</button>
        <button class="stendardo" style="background-color: #A9D1DF; color:black;" onclick="vai('giuramenti')">Sala dei Giuramenti</button>
    </main>

    <div id="login">
        <button class="btn-chiudi" onclick="chiudiModal('login')">TORNA INDIETRO</button>
        <div class="form-box">
            <h1 style="text-align:center; margin-bottom:20px;">IDENTIFICAZIONE VIANDANTE</h1>
            <div class="form-group">
                <label>E-mail registrata:</label>
                <input type="email" placeholder="...">
            </div>
            <div class="form-group">
                <label>Parola d'Ordine:</label>
                <input type="password" placeholder="***">
            </div>
            <div class="form-group">
                <label>Codice Personale Scheda:</label>
                <input type="text" placeholder="####">
            </div>
            <button class="btn btn-cremisi">ACCEDI ALLA SCHEDA PERSONALE</button>
        </div>
    </div>

    <div id="admin">
        <button class="btn-chiudi" onclick="chiudiModal('admin')">CHIUDI MATRICE</button>
        <h2 style="margin-bottom:20px;">MATRICE PUBBLICITARIA | GESTIONE COMPLETA</h2>
        <div id="inputs-area"></div>
        <button class="btn btn-cremisi" onclick="salvaTutto()">SIGILLA E ATTIVA</button>
    </div>

    <div class="trig" onclick="apriAdmin()">[GESTIONE]</div>

    <script>
        const VIE = ['home', 'cronache', 'taverna', 'sigilli', 'quest', 'ministero', 'giuramenti'];
        const SLOTS = ['sx-1', 'sx-2', 'sx-3', 'sx-4', 'dx-1', 'dx-2', 'dx-3', 'dx-4'];
        let viaAttuale = 'home';

        window.onload = () => {
            const area = document.getElementById('inputs-area');
            VIE.forEach(v => {
                let h = `<div class="sez-box"><h3>CONFIGURAZIONE: ${v.toUpperCase()}</h3>`;
                SLOTS.forEach(s => { h += `<label>${s}</label><textarea id="in-${v}-${s}"></textarea>`; });
                h += `</div>`;
                area.innerHTML += h;
            });
            vai('home');
        };

        // FUNZIONE CORNO (SOLO HOMEPAGE)
        function gestisciMusica() {
            const audio = document.getElementById('musica-home');
            if (viaAttuale === 'home') {
                if (audio.paused) audio.play();
                else audio.pause();
            } else {
                alert("Il corno può essere suonato solo nella Homepage.");
            }
        }

        // NAVIGAZIONE E REFRESH ADS
        function vai(nomeVia) {
            viaAttuale = nomeVia;
            SLOTS.forEach(s => {
                const contenitore = document.getElementById(`sl-${s}`);
                const codice = localStorage.getItem(`ads-${nomeVia}-${s}`);
                contenitore.innerHTML = "";
                if (codice && codice.trim() !== "") {
                    const range = document.createRange();
                    contenitore.appendChild(range.createContextualFragment(codice));
                } else {
                    contenitore.innerHTML = "<small style='color:#666'>Slot Libero</small>";
                }
            });
        }

        // ACCESSO PROTETTO MINISTERO
        function vaiMinistero() {
            if(prompt("Sigillo di Frontiera del Ministero:") === "ISEKAI0") {
                let auth = prompt("Codice Identificativo Staff:");
                if(auth === "SILVER00") { alert("Gestione Completa."); apriAdmin(); vai('ministero'); }
                else if(auth === "TITAN01") { alert("Redazione Media/Testi."); vai('ministero'); }
                else if(auth === "TITAN02") { alert("Archivio Schede Personali."); vai('ministero'); }
                else if(auth === "TITAN03") { alert("Ufficio Abbonamenti Fisici."); vai('ministero'); }
                else { alert("Codice Reparto Errato."); }
            }
        }

        function apriLogin() { document.getElementById('login').style.display = 'block'; }
        function chiudiModal(id) { document.getElementById(id).style.display = 'none'; }

        function apriAdmin() {
            if (prompt("Chiave Management:") === "SILVER00") {
                document.getElementById('admin').style.display = 'block';
                VIE.forEach(v => SLOTS.forEach(s => {
                    document.getElementById(`in-${v}-${s}`).value = localStorage.getItem(`ads-${v}-${s}`) || "";
                }));
            }
        }

        function salvaTutto() {
            VIE.forEach(v => SLOTS.forEach(s => {
                localStorage.setItem(`ads-${v}-${s}`, document.getElementById(`in-${v}-${s}`).value);
            }));
            location.reload();
        }
    </script>
</body>
</html>
