index.html
<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ISEKAIZONE | Gilda dell'Attivismo</title>
    <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js"></script>
    <style>
        :root {
            --nero-inchiostro: #1A1A1B; --cremisi: #990000;
            --pergamena: #f4e4bc; --pergamena-scura: #e2d1a6;
            --lapis: #26619C; --rame: #B87333; --smeraldo: #50C878; 
            --rubino: #E0115F; --argento: #C0C0C0;
        }

        body { background-color: white; color: var(--nero-inchiostro); font-family: 'Georgia', serif; margin: 0; }

        /* HEADER CORNICE PERGAMENA */
        header {
            background: linear-gradient(to bottom, var(--pergamena), var(--pergamena-scura));
            border-bottom: 4px double var(--nero-inchiostro);
            padding: 40px 20px;
            text-align: center;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }

        header h1 {
            margin: 0;
            font-size: 4em;
            letter-spacing: 12px;
            text-transform: uppercase;
            /* Effetto bicolore Inchiostro e Cremisi */
            color: var(--nero-inchiostro);
            text-shadow: 3px 3px 0px var(--cremisi);
        }

        /* LAYOUT STRUTTURALE */
        .layout { display: grid; grid-template-columns: 220px 1fr 220px; min-height: 80vh; }
        .sidebar { padding: 25px; border: 1px solid #f0f0f0; background: #fff; text-align: center; }
        .sidebar img { max-width: 100%; margin-bottom: 25px; border: 1px solid #ddd; }

        /* NAVIGAZIONE A STENDARDI */
        nav { display: flex; justify-content: center; gap: 18px; padding: 40px 0; }
        .stendardo {
            width: 115px; height: 160px; display: flex; align-items: center; justify-content: center;
            text-align: center; color: white; font-weight: bold; text-decoration: none; font-size: 0.9em;
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 88%, 0% 100%); transition: 0.3s;
            cursor: pointer; border: none;
        }
        .stendardo:hover { transform: translateY(12px); filter: brightness(1.1); }
        .lapis { background-color: var(--lapis); } .rame { background-color: var(--rame); }
        .smeraldo { background-color: var(--smeraldo); } .rubino { background-color: var(--rubino); }
        .argento { background-color: var(--argento); color: var(--nero-inchiostro); }

        /* CONTENUTI */
        main { padding: 20px 60px; }
        .page-title { color: var(--cremisi); text-align: center; text-transform: uppercase; font-size: 2.2em; }
        .content-box { line-height: 1.9; font-size: 1.2em; margin-top: 35px; }
        img.main-img { width: 100%; border-radius: 10px; box-shadow: 0 5px 15px rgba(0,0,0,0.1); margin: 30px 0; }

        /* DASHBOARD GESTIONE (WP STYLE) */
        #admin-panel { 
            display: none; background: #f0f0f1; border-top: 8px solid var(--cremisi); 
            padding: 50px; position: fixed; top: 0; left: 0; width: 100%; 
            height: 100vh; z-index: 10000; box-sizing: border-box; overflow-y: auto;
        }
        .card { background: white; padding: 30px; border: 1px solid #ccd0d4; margin-bottom: 25px; }
        input, textarea, select { width: 100%; padding: 12px; margin: 10px 0; border: 1px solid #8c8f94; border-radius: 4px; font-size: 1em; }
        .btn { background: #2271b1; color: white; border: none; padding: 15px 30px; cursor: pointer; border-radius: 4px; font-weight: bold; }
        .btn-danger { background: var(--cremisi); float: right; }
    </style>
</head>
<body>

<header>
    <h1>ISEKAIZONE</h1>
</header>

<div class="layout">
    <aside class="sidebar" id="ads-l"></aside>

    <main>
        <nav>
            <button onclick="changePage('news')" class="stendardo lapis">NEWS</button>
            <button onclick="changePage('forum')" class="stendardo rame">FORUM</button>
            <button onclick="changePage('abbonamenti')" class="stendardo smeraldo">ABBONAMENTI</button>
            <button onclick="changePage('quest')" class="stendardo rubino">QUEST</button>
            <button onclick="openGestione()" class="stendardo argento">GESTIONE</button>
        </nav>

        <div id="content-display">
            <h2 id="view-h2" class="page-title">Cronache Caricate</h2>
            <div id="view-media"></div>
            <div id="view-body" class="content-box"></div>
        </div>
    </main>

    <aside class="sidebar" id="ads-r"></aside>
</div>

<div id="admin-panel">
    <button class="btn btn-danger" onclick="closeGestione()">CHIUDI BACHECA</button>
    <h2>Bacheca Amministratore (Codice: SILVER00)</h2>
    <hr>
    
    <div class="card">
        <h3>Modifica Sezione Pagina</h3>
        <select id="adm-page" onchange="syncFields()">
            <option value="news">News</option>
            <option value="forum">Forum</option>
            <option value="abbonamenti">Abbonamenti</option>
            <option value="quest">Quest</option>
        </select>
        <input type="text" id="adm-title" placeholder="Titolo Pagina">
        <textarea id="adm-text" rows="8" placeholder="Testo principale (Puoi usare tag HTML)"></textarea>
        <input type="text" id="adm-img" placeholder="URL Immagine o Video">
        <input type="text" id="adm-link" placeholder="URL Collegamento Ipertestuale (es. Piattaforma Pagamenti)">
        <button class="btn" onclick="pushUpdates()">Pubblica Modifiche</button>
    </div>

    <div class="card">
        <h3>Sponsor Sidebar</h3>
        <select id="adm-side">
            <option value="left">Sinistra</option>
            <option value="right">Destra</option>
        </select>
        <input type="text" id="adm-ad-img" placeholder="URL Banner">
        <input type="text" id="adm-ad-link" placeholder="Link Destinazione">
        <button class="btn" onclick="pushAd()">Aggiorna Sponsor</button>
    </div>
</div>

<script>
    // COLLEGA QUI IL TUO SUPABASE
    const URL_SB = 'IL_TUO_URL_SUPABASE';
    const KEY_SB = 'LA_TUA_ANON_KEY';
    const sb = supabase.createClient(URL_SB, KEY_SB);

    async function changePage(slug) {
        const { data } = await sb.from('isekai_pages').select('*').eq('slug', slug).single();
        if (data) {
            document.getElementById('view-h2').innerText = data.title;
            document.getElementById('view-body').innerHTML = data.content;
            document.getElementById('view-media').innerHTML = data.media_url ? `<img src="${data.media_url}" class="main-img">` : '';
            if(data.external_link) {
                document.getElementById('view-body').innerHTML += `<div style="text-align:center; margin-top:30px;"><a href="${data.external_link}" target="_blank" style="color:var(--cremisi); font-weight:bold; text-decoration:underline;">[ ACCEDI AL COLLEGAMENTO ]</a></div>`;
            }
        }
        loadSidebars();
    }

    async function loadSidebars() {
        const { data } = await sb.from('isekai_ads').select('*');
        const l = document.getElementById('ads-l'); const r = document.getElementById('ads-r');
        l.innerHTML = ''; r.innerHTML = '';
        data.forEach(ad => {
            const h = `<a href="${ad.link_url}" target="_blank"><img src="${ad.img_url}"></a>`;
            if(ad.position === 'left') l.innerHTML += h; else r.innerHTML += h;
        });
    }

    function openGestione() {
        if(prompt("Codice Team Gestionale:") === "SILVER00") {
            document.getElementById('admin-panel').style.display = 'block';
            syncFields();
        } else { alert("Accesso Negato."); }
    }

    function closeGestione() { document.getElementById('admin-panel').style.display = 'none'; }

    async function syncFields() {
        const s = document.getElementById('adm-page').value;
        const { data } = await sb.from('isekai_pages').select('*').eq('slug', s).single();
        if(data) {
            document.getElementById('adm-title').value = data.title;
            document.getElementById('adm-text').value = data.content;
            document.getElementById('adm-img').value = data.media_url || '';
            document.getElementById('adm-link').value = data.external_link || '';
        }
    }

    async function pushUpdates() {
        const s = document.getElementById('adm-page').value;
        const upd = {
            title: document.getElementById('adm-title').value,
            content: document.getElementById('adm-text').value,
            media_url: document.getElementById('adm-img').value,
            external_link: document.getElementById('adm-link').value
        };
        await sb.from('isekai_pages').update(upd).eq('slug', s);
        alert("Gilda Aggiornata!");
        changePage(s);
    }

    async function pushAd() {
        const p = document.getElementById('adm-side').value;
        const upd = { img_url: document.getElementById('adm-ad-img').value, link_url: document.getElementById('adm-ad-link').value };
        await sb.from('isekai_ads').update(upd).eq('position', p);
        alert("Sponsor Aggiornato!");
        loadSidebars();
    }

    window.onload = () => changePage('news');
</script>
</body>
</html>
