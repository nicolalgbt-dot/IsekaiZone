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

        /* BARRA ADMIN (WORDPRESS STYLE) */
        #wp-admin-bar {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 55px;
            background: #1d2327; color: #fff; z-index: 10001; align-items: center; padding: 0 20px;
            justify-content: space-between; border-bottom: 2px solid var(--cremisi);
        }
        #wp-admin-bar button { font-family: sans-serif !important; font-weight: bold; border-radius: 4px; border: none; cursor: pointer; padding: 8px 15px; }

        /* HEADER CENTRATO CREMISI */
        header {
            background: linear-gradient(to bottom, var(--pergamena-header), var(--pergamena-scura));
            border-bottom: 8px double var(--nero);
            height: 300px; display: flex; align-items: center; justify-content: center; width: 100%;
        }
        header h1 { 
            font-size: var(--font-header); letter-spacing: 15px; text-transform: uppercase; 
            color: var(--cremisi); margin: 0; text-align: center; width: 100%;
        }

        /* STENDARDI */
        .stendardi-wrap { display: flex; justify-content: center; gap: 20px; padding: 30px 0; flex-wrap: wrap; }
        .stendardo {
            min-width: 140px; height: 180px; display: flex; align-items: center; justify-content: center;
            text-align: center; color: white; font-weight: bold; font-size: 20px;
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 85%, 0% 100%);
            cursor: pointer; transition: 0.3s;
        }

        /* PERGAMENE */
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

        /* VAULT DI ACCESSO */
        #access-vault {
            display: none; position: fixed; top:0; left:0; width:100%; height:100%;
            background: rgba(0,0,0,0.95); z-index: 10000; align-items: center; justify-content: center;
        }
        .vault-card { background: var(--pergamena-header); border: 8px double var(--cremisi); padding: 50px; text-align: center; width: 400px; }
        .vault-input { font-size: 30px; text-align: center; width: 100%; border: none; border-bottom: 3px solid var(--nero); background: transparent; outline: none; margin: 20px 0; }

        /* MODALE PAGINE */
        #new-page-modal { display: none; position: fixed; top:0; left:0; width:100%; height:100%; background: rgba(0,0,0,0.8); z-index: 10002; align-items: center; justify-content: center; }
        .modal-box { background: white; padding: 30px; border-radius: 8px; width: 400px; font-family: sans-serif !important; }
        .modal-box * { font-family: sans-serif !important; }
    </style>
</head>
<body>

<div id="wp-admin-bar">
    <div style="display:flex; gap:15px;">
        <button onclick="openNewPageModal()" style="background:#50C878; color:white;">+ NUOVA PERGAMENA</button>
    </div>
    <div style="display:flex; align-items:center; gap:20px;">
        <span style="font-family:sans-serif !important;">PAGINA: <strong id="current-page-display">NEWS</strong></span>
        <button onclick="publishChanges()" style="background:#2271b1; color:white;">PUBBLICA</button>
        <button onclick="location.reload()" style="background:#d63638; color:white;">ESCI</button>
    </div>
</div>

<header>
    <h1>ISEKAIZONE</h1>
</header>

<div class="stendardi-wrap">
    <div onclick="changePage('news')" class="stendardo" style="background:#26619C">NEWS</div>
    <div onclick="changePage('forum')" class="stendardo" style="background:#B87333">FORUM</div>
    <div onclick="changePage('abbonamenti')" class="stendardo" style="background:#50C878">SUBS</div>
    <div onclick="changePage('quest')" class="stendardo" style="background:#E0115F">QUEST</div>
    <div onclick="showVault()" class="stendardo" style="background:#333">GESTORI</div>
</div>

<main>
    <div class="pergamene-grid" id="sub-nav"></div>
    <h2 id="page-title" class="editable-title">Benvenuto</h2>
    <div id="page-content" class="editable-content">
        Seleziona una sezione per iniziare.
    </div>
</main>

