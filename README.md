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
            --pergamena-header: #f4e4bc; --pergamena-scura: #e2d1a6;
            --font-small: 20px; --font-medium: 26px; --font-large: 45px; --font-header: 100px;
        }

        body { background-color: var(--pergamena-chiara); color: var(--nero); margin: 0; padding: 0; transition: padding 0.3s; }

        /* BARRA ADMIN WORDPRESS */
        #wp-admin-bar {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 50px;
            background: #1d2327; color: #fff; z-index: 10001; align-items: center; padding: 0 20px;
            justify-content: space-between; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif !important;
        }
        #wp-admin-bar * { font-family: sans-serif !important; font-size: 14px; }

        /* HEADER: CENTRATURA ASSOLUTA E COLORE CREMISI */
        header {
            background: linear-gradient(to bottom, var(--pergamena-header), var(--pergamena-scura));
            border-bottom: 8px double var(--nero);
            height: 320px; display: flex; align-items: center; justify-content: center; width: 100%;
            position: relative;
        }
        header h1 { 
            font-size: var(--font-header); letter-spacing: 20px; text-transform: uppercase; 
            color: var(--cremisi); margin: 0; text-align: center; width: 100%; line-height: 1;
        }

        /* NAVIGAZIONE STENDARDI */
        .stendardi-wrap { display: flex; justify-content: center; gap: 20px; padding: 40px 0; flex-wrap: wrap; }
        .stendardo {
            min-width: 150px; height: 190px; display: flex; align-items: center; justify-content: center;
            text-align: center; color: white; font-weight: bold; font-size: 22px;
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 85%, 0% 100%);
            cursor: pointer; transition: 0.3s; padding: 10px;
        }
        .stendardo:hover { transform: translateY(15px); filter: brightness(1.1); }

        /* NAVIGAZIONE PERGAMENE (COLLEGAMENTI IPERTESTUALI) */
        .pergamene-grid { display: flex; flex-wrap: wrap; justify-content: center; gap: 25px; margin-bottom: 50px; }
        .scroll-link {
            background: #e2d1a6; padding: 15px 40px; position: relative; cursor: pointer;
            text-decoration: none; color: var(--cremisi); font-size: 24px; font-weight: bold;
            border-top: 2px solid #b8a078; border-bottom: 2px solid #b8a078;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1); transition: 0.3s; display: inline-block;
        }
        .scroll-link::before, .scroll-link::after {
            content: ''; position: absolute; top: -5px; bottom: -5px; width: 18px;
            background: #ceb888; border: 2px solid #b8a078; border-radius: 4px;
        }
        .scroll-link::before { left: -9px; }
        .scroll-link::after { right: -9px; }
        .scroll-link:hover { transform: scale(1.05); background: #f4e4bc; }

        /* AREA CONTENUTO */
        main { max-width: 1200px; margin: 0 auto; padding: 0 50px 100px 50px; }
        .editable-title { color: var(--cremisi); text-align: center; font-size: var(--font-large); text-transform: uppercase; margin-bottom: 30px; outline: none; }
        .editable-content { font-size: var(--font-medium); line-height: 1.9; text-align: justify; outline: none; min-height: 300px; }

        /* VAULT ACCESSO */
        #access-vault {
            display: none; position: fixed; top:0; left:0; width:100%; height:100%;
            background: rgba(0,0,0,0.98); z-index: 9999; align-items: center; justify-content: center;
        }
        .vault-card { background: var(--pergamena-header); border: 10px solid var(--cremisi); padding: 60px; text-align: center; box-shadow: 0 0 100px rgba(153,0,0,0.5); }
        .vault-input { font-size: 4em; text-align: center; width: 100%; background: transparent; border: none; border-bottom: 4px solid var(--nero); outline: none; }

        /* MODALE NUOVA PAGINA */
        #new-page-modal {
            display: none; position: fixed; top:0; left:0; width:100%; height:100%;
            background: rgba(0,0,0,0.85); z-index: 10002; align-items: center; justify-content: center;
        }
        .modal-box { background: white; padding: 40px; border-radius: 10px; width: 450px; }
        .modal-box input { width: 100%; padding: 15px; margin: 15px 0; font-size: 18px; }
    </style>
</head>
<body>

<div id="wp-admin-bar">
    <div style="display:flex; gap:20px;">
        <button onclick="openNewPageModal()" style="background:#50C878; color:white; border:none; padding:8px 20px; cursor:pointer; font-weight:bold; border-radius:4px;">+ CREA PERGAMENA</button>
    </div>
    <div style="display:flex; align-items:center; gap:20px;">
        <span>MODALITÀ EDITING: <strong id="current-page-display" style="color:#50C878">NEWS</strong></span>
        <button onclick="publishChanges()" style="background:#2271b1; color:white; border:none; padding:8px 25px; cursor:pointer; font-weight:bold; border-radius:4px;">PUBBLICA</button>
        <button onclick="location.reload()" style="background:#d63638; color:white; border:none; padding:8px 15px; cursor:pointer; border-radius:4px;">ESCI</button>
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

    <div id="editor-container">
        <h2 id="page-title" class="editable-title"></h2>
        <div id="page-content" class="editable-content"></div>
    </div>
