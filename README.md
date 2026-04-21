<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DentStat Makala - Plateforme de Recherche Extractions Dentaires</title>
    <meta name="description" content="Plateforme de collecte de données cliniques pour la recherche sur les extractions dentaires à l'HGR Makala.">
    
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

        /* HEADER PREMIUM */
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
        
        /* NAVIGATION */
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

        /* CONTENT AREA */
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

        /* SECTIONS */
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
        
        /* FORM GRID */
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

        /* PILLS & RADIOS */
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

        /* BUTTONS */
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

        /* TABLES */
        .data-table-container { 
            overflow-x: auto; 
            border-radius: var(--radius-md); 
            border: 1px solid var(--border); 
            background: #fff;
            margin-top: 20px;
            box-shadow: var(--shadow-sm);
            margin-bottom: 40px;
        }
        table { width: 100%; border-collapse: collapse; min-width: 600px; }
        th { background: #f8fafc; padding: 18px; text-align: left; font-size: 11px; font-weight: 800; color: var(--text-sec); text-transform: uppercase; border-bottom: 2px solid var(--border); letter-spacing: 1px; }
        td { padding: 15px 18px; border-bottom: 1px solid var(--border); font-size: 14px; }
        tr:nth-child(even) { background-color: #fcfdfe; }
        .row-total { font-weight: bold; background: var(--primary-light) !important; }

        /* MODALS */
        .modal { display: none; position: fixed; z-index: 2000; left: 0; top: 0; width: 100%; height: 100%; background: rgba(15, 23, 42, 0.7); backdrop-filter: blur(10px); padding: 20px; }
        .modal-content { background: #fff; margin: 5vh auto; padding: 50px; border-radius: var(--radius-lg); width: 100%; max-width: 1000px; max-height: 85vh; overflow-y: auto; box-shadow: var(--shadow-lg); position: relative; }
        .close-btn { font-size: 40px; cursor: pointer; color: var(--text-sec); transition: var(--transition); }
        .close-btn:hover { color: var(--primary); transform: rotate(90deg); }
        
        .detail-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 24px; }
        .detail-item { padding: 20px; background: #f8fafc; border-radius: var(--radius-sm); border-left: 6px solid var(--primary); transition: var(--transition); }
        .detail-item:hover { transform: translateX(5px); background: #fff; box-shadow: var(--shadow-sm); }

        /* INDICATORS */
        #sync-indicator { font-size: 12px; font-weight: 800; color: var(--primary); display: flex; align-items: center; gap: 12px; margin-bottom: 25px; background: var(--primary-light); padding: 12px 24px; border-radius: 40px; width: fit-content; border: 1px solid var(--primary); }
        .pulse { width: 12px; height: 12px; background: var(--primary); border-radius: 50%; animation: pulse 2s infinite; }
        @keyframes pulse { 0% { transform: scale(0.8); opacity: 1; box-shadow: 0 0 0 0 rgba(0, 137, 123, 0.7); } 70% { transform: scale(1.2); opacity: 0; box-shadow: 0 0 0 10px rgba(0, 137, 123, 0); } 100% { transform: scale(0.8); opacity: 1; box-shadow: 0 0 0 0 rgba(0, 137, 123, 0); } }

        /* TOAST */
        .toast { position: fixed; bottom: 40px; left: 50%; transform: translateX(-50%) translateY(100px); background: #fff; color: var(--text-main); padding: 16px 32px; border-radius: 40px; box-shadow: var(--shadow-lg); z-index: 3000; display: flex; align-items: center; gap: 12px; font-weight: 700; transition: var(--transition); border: 2px solid var(--primary); pointer-events: none; opacity: 0; }
        .toast.show { transform: translateX(-50%) translateY(0); opacity: 1; }

        /* BACK TO TOP */
        #backToTop { display:none; position:fixed; bottom:30px; right:30px; width:50px; height:50px; border-radius:50%; background:var(--primary); color:white; border:none; box-shadow:var(--shadow-lg); cursor:pointer; z-index:1000; font-size:20px; transition:var(--transition); }
        #backToTop:hover { transform: scale(1.1); background: var(--primary-dark); }

        /* STATS SPECIFIC */
        .stat-card-title { font-size: 14px; font-weight: 800; color: var(--primary-dark); margin: 30px 0 15px 0; display: flex; align-items: center; gap: 10px; }
        .stat-card-title i { font-style: normal; }

        /* MOBILE OPTIMIZATION */
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

    <section id="tab-3" class="content-area">
        <div class="section-title"><span>📈 ANALYSE STATISTIQUE</span></div>
        
        <div class="field-grid" style="grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); margin-bottom: 40px;">
            <div class="detail-item" style="text-align:center;">
                <small>Échantillon (N)</small>
                <div id="stat-total" style="font-size:32px; font-weight:800; color:var(--primary); margin-top:10px;">0</div>
            </div>
            <div class="detail-item" style="text-align:center;">
                <small>Âge Moyen</small>
                <div id="stat-age" style="font-size:32px; font-weight:800; color:var(--accent); margin-top:10px;">0</div>
            </div>
            <div class="detail-item" style="text-align:center;">
                <small>Sexe Ratio (M/F)</small>
                <div id="stat-ratio" style="font-size:32px; font-weight:800; color:var(--primary-dark); margin-top:10px;">0</div>
            </div>
        </div>

        <div class="section-title"><span>1. ANALYSE DE FRÉQUENCE</span></div>
        
        <div class="stat-card-title"><i>📊</i> Répartition par Âge et Sexe</div>
        <div class="data-table-container"><table id="table-age-sexe"></table></div>

        <div class="stat-card-title"><i>📊</i> Motif de Consultation</div>
        <div class="data-table-container"><table id="table-motif"></table></div>

        <div class="stat-card-title"><i>📊</i> Indication de l'extraction</div>
        <div class="data-table-container"><table id="table-indication"></table></div>

        <div class="stat-card-title"><i>📊</i> Type d'extraction</div>
        <div class="data-table-container"><table id="table-type-extraction"></table></div>

        <div class="stat-card-title"><i>📊</i> Complications</div>
        <div class="data-table-container"><table id="table-complications"></table></div>

        <div class="section-title" style="margin-top:60px;"><span>2. ANALYSE DE CROISEMENT (Bivariée)</span></div>

        <div class="stat-card-title"><i>🔄</i> Âge + Indications</div>
        <div class="data-table-container"><table id="cross-age-indication"></table></div>

        <div class="stat-card-title"><i>🔄</i> Indications + Type d'extraction</div>
        <div class="data-table-container"><table id="cross-indication-type"></table></div>

        <div class="stat-card-title"><i>🔄</i> Dent concernée + Indications</div>
        <div class="data-table-container"><table id="cross-dent-indication"></table></div>

        <div class="stat-card-title"><i>🔄</i> Type d'extraction + Complications</div>
        <div class="data-table-container"><table id="cross-type-complication"></table></div>

        <div class="stat-card-title"><i>🔄</i> Examen complémentaire + Type d'extraction</div>
        <div class="data-table-container"><table id="cross-exam-type"></table></div>

        <div class="stat-card-title"><i>🔄</i> Traitement médical + Complications</div>
        <div class="data-table-container"><table id="cross-traitement-complication"></table></div>
    </section>

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
            <button class="check-pill" style="justify-content:center;" onclick="window.print()">🖨️ Imprimer le rapport</button>
            <button class="check-pill" style="justify-content:center;" onclick="exportToCSV()">📥 Exporter la Matrice (CSV)</button>
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
            age: parseInt(document.getElementById('age').value) || 0,
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
            generateAllTables(items); // Nouvelle fonction stats
            items.reverse().forEach(d => addRow(d));
        });
    }

    // --- LOGIQUE STATISTIQUE AVANCÉE ---

    function generateAllTables(items) {
        if(!items.length) return;

        // Fréquences
        renderFrequencyTable('table-age-sexe', items, 'age_groups', 'sexe');
        renderSimpleFrequency('table-motif', items, 'motif');
        renderSimpleFrequency('table-indication', items, 'indication');
        renderSimpleFrequency('table-type-extraction', items, 'type');
        renderSimpleFrequency('table-complications', items, 'complications');

        // Croisements
        renderCrossTable('cross-age-indication', items, 'age_groups', 'indication');
        renderCrossTable('cross-indication-type', items, 'indication', 'type');
        renderCrossTable('cross-dent-indication', items, 'dent', 'indication');
        renderCrossTable('cross-type-complication', items, 'type', 'complications');
        renderCrossTable('cross-exam-type', items, 'examens', 'type');
        renderCrossTable('cross-traitement-complication', items, 'traitement_summary', 'complications');
    }

    function getAgeGroup(age) {
        if(age < 15) return "0-14 ans";
        if(age < 30) return "15-29 ans";
        if(age < 45) return "30-44 ans";
        if(age < 60) return "45-59 ans";
        return "60+ ans";
    }

    function renderSimpleFrequency(tableId, items, field) {
        const counts = {};
        items.forEach(item => {
            const val = item[field] || 'Non spécifié';
            val.split(', ').forEach(v => {
                counts[v] = (counts[v] || 0) + 1;
            });
        });

        let html = `<thead><tr><th>${field.toUpperCase()}</th><th>Fréquence (n)</th><th>Pourcentage (%)</th></tr></thead><tbody>`;
        const total = Object.values(counts).reduce((a, b) => a + b, 0);
        
        Object.entries(counts).sort((a,b) => b[1] - a[1]).forEach(([label, count]) => {
            const pct = ((count / items.length) * 100).toFixed(1);
            html += `<tr><td>${label}</td><td>${count}</td><td>${pct}%</td></tr>`;
        });
        html += `<tr class="row-total"><td>TOTAL (N=${items.length})</td><td>-</td><td>100%</td></tr></tbody>`;
        document.getElementById(tableId).innerHTML = html;
    }

    function renderCrossTable(tableId, items, rowField, colField) {
        const rows = new Set();
        const cols = new Set();
        const data = {};

        items.forEach(item => {
            let rVal = (rowField === 'age_groups') ? getAgeGroup(item.age) : (item[rowField] || '-');
            let cVal = item[colField] || '-';
            
            if(rowField === 'traitement_summary') {
                rVal = Object.values(item.traitement || {}).some(v => v) ? "Avec Traitement" : "Sans Traitement";
            }

            // Gérer les choix multiples
            const rList = rVal.split(', ');
            const cList = cVal.split(', ');

            rList.forEach(r => {
                rows.add(r);
                cList.forEach(c => {
                    cols.add(c);
                    const key = `${r}||${c}`;
                    data[key] = (data[key] || 0) + 1;
                });
            });
        });

        const sortedCols = Array.from(cols).sort();
        let html = `<thead><tr><th>${rowField} / ${colField}</th>`;
        sortedCols.forEach(c => html += `<th>${c}</th>`);
        html += `<th>Total</th></tr></thead><tbody>`;

        Array.from(rows).sort().forEach(r => {
            let rowSum = 0;
            html += `<tr><td><b>${r}</b></td>`;
            sortedCols.forEach(c => {
                const count = data[`${r}||${c}`] || 0;
                html += `<td>${count}</td>`;
                rowSum += count;
            });
            html += `<td style="background:#f0f4f8"><b>${rowSum}</b></td></tr>`;
        });
        html += `</tbody>`;
        document.getElementById(tableId).innerHTML = html;
    }

    function renderFrequencyTable(tableId, items, rowField, colField) {
        renderCrossTable(tableId, items, rowField, colField);
    }

    // --- FONCTIONS EXISTANTES ---

    function updateStats(items) {
        if(!items.length) return;
        document.getElementById('stat-total').innerText = items.length;
        const validAges = items.map(i => parseInt(i.age)).filter(a => !isNaN(a));
        const avg = validAges.length ? (validAges.reduce((a,b) => a+b, 0) / validAges.length) : 0;
        document.getElementById('stat-age').innerText = Math.round(avg) + " ans";
        const m = items.filter(i => i.sexe === 'Masculin').length;
        const f = items.filter(i => i.sexe === 'Féminin').length;
        document.getElementById('stat-ratio').innerText = f > 0 ? (m/f).toFixed(1) : m;
    }

    function addRow(d) {
        const body = document.getElementById('matrix-body');
        const row = document.createElement('tr');
        const safeData = btoa(unescape(encodeURIComponent(JSON.stringify(d))));
        row.innerHTML = `
            <td><b style="color:var(--primary)">#${d.fiche}</b></td>
            <td>${d.age} ans</td>
            <td>${d.sexe}</td>
            <td>${d.type}</td>
            <td>
                <div style="display:flex; gap:10px;">
                    <button onclick="viewDetails('${safeData}')" style="cursor:pointer; border:1px solid var(--border); padding:5px 10px; border-radius:8px;">👁️</button>
                    <button onclick="deleteEntry('${d.key}')" style="cursor:pointer; border:1px solid #ff4444; color:#ff4444; padding:5px 10px; border-radius:8px; background:transparent;">🗑️</button>
                </div>
            </td>
        `;
        body.appendChild(row);
    }

    window.deleteEntry = function(key) {
        if(confirm("Confirmer la suppression de cette fiche ?")) {
            db.ref('extractions').child(key).remove().then(() => alert("Supprimé."));
        }
    }

    window.viewDetails = function(encoded) {
        const d = JSON.parse(decodeURIComponent(escape(atob(encoded))));
        const body = document.getElementById('modalBody');
        body.innerHTML = "";
        const labels = {fiche:"N° Fiche",enqueteur:"Enquêteur",age:"Âge",sexe:"Sexe",motif:"Motif",dent:"Dent",indication:"Indication",type:"Type",complications:"Complications"};
        Object.keys(labels).forEach(k => {
            if(d[k]) body.innerHTML += `<div class="detail-item"><small>${labels[k]}</small><div>${d[k]}</div></div>`;
        });
        document.getElementById('detailModal').style.display = "block";
    }

    window.closeModal = function() { document.getElementById('detailModal').style.display = "none"; }

    function exportToCSV() {
        db.ref('extractions').once('value', snap => {
            let csv = "Fiche,Age,Sexe,Motif,Dent,Indication,Type,Complications\n";
            snap.forEach(c => {
                const d = c.val();
                csv += `"${d.fiche}","${d.age}","${d.sexe}","${d.motif}","${d.dent}","${d.indication}","${d.type}","${d.complications}"\n`;
            });
            const blob = new Blob([csv], { type: 'text/csv' });
            const url = window.URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.setAttribute('href', url);
            a.setAttribute('download', 'Donnees_Makala_Extractions.csv');
            a.click();
        });
    }

    window.onscroll = () => {
        document.getElementById('backToTop').style.display = (window.scrollY > 500) ? "block" : "none";
    };
    document.getElementById('backToTop').onclick = () => window.scrollTo({top:0, behavior:'smooth'});
</script>
</body>
</html>
