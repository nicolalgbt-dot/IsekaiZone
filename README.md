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

        /* WP ADMIN BAR */
        #wp-admin-bar {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 50px;
            background: #1d2327; color: #fff; z-index: 10001; align-items: center; padding: 0 20px;
            justify-content: space-between; font-family: sans-serif !important;
        }

        header {
            background: linear-gradient(to bottom, var(--pergamena-header), var(--pergamena-scura));
            border-bottom: 8px double var(--nero);
            height: 280px; display: flex; align-items: center; justify-content: center;
        }
        header h1 { font-size: var(--font-header); letter-spacing: 20px; text-transform: uppercase; color: var(--cremisi); margin: 0; text-align: center; }

        /* NAVIGAZIONE STENDARDI (PRINCIPALE) */
        .stendardi-wrap { display: flex; justify-content: center; gap: 20px; padding: 30px 0; }
        .stendardo {
            min-width: 120px; height: 160px; display: flex; align-items: center; justify-content: center;
            text-align: center; color: white; font-weight: bold; font-size: 18px;
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 85%, 0% 100%);
            cursor: pointer; transition: 0.3s;
        }

        /* NAVIGAZIONE PERGAMENE (PAGINE SECONDARIE) */
        .pergamene-grid {
            display: flex; flex-wrap: wrap; justify-content: center; gap: 30px; margin: 40px 0;
        }
        .scroll-link {
            background: #e2d1a6;
            padding: 15px 40px;
            position: relative;
            cursor: pointer;
            text-decoration: none;
            color: var(--cremisi);
            font-size: 24px;
            font-weight: bold;
            border-top: 2px solid #b8a078;
            border-bottom: 2px solid #b8a078;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            transition: 0.3s;
        }
        /* Effetto Arrotolato ai lati */
        .scroll-link::before, .scroll-link::after {
            content: '';
            position: absolute;
            top: -5px; bottom: -5px;
            width: 20px;
            background: #ceb888;
            border: 2px solid #b8a078;
            border-radius: 5px;
        }
        .scroll-link::before { left: -10px; }
        .scroll-link::after { right: -10px; }
        .scroll-link:hover { transform: scale(1.05); background: #f4e4bc; }

        main { max-width: 1200px; margin: 0 auto; padding: 0 40px; }
        .editable-title { color: var(--cremisi); text-align: center; font-size: var(--font-large); text-transform: uppercase; margin-top: 20px; }
        .editable-content { font-size: var(--font-medium); line-height: 1.9; text-align: justify; margin-top: 30px; }

        /* MODALE */
        #new-page-modal {
            display: none; position: fixed; top:0; left:0; width:100%; height:100%;
            background: rgba(0,0,0,0.8); z-index: 10002; align-items: center; justify-content: center;
        }
        .modal-box { background: white; padding: 40px; border-radius: 8px; width: 450px; font-family: sans-serif !important; }

        #access-vault {
            display: none; position: fixed; top:0; left:0; width:100%; height:100%;
            background: rgba(0,0,0,0.98); z-index: 9999; align-items: center; justify-content: center;
        }
        .vault-input { font-size: 4em; text-align: center; border: none; border-bottom: 4px solid var(--nero); background: transparent; color: var(--nero); }
    </style>
</head>
<body>

<div id="wp-admin-bar">
    <div style="display:flex; gap:15px;">
        <button onclick="openNewPageModal()" style="background:#50C878; border:none; color:white; padding:5px 15px; cursor:pointer;">+ CREA PERGAMENA</button>
    </div>
    <div>
        <span id="current-page-display">HOME</span>
        <button onclick="publishChanges()" style="background:#2271b1; border:none; color:white; padding:5px 15px; margin-left:10px; cursor:pointer;">PUBBLICA</button>
        <button onclick="location.reload()" style="background:#d63638; border:none; color:white; padding:5px 15px; margin-left:10px; cursor:pointer;">ESCI</button>
    </div>
</div>

<header>
    <h1>ISEKAIZONE</h1>
</header>

<div class="stendardi-wrap" id="main-nav">
    </div>

<main>
    <div class="pergamene-grid" id="sub-nav"></div>

    <h2 id="page-title" class="editable-title">In attesa...</h2>
    <div id="page-content" class="editable-content"></div>
</main>

