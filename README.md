index.html
<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ISEKAIZONE | Gilda dell'Attivismo</title>
    <link href="https://fonts.googleapis.com/css2?family=MedievalSharp&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js"></script>
    <style>
        :root {
            --nero-inchiostro: #1A1A1B; --cremisi: #990000;
            --pergamena: #f4e4bc; --pergamena-scura: #e2d1a6;
            --lapis: #26619C; --rame: #B87333; --smeraldo: #50C878; 
            --rubino: #E0115F; --argento: #C0C0C0;
        }

        /* Applicazione universale del font MedievalSharp */
        * { font-family: 'MedievalSharp', cursive; }

        body { background-color: white; color: var(--nero-inchiostro); margin: 0; }

        /* HEADER */
        header {
            background: linear-gradient(to bottom, var(--pergamena), var(--pergamena-scura));
            border-bottom: 4px double var(--nero-inchiostro);
            padding: 40px 20px; text-align: center;
        }
        header h1 { margin: 0; font-size: 4.5em; letter-spacing: 12px; text-transform: uppercase; color: var(--nero-inchiostro); text-shadow: 3px 3px 0px var(--cremisi); }

        /* LAYOUT */
        .layout { display: grid; grid-template-columns: 220px 1fr 220px; min-height: 85vh; }
        .sidebar { padding: 25px; border: 1px solid #f0f0f0; background: #fff; text-align: center; }
        .sidebar img { max-width: 100%; margin-bottom: 25px; border: 1px solid #ddd; }

        /* NAVIGAZIONE */
        nav { display: flex; justify-content: center; gap: 18px; padding: 40px 0; }
        .stendardo {
            width: 115px; height: 160px; display: flex; align-items: center; justify-content: center;
            text-align: center; color: white; font-weight: bold; text-decoration: none; font-size: 1em;
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 88%, 0% 100%); transition: 0.3s;
            cursor: pointer; border: none; padding: 10px;
        }
        .stendardo:hover { transform: translateY(12px); }
        .lapis { background-color: var(--lapis); } .rame { background-color: var(--rame); }
        .smeraldo { background-color: var(--smeraldo); } .rubino { background-color: var(--rubino); }
        .argento { background-color: var(--argento); color: var(--nero-inchiostro); }

        /* CONTENUTI */
        main { padding: 20px 60px; }
        .page-title { color: var(--cremisi); text-align: center; text-transform: uppercase; font-size: 2.5em; }
        .content-box { line-height: 1.8; font-size: 1.3em; margin-top: 35px; }
        img.main-img { width: 100%; border-radius: 10px; margin: 30px 0; border: 2px solid var(--pergamena-scura); }

        /* VAULT D'ACCESSO */
        #access-vault {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(26, 26, 27, 0.98); z-index: 10001;
            align-items: center; justify-content: center; flex-direction: column;
        }
        .vault-box { background: var(--pergamena); border: 4px solid var(--cremisi); padding: 50px; text-align: center; border-radius: 4px; box-shadow: 0 0 50px var(--cremisi); }
        .vault-input { background: transparent; border: none; border-bottom: 2px solid var(--nero-inchiostro); font-size: 2em; text-align: center; width: 100%; margin: 30px 0; outline: none; color: var(--nero-inchiostro); }

        /* GESTIONE LIVE */
        #admin-panel { 
            display: none; background: #f0f0f1; border-top: 8px solid var(--cremisi); 
            position: fixed; top: 0; left: 0; width: 100%; height: 100vh; z-index: 10002;
        }
        .admin-container { display: grid; grid-template-columns: 400px 1fr; height: 100%; }
        .admin-sidebar { background: white; border-right: 1px solid #ccd0d4; padding: 20px; overflow-y: auto; color: #333; }
        .admin-preview { background: white; overflow-y: auto; padding: 20px; border-left: 5px solid var(--argento); }
        
        .card { background: #f9f9f9; padding: 15px; border: 1px solid #ddd; margin-bottom: 15px; border-radius: 4px; }
        input, textarea, select { width: 100%; padding: 10px; margin: 8px 0; border: 1px solid #8c8f94; border-radius: 4px; font-size: 0.9em; }
        .btn-save { background: #2271b1; color: white; border: none; padding: 12px; width: 100%; cursor: pointer; font-weight: bold; margin-top: 10px; text-transform: uppercase; }
        .btn-exit { background: var(--cremisi); color: white; border: none; padding: 10px; cursor: pointer; margin-bottom: 10px; width: 100%; }
    </style>
</head>
<body>

<header>
    <h1>ISEKAIZONE</h1>
</header>

<div class="layout" id="public-layout">
    <aside class="sidebar" id="ads-l"></aside>
    <main>
        <nav id="public-nav">
            <button onclick="changePage('news')" class="stendardo lapis">NEWS</button>
            <button onclick="changePage('forum')" class="stendardo rame">FORUM</button>
            <button onclick="changePage('abbonamenti')" class="stendardo smeraldo">ABBONAMENTI</button>
            <button onclick="changePage('quest')" class="stendardo rubino">QUEST</button>
            <button onclick="showVault()" class="stendardo argento">GESTIONE</button>
        </nav>
        <div id="content-display">
            <h2 id="view-h2" class="page-title">Cronache In Corso...</h2>
            <div id="view-media"></div>
            <div id="view-body" class="content-box"></div>
        </div>
    </main>
    <aside class="sidebar" id="ads-r"></aside>
</div>

<div id="access-vault">
    <div class="vault-box">
        <h2>IDENTIFICAZIONE GILDA</h2>
        <p>Inserisci il sigillo SILVER00</p>
        <input type="password" id="vault-pass" class="vault-input">
        <button class="btn-save" onclick="validateVault()">SBLOCCA IL LIVE FORGE</button>
        <button onclick="hideVault()" style="background:none; border:none; color:var(--cremisi); cursor:pointer; margin-top:15px; text-decoration: underline;">Annulla</button>
    </div>
</div>

<div id="admin-panel">
    <div class="admin-container">
        <div class="admin-sidebar">
            <button class="btn-exit" onclick="closeGestione()">← ESCI DALLA BACHECA</button>
            <h2 style="color:var(--cremisi)">Live Forge</h2>
            <p>Le modifiche appaiono a destra istantaneamente.</p>
            
            <div class="card">
                <h3>Editor di Sezione</h3>
                <select id="adm-page" onchange="syncFields()">
                    <option value="news">News</option>
                    <option value="forum">Forum</option>
                    <option value="abbonamenti">Abbonamenti</option>
                    <option value="quest">Quest</option>
                </select>
                <input type="text" id="adm-title" oninput="updatePreview()" placeholder="Titolo della Pagina">
                <textarea id="adm-text" rows="10" oninput="updatePreview()" placeholder="Contenuto (HTML permesso)"></textarea>
                <input type="text" id="adm-img" oninput="updatePreview()" placeholder="URL Immagine Principale">
                <input type="text" id="adm-link" oninput="updatePreview()" placeholder="Link Ipertestuale Esterno">
                <button class="btn-save" onclick="pushUpdates()">SALVA PERMANENTEMENTE</button>
            </div>

            <div class="card">
                <h3>Sponsor Laterali</h3>
                <select id="adm-side">
                    <option value="left">Sinistra</option>
                    <option value="right">Destra</option>
                </select>
                <input type="text" id="adm-ad-img" placeholder="URL Banner Immagine">
                <input type="text" id="adm-ad-link" placeholder="Link Destinazione">
                <button class="btn-save" onclick="pushAd()">AGGIORNA SPONSOR</button>
            </div>
        </div>

        <div class="admin-preview" id="live-preview">
            <div style="background:var(--pergamena); padding:10px; border-bottom:2px solid var(--nero-inchiostro); text-align:center; font-weight:bold;">ANTEPRIMA LIVE DELLA GILDA</div>
            <h2 id="prev-h2" class="page-title"></h2>
            <div id="prev-media"></div>
            <div id="prev-body" class="content-box"></div>
        </div>
    </div>
</div>

<script>
    const URL_SB = 'IL_TUO_URL_SUPABASE';
    const KEY_SB = 'LA_TUA_ANON_KEY';
    const sb = supabase.createClient(URL_SB, KEY_SB);

    // Gestione Vault
    function showVault() { document.getElementById('access-vault').style.display = 'flex'; }
    function hideVault() { document.getElementById('access-vault').style.display = 'none'; }
    function validateVault() {
        if(document.getElementById('vault-pass').value === "SILVER00") {
            hideVault(); document.getElementById('admin-panel').style.display = 'block'; syncFields();
        } else { alert("Sigillo non riconosciuto."); }
    }
    function closeGestione() { document.getElementById('admin-panel').style.display = 'none'; location.reload(); }

    // Anteprima Live
    function updatePreview() {
        document.getElementById('prev-h2').innerText = document.getElementById('adm-title').value;
        document.getElementById('prev-body').innerHTML = document.getElementById('adm-text').value;
        const img = document.getElementById('adm-img').value;
        document.getElementById('prev-media').innerHTML = img ? `<img src="${img}" class="main-img" style="width:100%">` : '';
        const link = document.getElementById('adm-link').value;
        if(link) {
            document.getElementById('prev-body').innerHTML += `<div style="text-align:center;"><a href="${link}" target="_blank" style="color:var(--cremisi); font-weight:bold;">[ COLLEGAMENTO ATTIVO ]</a></div>`;
        }
    }

    // Caricamento Dati
    async function changePage(slug) {
        const { data } = await sb.from('isekai_pages').select('*').eq('slug', slug).single();
        if (data) {
            document.getElementById('view-h2').innerText = data.title;
            document.getElementById('view-body').innerHTML = data.content;
            document.getElementById('view-media').innerHTML = data.media_url ? `<img src="${data.media_url}" class="main-img" style="width:100%">` : '';
            if(data.external_link) document.getElementById('view-body').innerHTML += `<div style="text-align:center;"><a href="${data.external_link}" target="_blank" style="color:var(--cremisi); font-weight:bold;">[ ACCEDI ]</a></div>`;
        }
        loadSidebars();
    }

    async function loadSidebars() {
        const { data } = await sb.from('isekai_ads').select('*');
        const l = document.getElementById('ads-l'), r = document.getElementById('ads-r');
        l.innerHTML = ''; r.innerHTML = '';
        data.forEach(ad => {
            const h = `<a href="${ad.link_url}" target="_blank"><img src="${ad.img_url}" style="width:100%; border:1px solid #eee;"></a>`;
            if(ad.position === 'left') l.innerHTML += h; else r.innerHTML += h;
        });
    }

    async function syncFields() {
        const s = document.getElementById('adm-page').value;
        const { data } = await sb.from('isekai_pages').select('*').eq('slug', s).single();
        if(data) {
            document.getElementById('adm-title').value = data.title;
            document.getElementById('adm-text').value = data.content;
            document.getElementById('adm-img').value = data.media_url || '';
            document.getElementById('adm-link').value = data.external_link || '';
            updatePreview();
        }
    }

    async function pushUpdates() {
        const s = document.getElementById('adm-page').value;
        const upd = { title: document.getElementById('adm-title').value, content: document.getElementById('adm-text').value, media_url: document.getElementById('adm-img').value, external_link: document.getElementById('adm-link').value };
        await sb.from('isekai_pages').update(upd).eq('slug', s);
        alert("Modifiche salvate su Supabase!");
    }

    async function pushAd() {
        const p = document.getElementById('adm-side').value;
        const upd = { img_url: document.getElementById('adm-ad-img').value, link_url: document.getElementById('adm-ad-link').value };
        await sb.from('isekai_ads').update(upd).eq('position', p);
        alert("Sponsor aggiornato!");
        loadSidebars();
    }

    window.onload = () => changePage('news');
</script>
</body>
</html>
