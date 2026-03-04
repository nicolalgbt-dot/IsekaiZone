index.html
<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ISEKAIZONE | Gilda dell'Attivismo</title>

    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=MedievalSharp&display=swap" rel="stylesheet">

    <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>

    <style>
        * { 
            font-family: 'MedievalSharp', cursive !important; 
            box-sizing: border-box; 
        }

        :root {
            --nero: #1A1A1B; --cremisi: #990000;
            --pergamena-chiara: #FDF5E6; /* OldLace: un crema molto riposante */
            --pergamena-header: #f4e4bc;
            --pergamena-scura: #e2d1a6;
            --lapis: #26619C; --rame: #B87333; --smeraldo: #50C878; 
            --rubino: #E0115F; --argento: #C0C0C0;
            
            /* DIMENSIONI INCREMENTATE */
            --font-small: 20px;    /* Da 18px a 20px */
            --font-medium: 26px;   /* Da 22px a 26px */
            --font-large: 45px;    /* Da 38px a 45px */
            --font-header: 100px;  /* Da 85px a 100px */
        }

        body { 
            background-color: var(--pergamena-chiara); 
            color: var(--nero); 
            margin: 0; 
            font-size: var(--font-small); 
        }

        /* HEADER PERFETTAMENTE CENTRATO E MAESTOSO */
        header {
            background: linear-gradient(to bottom, var(--pergamena-header), var(--pergamena-scura));
            border-bottom: 8px double var(--nero);
            height: 300px; /* Altezza fissa per centratura assoluta */
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            width: 100%;
        }

        header h1 {
            margin: 0;
            padding: 0;
            font-size: var(--font-header);
            letter-spacing: 20px;
            text-transform: uppercase;
            color: var(--nero);
            text-shadow: 6px 6px 0px var(--cremisi);
            line-height: 1;
        }

        /* LAYOUT */
        .layout { display: grid; grid-template-columns: 260px 1fr 260px; min-height: 80vh; }
        .sidebar { padding: 30px; border: 1px solid rgba(0,0,0,0.05); text-align: center; }

        /* NAVIGAZIONE STENDARDI */
        nav { display: flex; justify-content: center; gap: 30px; padding: 60px 0; }
        .stendardo {
            width: 150px; height: 210px; display: flex; align-items: center; justify-content: center;
            text-align: center; color: white; font-weight: bold; 
            font-size: 24px;
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 85%, 0% 100%);
            cursor: pointer; border: none; transition: 0.3s; padding: 20px;
        }
        .stendardo:hover { transform: translateY(15px); filter: brightness(1.2); }
        
        .lapis { background-color: var(--lapis); }
        .rame { background-color: var(--rame); }
        .smeraldo { background-color: var(--smeraldo); }
        .rubino { background-color: var(--rubino); }
        .argento { background-color: var(--argento); color: var(--nero); }

        /* CONTENUTO PUBBLICO */
        main { padding: 0 60px; }
        .page-title { 
            color: var(--cremisi); 
            text-align: center; 
            font-size: var(--font-large); 
            text-transform: uppercase;
            margin-bottom: 50px;
            text-decoration: underline double var(--nero);
        }
        .content-box { 
            font-size: var(--font-medium); 
            line-height: 1.9; 
            text-align: justify;
        }

        /* VARCO ACCESSO */
        #access-vault {
            display: none; position: fixed; top:0; left:0; width:100%; height:100%;
            background: rgba(0,0,0,0.98); z-index: 9999;
            align-items: center; justify-content: center; flex-direction: column;
        }
        .vault-card { background: var(--pergamena-header); border: 10px solid var(--cremisi); padding: 70px; text-align: center; box-shadow: 0 0 120px rgba(153,0,0,0.6); }
        .vault-input { font-size: 4em; text-align: center; width: 100%; margin: 30px 0; border: none; border-bottom: 4px solid var(--nero); background: transparent; outline: none; color: var(--nero); }

        /* GESTIONE LIVE */
        #admin-panel { 
            display: none; position: fixed; top:0; left:0; width:100%; height:100vh;
            background: #f0f0f1; z-index: 10000;
        }
        .admin-grid { display: grid; grid-template-columns: 500px 1fr; height: 100%; }
        .admin-ctrl { background: #fff; border-right: 5px solid var(--argento); padding: 40px; overflow-y: auto; }
        .admin-live { background: var(--pergamena-chiara); padding: 60px; overflow-y: auto; }

        .form-card { background: #fdfdfd; border: 2px solid #bbb; padding: 25px; margin-bottom: 30px; }
        input, textarea, select { font-size: 18px; width: 100%; padding: 15px; margin: 10px 0; }
        .btn-wp { background: var(--lapis); color: white; border: none; padding: 25px; width: 100%; cursor: pointer; font-size: 26px; text-transform: uppercase; border-radius: 5px; }
        .btn-exit { background: var(--cremisi); color: white; border: none; padding: 15px; cursor: pointer; margin-bottom: 30px; width: 100%; font-size: 20px; }
    </style>
</head>
<body>

<header>
    <h1>ISEKAIZONE</h1>
</header>

<div class="layout">
    <aside class="sidebar" id="sidebar-left"></aside>

    <main>
        <nav>
            <div onclick="changePage('news')" class="stendardo lapis">NEWS</div>
            <div onclick="changePage('forum')" class="stendardo rame">FORUM</div>
            <div onclick="changePage('abbonamenti')" class="stendardo smeraldo">SUBS</div>
            <div onclick="changePage('quest')" class="stendardo rubino">QUEST</div>
            <div onclick="showVault()" class="stendardo argento">GESTORI</div>
        </nav>

        <div id="content-view">
            <h2 id="view-title" class="page-title">Cronache della Gilda</h2>
            <div id="view-media"></div>
            <div id="view-text" class="content-box">Le cronache stanno prendendo forma...</div>
        </div>
    </main>

    <aside class="sidebar" id="sidebar-right"></aside>
</div>

<div id="access-vault">
    <div class="vault-card">
        <h2 style="font-size: 50px; color:var(--cremisi)">LIVE FORGE</h2>
        <p style="font-size: 24px;">Inserisci il sigillo di comando</p>
        <input type="password" id="vault-pass" class="vault-input">
        <button class="btn-wp" onclick="verifyVault()">SBLOCCA</button>
        <p onclick="hideVault()" style="cursor:pointer; text-decoration: underline; margin-top: 40px; font-size: 22px;">Torna alla Gilda</p>
    </div>
</div>

<div id="admin-panel">
    <div class="admin-grid">
        <div class="admin-ctrl">
            <button class="btn-exit" onclick="location.reload()">← CHIUDI BACHECA</button>
            <h2 style="font-size: 38px; color:var(--cremisi)">Live Forge</h2>
            
            <div class="form-card">
                <h3 style="font-size: 28px;">Modifica Sezione</h3>
                <select id="adm-page" onchange="syncFields()">
                    <option value="news">News</option>
                    <option value="forum">Forum</option>
                    <option value="abbonamenti">Abbonamenti</option>
                    <option value="quest">Quest</option>
                </select>
                <input type="text" id="adm-title" oninput="livePrev()" placeholder="Titolo">
                <textarea id="adm-text" rows="14" oninput="livePrev()" placeholder="Contenuto HTML"></textarea>
                <input type="text" id="adm-media" oninput="livePrev()" placeholder="URL Immagine Principale">
                <input type="text" id="adm-link" oninput="livePrev()" placeholder="Link Ipertestuale (Pulsante)">
                <button class="btn-wp" onclick="saveData()">INCIDI NEL DATABASE</button>
            </div>
        </div>

        <div class="admin-live">
            <div style="text-align:center; opacity:0.4; font-size: 18px; margin-bottom: 20px;">[ ANTEPRIMA IN TEMPO REALE ]</div>
            <h2 id="prev-title" class="page-title"></h2>
            <div id="prev-media"></div>
            <div id="prev-text" class="content-box"></div>
        </div>
    </div>
</div>

<script>
    // COLLEGA QUI I TUOI DATI SUPABASE
    const SB_URL = 'IL_TUO_URL_SUPABASE';
    const SB_KEY = 'LA_TUA_ANON_KEY';
    const supabaseClient = supabase.createClient(SB_URL, SB_KEY);

    async function changePage(slug) {
        const { data } = await supabaseClient.from('isekai_pages').select('*').eq('slug', slug).single();
        if(data) {
            document.getElementById('view-title').innerText = data.title;
            document.getElementById('view-text').innerHTML = data.content;
            document.getElementById('view-media').innerHTML = data.media_url ? `<img src="${data.media_url}" style="width:100%; border-radius:15px; margin-bottom:30px; box-shadow: 0 10px 30px rgba(0,0,0,0.1);">` : '';
            if(data.external_link) document.getElementById('view-text').innerHTML += `<br><center><a href="${data.external_link}" target="_blank" style="color:var(--cremisi); font-weight:bold; font-size:1.2em;">[ ACCEDI AL COLLEGAMENTO ]</a></center>`;
        }
    }

    function showVault() { document.getElementById('access-vault').style.display = 'flex'; document.getElementById('vault-pass').focus(); }
    function hideVault() { document.getElementById('access-vault').style.display = 'none'; }
    function verifyVault() {
        if(document.getElementById('vault-pass').value === "SILVER00") {
            hideVault(); document.getElementById('admin-panel').style.display = 'block'; syncFields();
        } else { alert("Sigillo non riconosciuto."); }
    }

    function livePrev() {
        document.getElementById('prev-title').innerText = document.getElementById('adm-title').value;
        document.getElementById('prev-text').innerHTML = document.getElementById('adm-text').value;
        const img = document.getElementById('adm-media').value;
        document.getElementById('prev-media').innerHTML = img ? `<img src="${img}" style="width:100%; border-radius:15px;">` : '';
    }

    async function syncFields() {
        const s = document.getElementById('adm-page').value;
        const { data } = await supabaseClient.from('isekai_pages').select('*').eq('slug', s).single();
        if(data) {
            document.getElementById('adm-title').value = data.title;
            document.getElementById('adm-text').value = data.content;
            document.getElementById('adm-media').value = data.media_url || '';
            document.getElementById('adm-link').value = data.external_link || '';
            livePrev();
        }
    }

    async function saveData() {
        const s = document.getElementById('adm-page').value;
        const upd = {
            title: document.getElementById('adm-title').value,
            content: document.getElementById('adm-text').value,
            media_url: document.getElementById('adm-media').value,
            external_link: document.getElementById('adm-link').value
        };
        const { error } = await supabaseClient.from('isekai_pages').update(upd).eq('slug', s);
        if(!error) alert("Cronache aggiornate con successo!");
        else alert("Errore durante il salvataggio: " + error.message);
    }

    window.onload = () => changePage('news');
</script>
</body>
</html>