<div id="new-page-modal">
    <div class="modal-box">
        <h3>Forgia Nuova Pergamena</h3>
        <label>Titolo Visibile:</label>
        <input type="text" id="new-title" style="width:100%; padding:10px; margin:10px 0;">
        <label>Nome Tecnico (Slug):</label>
        <input type="text" id="new-slug" style="width:100%; padding:10px; margin:10px 0;" placeholder="es: missione-drago">
        <button onclick="createNewPage()" style="width:100%; padding:15px; background:#2271b1; color:white; border:none; cursor:pointer;">INCIDI PERGAMENA</button>
        <button onclick="closeNewPageModal()" style="width:100%; margin-top:10px; background:none; border:none; cursor:pointer;">Annulla</button>
    </div>
</div>

<div id="access-vault">
    <div style="background:var(--pergamena-header); padding:50px; border:10px solid var(--cremisi); text-align:center;">
        <h2>IDENTIFICAZIONE GESTORE</h2>
        <input type="password" id="vault-pass" class="vault-input">
        <button onclick="verifyVault()" style="width:100%; margin-top:20px; padding:20px; background:var(--cremisi); color:white; border:none; cursor:pointer;">SBLOCCA</button>
    </div>
</div>

<script>
    const SB_URL = 'IL_TUO_URL_SUPABASE';
    const SB_KEY = 'LA_TUA_ANON_KEY';
    const sb = supabase.createClient(SB_URL, SB_KEY);
    let isAdmin = false;
    let currentPage = 'news';

    // Carica la navigazione a pergamena
    async function loadInterface() {
        const { data } = await sb.from('isekai_pages').select('slug, title');
        
        // Stendardi Principali (Sempre visibili)
        const mainNav = document.getElementById('main-nav');
        mainNav.innerHTML = `
            <div onclick="changePage('news')" class="stendardo" style="background:#26619C">NEWS</div>
            <div onclick="changePage('forum')" class="stendardo" style="background:#B87333">FORUM</div>
            <div onclick="showVault()" class="stendardo" style="background:#333">GESTORI</div>
        `;

        // Pergamene Ipertestuali (Pagine create)
        const subNav = document.getElementById('sub-nav');
        subNav.innerHTML = '';
        data.filter(p => p.slug !== 'news' && p.slug !== 'forum').forEach(page => {
            subNav.innerHTML += `<a href="#" onclick="changePage('${page.slug}')" class="scroll-link">${page.title}</a>`;
        });
    }

    async function changePage(slug) {
        currentPage = slug;
        document.getElementById('current-page-display').innerText = slug.toUpperCase();
        const { data } = await sb.from('isekai_pages').select('*').eq('slug', slug).single();
        if(data) {
            document.getElementById('page-title').innerText = data.title;
            document.getElementById('page-content').innerHTML = data.content || '<p>Scrivi qui la tua storia...</p>';
            if(isAdmin) { tinymce.remove(); enableEditor(); }
        }
    }

    function enableEditor() {
        tinymce.init({
            selector: '.editable-content',
            inline: true,
            menubar: false,
            plugins: 'lists link image table code',
            toolbar: 'undo redo | bold italic underline forecolor | bullist numlist | link image | code',
            fixed_toolbar_container: '#wp-admin-bar'
        });
        document.getElementById('page-title').contentEditable = true;
    }

    function verifyVault() {
        if(document.getElementById('vault-pass').value === "SILVER00") {
            isAdmin = true;
            document.getElementById('access-vault').style.display = 'none';
            document.getElementById('wp-admin-bar').style.display = 'flex';
            document.body.style.paddingTop = '50px';
            enableEditor();
        }
    }

    function showVault() { document.getElementById('access-vault').style.display = 'flex'; }
    function openNewPageModal() { document.getElementById('new-page-modal').style.display = 'flex'; }
    function closeNewPageModal() { document.getElementById('new-page-modal').style.display = 'none'; }

    async function createNewPage() {
        const title = document.getElementById('new-title').value;
        const slug = document.getElementById('new-slug').value.toLowerCase();
        if(!title || !slug) return;

        const { error } = await sb.from('isekai_pages').insert([{ slug, title, content: '<p>Nuova pergamena incisa.</p>' }]);
        if(!error) {
            closeNewPageModal();
            await loadInterface();
            changePage(slug);
        }
    }

    async function publishChanges() {
        const title = document.getElementById('page-title').innerText;
        const content = tinymce.get('page-content').getContent();
        await sb.from('isekai_pages').update({ title, content }).eq('slug', currentPage);
        alert("La pergamena è stata sigillata nel database!");
    }

    window.onload = loadInterface;
</script>
</body>
</html>
