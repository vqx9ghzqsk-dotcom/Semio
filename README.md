<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tableau de Bord - Enquête Cancer du Sein</title>
    <!-- Tailwind CSS pour le style -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- SheetJS pour l'export Excel -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <style>
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        .form-section { margin-bottom: 2rem; padding: 1.5rem; background: #f9fafb; border-radius: 0.5rem; border: 1px solid #e5e7eb; }
        .form-section h3 { font-size: 1.25rem; font-weight: 600; color: #1f2937; margin-bottom: 1rem; border-bottom: 2px solid #3b82f6; padding-bottom: 0.5rem; }
        label { display: block; font-weight: 500; margin-top: 0.75rem; color: #374151; }
        input[type="text"], input[type="number"], select, textarea { width: 100%; padding: 0.5rem; margin-top: 0.25rem; border: 1px solid #d1d5db; border-radius: 0.375rem; }
        .radio-group, .checkbox-group { margin-top: 0.5rem; margin-bottom: 0.5rem; }
        .radio-group label, .checkbox-group label { display: inline-flex; align-items: center; font-weight: normal; margin-right: 1rem; margin-top: 0; }
        .radio-group input, .checkbox-group input { margin-right: 0.5rem; }
    </style>
</head>
<body class="bg-gray-100 font-sans">

    <!-- Navigation -->
    <nav class="bg-blue-600 text-white p-4 shadow-md">
        <div class="container mx-auto flex justify-between items-center">
            <h1 class="text-xl font-bold">Plateforme de Collecte - CAP Cancer du Sein</h1>
            <div>
                <button onclick="switchTab('collecte')" class="px-4 py-2 bg-blue-700 hover:bg-blue-800 rounded mr-2 focus:outline-none focus:ring-2 focus:ring-white">Fiche de Collecte</button>
                <button onclick="switchTab('matrice')" class="px-4 py-2 bg-blue-700 hover:bg-blue-800 rounded focus:outline-none focus:ring-2 focus:ring-white">Matrice (Données)</button>
            </div>
        </div>
    </nav>

    <div class="container mx-auto p-4 mt-4 bg-white rounded-lg shadow-lg">
        
        <!-- ONGLET 1: COLLECTE -->
        <div id="collecte" class="tab-content active">
            <h2 class="text-2xl font-bold mb-6 text-center text-gray-800">FICHE DE COLLECTE</h2>
            
            <form id="dataForm" class="space-y-6">
                <!-- Consentement -->
                <div class="form-section">
                    <h3>Consentement</h3>
                    <p class="mb-4 text-sm text-gray-600">Nous réalisons une enquête sur la connaissance, attitudes et pratiques des personnels soignants sur le cancer du sein. Votre participation est volontaire. Les données sont anonymes.</p>
                    <label>J’accepte de participer :</label>
                    <div class="radio-group">
                        <label><input type="radio" name="consentement" value="Oui" required> Oui</label>
                        <label><input type="radio" name="consentement" value="Non"> Non</label>
                    </div>
                    <label>Signature (Nom/Initiales) :</label>
                    <input type="text" name="signature" required>
                </div>

                <!-- Données Administratives -->
                <div class="form-section">
                    <h3>DONNÉES ADMINISTRATIVES</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div>
                            <label>Numéro d’identification du questionnaire :</label>
                            <input type="text" name="num_id" required>
                        </div>
                        <div>
                            <label>Age :</label>
                            <input type="number" name="age" required>
                        </div>
                        <div>
                            <label>Sexe :</label>
                            <div class="radio-group">
                                <label><input type="radio" name="sexe" value="M" required> M</label>
                                <label><input type="radio" name="sexe" value="F"> F</label>
                            </div>
                        </div>
                        <div>
                            <label>Province :</label>
                            <input type="text" name="province" required>
                        </div>
                        <div>
                            <label>Ethnie :</label>
                            <input type="text" name="ethnie" required>
                        </div>
                        <div>
                            <label>Statut matrimonial :</label>
                            <select name="statut_matrimonial" required>
                                <option value="">Sélectionnez</option>
                                <option value="Célibataire">Célibataire</option>
                                <option value="Marié(e)">Marié(e)</option>
                                <option value="Veuf(ve)">Veuf(ve)</option>
                                <option value="Divorcé(e)">Divorcé(e)</option>
                            </select>
                        </div>
                        <div>
                            <label>Niveau d’étude / diplôme professionnel :</label>
                            <input type="text" name="niveau_etude" required>
                        </div>
                        <div>
                            <label>Service :</label>
                            <input type="text" name="service" required>
                        </div>
                    </div>

                    <label class="mt-4">Statut professionnel :</label>
                    <div class="radio-group grid grid-cols-1 md:grid-cols-2 gap-2">
                        <label><input type="radio" name="statut_pro" value="Aide soignant(e)/infirmier(ère) assistant(e)" required> Aide soignant(e)/infirmier(ère) assistant(e)</label>
                        <label><input type="radio" name="statut_pro" value="Infirmier(ère) diplômé(e) d’état"> Infirmier(ère) diplômé(e) d’état</label>
                        <label><input type="radio" name="statut_pro" value="Infirmier(ère) spécialisé(e)/licence/bachelor"> Infirmier(ère) spécialisé(e)/licence/bachelor</label>
                        <label><input type="radio" name="statut_pro" value="Master/autres"> Master / autres</label>
                        <label><input type="radio" name="statut_pro" value="Médecin généraliste"> Médecin généraliste</label>
                        <label><input type="radio" name="statut_pro" value="Médecin spécialiste"> Médecin spécialiste</label>
                        <label><input type="radio" name="statut_pro" value="Sage femme"> Sage femme</label>
                        <label><input type="radio" name="statut_pro" value="Médecin stagiaire"> Médecin stagiaire</label>
                        <label><input type="radio" name="statut_pro" value="Stagiaire en médecine"> Stagiaire en médecine</label>
                        <label><input type="radio" name="statut_pro" value="Autres"> Autres (préciser) : <input type="text" name="statut_pro_autre" class="ml-2 w-32 border-b border-gray-400 outline-none"></label>
                    </div>

                    <div class="grid grid-cols-1 gap-4 mt-4">
                        <div>
                            <label>Nombre d’année au service :</label>
                            <input type="number" name="annees_service" required>
                        </div>
                        <div>
                            <label>Avez-vous déjà reçu une formation spécifique sur le cancer du sein ?</label>
                            <div class="radio-group">
                                <label><input type="radio" name="formation" value="Oui" required> Oui. Année : <input type="number" name="formation_annee" class="ml-2 w-24"></label>
                                <label><input type="radio" name="formation" value="Non"> Non</label>
                            </div>
                        </div>
                        <div>
                            <label>Avez-vous accès à des supports d’information (protocoles, brochures, livre, syllabus) sur le cancer du sein dans votre lieu de travail ?</label>
                            <div class="radio-group">
                                <label><input type="radio" name="supports" value="Oui" required> Oui</label>
                                <label><input type="radio" name="supports" value="Non"> Non</label>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- II. Connaissance -->
                <div class="form-section">
                    <h3>II. CONNAISSANCE DU CANCER DU SEIN</h3>
                    
                    <label>Q1. Comment définissez-vous le cancer du sein ?</label>
                    <div class="radio-group flex flex-col">
                        <label><input type="radio" name="q1" value="Une multiplication anarchique, incontrôlée des cellules au dépens de la glande mammaire." required> Une multiplication anarchique, incontrôlée des cellules au dépens de la glande mammaire.</label>
                        <label><input type="radio" name="q1" value="Comme une maladie caractérisée par l’apparition des nodules puis abcès mammaires"> Comme une maladie caractérisée par l’apparition des nodules puis abcès mammaires</label>
                        <label><input type="radio" name="q1" value="Maladie caractérisée par la croissance incontrôlée des cellules mammaires anormales qui forment alors une tumeur"> Maladie caractérisée par la croissance incontrôlée des cellules mammaires anormales qui forment alors une tumeur</label>
                        <label><input type="radio" name="q1" value="Pathologie mammaire d’origine inconnue caractérisée par la présence des cellules cancéreuses qui se répandent dans tout l’organisme"> Pathologie mammaire d’origine inconnue caractérisée par la présence des cellules cancéreuses qui se répandent dans tout l’organisme</label>
                        <label><input type="radio" name="q1" value="Autres"> Autres : <input type="text" name="q1_autre" class="ml-2 w-64"></label>
                    </div>

                    <label>Q2. Le cancer du sein est le plus fréquent chez la femme :</label>
                    <div class="radio-group">
                        <label><input type="radio" name="q2_frequent" value="Vrai" required> Vrai</label>
                        <label><input type="radio" name="q2_frequent" value="Faux"> Faux</label>
                    </div>

                    <label>Q2 (bis). Quelle est l’incidence du cancer du sein ? Cocher la bonne réponse</label>
                    <div class="radio-group flex flex-col">
                        <label><input type="radio" name="q2_incidence" value="1 – 1,5 millions" required> 1 – 1,5 millions</label>
                        <label><input type="radio" name="q2_incidence" value="1,5 millions - 2 millions"> 1,5 millions - 2 millions</label>
                        <label><input type="radio" name="q2_incidence" value="Plus de 2 millions"> Plus de 2 millions</label>
                    </div>

                    <label>Q3. Peut-on guérir du cancer du sein ? Cocher une réponse</label>
                    <div class="radio-group">
                        <label><input type="radio" name="q3" value="Oui" required> Oui</label>
                        <label><input type="radio" name="q3" value="Non"> Non</label>
                    </div>

                    <label>Q4. Quel est le pronostic du cancer du sein :</label>
                    <div class="ml-4">
                        <label class="font-semibold text-sm">Avec traitement ?</label>
                        <div class="radio-group">
                            <label><input type="radio" name="q4_avec" value="Bon"> Bon</label>
                            <label><input type="radio" name="q4_avec" value="Bon mais avec risque de récidive"> Bon mais avec risque de récidive</label>
                        </div>
                        <label class="font-semibold text-sm">Sans traitement ?</label>
                        <div class="radio-group">
                            <label><input type="radio" name="q4_sans" value="Mauvais"> Mauvais</label>
                            <label><input type="radio" name="q4_sans" value="Peut régresser"> Peut régresser</label>
                        </div>
                    </div>

                    <label>Q5. Quels sont les moyens préventifs contre le cancer du sein que vous connaissez ?</label>
                    <textarea name="q5" rows="3" required></textarea>

                    <label>Q6. Quels sont les facteurs de risque du cancer du sein que vous connaissez ?</label>
                    <div class="checkbox-group flex flex-col">
                        <label><input type="checkbox" name="q6" value="Antécédent familial"> Antécédent familial du cancer du sein</label>
                        <label><input type="checkbox" name="q6" value="Antécédent personnel"> Antécédent personnel du cancer du sein</label>
                        <label><input type="checkbox" name="q6" value="Tabagisme"> Tabagisme</label>
                        <label><input type="checkbox" name="q6" value="Consommation excessive d’alcool"> Consommation excessive d’alcool</label>
                        <label><input type="checkbox" name="q6" value="Grossesse précoce"> Grossesse précoce</label>
                        <label><input type="checkbox" name="q6" value="Ménopause tardive"> Ménopause tardive</label>
                        <label><input type="checkbox" name="q6" value="Obésité après ménopause"> Obésité après ménopause</label>
                        <label><input type="checkbox" name="q6" value="Aucun"> Aucun de ces facteurs</label>
                    </div>

                    <label>Q7. Quels sont les signes cliniques du cancer du sein que vous connaissez ? (Plusieurs réponses possibles)</label>
                    <div class="checkbox-group flex flex-col">
                        <label><input type="checkbox" name="q7" value="Masse/bosse dans le sein"> Masse/bosse dans le sein</label>
                        <label><input type="checkbox" name="q7" value="Écoulement mammelonnaire sanglant"> Écoulement mammelonnaire sanglant</label>
                        <label><input type="checkbox" name="q7" value="Rétraction du mamelon"> Rétraction du mamelon</label>
                        <label><input type="checkbox" name="q7" value="Rougeur et chaleur"> Rougeur et chaleur</label>
                        <label><input type="checkbox" name="q7" value="Douleur mammaire isolée"> Douleur mammaire isolée</label>
                        <label><input type="checkbox" name="q7" value="Autres"> Autres signes que vous connaissez : <input type="text" name="q7_autre" class="ml-2 w-64"></label>
                    </div>

                    <label>Q8. Quel est le moyen de diagnostic le plus fiable du cancer du sein que vous connaissez ?</label>
                    <div class="radio-group flex flex-col">
                        <label><input type="radio" name="q8" value="L’échographie mammaire" required> L’échographie mammaire</label>
                        <label><input type="radio" name="q8" value="Mammographie"> Mammographie</label>
                        <label><input type="radio" name="q8" value="Biopsie (examen anapath)"> Biopsie (examen anapath)</label>
                        <label><input type="radio" name="q8" value="Examen clinique seul"> Examen clinique seul</label>
                        <label><input type="radio" name="q8" value="Ne sait pas"> Ne sait pas</label>
                    </div>

                    <label>Q9. Quels sont les moyens thérapeutiques que vous connaissez ?</label>
                    <div class="checkbox-group flex flex-col">
                        <label><input type="checkbox" name="q9" value="Chirurgie"> Chirurgie (mastectomie ou tumorectomie)</label>
                        <label><input type="checkbox" name="q9" value="Chimiothérapie"> Chimiothérapie</label>
                        <label><input type="checkbox" name="q9" value="Radiothérapie"> Radiothérapie</label>
                        <label><input type="checkbox" name="q9" value="Hormonothérapie"> Hormonothérapie</label>
                        <label><input type="checkbox" name="q9" value="Médecine traditionnelle exclusivement"> Médecine traditionnelle exclusivement</label>
                        <label><input type="checkbox" name="q9" value="Ne sait pas"> Ne sait pas</label>
                    </div>
                </div>

                <!-- III. Attitude -->
                <div class="form-section">
                    <h3>III. ATTITUDE FACE AU CANCER DU SEIN</h3>
                    
                    <label>Q10. Quel est votre point de vue par rapport à la prévention contre le cancer du sein (pensez-vous que ça améliore son pronostic) ?</label>
                    <div class="radio-group flex flex-col">
                        <label><input type="radio" name="q10" value="Fortement en désaccord" required> Fortement en désaccord</label>
                        <label><input type="radio" name="q10" value="En désaccord"> En désaccord</label>
                        <label><input type="radio" name="q10" value="Neutre"> Neutre</label>
                        <label><input type="radio" name="q10" value="D’accord"> D’accord</label>
                        <label><input type="radio" name="q10" value="Fortement d’accord"> Fortement d’accord</label>
                    </div>

                    <label>Q11. Quel est votre point de vue par rapport au moyen de diagnostic du cancer du sein ? Pensez-vous que :</label>
                    <div class="radio-group flex flex-col">
                        <label><input type="radio" name="q11" value="C’est coûteux" required> C’est coûteux</label>
                        <label><input type="radio" name="q11" value="Inaccessible"> Inaccessible</label>
                        <label><input type="radio" name="q11" value="Pas utile"> Pas utile puisque ça fait perdre de l’argent et du temps au malade qui devait déjà commencer le traitement dès qu’il y a une forte présomption</label>
                        <label><input type="radio" name="q11" value="Très important"> Très important puisqu’il guide la prise en charge</label>
                    </div>

                    <label>Q12. Jugez la prise en charge du cancer du sein à l’HMC ?</label>
                    <textarea name="q12" rows="3" required></textarea>

                    <label>Q13. Quelle analyse faites-vous de la politique de lutte contre le cancer du sein en RDC ?</label>
                    <textarea name="q13" rows="3" required></textarea>
                </div>

                <!-- IV. Pratique -->
                <div class="form-section">
                    <h3>IV. PRATIQUE</h3>
                    
                    <label>Q14. Est-ce que vous examinez le sein lors de vos consultations ?</label>
                    <div class="radio-group">
                        <label><input type="radio" name="q14" value="Oui" required> Oui. Si oui quelle doit être la fréquence ? <input type="text" name="q14_frequence" class="ml-2 w-32"></label>
                        <label><input type="radio" name="q14" value="Non"> Non</label>
                    </div>

                    <label>Q15. La palpation mammaire systématique (auto-examen) permet de prévenir efficacement tous les cancers du sein ?</label>
                    <div class="radio-group">
                        <label><input type="radio" name="q15" value="Vrai" required> Vrai</label>
                        <label><input type="radio" name="q15" value="Faux"> Faux</label>
                        <label><input type="radio" name="q15" value="Ne sait pas"> Ne sait pas</label>
                    </div>

                    <label>Q16. Est-ce que vous conseillez l’autopalpation du sein aux femmes lors de vos consultations ?</label>
                    <div class="radio-group">
                        <label><input type="radio" name="q16" value="Oui" required> Oui. À quelle fréquence ? <input type="text" name="q16_frequence" class="ml-2 w-32"></label>
                        <label><input type="radio" name="q16" value="Non"> Non</label>
                    </div>

                    <label>Q17. Quel est l’âge d’après vous recommandé pour commencer le dépistage systématique pour la mammographie dans les pays à ressources limitées ?</label>
                    <div class="radio-group flex flex-col">
                        <label><input type="radio" name="q17" value="25 ans - 30 ans" required> 25 ans - 30 ans</label>
                        <label><input type="radio" name="q17" value="40 - 45 ans"> 40 - 45 ans</label>
                        <label><input type="radio" name="q17" value="50 ans"> 50 ans</label>
                        <label><input type="radio" name="q17" value="74 ans"> 74 ans</label>
                    </div>

                    <label>Q18. Lequel des examens suivants est le plus fiable pour confirmer le diagnostic du cancer du sein ?</label>
                    <div class="radio-group flex flex-col">
                        <label><input type="radio" name="q18" value="Échographie mammaire" required> a) Échographie mammaire</label>
                        <label><input type="radio" name="q18" value="Mammographie"> b) Mammographie</label>
                        <label><input type="radio" name="q18" value="Biopsie (examen d’anapath)"> c) Biopsie (examen d’anapath)</label>
                        <label><input type="radio" name="q18" value="Examen clinique seul"> d) Examen clinique seul</label>
                        <label><input type="radio" name="q18" value="Ne sait pas"> e) Ne sait pas</label>
                    </div>

                    <label>Q19. Quel est le meilleur traitement ?</label>
                    <div class="radio-group flex flex-col">
                        <label><input type="radio" name="q19" value="Chimiothérapie" required> Chimiothérapie</label>
                        <label><input type="radio" name="q19" value="Radiothérapie"> Radiothérapie</label>
                        <label><input type="radio" name="q19" value="Hormonothérapie"> Hormonothérapie</label>
                        <label><input type="radio" name="q19" value="Chirurgie (mastectomie)"> Chirurgie (mastectomie)</label>
                        <label><input type="radio" name="q19" value="Traitement traditionnel exclusivement"> Traitement traditionnel exclusivement</label>
                        <label><input type="radio" name="q19" value="Ne sait pas"> Ne sait pas</label>
                    </div>

                    <label>Q20. Avez-vous déjà entendu parler de la réunion de concertation multidisciplinaire (RCP) ?</label>
                    <div class="radio-group">
                        <label><input type="radio" name="q20" value="Oui" required> Oui</label>
                        <label><input type="radio" name="q20" value="Non"> Non</label>
                    </div>

                    <label>Q21. Si oui, quels sont les avantages de la RCP ?</label>
                    <textarea name="q21" rows="3"></textarea>

                    <label>Q22. En quelques mots, que faites-vous devant une suspicion de cancer du sein ?</label>
                    <textarea name="q22" rows="3" required></textarea>

                    <label>Q23. En quelques mots, que faites-vous devant un cancer du sein ?</label>
                    <textarea name="q23" rows="3" required></textarea>

                    <label>Q24. En quelques mots, quelles sont les difficultés que vous rencontrez dans la lutte contre le cancer ?</label>
                    <textarea name="q24" rows="3" required></textarea>
                </div>

                <div class="text-center pb-8">
                    <button type="submit" class="bg-green-600 text-white font-bold py-3 px-8 rounded hover:bg-green-700 focus:outline-none focus:ring-4 focus:ring-green-300 transition duration-300">Enregistrer et Soumettre la fiche</button>
                </div>
            </form>
        </div>

        <!-- ONGLET 2: MATRICE -->
        <div id="matrice" class="tab-content">
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-2xl font-bold text-gray-800">MATRICE DES DONNÉES</h2>
                <button id="btnExport" class="bg-green-600 text-white px-4 py-2 rounded shadow hover:bg-green-700 flex items-center">
                    <svg class="w-4 h-4 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 10v6m0 0l-3-3m3 3l3-3m2 8H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"></path></svg>
                    Télécharger Excel
                </button>
            </div>
            
            <div class="overflow-x-auto bg-white border border-gray-200 rounded-lg shadow">
                <table class="min-w-full divide-y divide-gray-200">
                    <thead class="bg-gray-50">
                        <tr>
                            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Date</th>
                            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">ID Fiche</th>
                            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Âge/Sexe</th>
                            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Statut Pro</th>
                            <th class="px-6 py-3 text-center text-xs font-medium text-gray-500 uppercase tracking-wider">Actions</th>
                        </tr>
                    </thead>
                    <tbody id="tableBody" class="bg-white divide-y divide-gray-200">
                        <!-- Lignes générées par JavaScript -->
                    </tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- MODAL POUR VISUALISER UNE FICHE -->
    <div id="viewModal" class="fixed inset-0 bg-gray-600 bg-opacity-50 hidden overflow-y-auto h-full w-full">
        <div class="relative top-10 mx-auto p-5 border w-11/12 md:w-3/4 lg:w-1/2 shadow-lg rounded-md bg-white mb-20">
            <div class="mt-3">
                <div class="flex justify-between items-center border-b pb-3">
                    <h3 class="text-lg leading-6 font-medium text-gray-900" id="modalTitle">Détails de la fiche</h3>
                    <button onclick="closeModal()" class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center">
                        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
                    </button>
                </div>
                <div class="mt-4 text-left" id="modalContent">
                    <!-- Contenu injecté ici -->
                </div>
                <div class="items-center px-4 py-3 mt-4 border-t">
                    <button onclick="closeModal()" class="px-4 py-2 bg-blue-500 text-white text-base font-medium rounded-md w-full shadow-sm hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-300">Fermer</button>
                </div>
            </div>
        </div>
    </div>

    <!-- SCRIPT DE GESTION -->
    <script>
        // Logique pour basculer entre les onglets
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.getElementById(tabId).classList.add('active');
        }

        // Logique du modal
        function closeModal() {
            document.getElementById('viewModal').classList.add('hidden');
        }
    </script>

    <!-- FIREBASE & LOGIQUE DE DONNÉES (Utilisation du CDN Module v10) -->
    <script type="module">
        // Importation des fonctions depuis le CDN Firebase
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getDatabase, ref, push, onValue, remove } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-database.js";

        // Configuration Firebase fournie
        const firebaseConfig = {
            apiKey: "AIzaSyAdPTxLwhR_8AurMVH73PJWyK1f_pkStfU",
            authDomain: "pierrot-1.firebaseapp.com",
            databaseURL: "https://pierrot-1-default-rtdb.firebaseio.com",
            projectId: "pierrot-1",
            storageBucket: "pierrot-1.firebasestorage.app",
            messagingSenderId: "248143479026",
            appId: "1:248143479026:web:38bfd4023b4ad2ba54506b",
            measurementId: "G-3S44GCW8RW"
        };

        // Initialisation
        const app = initializeApp(firebaseConfig);
        const db = getDatabase(app);
        const recordsRef = ref(db, 'fiches_collecte');

        let allRecordsData = [];

        // GESTION DU FORMULAIRE : Enregistrement des données
        document.getElementById('dataForm').addEventListener('submit', async (e) => {
            e.preventDefault();
            const form = e.target;
            const formData = new FormData(form);
            const dataObj = { date_soumission: new Date().toISOString() };

            // Récupération des valeurs (gestion spéciale pour les cases à cocher)
            for (let [key, value] of formData.entries()) {
                if (dataObj[key]) {
                    // Si la clé existe déjà, c'est une checkbox, on transforme en tableau
                    if (!Array.isArray(dataObj[key])) {
                        dataObj[key] = [dataObj[key]];
                    }
                    dataObj[key].push(value);
                } else {
                    dataObj[key] = value;
                }
            }

            try {
                // Envoi vers Firebase
                await push(recordsRef, dataObj);
                alert("Fiche enregistrée avec succès dans la base de données !");
                form.reset();
                switchTab('matrice'); // Redirection vers l'onglet matrice
            } catch (error) {
                console.error("Erreur lors de l'enregistrement : ", error);
                alert("Une erreur est survenue lors de l'enregistrement.");
            }
        });

        // LECTURE EN TEMPS RÉEL (Onglet Matrice)
        onValue(recordsRef, (snapshot) => {
            const data = snapshot.val();
            const tableBody = document.getElementById('tableBody');
            tableBody.innerHTML = '';
            allRecordsData = [];

            if (data) {
                for (let key in data) {
                    const record = data[key];
                    record.id = key; // Sauvegarder l'ID Firebase pour la suppression
                    allRecordsData.push(record);

                    const tr = document.createElement('tr');
                    const dateObj = new Date(record.date_soumission);
                    const dateFr = dateObj.toLocaleDateString('fr-FR') + ' ' + dateObj.toLocaleTimeString('fr-FR');

                    tr.innerHTML = `
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${dateFr}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">${record.num_id || 'N/A'}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${record.age || '-'} ans / ${record.sexe || '-'}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${record.statut_pro || 'Non précisé'}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-center text-sm font-medium">
                            <button class="text-blue-600 hover:text-blue-900 mr-3 btn-view" data-id="${key}">Voir</button>
                            <button class="text-red-600 hover:text-red-900 btn-delete" data-id="${key}">Supprimer</button>
                        </td>
                    `;
                    tableBody.appendChild(tr);
                }
            } else {
                tableBody.innerHTML = '<tr><td colspan="5" class="px-6 py-4 text-center text-gray-500">Aucune donnée trouvée.</td></tr>';
            }
        });

        // GESTION DES CLICS SUR LES BOUTONS DU TABLEAU (Délégation d'événements)
        document.getElementById('tableBody').addEventListener('click', async (e) => {
            if (e.target.classList.contains('btn-delete')) {
                const id = e.target.getAttribute('data-id');
                if (confirm("Êtes-vous sûr de vouloir supprimer cette fiche définitivement ?")) {
                    await remove(ref(db, 'fiches_collecte/' + id));
                }
            }
            
            if (e.target.classList.contains('btn-view')) {
                const id = e.target.getAttribute('data-id');
                const record = allRecordsData.find(r => r.id === id);
                if (record) {
                    afficherDetails(record);
                }
            }
        });

        // FONCTION POUR AFFICHER LES DÉTAILS DANS LE MODAL
        function afficherDetails(record) {
            const modalContent = document.getElementById('modalContent');
            let html = '<ul class="divide-y divide-gray-200">';
            
            // Formatage des clés pour un affichage plus propre
            for (const [key, value] of Object.entries(record)) {
                if(key !== 'id') {
                    const displayValue = Array.isArray(value) ? value.join(', ') : value;
                    html += `
                        <li class="py-2">
                            <span class="font-bold text-gray-700 capitalize">${key.replace(/_/g, ' ')} :</span> 
                            <span class="text-gray-600 ml-2">${displayValue}</span>
                        </li>
                    `;
                }
            }
            html += '</ul>';
            
            document.getElementById('modalTitle').innerText = "Fiche ID : " + (record.num_id || 'Inconnu');
            modalContent.innerHTML = html;
            document.getElementById('viewModal').classList.remove('hidden');
        }

        // EXPORTATION EXCEL
        document.getElementById('btnExport').addEventListener('click', () => {
            if (allRecordsData.length === 0) {
                alert("Aucune donnée à télécharger.");
                return;
            }
            
            // On nettoie les données avant l'export (enlever l'ID interne Firebase)
            const exportData = allRecordsData.map(record => {
                const { id, ...cleanRecord } = record;
                // Transformer les tableaux (checkboxes) en chaînes de caractères
                for (let key in cleanRecord) {
                    if (Array.isArray(cleanRecord[key])) {
                        cleanRecord[key] = cleanRecord[key].join('; ');
                    }
                }
                return cleanRecord;
            });

            const worksheet = XLSX.utils.json_to_sheet(exportData);
            const workbook = XLSX.utils.book_new();
            XLSX.utils.book_append_sheet(workbook, worksheet, "Base_Cancer_Sein");
            XLSX.writeFile(workbook, "Base_De_Donnees_CAP.xlsx");
        });
    </script>
</body>
</html>
