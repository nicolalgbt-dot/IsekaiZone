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
    <script src="https://cdn.tiny.cloud/1/no-api-key/tinymce/6/tinymce.min.js" referrerpolicy="origin"></script>

    <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>

    <style>
        * { font-family: 'MedievalSharp', cursive !important; box-sizing: border-box; }

        :root {
            --nero: #1A1A1B; --cremisi: #990000;
            --pergamena-chiara: #FDF5E6;
            --pergamena-header: #f4e4bc;
            --pergamena-scura: #e2d1a6;
            --lapis: #26619C; --rame: #B87333; --smeraldo: #50C878; 
            --rubino: #E0115F; --argento: #C0C0C0;
            
            --font-small: 20px;
            --font-medium: 26px;
            --font-large: 45px;
            --font-header: 100px;
        }

        body { background-color: var(--pergamena-chiara); color: var(--nero); margin: 0; font-size: var(--font-small); }

        /* HEADER: CENTRATURA ASSOLUTA E COLORE CREMISI */
        header {
            background: linear-gradient(to bottom, var(--pergamena-header), var(--pergamena-scura));
            border-bottom: 8px double var(--nero);
            height: 320px;
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
            color: var(--cremisi); /* Solo Cremisi come richiesto */
            line-height: 1;
            width: 100%;
        }

        /* LAYOUT */
        .layout { display: grid; grid-template-columns: 260px 1fr 260px; min-height: 80vh; }
        .sidebar { padding: 30px; border: 1px solid rgba(0,0,0,0.05); text-align: center; }

        /* NAVIGAZIONE STENDARDI */
        nav { display: flex; justify-content: center; gap: 30px; padding: 60px 0; }
        .stendardo {
            width: 150px; height: 210px; display: flex; align-items: center; justify-content: center;
            text-align: center; color: white; font-weight: bold; font-size: 24px;
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 85%, 0% 100%);
            cursor: pointer; border: none; transition: 0.3s; padding: 20px;
        }
        .stendardo:hover { transform: translateY(15px); filter: brightness(1.2); }
        
        .lapis { background-color: var(--lapis); }
        .rame { background-color: var(--rame); }
        .smeraldo { background-color: var(--smeraldo); }
        .rubino { background-color: var(--rubino); }
        .argento { background-color: var(--argento); color: var(--nero); }

        /* CONTENUTO */
        main { padding: 0 60px; }
        .page-title { color: var(--cremisi); text-align: center; font-size: var(--font-large); text-transform: uppercase; margin-bottom: 50px; }
        .content-box { font-size: var(--font-medium); line-height: 1.9; text-align: justify; }

        /* VARCO ACCESSO */
        #access-vault {
            display: none; position: fixed; top:0; left:0; width:100%; height:100%;
            background: rgba(0,0,0,0.98); z-index: 9999;
            align-items: center; justify-content: center; flex-direction: column;
        }
        .vault-card { background: var(--pergamena-header); border: 10px solid var(--cremisi); padding: 70px; text-align: center; }
        .vault-input { font-size: 4em; text-align: center; width: 100%; margin: 30px 0; border: none; border-bottom: 4px solid var(--nero); background: transparent; outline: none; }

        /* GESTIONE LIVE WORDPRESS STYLE */
        #admin-panel { display: none; position: fixed; top:0; left:0; width:100%; height:100vh; background: #f0f0f1; z-index: 10000; }
        .admin-grid { display: grid; grid-template-columns: 550px 1fr; height: 100%; }
        .admin-ctrl { background: #fff; border-right: 5px solid var(--argento); padding: 40px; overflow-y: auto; }
        .admin-live { background: var(--pergamena-chiara); padding: 60px; overflow-y: auto; }

        .form-card { background: #fdfdfd; border: 2px solid #bbb; padding: 25px; margin-bottom: 30px; }
        input, select { font-size: 18px; width: 100%; padding: 15px; margin: 10px 0; }
        .btn-wp { background: var(--lapis); color: white; border: none; padding: 25px; width: 100%; cursor: pointer; font-size: 26px; text-transform: uppercase; }
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
            <div id="view-text" class="content-box"></div>
        </div>
    </main>
    <aside class="sidebar" id="sidebar-right"></aside>
</div>

<div id="access-vault">
    <div class="vault-card">
        <h2 style="font-size: 50px; color:var(--cremisi)">LIVE FORGE</h2>
        <input type="password" id="vault-pass" class="vault-input">
        <button class="btn-wp" onclick="verifyVault()">SBLOCCA</button>
        <p onclick="hideVault()" style="cursor:pointer; text-decoration: underline; margin-top: 40px;">Indietro</p>
    </div>
</div>

<div id="admin-panel">
    <div class="admin-grid">
        <div class="admin-ctrl">
            <button class="btn-exit" onclick="location.reload()">← ESCI E SALVA</button>
            <div class="form-card">
                <h3>Editor WP</h3>
                <select id="adm-page" onchange="syncFields()">
                    <option value="news">News</option>
                    <option value="forum">Forum</option>
                    <option value="abbonamenti">Abbonamenti</option>
                    <option value="quest">Quest</option>
                </select>
                <input type="text" id="adm-title" oninput="livePrev()" placeholder="Titolo">
                <textarea id="wp-editor"></textarea>
                <input type="text" id="adm-media" oninput="livePrev()" placeholder="URL Immagine">
                <input type="text" id="adm-link" oninput="livePrev()" placeholder="Link Pulsante">
                <button class="btn-wp" onclick="saveData()">INCIDI CRONACHE</button>
            </div>
        </div>
        <div class="admin-live">
            <h2 id="prev-title" class="page-title"></h2>
            <div id="prev-media"></div>
            <div id="prev-text" class="content-box"></div>
        </div>
    </div>
</div>

<script>
    const SB_URL = 'IL_TUO_URL_SUPABASE';
    const SB_KEY = 'LA_TUA_ANON_KEY';
    const supabaseClient = supabase.createClient(SB_URL, SB_KEY);

    // Inizializzazione TinyMCE (Editor WordPress Style)
    tinymce.init({
        selector: '#wp-editor',
        plugins: 'lists link image table code',
        toolbar: 'undo redo | bold italic underline | bullist numlist | link image | table code',
        setup: function (editor) {
            editor.on('keyup change', function () { livePrev(); });
        }
    });

    async function changePage(slug) {
        const { data } = await supabaseClient.from('isekai_pages').select('*').eq('slug', slug).single();
        if(data) {
            document.getElementById('view-title').innerText = data.title;
            document.getElementById('view-text').innerHTML = data.content;
            document.getElementById('view-media').innerHTML = data.media_url ? `<img src="${data.media_url}" style="width:100%; border-radius:15px; margin-bottom:30px;">` : '';
        }
    }

    function showVault() { document.getElementById('access-vault').style.display = 'flex'; }
    function hideVault() { document.getElementById('access-vault').style.display = 'none'; }
    
    function verifyVault() {
        if(document.getElementById('vault-pass').value === "SILVER00") {
            hideVault(); document.getElementById('admin-panel').style.display = 'block'; syncFields();
        } else { alert("Codice errato."); }
    }

    function livePrev() {
        document.getElementById('prev-title').innerText = document.getElementById('adm-title').value;
        document.getElementById('prev-text').innerHTML = tinymce.get('wp-editor').getContent();
        const img = document.getElementById('adm-media').value;
        document.getElementById('prev-media').innerHTML = img ? `<img src="${img}" style="width:100%; border-radius:15px;">` : '';
    }

    async function syncFields() {
        const s = document.getElementById('adm-page').value;
        const { data } = await supabaseClient.from('isekai_pages').select('*').eq('slug', s).single();
        if(data) {
            document.getElementById('adm-title').value = data.title;
            tinymce.get('wp-editor').setContent(data.content || '');
            document.getElementById('adm-media').value = data.media_url || '';
            document.getElementById('adm-link').value = data.external_link || '';
            livePrev();
        }
    }

    async function saveData() {
        const s = document.getElementById('adm-page').value;
        const upd = {
            title: document.getElementById('adm-title').value,
            content: tinymce.get('wp-editor').getContent(),
            media_url: document.getElementById('adm-media').value,
            external_link: document.getElementById('adm-link').value
        };
        await supabaseClient.from('isekai_pages').update(upd).eq('slug', s);
        alert("Modifiche salvate!");
    }

    window.onload = () => changePage('news');
</script>
</body>
</html>
