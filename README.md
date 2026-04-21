<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DentStat Makala - Plateforme de Recherche Extractions Dentaires</title>
    <meta name="description" content="Plateforme de collecte de données cliniques pour la recherche sur les extractions dentaires à l'HGR Makala.">
    
    <!-- Google Fonts & Firebase SDK -->
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700;800&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-database-compat.js"></script>
    
    <style>
        :root {
            --primary: #00897b;
            --primary-dark: #00695c;
            --primary-light: #e0f2f1;
            --accent: #ffb300;
            --bg: #f5f7fa;
            --card-bg: #ffffff;
            --text-main: #2d3748;
            --text-sec: #718096;
            --border: #edf2f7;
            --radius-lg: 24px;
            --radius-md: 16px;
            --radius-sm: 12px;
            --shadow-sm: 0 4px 6px -1px rgba(0, 0, 0, 0.05);
            --shadow-lg: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
            --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        * { box-sizing: border-box; }

        body { 
            font-family: 'Inter', system-ui, -apple-system, sans-serif; 
            background: var(--bg); 
            margin: 0; 
            padding: 20px; 
            color: var(--text-main);
            line-height: 1.6;
            -webkit-font-smoothing: antialiased;
        }

        .app-shell { 
            max-width: 1200px; 
            margin: 0 auto; 
            background: var(--card-bg); 
            border-radius: var(--radius-lg); 
            box-shadow: var(--shadow-lg); 
            overflow: hidden; 
            min-height: 90vh; 
            display: flex; 
            flex-direction: column;
            border: 1px solid rgba(0,0,0,0.02);
        }

        .app-header { 
            background: linear-gradient(135deg, var(--primary-dark), var(--primary)); 
            padding: 40px 60px; 
            text-align: center;
            color: white;
            position: relative;
            overflow: hidden;
        }
        .app-header::before {
            content: "";
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(255,255,255,0.1) 0%, transparent 70%);
            animation: pulse-bg 15s linear infinite;
        }
        @keyframes pulse-bg {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .header-sub {
            font-family: 'Montserrat', sans-serif;
            font-weight: 800;
            font-size: 11px;
            opacity: 0.9;
            letter-spacing: 3px;
            margin-bottom: 12px;
            text-transform: uppercase;
            position: relative;
        }
        .thesis-title { 
            font-family: 'Montserrat', sans-serif; 
            font-size: 1.4rem; 
            font-weight: 800; 
            text-transform: uppercase; 
            line-height: 1.3; 
            margin: 0;
            letter-spacing: 0.5px;
            position: relative;
            text-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        
        .nav-tabs { 
            background: #ffffff; 
            display: flex; 
            padding: 10px 60px; 
            gap: 15px; 
            border-bottom: 1px solid var(--border); 
            overflow-x: auto; 
            scrollbar-width: none; 
            align-items: center;
            position: sticky;
            top: 0;
            z-index: 100;
        }
        .nav-tabs::-webkit-scrollbar { display: none; }

        .tab-btn { 
            padding: 14px 28px; 
            border: none; 
            border-radius: var(--radius-sm); 
            font-weight: 700; 
            font-size: 12px; 
            text-transform: uppercase; 
            cursor: pointer; 
            transition: var(--transition); 
            background: transparent; 
            color: var(--text-sec); 
            white-space: nowrap;
            display: flex;
            align-items: center;
            gap: 10px;
            letter-spacing: 1px;
        }
        .tab-btn:hover { background: var(--primary-light); color: var(--primary); transform: translateY(-1px); }
        .tab-btn.active { 
            background: var(--primary); 
            color: #fff; 
            box-shadow: 0 8px 16px rgba(0, 137, 123, 0.2); 
        }

        .content-area { 
            padding: 50px 100px; 
            display: none; 
            flex-grow: 1;
            background: #fff;
        }
        .content-area.active { display: block; animation: slideUp 0.6s cubic-bezier(0.16, 1, 0.3, 1); }
        @keyframes slideUp { 
            from { opacity: 0; transform: translateY(30px); } 
            to { opacity: 1; transform: translateY(0); } 
        }

        .section-box { 
            margin-bottom: 60px; 
            padding-bottom: 20px;
            border-bottom: 1px solid #f7fafc;
        }
        .section-title { 
            font-family: 'Montserrat', sans-serif; 
            font-weight: 800; 
            font-size: 14px; 
            color: var(--primary); 
            display: flex;
            align-items: center;
            gap: 15px;
            margin-bottom: 35px; 
            text-transform: uppercase; 
            letter-spacing: 2px;
        }
        .section-title::after {
            content: "";
            flex-grow: 1;
            height: 2px;
            background: linear-gradient(to right, var(--primary-light), transparent);
        }
        
        .field-grid { 
            display: grid; 
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); 
            gap: 30px; 
            margin-bottom: 30px; 
        }
        .field-group { display: flex; flex-direction: column; gap: 10px; }
        label { font-size: 11px; font-weight: 700; color: var(--text-sec); text-transform: uppercase; letter-spacing: 1px; }
        
        input, select, textarea { 
            padding: 16px 20px; 
            border: 2px solid #edf2f7; 
            border-radius: var(--radius-sm); 
            font-size: 16px; 
            font-family: inherit; 
            background: #f8fafc; 
            transition: var(--transition); 
            color: var(--text-main);
            width: 100%;
        }
        input:focus, select:focus, textarea:focus { 
            border-color: var(--primary); 
            outline: none; 
            background: #fff; 
            box-shadow: 0 0 0 4px rgba(0, 137, 123, 0.08);
        }

        .pills-grid { 
            display: grid; 
            grid-template-columns: repeat(auto-fill, minmax(220px, 1fr)); 
            gap: 15px; 
            padding: 30px; 
            background: #f8fafc; 
            border-radius: var(--radius-md); 
            border: 1px solid var(--border); 
            margin-bottom: 15px;
        }
        .check-pill { 
            display: flex; 
            align-items: center; 
            gap: 15px; 
            padding: 14px 20px; 
            background: #fff; 
            border-radius: var(--radius-sm); 
            border: 1px solid var(--border); 
            cursor: pointer; 
            transition: var(--transition); 
            font-size: 15px; 
            font-weight: 600; 
            user-select: none;
            box-shadow: var(--shadow-sm);
        }
        .check-pill:hover { border-color: var(--primary); transform: translateY(-2px); box-shadow: 0 6px 12px rgba(0,0,0,0.05); }
        .check-pill input { width: 22px; height: 22px; accent-color: var(--primary); margin: 0; }
        
        .radio-group { display: flex; flex-wrap: wrap; gap: 40px; margin-top: 10px; }
        .radio-choice { display: flex; align-items: center; gap: 12px; cursor: pointer; font-size: 15px; font-weight: 600; }
        .radio-choice input { width: 22px; height: 22px; accent-color: var(--primary); cursor: pointer; }

        .btn-action { 
            background: linear-gradient(to right, var(--primary), var(--primary-dark)); 
            color: #fff; 
            padding: 24px; 
            border: none; 
            border-radius: var(--radius-md); 
            font-weight: 800; 
            text-transform: uppercase; 
            cursor: pointer; 
            width: 100%; 
            font-size: 18px; 
            letter-spacing: 2px; 
            box-shadow: 0 10px 25px -5px rgba(0, 137, 123, 0.4);
            transition: var(--transition);
            margin-top: 40px;
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 15px;
        }
        .btn-action:hover { transform: translateY(-3px); box-shadow: 0 20px 35px -5px rgba(0, 121, 107, 0.5); }

        .data-table-container { 
            overflow-x: auto; 
            border-radius: var(--radius-md); 
            border: 1px solid var(--border); 
            background: #fff;
            margin-top: 20px;
            box-shadow: var(--shadow-sm);
        }
        table { width: 100%; border-collapse: collapse; min-width: 900px; }
        th { background: #f8fafc; padding: 24px; text-align: left; font-size: 11px; font-weight: 800; color: var(--text-sec); text-transform: uppercase; border-bottom: 2px solid var(--border); letter-spacing: 1px; }
        td { padding: 20px 24px; border-bottom: 1px solid var(--border); font-size: 15px; }
        tr:hover td { background: #fcfdfe; }

        .modal { display: none; position: fixed; z-index: 2000; left: 0; top: 0; width: 100%; height: 100%; background: rgba(15, 23, 42, 0.7); backdrop-filter: blur(10px); padding: 20px; }
        .modal-content { background: #fff; margin: 5vh auto; padding: 50px; border-radius: var(--radius-lg); width: 100%; max-width: 1000px; max-height: 85vh; overflow-y: auto; box-shadow: var(--shadow-lg); position: relative; }
        .close-btn { font-size: 40px; cursor: pointer; color: var(--text-sec); transition: var(--transition); }
        .close-btn:hover { color: var(--primary); transform: rotate(90deg); }
        
        .detail-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 24px; }
        .detail-item { padding: 20px; background: #f8fafc; border-radius: var(--radius-sm); border-left: 6px solid var(--primary); transition: var(--transition); }
        .detail-item:hover { transform: translateX(5px); background: #fff; box-shadow: var(--shadow-sm); }

        #sync-indicator { font-size: 12px; font-weight: 800; color: var(--primary); display: flex; align-items: center; gap: 12px; margin-bottom: 25px; background: var(--primary-light); padding: 12px 24px; border-radius: 40px; width: fit-content; border: 1px solid var(--primary); }
        .pulse { width: 12px; height: 12px; background: var(--primary); border-radius: 50%; animation: pulse 2s infinite; }
        @keyframes pulse { 0% { transform: scale(0.8); opacity: 1; box-shadow: 0 0 0 0 rgba(0, 137, 123, 0.7); } 70% { transform: scale(1.2); opacity: 0; box-shadow: 0 0 0 10px rgba(0, 137, 123, 0); } 100% { transform: scale(0.8); opacity: 1; box-shadow: 0 0 0 0 rgba(0, 137, 123, 0); } }

        .toast { position: fixed; bottom: 40px; left: 50%; transform: translateX(-50%) translateY(100px); background: #fff; color: var(--text-main); padding: 16px 32px; border-radius: 40px; box-shadow: var(--shadow-lg); z-index: 3000; display: flex; align-items: center; gap: 12px; font-weight: 700; transition: var(--transition); border: 2px solid var(--primary); pointer-events: none; opacity: 0; }
        .toast.show { transform: translateX(-50%) translateY(0); opacity: 1; }

        #backToTop { display:none; position:fixed; bottom:30px; right:30px; width:50px; height:50px; border-radius:50%; background:var(--primary); color:white; border:none; box-shadow:var(--shadow-lg); cursor:pointer; z-index:1000; font-size:20px; transition:var(--transition); }
        #backToTop:hover { transform: scale(1.1); background: var(--primary-dark); }

        /* === STYLES TABLEAUX STATISTIQUES === */
        .stat-section { margin-top: 60px; }
        .stat-section-title {
            font-family: 'Montserrat', sans-serif;
            font-weight: 800;
            font-size: 20px;
            color: var(--primary-dark);
            margin-bottom: 30px;
            padding-bottom: 15px;
            border-bottom: 3px solid var(--primary);
        }
        .stat-subtitle {
            font-family: 'Inter', sans-serif;
            font-weight: 700;
            font-size: 14px;
            color: var(--text-main);
            margin: 35px 0 12px 0;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }
        .stat-note {
            font-size: 12px;
            color: var(--text-sec);
            margin: 0 0 12px 0;
            font-style: italic;
        }
        .stat-table-wrap {
            overflow-x: auto;
            border-radius: var(--radius-md);
            border: 1px solid var(--border);
            background: #fff;
            margin-bottom: 20px;
            box-shadow: var(--shadow-sm);
        }
        .stat-table {
            width: 100%;
            border-collapse: collapse;
            min-width: auto;
            font-size: 14px;
        }
        .stat-table th {
            background: var(--primary);
            color: #fff;
            padding: 14px 16px;
            text-align: center;
            font-size: 11px;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            border: 1px solid rgba(255,255,255,0.15);
            white-space: nowrap;
        }
        .stat-table th:first-child {
            text-align: left;
            border-left: none;
        }
        .stat-table td {
            padding: 11px 16px;
            border: 1px solid var(--border);
            text-align: center;
            font-weight: 500;
            font-size: 14px;
        }
        .stat-table td:first-child {
            text-align: left;
            font-weight: 700;
            background: #f8fafc;
            color: var(--text-main);
            border-left: none;
            white-space: nowrap;
        }
        .stat-table tbody tr:hover td {
            background: var(--primary-light);
        }
        .stat-table tbody tr:hover td:first-child {
            background: #d5eded;
        }
        .stat-table .total-row td {
            background: var(--primary-dark);
            color: #fff;
            font-weight: 800;
            border-color: rgba(0,0,0,0.1);
        }
        .stat-table .total-row td:first-child {
            background: var(--primary-dark);
            color: #fff;
        }
        .stat-empty {
            text-align: center;
            padding: 50px 20px;
            color: var(--text-sec);
            font-size: 14px;
            font-weight: 500;
        }

        @media (max-width: 1024px) {
            .content-area { padding: 40px 60px; }
        }

        @media (max-width: 768px) {
            body { padding: 10px; }
            .app-header { padding: 30px 20px; }
            .thesis-title { font-size: 1.1rem; }
            .nav-tabs { padding: 10px 20px; gap: 8px; }
            .tab-btn { padding: 12px 18px; font-size: 10px; }
            .content-area { padding: 30px 20px; }
            .field-grid { grid-template-columns: 1fr; gap: 20px; }
            .pills-grid { padding: 20px; grid-template-columns: 1fr; }
            .radio-group { gap: 20px; }
            .modal-content { padding: 30px 20px; margin: 0; max-height: 100vh; border-radius: 0; }
            .detail-grid { grid-template-columns: 1fr; }
            .stat-table th, .stat-table td { padding: 8px 10px; font-size: 11px; }
            .stat-section-title { font-size: 16px; }
        }
    </style>
</head>
<body>

<div id="toast" class="toast"><span>✨</span> Données synchronisées avec succès !</div>

<main class="app-shell">
    <header class="app-header">
        <div class="header-sub">FICHE D'ENQUÊTE CLINIQUE</div>
        <h1 class="thesis-title">
            Indications de l'extraction dentaire à l'Hôpital Général de Référence de Makala
        </h1>
    </header>

    <nav class="nav-tabs" aria-label="Navigation principale">
        <button class="tab-btn active" onclick="switchTab(1)">📝 Collecte</button>
        <button class="tab-btn" onclick="switchTab(2)">📊 Matrice</button>
        <button class="tab-btn" onclick="switchTab(3)">📈 Statistiques</button>
        <button class="tab-btn" onclick="switchTab(4)">⚙️ Paramètres</button>
    </nav>

    <!-- TAB 1: COLLECTE -->
    <section id="tab-1" class="content-area active">
        <form id="dentalForm">
            <div class="section-box">
                <div class="section-title"><span>I. IDENTIFICATION</span></div>
                <div class="field-grid">
                    <div class="field-group"><label>Numéro de fiche :</label><input type="text" id="n_fiche" placeholder="Ex: 001" required></div>
                    <div class="field-group"><label>Service :</label><input type="text" id="service" placeholder="..."></div>
                    <div class="field-group"><label>Nom de l'enquêteur :</label><input type="text" id="enqueteur" placeholder="..."></div>
                </div>
            </div>

            <div class="section-box">
                <div class="section-title"><span>II. DATE DE CONSULTATION</span></div>
                <div class="field-grid">
                    <div class="field-group"><label>Date :</label><input type="date" id="date_consult"></div>
                </div>
            </div>

            <div class="section-box">
                <div class="section-title"><span>III. DONNÉES SOCIODÉMOGRAPHIQUES</span></div>
                <div class="field-grid">
                    <div class="field-group"><label>Âge :</label><input type="number" id="age" min="0"></div>
                    <div class="field-group">
                        <label>Sexe :</label>
                        <div class="radio-group">
                            <label class="radio-choice"><input type="radio" name="sexe" value="Masculin"> Masculin</label>
                            <label class="radio-choice"><input type="radio" name="sexe" value="Féminin"> Féminin</label>
                        </div>
                    </div>
                </div>
            </div>

            <div class="section-box">
                <div class="section-title"><span>IV. DONNÉES CLINIQUES</span></div>
                
                <div class="field-group" style="margin-bottom: 30px;">
                    <label>1. Motif de consultation :</label>
                    <div class="pills-grid" id="motif_list">
                        <label class="check-pill"><input type="checkbox" name="motif" value="Douleur dentaire"><span>Douleur dentaire</span></label>
                        <label class="check-pill"><input type="checkbox" name="motif" value="Carie"><span>Carie</span></label>
                        <label class="check-pill"><input type="checkbox" name="motif" value="Mobilité dentaire"><span>Mobilité dentaire</span></label>
                        <label class="check-pill"><input type="checkbox" name="motif" value="Infection"><span>Infection</span></label>
                        <label class="check-pill"><input type="checkbox" name="motif" value="Traumatisme"><span>Traumatisme</span></label>
                        <label class="check-pill"><input type="checkbox" name="motif" value="Orthodontique"><span>Indication orthodontique</span></label>
                    </div>
                    <input type="text" id="motif_autres" placeholder="Autres motif...">
                </div>

                <div class="field-grid">
                    <div class="field-group"><label>2. Dent concernée :</label><input type="text" id="dent_concernee"></div>
                    <div class="field-group">
                        <label>3. Arcade :</label>
                        <div class="radio-group">
                            <label class="radio-choice"><input type="radio" name="arcade" value="Maxillaire"> Maxillaire</label>
                            <label class="radio-choice"><input type="radio" name="arcade" value="Mandibulaire"> Mandibulaire</label>
                        </div>
                    </div>
                </div>

                <div class="field-group" style="margin: 30px 0;">
                    <label>4. Indication de l'extraction :</label>
                    <div class="pills-grid" id="indication_list">
                        <label class="check-pill"><input type="checkbox" name="indication" value="Carie compliquée"><span>Carie compliquée</span></label>
                        <label class="check-pill"><input type="checkbox" name="indication" value="Nécrose pulpaire"><span>Nécrose pulpaire</span></label>
                        <label class="check-pill"><input type="checkbox" name="indication" value="Parodontite avancée"><span>Parodontite avancée</span></label>
                        <label class="check-pill"><input type="checkbox" name="indication" value="Dent incluse"><span>Dent incluse</span></label>
                        <label class="check-pill"><input type="checkbox" name="indication" value="Dent fracturée"><span>Dent fracturée</span></label>
                        <label class="check-pill"><input type="checkbox" name="indication" value="Orthodontique"><span>Orthodontique</span></label>
                    </div>
                    <input type="text" id="indication_autres" placeholder="Autres indication...">
                </div>

                <div class="field-group" style="margin-bottom: 30px;">
                    <label>5. Examen(s) complémentaire(s) :</label>
                    <div class="pills-grid" id="exam_list">
                        <label class="check-pill"><input type="checkbox" name="examen" value="Radiographie"><span>Radiographie</span></label>
                        <label class="check-pill"><input type="checkbox" name="examen" value="Panoramique"><span>Panoramique</span></label>
                        <label class="check-pill"><input type="checkbox" name="examen" value="Aucun"><span>Aucun</span></label>
                    </div>
                    <input type="text" id="examen_autres" placeholder="Autres examens...">
                </div>

                <div class="field-group" style="margin-bottom: 30px;">
                    <label>6. Type d'extraction :</label>
                    <div class="radio-group">
                        <label class="radio-choice"><input type="radio" name="type_extraction" value="Simple"> Simple</label>
                        <label class="radio-choice"><input type="radio" name="type_extraction" value="Chirurgicale"> Chirurgicale</label>
                    </div>
                </div>

                <div class="field-group" style="margin-bottom: 30px;">
                    <label>7. Traitement médical associé :</label>
                    <div class="field-grid">
                        <div class="field-group"><label>Antalgique :</label><input type="text" id="trait_antalgique" placeholder="..."></div>
                        <div class="field-group"><label>Antibiotique :</label><input type="text" id="trait_antibiotique" placeholder="..."></div>
                        <div class="field-group"><label>AINS :</label><input type="text" id="trait_ains" placeholder="..."></div>
                        <div class="field-group"><label>Bain de bouche :</label><input type="text" id="trait_bain_bouche" placeholder="..."></div>
                    </div>
                </div>

                <div class="field-group" style="margin-bottom: 30px;">
                    <label>8. Complications :</label>
                    <div class="pills-grid" id="complications_list">
                        <label class="check-pill"><input type="checkbox" name="complication" value="Aucune"><span>Aucune</span></label>
                        <label class="check-pill"><input type="checkbox" name="complication" value="Hémorragie"><span>Hémorragie</span></label>
                        <label class="check-pill"><input type="checkbox" name="complication" value="Infection"><span>Infection</span></label>
                        <label class="check-pill"><input type="checkbox" name="complication" value="Alvéolite"><span>Alvéolite</span></label>
                        <label class="check-pill"><input type="checkbox" name="complication" value="Douleur persistante"><span>Douleur persistante</span></label>
                    </div>
                    <input type="text" id="complication_autres" placeholder="Autres complications...">
                </div>

                <div class="field-group">
                    <label>Observations complémentaires :</label>
                    <textarea id="observations" rows="5" placeholder="Notes additionnelles..."></textarea>
                </div>
            </div>

            <button type="button" class="btn-action" id="saveBtn">🚀 Envoyer les Données</button>
        </form>
    </section>

    <!-- TAB 2: MATRICE -->
    <section id="tab-2" class="content-area">
        <div id="sync-indicator"><div class="pulse"></div> Données en temps réel</div>
        <div class="data-table-container">
            <table>
                <thead>
                    <tr>
                        <th>Fiche</th>
                        <th>Âge</th>
                        <th>Sexe</th>
                        <th>Extraction</th>
                        <th>Actions</th>
                    </tr>
                </thead>
                <tbody id="matrix-body"></tbody>
            </table>
        </div>
    </section>

    <!-- TAB 3: STATISTIQUES -->
    <section id="tab-3" class="content-area">
        <div class="section-title"><span>📈 Aperçu Statistique</span></div>
        <div class="field-grid" style="grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));">
            <div class="detail-item" style="text-align:center;">
                <small>Total Fiches</small>
                <div id="stat-total" style="font-size:32px; font-weight:800; color:var(--primary); margin-top:10px;">0</div>
            </div>
            <div class="detail-item" style="text-align:center;">
                <small>Âge Moyen</small>
                <div id="stat-age" style="font-size:32px; font-weight:800; color:var(--accent); margin-top:10px;">0</div>
            </div>
            <div class="detail-item" style="text-align:center;">
                <small>Ratio Sexe (M/F)</small>
                <div id="stat-ratio" style="font-size:32px; font-weight:800; color:var(--primary-dark); margin-top:10px;">0:0</div>
            </div>
        </div>

        <!-- ===== 1. FRÉQUENCE ===== -->
        <div class="stat-section">
            <div class="stat-section-title">1. Fréquence</div>

            <h3 class="stat-subtitle">Tableau I : Répartition selon l'âge et le sexe</h3>
            <div id="stat-age-sexe" class="stat-table-wrap"><div class="stat-empty">En attente de données...</div></div>

            <h3 class="stat-subtitle">Tableau II : Répartition selon le motif de consultation</h3>
            <p class="stat-note">* Question à réponses multiples — le total des effectifs peut être supérieur à N.</p>
            <div id="stat-motif" class="stat-table-wrap"><div class="stat-empty">En attente de données...</div></div>

            <h3 class="stat-subtitle">Tableau III : Répartition selon l'indication de l'extraction</h3>
            <p class="stat-note">* Question à réponses multiples — le total des effectifs peut être supérieur à N.</p>
            <div id="stat-indication" class="stat-table-wrap"><div class="stat-empty">En attente de données...</div></div>

            <h3 class="stat-subtitle">Tableau IV : Répartition selon le type d'extraction</h3>
            <div id="stat-type" class="stat-table-wrap"><div class="stat-empty">En attente de données...</div></div>

            <h3 class="stat-subtitle">Tableau V : Répartition selon les complications</h3>
            <p class="stat-note">* Question à réponses multiples — le total des effectifs peut être supérieur à N.</p>
            <div id="stat-complications" class="stat-table-wrap"><div class="stat-empty">En attente de données...</div></div>
        </div>

        <!-- ===== 2. CROISEMENT ===== -->
        <div class="stat-section">
            <div class="stat-section-title">2. Croisement des variables</div>

            <h3 class="stat-subtitle">Tableau VI : Âge × Indications de l'extraction</h3>
            <div id="cross-age-indication" class="stat-table-wrap"><div class="stat-empty">En attente de données...</div></div>

            <h3 class="stat-subtitle">Tableau VII : Âge × Sexe</h3>
            <div id="cross-age-sexe" class="stat-table-wrap"><div class="stat-empty">En attente de données...</div></div>

            <h3 class="stat-subtitle">Tableau VIII : Indications de l'extraction × Type d'extraction</h3>
            <div id="cross-indication-type" class="stat-table-wrap"><div class="stat-empty">En attente de données...</div></div>

            <h3 class="stat-subtitle">Tableau IX : Dent concernée × Indications de l'extraction</h3>
            <div id="cross-dent-indication" class="stat-table-wrap"><div class="stat-empty">En attente de données...</div></div>

            <h3 class="stat-subtitle">Tableau X : Type d'extraction × Complications</h3>
            <div id="cross-type-complication" class="stat-table-wrap"><div class="stat-empty">En attente de données...</div></div>

            <h3 class="stat-subtitle">Tableau XI : Examen complémentaire × Type d'extraction</h3>
            <div id="cross-examen-type" class="stat-table-wrap"><div class="stat-empty">En attente de données...</div></div>

            <h3 class="stat-subtitle">Tableau XII : Traitement médical × Complications</h3>
            <div id="cross-traitement-complication" class="stat-table-wrap"><div class="stat-empty">En attente de données...</div></div>
        </div>
    </section>

    <!-- TAB 4: PARAMÈTRES -->
    <section id="tab-4" class="content-area">
        <div class="section-title"><span>⚙️ Système</span></div>
        <div class="detail-item" style="display:flex; justify-content:space-between; align-items:center; border-left:none; border:1px solid var(--border);">
            <div>
                <h4 style="margin:0;">Status Connectivité</h4>
                <p id="db-status" style="margin:5px 0 0 0; font-size:14px; color:var(--text-sec);">Initialisation...</p>
            </div>
            <div class="pulse"></div>
        </div>
        <div style="margin-top:40px; display:grid; gap:20px;">
            <button class="check-pill" style="justify-content:center;" onclick="window.print()">🖨️ Imprimer la page</button>
            <button class="check-pill" style="justify-content:center;" onclick="alert('Exportation CSV demandée...')">📥 Exporter en CSV</button>
        </div>
    </section>
</main>

<button id="backToTop">↑</button>

<div id="detailModal" class="modal">
    <div class="modal-content">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:30px;">
            <h2 style="margin:0; color:var(--primary); font-size:1.2rem;">DÉTAILS LA FICHE</h2>
            <span class="close-btn" onclick="closeModal()">&times;</span>
        </div>
        <div id="modalBody" class="detail-grid"></div>
    </div>
</div>

<script>
    // --- FIREBASE CONFIG ---
    const firebaseConfig = {
      apiKey: "AIzaSyCmtQJNjuvkAST5bBXbUzWp7ykvcMgtYZg",
      authDomain: "pierrot-8737b.firebaseapp.com",
      projectId: "pierrot-8737b",
      storageBucket: "pierrot-8737b.firebasestorage.app",
      messagingSenderId: "884392310610",
      appId: "1:884392310610:web:7b34c50854f4715c362c8b",
      measurementId: "G-RVKZ3VG6MJ",
      databaseURL: "https://pierrot-8737b-default-rtdb.firebaseio.com"
    };
    
    let db = null;
    try {
        firebase.initializeApp(firebaseConfig);
        db = firebase.database();
        const stat = document.getElementById('db-status');
        stat.innerText = "✅ Connecté au serveur cloud";
        stat.style.color = "var(--primary)";
    } catch(e) {
        document.getElementById('db-status').innerText = "⚠️ Mode Hors-Ligne";
    }

    // --- TAB LOGIC ---
    function switchTab(n) {
        document.querySelectorAll('.content-area').forEach(a => a.classList.remove('active'));
        document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
        document.getElementById('tab-'+n).classList.add('active');
        document.querySelectorAll('.tab-btn')[n-1].classList.add('active');
        window.scrollTo({ top: 0, behavior: 'smooth' });
    }

    // --- DATA HANDLING ---
    function getChecked(name) {
        return Array.from(document.querySelectorAll(`input[name="${name}"]:checked`)).map(c => c.value).join(', ');
    }

    function showToast() {
        const t = document.getElementById('toast');
        t.classList.add('show');
        setTimeout(() => t.classList.remove('show'), 3000);
    }

    document.getElementById('saveBtn').addEventListener('click', async () => {
        const btn = document.getElementById('saveBtn');
        const data = {
            fiche: document.getElementById('n_fiche').value,
            enqueteur: document.getElementById('enqueteur').value,
            service: document.getElementById('service').value,
            date: document.getElementById('date_consult').value,
            age: document.getElementById('age').value,
            sexe: document.querySelector('input[name="sexe"]:checked')?.value || '-',
            motif: getChecked('motif') + (document.getElementById('motif_autres').value ? ' | ' + document.getElementById('motif_autres').value : ''),
            dent: document.getElementById('dent_concernee').value,
            arcade: document.querySelector('input[name="arcade"]:checked')?.value || '-',
            indication: getChecked('indication') + (document.getElementById('indication_autres').value ? ' | ' + document.getElementById('indication_autres').value : ''),
            examens: getChecked('examen') + (document.getElementById('examen_autres').value ? ' | ' + document.getElementById('examen_autres').value : ''),
            type: document.querySelector('input[name="type_extraction"]:checked')?.value || '-',
            traitement: {
                antalgique: document.getElementById('trait_antalgique').value,
                antibiotique: document.getElementById('trait_antibiotique').value,
                ains: document.getElementById('trait_ains').value,
                bain_bouche: document.getElementById('trait_bain_bouche').value
            },
            complications: getChecked('complication') + (document.getElementById('complication_autres').value ? ' | ' + document.getElementById('complication_autres').value : ''),
            observations: document.getElementById('observations').value,
            timestamp: new Date().toISOString()
        };

        if(!data.fiche) return alert("Fiche obligatoire");

        btn.disabled = true;
        btn.innerText = "Patientez...";

        if(db) {
            await db.ref('extractions').push(data);
            showToast();
            document.getElementById('dentalForm').reset();
            switchTab(2);
        } else {
            addRow(data);
            alert("Enregistré localement");
        }
        
        btn.disabled = false;
        btn.innerText = "🚀 Envoyer les Données";
    });

    if(db) {
        db.ref('extractions').on('value', snap => {
            const body = document.getElementById('matrix-body');
            body.innerHTML = "";
            const items = [];
            snap.forEach(c => {
                const item = c.val();
                item.key = c.key;
                items.push(item);
            });
            updateStats(items);
            items.reverse().forEach(d => addRow(d));
        });
    }

    window.deleteEntry = function(key) {
        if(confirm("Confirmer la suppression de cette fiche ?")) {
            db.ref('extractions').child(key).remove()
              .then(() => { alert("Supprimé."); })
              .catch(e => alert(e.message));
        }
    }

    function updateStats(items) {
        document.getElementById('stat-total').innerText = items.length;
        if(items.length) {
            const avg = items.reduce((acc, c) => acc + (parseInt(c.age) || 0), 0) / items.length;
            document.getElementById('stat-age').innerText = Math.round(avg) + " ans";
            const m = items.filter(i => i.sexe === 'Masculin').length;
            const f = items.filter(i => i.sexe === 'Féminin').length;
            document.getElementById('stat-ratio').innerText = m + ':' + f;
        } else {
            document.getElementById('stat-age').innerText = "0";
            document.getElementById('stat-ratio').innerText = "0:0";
        }
        renderAllStatistics(items);
    }

    function addRow(d) {
        const body = document.getElementById('matrix-body');
        const row = document.createElement('tr');
        const safeData = btoa(unescape(encodeURIComponent(JSON.stringify(d))));
        row.innerHTML =
            '<td><b style="color:var(--primary)">#' + d.fiche + '</b></td>' +
            '<td>' + d.age + ' ans</td>' +
            '<td>' + d.sexe + '</td>' +
            '<td>' + d.type + '</td>' +
            '<td><div style="display:flex; gap:10px;">' +
            '<button onclick="viewDetails(\'' + safeData + '\')" style="cursor:pointer; border:1px solid var(--border); padding:5px 10px; border-radius:8px;">👁️</button>' +
            '<button onclick="deleteEntry(\'' + d.key + '\')" style="cursor:pointer; border:1px solid #ff4444; color:#ff4444; padding:5px 10px; border-radius:8px; background:transparent;">🗑️</button>' +
            '</div></td>';
        body.appendChild(row);
    }

    window.viewDetails = function(encoded) {
        const d = JSON.parse(decodeURIComponent(escape(atob(encoded))));
        const body = document.getElementById('modalBody');
        body.innerHTML = "";
        const labels = {
            fiche: "N° Fiche", enqueteur: "Enquêteur", service: "Service",
            date: "Date Consultation", age: "Âge", sexe: "Sexe",
            motif: "Motif de Consultation", dent: "Dent concernée", arcade: "Arcade",
            indication: "Indication d'extraction", examens: "Examens complémentaires",
            type: "Type d'extraction", traitement: "Traitement associé",
            complications: "Complications post-op", observations: "Observations"
        };
        Object.keys(labels).forEach(k => {
            if(!d[k]) return;
            let val = d[k];
            if(k === 'traitement') {
                val = Object.entries(val)
                    .filter(function(pair) { return pair[1]; })
                    .map(function(pair) { return '<span style="color:var(--primary)">' + pair[0] + ':</span> ' + pair[1]; })
                    .join('<br>');
            }
            if(!val) return;
            body.innerHTML += '<div class="detail-item"><small style="text-transform:uppercase; color:var(--text-sec); font-size:10px; font-weight:800;">' + labels[k] + '</small><div style="margin-top:5px; font-weight:600;">' + val + '</div></div>';
        });
        document.getElementById('detailModal').style.display = "block";
    }

    window.closeModal = function() { document.getElementById('detailModal').style.display = "none"; }
    window.onclick = function(e) { if(e.target == document.getElementById('detailModal')) closeModal(); }

    window.onscroll = function() {
        document.getElementById('backToTop').style.display = (window.scrollY > 500) ? "block" : "none";
    };
    document.getElementById('backToTop').onclick = function() { window.scrollTo({top:0, behavior:'smooth'}); };


    /* ==========================================================
       STATISTIQUES — Fréquence & Croisement
       Ces fonctions lisent automatiquement TOUTES les fiches
       présentes dans Firebase et génèrent les 12 tableaux.
       ========================================================== */

    var AGE_GROUPS = ['0-10 ans', '11-20 ans', '21-30 ans', '31-40 ans', '41-50 ans', '51-60 ans', '> 60 ans'];

    function getAgeGroup(age) {
        var a = parseInt(age);
        if (isNaN(a) || a < 0) return 'Non renseigné';
        if (a <= 10) return '0-10 ans';
        if (a <= 20) return '11-20 ans';
        if (a <= 30) return '21-30 ans';
        if (a <= 40) return '31-40 ans';
        if (a <= 50) return '41-50 ans';
        if (a <= 60) return '51-60 ans';
        return '> 60 ans';
    }

    // Découper un champ à réponses multiples (séparateurs ", " ou " | ")
    function splitMulti(val) {
        if (!val || val === '-' || val.trim() === '') return [];
        return val.split(/[,|]/).map(function(v) { return v.trim(); }).filter(function(v) { return v && v !== '-'; });
    }

    // Compter les occurrences d'une variable simple ou multiple
    function countFreq(items, getField) {
        var counts = {};
        items.forEach(function(item) {
            var vals = getField(item);
            if (Array.isArray(vals)) {
                vals.forEach(function(v) { counts[v] = (counts[v] || 0) + 1; });
            } else {
                if (vals && vals !== '-') counts[vals] = (counts[vals] || 0) + 1;
            }
        });
        return counts;
    }

    // Construire une matrice de croisement { ligne: { colonne: n } }
    function buildCross(items, getRows, getCols) {
        var matrix = {};
        var colSet = {};
        items.forEach(function(item) {
            var rows = getRows(item);
            var cols = getCols(item);
            var rArr = Array.isArray(rows) ? rows : [rows];
            var cArr = Array.isArray(cols) ? cols : [cols];
            rArr.forEach(function(r) {
                if (!r || r === '-') return;
                if (!matrix[r]) matrix[r] = {};
                cArr.forEach(function(c) {
                    if (!c || c === '-') return;
                    colSet[c] = true;
                    matrix[r][c] = (matrix[r][c] || 0) + 1;
                });
            });
        });
        return { matrix: matrix, columns: Object.keys(colSet) };
    }

    // --- Rendu : Tableau de fréquence simple (Variable, n, %) ---
    function renderFreq(containerId, counts, totalN, sortDesc) {
        var el = document.getElementById(containerId);
        if (!el) return;
        var entries = Object.entries(counts);
        if (!entries.length) { el.innerHTML = '<div class="stat-empty">Aucune donnée disponible</div>'; return; }
        entries.sort(function(a, b) { return sortDesc ? b[1] - a[1] : a[1] - b[1]; });

        var totalResp = 0;
        var html = '<table class="stat-table"><thead><tr><th>Variable</th><th>Effectif (n)</th><th>Fréquence (%)</th></tr></thead><tbody>';
        entries.forEach(function(e) {
            totalResp += e[1];
            var pct = totalN > 0 ? ((e[1] / totalN) * 100).toFixed(1) : '0.0';
            html += '<tr><td>' + e[0] + '</td><td>' + e[1] + '</td><td>' + pct + '%</td></tr>';
        });
        html += '<tr class="total-row"><td>TOTAL</td><td>' + totalResp + '</td><td>—</td></tr>';
        html += '</tbody></table>';
        el.innerHTML = html;
    }

    // --- Rendu : Tableau Âge × Sexe (fréquence avec sous-colonnes) ---
    function renderAgeSexFreq(containerId, items, totalN) {
        var el = document.getElementById(containerId);
        if (!el) return;
        if (!totalN) { el.innerHTML = '<div class="stat-empty">Aucune donnée disponible</div>'; return; }

        var groups = AGE_GROUPS.slice();
        var hasNR = false;
        items.forEach(function(item) {
            if (getAgeGroup(item.age) === 'Non renseigné') hasNR = true;
        });
        if (hasNR) groups.push('Non renseigné');

        var data = {};
        groups.forEach(function(g) { data[g] = { M: 0, F: 0, NR: 0 }; });

        items.forEach(function(item) {
            var g = getAgeGroup(item.age);
            if (!data[g]) data[g] = { M: 0, F: 0, NR: 0 };
            if (item.sexe === 'Masculin') data[g].M++;
            else if (item.sexe === 'Féminin') data[g].F++;
            else data[g].NR++;
        });

        var html = '<table class="stat-table"><thead><tr><th>Tranche d\'âge</th><th>Masculin (n)</th><th>Féminin (n)</th><th>Non précisé (n)</th><th>Total (n)</th><th>%</th></tr></thead><tbody>';
        var grandM = 0, grandF = 0, grandNR = 0;
        groups.forEach(function(g) {
            var m = data[g].M, f = data[g].F, nr = data[g].NR;
            var t = m + f + nr;
            grandM += m; grandF += f; grandNR += nr;
            var pct = totalN > 0 ? ((t / totalN) * 100).toFixed(1) : '0.0';
            html += '<tr><td>' + g + '</td><td>' + m + '</td><td>' + f + '</td><td>' + nr + '</td><td><b>' + t + '</b></td><td>' + pct + '%</td></tr>';
        });
        var grandT = grandM + grandF + grandNR;
        html += '<tr class="total-row"><td>TOTAL</td><td>' + grandM + '</td><td>' + grandF + '</td><td>' + grandNR + '</td><td>' + grandT + '</td><td>100%</td></tr>';
        html += '</tbody></table>';
        el.innerHTML = html;
    }

    // --- Rendu : Tableau de croisement générique ---
    function renderCross(containerId, crossData, rowOrder) {
        var el = document.getElementById(containerId);
        if (!el) return;
        var matrix = crossData.matrix;
        var columns = crossData.columns;
        var rows = rowOrder || Object.keys(matrix);

        if (!rows.length || !columns.length) { el.innerHTML = '<div class="stat-empty">Aucune donnée disponible</div>'; return; }

        var html = '<table class="stat-table"><thead><tr><th></th>';
        columns.forEach(function(c) { html += '<th>' + c + '</th>'; });
        html += '<th>Total</th></tr></thead><tbody>';

        var grandTotal = 0;
        var colTotals = {};
        columns.forEach(function(c) { colTotals[c] = 0; });

        rows.forEach(function(r) {
            html += '<tr><td>' + r + '</td>';
            var rowTotal = 0;
            columns.forEach(function(c) {
                var val = (matrix[r] && matrix[r][c]) ? matrix[r][c] : 0;
                html += '<td>' + (val || '—') + '</td>';
                rowTotal += val;
                colTotals[c] += val;
            });
            html += '<td><b>' + (rowTotal || '—') + '</b></td></tr>';
            grandTotal += rowTotal;
        });

        html += '<tr class="total-row"><td>TOTAL</td>';
        columns.forEach(function(c) { html += '<td>' + (colTotals[c] || '—') + '</td>'; });
        html += '<td>' + grandTotal + '</td></tr>';
        html += '</tbody></table>';
        el.innerHTML = html;
    }

    // --- Croisement spécial : Traitement médical × Complications ---
    function buildCrossTraitComp(items) {
        var matrix = {};
        var colSet = {};
        var labels = { antalgique: 'Antalgique', antibiotique: 'Antibiotique', ains: 'AINS', bain_bouche: 'Bain de bouche' };
        items.forEach(function(item) {
            if (!item.traitement || typeof item.traitement !== 'object') return;
            var comps = splitMulti(item.complications);
            Object.keys(labels).forEach(function(key) {
                var val = item.traitement[key];
                if (val && val.trim()) {
                    var label = labels[key];
                    if (!matrix[label]) matrix[label] = {};
                    comps.forEach(function(c) {
                        if (!c || c === '-') return;
                        colSet[c] = true;
                        matrix[label][c] = (matrix[label][c] || 0) + 1;
                    });
                }
            });
        });
        return { matrix: matrix, columns: Object.keys(colSet) };
    }

    // --- Fonction principale : calculer et rendre les 12 tableaux ---
    function renderAllStatistics(items) {
        var N = items.length;

        // ===== 1. FRÉQUENCE =====

        // Tableau I : Âge × Sexe
        renderAgeSexFreq('stat-age-sexe', items, N);

        // Tableau II : Motif de consultation
        renderFreq('stat-motif', countFreq(items, function(i) { return splitMulti(i.motif); }), N, true);

        // Tableau III : Indication de l'extraction
        renderFreq('stat-indication', countFreq(items, function(i) { return splitMulti(i.indication); }), N, true);

        // Tableau IV : Type d'extraction
        renderFreq('stat-type', countFreq(items, function(i) { return (i.type && i.type !== '-') ? i.type : null; }), N, true);

        // Tableau V : Complications
        renderFreq('stat-complications', countFreq(items, function(i) { return splitMulti(i.complications); }), N, true);

        // ===== 2. CROISEMENT =====

        // Filtrer les tranches d'âge présentes dans les données
        function activeAgeRows(crossObj) {
            return AGE_GROUPS.filter(function(g) { return crossObj.matrix[g]; });
        }

        // Tableau VI : Âge × Indications
        var c1 = buildCross(items, function(i) { return [getAgeGroup(i.age)]; }, function(i) { return splitMulti(i.indication); });
        renderCross('cross-age-indication', c1, activeAgeRows(c1));

        // Tableau VII : Âge × Sexe
        var c2 = buildCross(items, function(i) { return [getAgeGroup(i.age)]; }, function(i) { return [i.sexe]; });
        var c2rows = activeAgeRows(c2);
        if (c2.matrix['Non renseigné']) c2rows.push('Non renseigné');
        renderCross('cross-age-sexe', c2, c2rows);

        // Tableau VIII : Indications × Type d'extraction
        var c3 = buildCross(items, function(i) { return splitMulti(i.indication); }, function(i) { return [i.type]; });
        renderCross('cross-indication-type', c3);

        // Tableau IX : Dent concernée × Indications
        var c4 = buildCross(items, function(i) { return i.dent ? [i.dent] : []; }, function(i) { return splitMulti(i.indication); });
        renderCross('cross-dent-indication', c4);

        // Tableau X : Type d'extraction × Complications
        var c5 = buildCross(items, function(i) { return [i.type]; }, function(i) { return splitMulti(i.complications); });
        renderCross('cross-type-complication', c5);

        // Tableau XI : Examen complémentaire × Type d'extraction
        var c6 = buildCross(items, function(i) { return splitMulti(i.examens); }, function(i) { return [i.type]; });
        renderCross('cross-examen-type', c6);

        // Tableau XII : Traitement médical × Complications
        var c7 = buildCrossTraitComp(items);
        renderCross('cross-traitement-complication', c7);
    }
</script>
</body>
</html>