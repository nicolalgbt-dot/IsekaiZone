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
            --font-medium: 22px; --font-large: 40px; --font-header: 80px;
        }

        body { background-color: var(--pergamena-chiara); color: var(--nero); margin: 0; padding: 0; overflow-x: hidden; }

        /* BARRA GESTIONE */
        #wp-admin-bar {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 50px;
            background: #1d2327; color: #fff; z-index: 10001; align-items: center; padding: 0 20px;
            justify-content: space-between; border-bottom: 2px solid var(--cremisi);
        }
        #wp-admin-bar button { font-family: sans-serif !important; font-weight: bold; border: none; cursor: pointer; padding: 6px 12px; border-radius: 4px; }

        /* HEADER */
        header {
            background: linear-gradient(to bottom, var(--pergamena-header), var(--pergamena-scura));
            border-bottom: 6px double var(--nero);
            padding: 40px 0; display: flex; align-items: center; justify-content: center; width: 100%;
        }
        header h1 { font-size: var(--font-header); letter-spacing: 10px; text-transform: uppercase; color: var(--cremisi); margin: 0; text-align: center; }

        /* STENDARDI ARALDICI REALI */
        .stendardi-wrap { 
            display: flex; justify-content: center; gap: 15px; padding: 40px 10px; 
            flex-wrap: wrap; perspective: 1000px;
        }

        @keyframes swing {
            0% { transform: rotateX(0deg); }
            50% { transform: rotateX(6deg); }
            100% { transform: rotateX(0deg); }
        }

        .stendardo {
            width: 160px; height: 240px;
            display: flex; align-items: center; justify-content: center;
            text-align: center; color: white; font-weight: bold; font-size: 16px; line-height: 1.2;
            padding: 20px; cursor: pointer; position: relative;
            transition: 0.4s ease-in-out;
            transform-origin: top;
            animation: swing 4s infinite ease-in-out;
            
            /* Forma a punta reale */
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 88%, 0% 100%);
            
            /* Asta di legno */
            border-top: 8px solid #3e2723;
            
            /* Effetto tessuto */
            background-image: linear-gradient(135deg, rgba(255,255,255,0.1) 0%, rgba(0,0,0,0.2) 100%);
            box-shadow: 0 10px 15px rgba(0,0,0,0.2);
        }

        .stendardo:hover { 
            transform: translateY(10px) rotateX(12deg) scale(1.05);
            filter: brightness(1.1);
            animation-play-state: paused;
        }

        /* PERGAMENE */
        .pergamene-grid { display: flex; flex-wrap: wrap; justify-content: center; gap: 20px; margin: 30px 0; }
        .scroll-link {
            background: #e2d1a6; padding: 10px 25px; position: relative; cursor: pointer;
            text-decoration: none; color: var(--cremisi); font-size: 20px; font-weight: bold;
            border-top: 2px solid #b8a078; border-bottom: 2px solid #b8a078; transition: 0.3s;
        }
        .scroll-link::before, .scroll-link::after {
            content: ''; position: absolute; top: -4px; bottom: -4px; width: 12px;
            background: #ceb888; border: 2px solid #b8a078; border-radius: 3px;
        }
        .scroll-link::before { left: -7px; } .scroll-link::after { right: -7px; }

        /* CONTENUTO */
        main { max-width: 900px; margin: 0 auto; padding: 20px 25px 100px; }
        .editable-title { color: var(--cremisi); text-align: center; font-size: var(--font-large); margin-bottom: 20px; text-transform: uppercase; }
        .editable-content { font-size: var(--font-medium); line-height: 1.7; text-align: justify; min-height: 250px; }

        /* MODALI */
        #access-vault, #new-page-modal {
            display: none; position: fixed; top:0; left:0; width:100%; height:100%;
            background: rgba(0,0,0,0.9); z-index: 10000; align-items: center; justify-content: center;
        }
        .vault-card { background: var(--pergamena-header); border: 6px double var(--cremisi); padding: 40px; text-align: center; width: 90%; max-width: 400px; }
        .vault-input { font-size: 25px; text-align: center; width: 100%; border: none; border-bottom: 2px solid var(--nero); background: transparent; outline: none; margin: 20px 0; }
    </style>
</head>
<body>

<div id="wp-admin-bar">
    <div style="display:flex; gap:10px;">
        <button onclick="openNewPageModal()" style="background:#50C878; color:white;">+ PERGAMENA</button>
    </div>
    <div style="display:flex; align-items:center; gap:10px;">
        <button onclick="publishChanges()" style="background:#2271b1; color:white;">SALVA</button>
        <button onclick="location.reload()" style="background:#d63638; color:white;">X</button>
    </div>
