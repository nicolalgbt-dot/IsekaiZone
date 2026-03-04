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
            font-family: 'Georgia', serif;
            margin: 0;
            overflow-x: hidden;
        }

        /* Layout con Sidebar Pubblicitarie */
        .layout {
            display: grid;
            grid-template-columns: 200px 1fr 200px;
            min-height: 100vh;
        }

        .sidebar {
            background-color: #ffffff;
            border-left: 1px solid #f2f2f2;
            border-right: 1px solid #f2f2f2;
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .ad-box {
            width: 100%;
            min-height: 250px;
            border: 1px solid #eee;
            background-color: #fafafa;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.7em;
            color: #999;
        }

        /* Navigazione a Stendardi */
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
            cursor: pointer;
            font-size: 0.8em;
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 50% 85%, 0% 100%);
            transition: transform 0.3s ease;
            border: none;
        }

        .stendardo:hover { transform: translateY(10px); }

        .btn-lapis { background-color: var(--lapis); }
        .btn-rame { background-color: var(--rame); }
        .btn-smeraldo { background-color: var(--smeraldo); }
        .btn-rubino { background-color: var(--rubino); }
        .btn-argento { background-color: var(--argento); color: var(--nero-inchiostro); }

        /* Contenuti */
        main { padding: 20px 40px; }
        h1, h2 { color: var(--cremisi); text-align: center; text-transform: uppercase; }
        
        .section { display: none; animation: fadeIn 0.5s; }
        .active { display: block; }

        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        /* Motto Abbonamenti */
        .motto {
            font-style: italic;
            font-size: 1.4em;
            text-align: center;
            margin: 50px auto;
            max-width: 700px;
            line-height: 1.6;
        }
        .motto b { color: var(--cremisi); }

        /* Form di Gestione */
        .admin-form {
            max-width: 500px;
            margin: 0 auto;
            border: 1px solid var(--argento);
            padding: 20px;
            background: #fdfdfd;
        }

        input, textarea {
            width: 100%;
            margin-bottom: 15px;
            padding: 10px;
            box-sizing: border-box;
            border: 1px solid #ccc;
        }

        .quest-card {
            border-bottom: 2px solid var(--cremisi);
            margin-bottom: 30px;
            padding-bottom: 20px;
        }
        
        .quest-card img { max-width: 100%; border-radius: 8px; }
    </style>
</head>
<body>

<div class="layout">
    <aside class="sidebar" id="ads-left">
        <div class="ad-box">PUBBLICITÀ ETICA 1</div>
        <div class="ad-box">PUBBLICITÀ ETICA 2</div>
    </aside>

    <main>
        <h1>IsekaiZone</h1>
        
        <nav>
            <button class="stendardo btn-lapis" onclick="show('news')">NEWS</button>
            <button class="stendardo btn-rame" onclick="show('forum')">FORUM</button>
            <button class="stendardo btn-smeraldo" onclick="show('abbonamenti')">ABBONAMENTI</button>
            <button class="stendardo btn-rubino" onclick="show('quest')">QUEST</button>
            <button class="stendardo btn-argento" onclick="checkAdmin()">GESTIONE</button>
        </nav>

        <section id="news" class="section active">
            <h2>Aggiornamenti Gilda</h2>
            <div id="news-list">Qui appariranno le ultime news caricate dal team...</div>
        </section>

        <section id="forum" class="section">
            <h2>Forum della Gilda</h2>
            <p style="text-align:center">Spazio di discussione per la lotta sociale.</p>
        </section>

        <section id="abbonamenti" class="section">
            <div
