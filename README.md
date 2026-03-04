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
            
            /* SCELTA DEL LEGNO PER LE ASTE */
            --legno-ciliegio: #7c3a27; /* Caldo e rossiccio */
            --legno-betulla: #d9c5a3;  /* Chiaro e naturale */
            
            --legno-asta: var(--legno-ciliegio); /* Cambia qui in var(--legno-betulla) se preferisci */
        }

        body { background-color: var(--pergamena-chiara); color: var(--nero); margin: 0; padding: 0; overflow-x: hidden; }

        /* BARRA GESTIONE */
        #wp-admin-bar {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 55px;
            background: #1d2327; color: #fff; z-index: 10001; align-items: center; padding: 0 20px;
            justify-content: space-between; border-bottom: 2px solid var(--cremisi);
        }
        #wp-admin-bar button { font-family: sans-serif !important; font-weight: bold; border: none; cursor: pointer; padding: 8px 15px; border-radius: 4px; }

        header {
            background: linear-gradient(to bottom, var(--pergamena-header), var(--pergamena-scura));
            border-bottom: 8px double var(--nero);
            padding: 50px 0; display: flex; align-items: center; justify-content: center; width: 100%;
        }
        header h1 { font-size: var(--font-header); letter-spacing: 12px; text-transform: uppercase; color: var(--cremisi); margin: 0; text-align: center; }

        /* GRIGLIA STENDARDI 3x3 */
        .stendardi-wrap { 
            display: flex; justify-content: center; gap: 25px; padding: 60px 10px; 
            flex-wrap: wrap; perspective: 1200px; max-width: 650px; margin: 0 auto;
        }

        @keyframes swing {
            0% { transform: rotateX(0deg); }
            50% { transform: rotateX(8deg); }
            100% { transform: rotateX(0deg); }
        }

        .stendardo {
            width: 175px; height: 260px;
            display: flex; align-items: center; justify-content: center;
            text-align: center; color: white; font-weight: bold; font-size: 16px; line-height: 1.2;
            padding: 20px; cursor: pointer; position: relative;
            transition: 0.5s ease; transform-origin: top;
            animation: swing 4.5s infinite ease-in-out;
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 88%, 0% 100%);
            
            /* ASTA UNIFORME */
            border-top: 14px solid var(--legno-asta);
            /* Effetto venatura/riflesso sull'asta */
            box-shadow: inset 0 2px 2px rgba(255,255,255,0.2), 0 10px 15px rgba(0,0,0,0.3);
            
            background-image: linear-gradient(135deg, rgba(255,255,255,0.1) 0%, rgba(0,0,0,0.2) 100%);
        }

        .stendardo:hover { 
            transform: translateY(15px) rotateX(15deg) scale(1.1); 
            filter: brightness(1.1);
            animation-play-state: paused; 
        }

        .diamante { 
            background-color: #b9f2ff !important; 
            color: #1a4670 !important; 
            text-shadow: 1px 1px 0px rgba(255,255,255,0.5);
        }

        /* NAVIGAZIONE EXTRA */
        .pergamene-grid { display: flex; flex-wrap: wrap; justify-content: center; gap: 25px; margin: 40px 0; }
        .scroll-link {
            background: #e2d1a6; padding: 12px 30px; position: relative; cursor: pointer;
            text-decoration: none; color: var(--cremisi); font-size: 22px; font-weight: bold;
            border-top: 2px solid #b8a078; border-bottom: 2px solid #b8a078; transition: 0.3s;
        }
        .scroll-link::before, .scroll-link::after {
            content: ''; position: absolute; top: -5px; bottom: -5px; width: 15px;
            background: #ceb888; border: 2px solid #b8a078; border-radius: 4px;
        }
        .scroll-link::before { left: -10px; } .scroll-link::after { right: -10px; }

        main { max-width: 1000px; margin: 0 auto; padding: 0 30px 120px; }
        .editable-title { color: var(--cremisi); text-align: center; font-size: var(--font-large); margin-bottom: 30px; text-transform: uppercase; outline: none; }
        .editable-content { font-size: var(--font-medium); line-height: 1.8; text-align: justify; min-height: 350px; outline: none; }

        .overlay {
            display: none; position: fixed; top:0; left:0; width:100%; height:100%;
            background: rgba(0,0,0,0.92); z-index: 10000; align-items: center; justify-content: center;
        }
        .card { background: var(--pergamena-header); border: 8px double var(--cremisi); padding: 50px; text-align: center; width: 450px; }
        .input-medieval { font-size: 24px; text-align: center; width: 100%; border: none; border-bottom: 3px solid var(--nero); background: transparent; outline: none; margin: 20px 0; color: var(--cremisi); }
    </style>