</div>

<header>
    <h1>ISEKAIZONE</h1>
</header>

<div class="stendardi-wrap">
    <div onclick="changePage('news')" class="stendardo" style="background-color:#1a4670">CRONACHE DELLA GILDA</div>
    <div onclick="changePage('forum')" class="stendardo" style="background-color:#8d5524">TAVERNA DELLE BALLATE</div>
    <div onclick="changePage('abbonamenti')" class="stendardo" style="background-color:#388e3c">SIGILLI DELLA PERSEVERANZA</div>
    <div onclick="changePage('quest')" class="stendardo" style="background-color:#ad0e4a">ALBERO DELLE QUEST</div>
    <div onclick="showVault()" class="stendardo" style="background-color:#212121">MINISTERO DELLE VIE</div>
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
        <button onclick="verifyVault()" style="width:100%; padding:15px; background:var(--cremisi); color:white; border:none; cursor:pointer; font-weight:bold;">SBLOCCA</button>
        <p onclick="document.getElementById('access-vault').style.display='none'" style="cursor:pointer; margin-top:15px; text-decoration:underline; font-size: 14px;">Chiudi</p>
    </div>
</div>

<div id="new-page-modal">
    <div class="vault-card">
        <h3>Nuova Pergamena</h3>
        <input type="text" id="new-title" placeholder="Titolo" style="width:100%; padding:10px; margin-bottom:10px;">
        <input type="text" id="new-slug" placeholder="slug-tecnico" style="width:100%; padding:10px; margin-bottom:10px;">
        <button onclick="createNewPage()" style="width:100%; padding:10px; background:#2271b1; color:white; border:none; font-weight:bold;">CREA</button>
        <button onclick="document.getElementById('new-page-modal').style.display='none'" style="width:100%; margin-top:10px; background:none; border:none; text-decoration:underline;">Annulla</button>
    </div>
</div>

<script>
    const SB_URL = 'IL_TUO_URL_SUPABASE';
    const SB_KEY = 'LA_TUA_ANON_KEY';
    
    let sb = null;
    if (SB_URL !== 'IL_TUO_URL_SUPABASE') sb = supabase.createClient(SB_URL, SB_KEY);

    let isAdmin = false;
    let currentPage = 'news';

    function verifyVault() {
        if(document.getElementById('vault-pass').value === "SILVER00") {
            isAdmin = true;
            document.getElementById('access-vault').style.display = 'none';
            document.getElementById('wp-admin-bar').style.display = 'flex';
            document.body.style.paddingTop = '50px';
            enableEditor();
        } else { alert("Sigillo errato."); }
    }

    function showVault() { document.getElementById('access-vault').style.display = 'flex'; }

    async function changePage(slug) {
        currentPage = slug;
        if(!sb) {
            document.getElementById('page-title').innerText = "Configura Supabase";
            return;
        }
        const { data } = await sb.from('isekai_pages').select('*').eq('slug', slug).single();
        if(data) {
            document.getElementById('page-title').innerText = data.title;
            document.getElementById('page-content').innerHTML = data.content || '<p>Contenuto vuoto.</p>';
            if(isAdmin) { tinymce.remove(); enableEditor(); }
        }
    }

    function enableEditor() {
        tinymce.init({
            selector: '.editable-content',
            inline: true,
            menubar: false,
            plugins: 'lists link image table code emoticons',
            toolbar: 'undo redo | blocks | bold italic underline forecolor | aligncenter | bullist numlist | link image | code',
            fixed_toolbar_container: '#wp-admin-bar'
        });
        document.getElementById('page-title').contentEditable = true;
    }

    async function publishChanges() {
        const title = document.getElementById('page-title').innerText;
        const content = tinymce.get('page-content').getContent();
        const { error } = await sb.from('isekai_pages').update({ title, content }).eq('slug', currentPage);
        if(!error) alert("Salvato!");
    }

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

    function openNewPageModal() { document.getElementById('new-page-modal').style.display = 'flex'; }
    async function createNewPage() {
        const title = document.getElementById('new-title').value;
        const slug = document.getElementById('new-slug').value.toLowerCase().replace(/\s+/g, '-');
        const { error } = await sb.from('isekai_pages').insert([{ slug, title, content: '<p>Nuova pagina.</p>' }]);
        if(!error) location.reload();
    }

    window.onload = () => { loadPergamene(); changePage('news'); };
</script>
</body>
</html>