<div id="access-vault">
    <div class="vault-card">
        <h3>SIGILLO GESTORE</h3>
        <input type="password" id="vault-pass" class="vault-input" placeholder="Codice...">
        <button onclick="verifyVault()" style="width:100%; padding:15px; background:var(--cremisi); color:white; border:none; cursor:pointer;">SBLOCCA</button>
        <p onclick="document.getElementById('access-vault').style.display='none'" style="cursor:pointer; margin-top:15px; text-decoration:underline;">Chiudi</p>
    </div>
</div>

<div id="new-page-modal">
    <div class="modal-box">
        <h3>Crea Nuova Pergamena</h3>
        <input type="text" id="new-title" placeholder="Titolo Pagina" style="width:100%; padding:10px; margin-bottom:10px;">
        <input type="text" id="new-slug" placeholder="slug-senza-spazi" style="width:100%; padding:10px; margin-bottom:10px;">
        <button onclick="createNewPage()" style="width:100%; padding:10px; background:#2271b1; color:white; border:none;">INCIDI</button>
        <button onclick="document.getElementById('new-page-modal').style.display='none'" style="width:100%; margin-top:5px; background:none; border:none;">Annulla</button>
    </div>
</div>

<script>
    // CONFIGURAZIONE
    const SB_URL = 'IL_TUO_URL_SUPABASE';
    const SB_KEY = 'LA_TUA_ANON_KEY';
    
    let sb = null;
    if (SB_URL !== 'IL_TUO_URL_SUPABASE') {
        sb = supabase.createClient(SB_URL, SB_KEY);
    }

    let isAdmin = false;
    let currentPage = 'news';

    // 1. VERIFICA CODICE (Indipendente da Supabase per sicurezza)
    function verifyVault() {
        const pass = document.getElementById('vault-pass').value;
        if(pass === "SILVER00") {
            isAdmin = true;
            document.getElementById('access-vault').style.display = 'none';
            document.getElementById('wp-admin-bar').style.display = 'flex';
            document.body.style.paddingTop = '55px';
            enableEditor();
            alert("Accesso Gestore Attivato.");
        } else {
            alert("Codice Errato.");
        }
    }

    function showVault() { document.getElementById('access-vault').style.display = 'flex'; }

    // 2. CAMBIO PAGINA
    async function changePage(slug) {
        currentPage = slug;
        document.getElementById('current-page-display').innerText = slug.toUpperCase();
        
        if(!sb) {
            document.getElementById('page-title').innerText = slug.toUpperCase();
            document.getElementById('page-content').innerHTML = "Configura Supabase per caricare i dati reali.";
            return;
        }

        const { data } = await sb.from('isekai_pages').select('*').eq('slug', slug).single();
        if(data) {
            document.getElementById('page-title').innerText = data.title;
            document.getElementById('page-content').innerHTML = data.content || '';
            if(isAdmin) { tinymce.remove(); enableEditor(); }
        }
    }

    // 3. EDITOR VISUALE
    function enableEditor() {
        tinymce.init({
            selector: '.editable-content',
            inline: true,
            menubar: false,
            plugins: 'lists link image table code emoticons',
            toolbar: 'undo redo | blocks | bold italic underline forecolor | alignleft aligncenter alignright | bullist numlist | link image emoticons | code',
            fixed_toolbar_container: '#wp-admin-bar'
        });
        document.getElementById('page-title').contentEditable = true;
    }

    // 4. SALVATAGGIO
    async function publishChanges() {
        if(!sb) return alert("Database non collegato.");
        const title = document.getElementById('page-title').innerText;
        const content = tinymce.get('page-content').getContent();
        const { error } = await sb.from('isekai_pages').update({ title, content }).eq('slug', currentPage);
        if(!error) alert("Cronache salvate!");
    }

    // 5. NUOVA PAGINA
    function openNewPageModal() { document.getElementById('new-page-modal').style.display = 'flex'; }
    async function createNewPage() {
        const title = document.getElementById('new-title').value;
        const slug = document.getElementById('new-slug').value.toLowerCase().replace(/\s+/g, '-');
        if(!sb) return alert("Database non collegato.");
        const { error } = await sb.from('isekai_pages').insert([{ slug, title, content: '<p>Nuova pagina.</p>' }]);
        if(!error) location.reload();
    }

    // CARICAMENTO INIZIALE
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
