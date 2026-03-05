index.html
<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <title>ISEKAIZONE</title>
    <link href="https://fonts.googleapis.com/css2?family=MedievalSharp&display=swap" rel="stylesheet">
    <style>
        :root { --p: #F4E4BC; --c: #990000; --n: #1A1A1B; --o: #E6B325; --side-w: 200px; }
        body { background: var(--p); font-family: 'MedievalSharp'; margin: 0; padding: 0 var(--side-w); min-height: 100vh; }

        /* STRISCE PUBBLICITARIE LATERALI */
        .torre {
            position: fixed; top: 0; width: var(--side-w); height: 100vh;
            background: rgba(0,0,0,0.03); border-left: 2px solid var(--n); border-right: 2px solid var(--n);
            display: flex; flex-direction: column; align-items: center; padding-top: 15px; overflow-y: auto;
        }
        .sx { left: 0; } .dx { right: 0; }
        .slot { width: 170px; height: 190px; background: white; border: 1px solid var(--n); margin-bottom: 20px; position: relative; }

        header { text-align: center; padding: 50px; border-bottom: 8px double var(--n); }
        .logo { font-size: 5rem; color: var(--c); letter-spacing: 12px; }
        .griglia { display: grid; grid-template-columns: 1fr 1fr; gap: 30px; padding: 50px; max-width: 1000px; margin: auto; }

        /* STENDARDI NERI */
        .stendardo {
            height: 180px; background: var(--n); color: white; border: none; font-size: 1.8rem; cursor: pointer;
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 90%, 0% 100%);
        }

        /* GESTIONE (SILVER00) */
        #admin { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.98); z-index: 9999; color: white; padding: 30px; overflow-y: auto; }
        .sez-box { background: #222; padding: 20px; margin-bottom: 30px; border: 1px solid var(--o); }
        textarea { width: 100%; height: 60px; background: #000; color: #0f0; font-family: monospace; font-size: 10px; margin-top: 5px; }
        .save-btn { width: 100%; padding: 20px; background: var(--c); color: white; border: none; font-size: 1.5rem; cursor: pointer; }
        .trig { position: fixed; bottom: 5px; left: 5px; font-size: 8px; opacity: 0.1; cursor: pointer; }
    </style>
</head>
<body>

    <aside class="torre sx">
        <div class="slot" id="sl-sx-1"></div><div class="slot" id="sl-sx-2"></div>
        <div class="slot" id="sl-sx-3"></div><div class="slot" id="sl-sx-4"></div>
    </aside>

    <aside class="torre dx">
        <div class="slot" id="sl-dx-1"></div><div class="slot" id="sl-dx-2"></div>
        <div class="slot" id="sl-dx-3"></div><div class="slot" id="sl-dx-4"></div>
    </aside>

    <header><div class="logo">ISEKAIZONE</div></header>

    <main class="griglia">
        <button class="stendardo" onclick="vai('cronache')">Cronache della Gilda</button>
        <button class="stendardo" onclick="vai('taverna')">Taverna delle ballate</button>
        <button class="stendardo" onclick="vai('sigilli')">Sigilli della Perseveranza</button>
        <button class="stendardo" onclick="vai('quest')">Albero delle Quest</button>
        <button class="stendardo" onclick="vaiMinistero()">Ministero delle Vie</button>
        <button class="stendardo" onclick="vai('giuramenti')">Sala dei Giuramenti</button>
    </main>

    <div id="admin">
        <h2>Gestione Matrice Pubblicitaria</h2>
        <div id="inputs-area"></div>
        <button class="save-btn" onclick="salvaTutto()">Salva Configurazione</button>
    </div>

    <div class="trig" onclick="apriAdmin()">[Gestione]</div>

    <script>
        const VIE = ['home', 'cronache', 'taverna', 'sigilli', 'quest', 'ministero', 'giuramenti'];
        const SLOTS = ['sx-1', 'sx-2', 'sx-3', 'sx-4', 'dx-1', 'dx-2', 'dx-3', 'dx-4'];

        window.onload = () => {
            const area = document.getElementById('inputs-area');
            VIE.forEach(v => {
                let h = `<div class="sez-box"><h3>Sezione: ${v.toUpperCase()}</h3>`;
                SLOTS.forEach(s => { h += `<label>${s}</label><textarea id="in-${v}-${s}"></textarea>`; });
                h += `</div>`;
                area.innerHTML += h;
            });
            vai('home');
        };

        function vaiMinistero() {
            if(prompt("Inserire Sigillo di Frontiera:") === "ISEKAI0") {
                let auth = prompt("Codice Identificativo Staff:");
                if(auth === "TITAN01") { vai('ministero'); alert("Redazione Media e Testi"); }
                else if(auth === "TITAN02") { vai('ministero'); alert("Visualizzazione e Stampa Schede"); }
                else if(auth === "TITAN03") { vai('ministero'); alert("Abbonamenti di persona"); }
                else if(auth === "SILVER00") { vai('ministero'); apriAdmin(); }
                else { alert("Codice non riconosciuto."); }
            }
        }

        function apriAdmin() {
            if (prompt("Accesso Management:") === "SILVER00") {
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

        function vai(nome) {
            SLOTS.forEach(s => {
                const contenitore = document.getElementById(`sl-${s}`);
                const codice = localStorage.getItem(`ads-${nome}-${s}`);
                contenitore.innerHTML = "";
                if (codice && codice.trim() !== "") {
                    const range = document.createRange();
                    contenitore.appendChild(range.createContextualFragment(codice));
                }
            });
        }
    </script>
</body>
</html>
