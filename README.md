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
            --pergamena-chiara: #FDF5E6; --pergamena-header: #f4e4bc; --pergamena-scura: #e2d1a6;
            --font-small: 20px; --font-medium: 26px; --font-large: 45px; --font-header: 100px;
        }

        body { background-color: var(--pergamena-chiara); color: var(--nero); margin: 0; padding: 0; }

        /* BARRA ADMIN */
        #wp-admin-bar {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 55px;
            background: #1d2327; color: #fff; z-index: 10001; align-items: center; padding: 0 20px;
            justify-content: space-between; border-bottom: 2px solid var(--cremisi);
        }
        #wp-admin-bar button { font-family: sans-serif !important; font-weight: bold; border-radius: 4px; border: none; cursor: pointer; padding: 8px 15px; }

        /* HEADER CENTRATO */
        header {
            background: linear-gradient(to bottom, var(--pergamena-header), var(--pergamena-scura));
            border-bottom: 8px double var(--nero);
            height: 300px; display: flex; align-items: center; justify-content: center; width: 100%;
        }
        header h1 { font-size: var(--font-header); letter-spacing: 15px; text-transform: uppercase; color: var(--cremisi); margin: 0; text-align: center; width: 100%; }

        /* STENDARDI */
        .stendardi-wrap { display: flex; justify-content: center; gap: 15px; padding: 30px 0; flex-wrap: wrap; }
        .stendardo {
            min-width: 160px; height: 200px; display: flex; align-items: center; justify-content: center;
            text-align: center; color: white; font-weight: bold; font-size: 18px; line-height: 1.2;
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 85%, 0% 100%);
            cursor: pointer; transition: 0.3s; padding: 15px;
        }
        .stendardo:hover { transform: translateY(10px); filter: brightness(1.1); }

        /* PERGAMENE DINAMICHE */
        .pergamene-grid { display: flex; flex-wrap: wrap; justify-content: center; gap: 25px; margin: 20px 0 40px 0; }
        .scroll-link {
            background: #e2d1a6; padding: 12px 35px; position: relative; cursor: pointer;
            text-decoration: none; color: var(--cremisi); font-size: 22px; font-weight: bold;
            border-top: 2px solid #b8a078; border-bottom: 2px solid #b8a078; box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }
        .scroll-link::before, .scroll-link::after {
            content: ''; position: absolute; top: -4px; bottom: -4px; width: 15px;
            background: #ceb888; border: 2px solid #b8a078; border-radius: 4px;
        }
        .scroll-link::before { left: -8px; } .scroll-link::after { right: -8px; }

        main { max-width: 1100px; margin: 0 auto; padding: 0 40px 100px; }
        .editable-title { color: var(--cremisi); text-align: center; font-size: var(--font-large); margin-bottom: 30px; outline: none; }
        .editable-content { font-size: var(--font-medium); line-height: 1.8; text-align: justify; min-height: 300px; outline: none; }

        /* VAULT */
        #access-vault {
            display: none; position: fixed; top:0; left:0; width:100%; height:100%;
            background: rgba(0,0,0,0.95); z-index: 10000; align-items: center; justify-content: center;
        }
        .vault-card { background: var(--pergamena-header); border: 8px double var(--cremisi); padding: 50px; text-align: center; width: 450px; }
        .vault-input { font-size: 30px; text-align: center; width: 100%; border: none; border-bottom: 3px solid var(--nero); background: transparent; outline: none; margin: 20px 0; }
    </style>
</head>
<body>

<div id="wp-admin-bar">
    <div style="display:flex; gap:15px;">
        <button onclick="openNewPageModal()" style="background:#50C878; color:white;">+ NUOVA PERGAMENA</button>
    </div>
    <div style="display:flex; align-items:center; gap:20px;">
        <span style="font-family:sans-serif !important;">PAGINA ATTUALE: <strong id="current-page-display">NEWS</strong></span>
        <button onclick="publishChanges()" style="background:#2271b1; color:white;">PUBBLICA MODIFICHE</button>
        <button onclick="location.reload()" style="background:#d63638; color:white;">ESCI</button>
    </div>
</div>

<header>
    <h1>ISEKAIZONE</h1>
</header>