</main>

<div id="new-page-modal">
    <div class="modal-box">
        <h2 style="font-family:sans-serif !important;">Nuova Pergamena</h2>
        <input type="text" id="new-title" placeholder="Titolo della Pagina">
        <input type="text" id="new-slug" placeholder="Nome-tecnico (es: gilda-viola)">
        <button onclick="createNewPage()" style="width:100%; padding:15px; background:#2271b1; color:white; border:none; cursor:pointer; font-weight:bold;">CREA PAGINA</button>
        <button onclick="closeNewPageModal()" style="width:100%; margin-top:10px; background:none; border:none; cursor:pointer; text-decoration:underline;">Annulla</button>
    </div>
</div>

<div id="access-vault">
    <div class="vault-card">
        <h2>INSERISCI SIGILLO</h2>
        <input type="password" id="vault-pass" class="vault-input">
        <button onclick="verifyVault()" style="width:100%; margin-top:30px; padding:20px; background:var(--cremisi); color:white; border:none; cursor:pointer; font-size:24px;">ACCEDI ALLA FORGIA</button>
    </div>
</div>

<script>
    // --- INSERISCI QUI I TUOI DATI ---
    const SB_URL = 'IL_TUO_URL_SUPABASE';
    const SB_KEY = 'LA_TUA_ANON_KEY';
    const sb = supabase.createClient(SB_URL, SB_KEY);

    let isAdmin = false;
    let currentPage = 'news';

    // Carica le pergamene (pagine extra)
    async function loadPergamene() {
        const { data } = await sb.from('isekai_pages').select('slug, title');
        const subNav = document.getElementById('sub-nav');
        subNav.innerHTML = '';
        const standard = ['news', 'forum', 'abbonamenti', 'quest'];
        
        if(data) {
            data.filter(p => !standard.includes(p.slug)).forEach(page => {
                subNav.innerHTML += `<a href="javascript:void(0)" onclick="changePage('${page.slug}')" class="scroll-link">${page.title}</a>`;
            });
        }
    }

    // Cambia pagina e abilita editing se admin
    async function changePage(slug) {
        currentPage = slug;
        document.getElementById('current-page-display').innerText = slug.toUpperCase();
        
        const { data } = await sb.from('isekai_pages').select('*').eq('slug', slug).single();
        if(data) {
            document.getElementById('page-title').innerText = data.title;
            document.getElementById('page-content').innerHTML = data.content || '<p>Inizia a scrivere la storia...</p>';
            if(isAdmin) { tinymce.remove(); enableWPEditor(); }
        }
    }

    // Attiva TinyMCE (Editor WP Style)
    function enableWPEditor() {
        tinymce.init({
            selector: '.editable-content',
            inline: true,
            menubar: false,
            plugins: 'lists link image table code emoticons',
            toolbar: 'undo redo | blocks | bold italic underline forecolor | alignleft aligncenter alignright | bullist numlist | link image | code',
            fixed_toolbar_container: '#wp-admin-bar'
        });
        document.getElementById('page-title').contentEditable = true;
    }

    // Accesso Gestori
    function showVault() { document.getElementById('access-vault').style.display = 'flex'; }
    function verifyVault() {
        if(document.getElementById('vault-pass').value === "SILVER00") {
            isAdmin = true;
            document.getElementById('access-vault').style.display = 'none';
            document.getElementById('wp-admin-bar').style.display = 'flex';
            document.body.style.paddingTop = '50px';
            enableWPEditor();
        } else { alert("Sigillo non riconosciuto"); }
    }

    // Gestione Nuove Pagine
    function openNewPageModal() { document.getElementById('new-page-modal').style.display = 'flex'; }
    function closeNewPageModal() { document.getElementById('new-page-modal').style.display = 'none'; }
    
    async function createNewPage() {
        const title = document.getElementById('new-title').value;
        const slug = document.getElementById('new-slug').value.toLowerCase().replace(/\s+/g, '-');
        if(!title || !slug) return;

        const { error } = await sb.from('isekai_pages').insert([{ slug, title, content: '<p>Nuova pergamena incisa.</p>' }]);
        if(!error) {
            closeNewPageModal();
            await loadPergamene();
            changePage(slug);
        } else { alert(error.message); }
    }

    // Salvataggio Finale
    async function publishChanges() {
        const title = document.getElementById('page-title').innerText;
        const content = tinymce.get('page-content').getContent();
        const { error } = await sb.from('isekai_pages').update({ title, content }).eq('slug', currentPage);
        if(!error) alert("Cronache salvate nel database!");
        else alert(error.message);
    }

    window.onload = () => {
        loadPergamene();
        changePage('news');
    };
</script>
</body>
</html>
