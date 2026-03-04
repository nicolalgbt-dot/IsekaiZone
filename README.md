index.html
<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ISEKAIZONE | Portale della Gilda</title>
    <link href="https://fonts.googleapis.com/css2?family=MedievalSharp&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/appwrite@13.0.1"></script>
    <style>
        /* --- ESTETICA GENERALE --- */
        * { font-family: 'MedievalSharp', cursive !important; box-sizing: border-box; }
        :root { --nero: #1A1A1B; --cremisi: #990000; --pergamena: #FDF5E6; --oro: #ceb888; --verde: #155724; }
        body { background: var(--pergamena); color: var(--nero); margin: 0; line-height: 1.6; }
        
        header { background: #f4e4bc; border-bottom: 5px double var(--nero); padding: 25px; text-align: center; }
        .nav-gilda { display: flex; justify-content: center; gap: 10px; padding: 15px; background: var(--nero); position: sticky; top: 0; z-index: 100; }
        .nav-btn { background: transparent; color: var(--oro); border: 1px solid var(--oro); padding: 8px 15px; cursor: pointer; }
        .container { max-width: 1000px; margin: 20px auto; padding: 20px; }

        /* --- COMPONENTI GRAFICI --- */
        .pergamena { background: white; border: 8px double var(--cremisi); padding: 40px; position: relative; box-shadow: 10px 10px 0 rgba(0,0,0,0.1); background-image: url('https://www.transparenttextures.com/patterns/paper-1.png'); margin-bottom: 30px; }
        .id-tag { position: absolute; top: 15px; right: 15px; font-size: 14px; color: var(--cremisi); border: 1px solid; padding: 5px; }
        .msg-etico { font-style: italic; color: var(--cremisi); border-left: 4px solid; padding-left: 15px; margin-bottom: 25px; }
        
        /* --- FORUM & CRONACHE --- */
        .post-forum { border-bottom: 1px solid #ddd; padding: 15px 0; }
        .post-author { font-weight: bold; color: var(--cremisi); font-size: 0.9rem; }
        .media-box { width: 100%; max-height: 400px; object-fit: cover; border: 3px solid var(--nero); margin: 15px 0; }

        /* --- MINISTERO & UFFICI --- */
        .grid-ministero { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-top: 20px; }
        .btn-ufficio { padding: 30px; border: 3px double var(--nero); background: #eee; cursor: pointer; text-align: center; font-weight: bold; }
        .btn-ufficio:hover { background: var(--oro); }
        .ufficio-box { display: none; background: #fff; border: 4px solid var(--nero); padding: 20px; margin-top: 20px; }
        
        /* --- STATUS & FORM --- */
        input { border: none; border-bottom: 2px solid var(--nero); width: 100%; font-size: 18px; background: transparent; margin: 10px 0; outline: none; }
        .status { padding: 10px; text-align: center; font-weight: bold; margin-top: 10px; border-radius: 4px; border: 2px solid; }
        .status-ok { background: #d4edda; color: var(--verde); border-color: var(--verde); }
        .status-no { background: #f8d7da; color: var(--cremisi); border-color: var(--cremisi); }

        @media print {
            .no-print, button, nav, header, .edit-area { display: none !important; }
            body { background: white; }
            .pergamena { border: 5px solid black; box-shadow: none; width: 100%; }
        }
    </style>
</head>
<body>

<header>
    <h1>ISEKAIZONE</h1>
    <p>Il Portale della Gilda</p>
</header>

<nav class="nav-gilda no-print">
    <button class="nav-btn" onclick="showSec('cronache')">CRONACHE</button>
    <button class="nav-btn" onclick="showSec('forum')">FORUM</button>
    <button class="nav-btn" id="btn-miaprofilo" style="display:none;" onclick="showSec('profilo')">MIA SCHEDA</button>
    <button class="nav-btn" onclick="showSec('ministero')" style="background:var(--cremisi); color:white;">MINISTERO DELLE VIE</button>
    <button class="nav-btn" id="auth-btn" onclick="openLogin()">ACCEDI</button>
</nav>

<div class="container">
    
    <section id="cronache">
        <div class="pergamena">
            <h2>Cronache dell'Isekai</h2>
            <img src="https://via.placeholder.com/800x400?text=Immagine+della+Gilda" class="media-box" id="cronaca-img">
            <p id="cronaca-testo">Le leggende narrano di una Gilda nata tra i mondi...</p>
        </div>
    </section>

    <section id="forum" style="display:none;">
        <div class="pergamena">
            <h2>Bacheca della Locanda</h2>
            <div id="forum-list">
                <p>Nessun messaggio nella bacheca al momento.</p>
            </div>
            <div class="no-print" style="margin-top:20px;">
                <input type="text" id="forum-msg" placeholder="Scrivi un messaggio alla Gilda...">
                <button onclick="postForum()" style="width:100%; padding:10px; background:var(--nero); color:white; border:none; cursor:pointer;">APPENDI ALLA BACHECA</button>
            </div>
        </div>
    </section>

    <section id="profilo" style="display:none;">
        <div class="pergamena">
            <div class="id-tag" id="my-id">ID: ----</div>
            <div class="msg-etico">"Non chiediamo i nomi anagrafici, abbracciamo i nomi di elezione, no discriminazione"</div>
            <h2 id="p-name">Nome di Elezione</h2>
            <p>Pronomi: <span id="p-pron">---</span></p>
            <p>Identità: <span id="p-ident">---</span></p>
            <div id="p-status" class="status">Verifica in corso...</div>
            
            <div class="no-print edit-area" style="margin-top:30px; border-top:1px dashed #ccc; padding-top:15px;">
                <h3>Modifica i tuoi dati</h3>
                <input type="text" id="in-name" placeholder="Nome di Elezione">
                <input type="text" id="in-pron" placeholder="Pronomi">
                <input type="text" id="in-ident" placeholder="Identità di Genere">
                <button onclick="saveData()" style="width:100%; padding:10px; background:var(--nero); color:white; border:none; cursor:pointer;">AGGIORNA DIGITALMENTE</button>
            </div>
        </div>
    </section>

    <section id="ministero" style="display:none;">
        <div class="pergamena">
            <h2 style="text-align:center;">MINISTERO DELLE VIE</h2>
            <div class="grid-ministero no-print">
                <div class="btn-ufficio" onclick="access('LIBRA','TITAN03')">⚖️ LIBRA<br><small>Abbonamenti</small></div>
                <div class="btn-ufficio" onclick="access('AURORA','TITAN02')">🌅 AURORA<br><small>Media & Testi</small></div>
                <div class="btn-ufficio" onclick="access('IPERION','TITAN01')">👁️ IPERION<br><small>Stampa</small></div>
                <div class="btn-ufficio" onclick="access('NYX','SILVER00')">🌑 NYX<br><small>Supremo</small></div>
            </div>

            <div id="u-LIBRA" class="ufficio-box">
                <h3>Ufficio LIBRA</h3>
                <input type="text" id="search-id" placeholder="Cerca ID Utente...">
                <button onclick="checkStatus()" style="padding:10px; width:100%;">VERIFICA</button>
                <div id="res-libra" style="margin-top:10px;"></div>
                <button id="btn-attiva" style="display:none; margin-top:10px; background:green; color:white; border:none; padding:10px; width:100%; cursor:pointer;" onclick="payCash()">REGISTRA PAGAMENTO CONTANTI</button>
            </div>

            <div id="u-AURORA" class="ufficio-box">
                <h3>Ufficio AURORA</h3>
                <p>Modifica testi Cronache:</p>
                <input type="text" id="edit-cronaca-testo" placeholder="Nuovo testo cronaca...">
                <button onclick="updateCronaca()" style="padding:10px; background:var(--oro); border:none; width:100%; cursor:pointer;">AGGIORNA CRONACHE</button>
            </div>

            <div id="u-IPERION" class="ufficio-box">
                <h3>Ufficio IPERION</h3>
                <p>Prepara la pergamena annuale per la sede fisica.</p>
                <button onclick="window.print()" style="background:var(--oro); padding:10px; width:100%; border:none; cursor:pointer;">STAMPA SCHEDA FISICA</button>
            </div>

            <div id="u-NYX" class="ufficio-box" style="background:var(--nero); color:white;"><h3>Ufficio NYX</h3><p>Accesso Totale Autorizzato.</p></div>
        </div>
    </section>
</div>

<div id="l-modal" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.9); z-index:1000; align-items:center; justify-content:center;">
    <div style="background:#f4e4bc; padding:30px; border:5px double var(--cremisi); width:320px; text-align:center;">
        <h3>IDENTITÀ GILDA</h3>
        <input type="email" id="l-mail" placeholder="Email">
        <input type="password" id="l-pass" placeholder="Password">
        <button onclick="doLogin()" style="width:100%; padding:10px; background:var(--cremisi); color:white; border:none; margin-top:10px; cursor:pointer;">ENTRA</button>
        <button onclick="document.getElementById('l-modal').style.display='none'" style="background:none; border:none; margin-top:10px; cursor:pointer; font-size:12px;">CHIUDI</button>
    </div>
</div>

<script>
    const sdk = new Appwrite.Client().setEndpoint('https://cloud.appwrite.io/v1').setProject('ID_PROGETTO');
    const auth = new Appwrite.Account(sdk), db = new Appwrite.Databases(sdk);
    const D='ID_DB', C='profiles';
    let user=null, target=null;

    async function init() {
        try {
            user = await auth.get();
            document.getElementById('btn-miaprofilo').style.display = 'block';
            document.getElementById('auth-btn').innerText = 'ESCI';
            document.getElementById('auth-btn').onclick = () => auth.deleteSession('current').then(()=>location.reload());
            const p = await db.getDocument(D, C, user.$id);
            document.getElementById('my-id').innerText = "ID: " + user.$id.substring(0, 8).toUpperCase();
            document.getElementById('p-name').innerText = p.name || 'Senza Nome';
            document.getElementById('p-pron').innerText = p.pronouns || '---';
            document.getElementById('p-ident').innerText = p.identity || '---';
            document.getElementById('p-status').innerText = p.is_active ? 'Abbonamento Effettuato' : 'Abbonamento Non Effettuato';
            document.getElementById('p-status').className = 'status ' + (p.is_active ? 'status-ok' : 'status-no');
        } catch(e) { console.log("Guest"); }
    }

    async function checkStatus() {
        const sid = document.getElementById('search-id').value.toLowerCase();
        try {
            const r = await db.listDocuments(D, C, [Appwrite.Query.startsWith('$id', sid)]);
            if(r.documents.length > 0) {
                target = r.documents[0];
                document.getElementById('res-libra').innerHTML = `Viandante: ${target.name} <br> Stato: ${target.is_active ? 'ATTIVO' : 'NON ATTIVO'}`;
                document.getElementById('btn-attiva').style.display = target.is_active ? 'none' : 'block';
            }
        } catch(e) { alert("Errore"); }
    }

    async function payCash() {
        await db.updateDocument(D, C, target.$id, { is_active: true });
        alert("Pagamento registrato!"); checkStatus();
    }

    async function saveData() {
        const o = { name: document.getElementById('in-name').value, pronouns: document.getElementById('in-pron').value, identity: document.getElementById('in-ident').value };
        try { await db.updateDocument(D, C, user.$id, o); alert("Salvato!"); location.reload(); }
        catch(e) { await db.createDocument(D, C, user.$id, o); location.reload(); }
    }

    function access(n, c) { if(prompt(`Sigillo ${n}:`) === c) { document.querySelectorAll('.ufficio-box').forEach(b=>b.style.display='none'); document.getElementById(`u-${n}`).style.display='block'; } }
    
    function showSec(id) { ['cronache','forum','profilo','ministero'].forEach(s=>document.getElementById(s).style.display='none'); document.getElementById(id).style.display='block'; }
    
    function openLogin() { document.getElementById('l-modal').style.display='flex'; }
    
    async function doLogin() {
        try { await auth.createEmailPasswordSession(document.getElementById('l-mail').value, document.getElementById('l-pass').value); location.reload(); }
        catch(e) { alert("Errore credenziali"); }
    }

    window.onload = init;
</script>
</body>
</html>