<div class="stendardi-wrap">
    <div onclick="changePage('news')" class="stendardo" style="background:#26619C">CRONACHE DELLA GILDA</div>
    <div onclick="changePage('forum')" class="stendardo" style="background:#B87333">TAVERNA DELLE BALLATE</div>
    <div onclick="changePage('abbonamenti')" class="stendardo" style="background:#50C878">SIGILLI DELLA PERSEVERANZA</div>
    <div onclick="changePage('quest')" class="stendardo" style="background:#E0115F">ALBERO DELLE QUEST</div>
    <div onclick="showVault()" class="stendardo" style="background:#333">MINISTERO DELLE VIE</div>
</div>

<main>
    <div class="pergamene-grid" id="sub-nav"></div>
    <h2 id="page-title" class="editable-title"></h2>
    <div id="page-content" class="editable-content"></div>
</main>

<div id="access-vault">
    <div class="vault-card">
        <h3>SIGILLO DEL MINISTERO</h3>
        <input type="password" id="vault-pass" class="vault-input" placeholder="Codice...">
        <button onclick="verifyVault()" style="width:100%; padding:15px; background:var(--cremisi); color:white; border:none; cursor:pointer;">ACCEDI ALLA GESTIONE</button>
    </div>
</div>

<script>
    // --- INSERISCI QUI LE TUE CHIAVI ---
    const SB_URL = 'IL_TUO_URL_SUPABASE';
    const SB_KEY = 'LA_TUA_ANON_KEY';
    
    let sb = null;
    if (SB_URL !== 'IL_TUO_URL_SUPABASE') {
        sb = supabase.createClient(SB_URL, SB_KEY);
    }

    let isAdmin = false;
    let currentPage = 'news';

    // 1. VERIFICA ACCESSO
    function verifyVault() {
        if(document.getElementById('vault-pass').value === "SILVER00") {
            isAdmin = true;
            document.getElementById('access-vault').style.display = 'none';
            document.getElementById('wp-admin-bar').style.display = 'flex';
            document.body.style.paddingTop = '55px';
            enableEditor();
        } else { alert("Accesso negato."); }
    }

    function showVault() { document.getElementById('access-vault').style.display = 'flex'; }

    // 2. CARICA PAGINA
    async function changePage(slug) {
        currentPage = slug;
        document.getElementById('current-page-display').innerText = slug.toUpperCase();
        
        if(!sb) {
            document.getElementById('page-title').innerText = "Database non collegato";
            document.getElementById('page-content').innerHTML = "Configura Supabase per vedere i contenuti.";
            return;
        }

        const { data } = await sb.from('isekai_pages').select('*').eq('slug', slug).single();
        if(data) {
            document.getElementById('page-title').innerText = data.title;
            document.getElementById('page-content').innerHTML = data.content || '<p>Scrivi qui...</p>';
            if(isAdmin) { tinymce.remove(); enableEditor(); }
        }
    }

    // 3. EDITOR WP STYLE
    function enableEditor() {
        tinymce.init({
            selector: '.editable-content',
            inline: true,
            menubar: false,
            plugins: 'lists link image table code emoticons',
            toolbar: 'undo redo | blocks | bold italic underline forecolor | bullist numlist | link image emoticons | code',
            fixed_toolbar_container: '#wp-admin-bar'
        });
        document.getElementById('page-title').contentEditable = true;
    }

    // 4. SALVATAGGIO
    async function publishChanges() {
        const title = document.getElementById('page-title').innerText;
        const content = tinymce.get('page-content').getContent();
        const { error } = await sb.from('isekai_pages').update({ title, content }).eq('slug', currentPage);
        if(!error) alert("Cronache aggiornate con successo!");
    }

    // 5. CARICAMENTO PERGAMENE (PAGINE EXTRA)
    async function loadPergamene() {
        if(!sb) return;
        const { data } = await sb.from('isekai_pages').select('slug, title');
        const subNav = document.getElementById('sub-nav');
        subNav.innerHTML = '';
        const fixed = ['news', 'forum', 'abbonamenti', 'quest'];
        if(data) {
            data.filter(p => !fixed.includes(p.slug)).forEach(page => {
                subNav.innerHTML += `<a href="javascript:void(0)" onclick="changePage('${page.slug}')" class="scroll-link">${page.title}</a>`;
            });
        }
    }

    window.onload = () => {
        loadPergamene();
        changePage('news');
    };
</script>
</body>
</html>