</head>
<body>

    <div id="wp-admin-bar">
        <div style="display:flex; gap:15px; align-items:center;">
            <button onclick="document.getElementById('new-page-modal').style.display='flex'" style="background:#50C878; color:white;">+ NUOVA PERGAMENA</button>
        </div>
        <div style="display:flex; gap:15px;">
            <button onclick="publishChanges()" style="background:#2271b1; color:white;">SALVA</button>
            <button onclick="location.reload()" style="background:#d63638; color:white;">CHIUDI</button>
        </div>
    </div>

    <header><h1>ISEKAIZONE</h1></header>

    <div class="stendardi-wrap">
        <div onclick="changePage('news')" class="stendardo" style="background-color:#1a4670">CRONACHE DELLA GILDA</div>
        <div onclick="changePage('forum')" class="stendardo" style="background-color:#8d5524">TAVERNA DELLE BALLATE</div>
        <div onclick="changePage('abbonamenti')" class="stendardo" style="background-color:#388e3c">SIGILLI DELLA PERSEVERANZA</div>
        <div onclick="changePage('quest')" class="stendardo" style="background-color:#ad0e4a">ALBERO DELLE QUEST</div>
        <div onclick="showVault()" class="stendardo" style="background-color:#212121">MINISTERO DELLE VIE</div>
        <div onclick="changePage('giuramenti')" class="stendardo diamante">SALA DEI GIURAMENTI</div>
    </div>

    <main>
        <div id="sub-nav" class="pergamene-grid"></div>
        <h2 id="page-title" class="editable-title"></h2>
        <div id="page-content" class="editable-content"></div>
    </main>

    <div id="access-vault" class="overlay">
        <div class="card">
            <h3>SIGILLO DEL MINISTERO</h3>
            <input type="password" id="vault-pass" class="input-medieval" placeholder="Codice...">
            <button onclick="verifyVault()" style="width:100%; padding:20px; background:var(--cremisi); color:white; border:none; cursor:pointer;">SBLOCCA</button>
            <p onclick="document.getElementById('access-vault').style.display='none'" style="cursor:pointer; margin-top:20px; text-decoration:underline;">Indietro</p>
        </div>
    </div>

    <div id="new-page-modal" class="overlay">
        <div class="card">
            <h3>INCIDI NUOVA CRONACA</h3>
            <input type="text" id="new-title" class="input-medieval" placeholder="Titolo">
            <input type="text" id="new-slug" class="input-medieval" placeholder="slug-tecnico">
            <button onclick="createNewPage()" style="width:100%; padding:20px; background:#2271b1; color:white; border:none; cursor:pointer;">CREA</button>
            <p onclick="document.getElementById('new-page-modal').style.display='none'" style="cursor:pointer; margin-top:20px; text-decoration:underline;">Annulla</p>
        </div>
    </div>

    <script>
        const SB_URL = 'IL_TUO_URL_SUPABASE';
        const SB_KEY = 'LA_TUA_ANON_KEY';
        const sb = supabase.createClient(SB_URL, SB_KEY);

        let isAdmin = false;
        let currentPage = 'news';

        async function changePage(slug) {
            currentPage = slug;
            const { data } = await sb.from('isekai_pages').select('*').eq('slug', slug).single();
            if(data) {
                document.getElementById('page-title').innerText = data.title;
                document.getElementById('page-content').innerHTML = data.content || '<p>Ancora nulla è stato scritto...</p>';
                if(isAdmin) { tinymce.remove(); enableEditor(); }
            }
            loadSubNav();
        }

        async function loadSubNav() {
            const { data } = await sb.from('isekai_pages').select('slug, title');
            const nav = document.getElementById('sub-nav'); nav.innerHTML = '';
            const fixed = ['news', 'forum', 'abbonamenti', 'quest', 'giuramenti'];
            if(data) {
                data.filter(p => !fixed.includes(p.slug)).forEach(p => {
                    nav.innerHTML += `<a href="javascript:void(0)" onclick="changePage('${p.slug}')" class="scroll-link">${p.title}</a>`;
                });
            }
        }

        function verifyVault() {
            if(document.getElementById('vault-pass').value === "SILVER00") {
                isAdmin = true; document.getElementById('access-vault').style.display = 'none';
                document.getElementById('wp-admin-bar').style.display = 'flex';
                document.body.style.paddingTop = '55px';
                enableEditor();
            } else { alert("Sigillo non riconosciuto."); }
        }

        function showVault() { document.getElementById('access-vault').style.display = 'flex'; }

        function enableEditor() {
            tinymce.init({
                selector: '.editable-content', inline: true, menubar: false,
                plugins: 'lists link image table code emoticons',
                toolbar: 'undo redo | blocks | bold italic underline forecolor | aligncenter | bullist numlist | link image | code',
                fixed_toolbar_container: '#wp-admin-bar'
            });
            document.getElementById('page-title').contentEditable = true;
        }

        async function publishChanges() {
            const { error } = await sb.from('isekai_pages').update({ 
                title: document.getElementById('page-title').innerText, 
                content: tinymce.activeEditor.getContent() 
            }).eq('slug', currentPage);
            if(!error) alert("Cronache Sigillate!");
        }

        async function createNewPage() {
            const title = document.getElementById('new-title').value;
            const slug = document.getElementById('new-slug').value.toLowerCase().replace(/\s+/g, '-');
            const { error } = await sb.from('isekai_pages').insert([{ slug, title, content: '<p>Nuova pergamena.</p>' }]);
            if(!error) { document.getElementById('new-page-modal').style.display = 'none'; loadSubNav(); }
        }

        window.onload = () => { changePage('news'); };
    </script>
</body>
</html>
