index.html
<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IsekaiZone | Gilda dell'Attivismo</title>
    <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js"></script>
    <style>
        :root {
            --nero-inchiostro: #1A1A1B;
            --cremisi: #990000;
            --lapis: #26619C;
            --rame: #B87333;
            --smeraldo: #50C878;
            --rubino: #E0115F;
            --argento: #C0C0C0;
        }

        body {
            background-color: white;
            color: var(--nero-inchiostro);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            display: flex;
            justify-content: center;
        }

        .layout {
            display: grid;
            grid-template-columns: 200px 1fr 200px;
            width: 100%;
            max-width: 1400px;
            min-height: 100vh;
        }

        /* Sidebar Pubblicità */
        .sidebar {
            background-color: #fafafa;
            border-left: 1px solid #eee;
            border-right: 1px solid #eee;
            padding: 15px;
            text-align: center;
            font-size: 0.8em;
        }

        .ad-space {
            margin-bottom: 20px;
            border: 1px dashed #ccc;
            padding: 10px;
        }

        /* Navigazione Stendardi */
        nav {
            display: flex;
            justify-content: center;
            gap: 15px;
            padding: 30px 0;
        }

        .stendardo {
            width: 100px;
            height: 140px;
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            color: white;
            font-weight: bold;
            text-decoration: none;
            font-size: 0.9em;
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 85%, 0% 100%);
            transition: 0.3s transform;
            cursor: pointer;
            border: none;
        }

        .stendardo:hover { transform: translateY(10px); }

        .news { background-color: var(--lapis); }
        .forum { background-color: var(--rame); }
        .abbonamenti { background-color: var(--smeraldo); }
        .quest { background-color: var(--rubino); }
        .gestione { background-color: var(--argento); color: var(--nero-inchiostro); }

        /* Contenuti */
        main { padding: 0 40px; }
        h1, h2 { color: var(--cremisi); text-align: center; }
        
        .motto {
            font-style: italic;
            font-size: 1.2em;
            text-align: center;
            margin: 40px 0;
            line-height: 1.6;
        }

        .motto span { color: var(--cremisi); font-weight: bold; }

        .btn-action {
            display: block;
            width: fit-content;
            margin: 20px auto;
            background-color: var(--smeraldo);
            color: white;
            padding: 15px 30px;
            text-decoration: none;
            border-radius: 5px;
            font-weight: bold;
        }

        #sezione-gestione { display: none; background: #f0f0f0; padding: 20px; border-radius: 10px; }
    </style>
</head>
<body>

<div class="layout">
    <aside class="sidebar" id="ads-left">
        <p>SPAZIO ETICO</p>
        </aside>

    <main>
        <h1>ISEKAI ZONE</h1>
        <nav>
            <div class="stendardo news">NEWS</div>
            <div class="stendardo forum">FORUM</div>
            <div class="stendardo abbonamenti" onclick="showSection('membership')">ABBONAMENTI</div>
            <div class="stendardo quest" onclick="showSection('quests')">QUEST</div>
            <div class="stendardo gestione" onclick="checkAccess()">GESTIONE</div>
        </nav>

        <section id="membership-section">
            <p class="motto">
                "<span>Il sangue ribolle</span> davanti alla quest decisiva, <span>lotta e ricerca</span> sono il motto del nostro cuore"
            </p>
            <a href="URL_PIATTAFORMA_ESTERNA" class="btn-action">ABBONATI (5€/mese)</a>
        </section>

        <section id="quests-section" style="display:none;">
            <h2>Bacheca delle Quest</h2>
            <div id="quest-list"></div>
        </section>

        <section id="sezione-gestione">
            <h2>Pannello di Controllo SILVER00</h2>
            <input type="text" id="q-title" placeholder="Titolo Quest">
            <textarea id="q-desc" placeholder="Descrizione"></textarea>
            <button onclick="saveQuest()">Pubblica Quest</button>
        </section>
    </main>

    <aside class="sidebar" id="ads-right">
        <p>SPAZIO ETICO</p>
    </aside>
</div>

<script>
    // Configurazione Supabase (Inserisci i tuoi dati qui)
    const SUPABASE_URL = 'IL_TUO_URL_SUPABASE';
    const SUPABASE_KEY = 'LA_TUA_API_KEY_ANON';
    const supabase = supabase.createClient(SUPABASE_URL, SUPABASE_KEY);

    // Funzione di Accesso Gestione
    function checkAccess() {
        const code = prompt("Inserisci il codice personale di gestione:");
        if (code === "SILVER00") {
            document.getElementById('sezione-gestione').style.display = 'block';
            alert("Accesso autorizzato, Comandante.");
        } else {
            alert("Codice errato.");
        }
    }

    function showSection(section) {
        document.getElementById('membership-section').style.display = (section === 'membership') ? 'block' : 'none';
        document.getElementById('quests-section').style.display = (section === 'quests') ? 'block' : 'none';
        if(section === 'quests') loadQuests();
    }

    async function loadQuests() {
        const { data, error } = await supabase.from('quests').select('*');
        const list = document.getElementById('quest-list');
        list.innerHTML = data.map(q => `
            <div style="border-bottom: 1px solid #ddd; padding: 20px;">
                <h3 style="color:var(--cremisi)">${q.title}</h3>
                <p>${q.description}</p>
            </div>
        `).join('');
    }

    async function saveQuest() {
        const title = document.getElementById('q-title').value;
        const desc = document.getElementById('q-desc').value;
        const { data, error } = await supabase.from('quests').insert([{ title: title, description: desc }]);
        if (!error) {
            alert("Quest pubblicata con successo!");
            showSection('quests');
        }
    }
</script>

</body>
</html>
