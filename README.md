<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tableau de Bord - Enquête Obstétricale</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <style>
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        .form-section { margin-bottom: 2rem; padding: 1.5rem; background: #f9fafb; border-radius: 0.5rem; border: 1px solid #e5e7eb; }
        .form-section h3 { font-size: 1.25rem; font-weight: 600; color: #1f2937; margin-bottom: 1rem; border-bottom: 2px solid #ec4899; padding-bottom: 0.5rem; }
        label { display: block; font-weight: 500; margin-top: 0.75rem; color: #374151; }
        input[type="text"], input[type="number"], input[type="date"], select, textarea { width: 100%; padding: 0.5rem; margin-top: 0.25rem; border: 1px solid #d1d5db; border-radius: 0.375rem; }
        .radio-group, .checkbox-group { margin-top: 0.5rem; margin-bottom: 0.5rem; }
        .radio-group label, .checkbox-group label { display: inline-flex; align-items: center; font-weight: normal; margin-right: 1rem; margin-top: 0; }
        .radio-group input, .checkbox-group input { margin-right: 0.5rem; }
        /* Changement de couleur thème pour se différencier (Rose maternel) */
        .bg-theme { background-color: #ec4899; }
        .bg-theme-hover:hover { background-color: #db2777; }
        .text-theme { color: #ec4899; }
        .border-theme { border-color: #ec4899; }
    </style>
</head>
<body class="bg-gray-100 font-sans">

    <nav class="bg-theme text-white p-4 shadow-md">
        <div class="container mx-auto flex flex-col md:flex-row justify-between items-center">
            <h1 class="text-xl font-bold mb-2 md:mb-0">Plateforme de Collecte - CAP Signes de Danger</h1>
            <div>
                <button onclick="switchTab('collecte')" class="px-4 py-2 bg-pink-700 hover:bg-pink-800 rounded mr-2 focus:outline-none focus:ring-2 focus:ring-white transition">Fiche de Collecte</button>
                <button onclick="switchTab('matrice')" class="px-4 py-2 bg-pink-700 hover:bg-pink-800 rounded focus:outline-none focus:ring-2 focus:ring-white transition">Matrice (Données)</button>
            </div>
        </div>
    </nav>

    <div class="container mx-auto p-4 mt-4 bg-white rounded-lg shadow-lg">
        
        <div id="collecte" class="tab-content active">
            <h2 class="text-2xl font-bold mb-2 text-center text-gray-800">FICHE D’ENQUÊTE</h2>
            <p class="text-center text-gray-600 mb-6 font-medium">Étude CAP sur les signes de danger des complications obstétricales</p>
            
            <form id="dataForm" class="space-y-6">
                <div class="form-section">
                    <h3>Consentement</h3>
                    <label>Consentement obtenu :</label>
                    <div class="radio-group">
                        <label><input type="radio" name="consentement" value="Oui" required> Oui</label>
                        <label><input type="radio" name="consentement" value="Non"> Non</label>
                    </div>
                </div>

                <div class="form-section">
                    <h3>1. Identification</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div>
                            <label>Numéro de fiche :</label>
                            <input type="text" name="num_fiche" required>
                        </div>
                        <div>
                            <label>Date :</label>
                            <input type="date" name="date_enquete" required>
                        </div>
                        <div>
                            <label>Lieu :</label>
                            <input type="text" name="lieu" value="HGR Makala">
                        </div>
                        <div>
                            <label>Enquêteur :</label>
                            <input type="text" name="enqueteur" required>
                        </div>
                    </div>
                </div>

                <div class="form-section">
                    <h3>2. Données sociodémographiques</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div>
                            <label>Âge réel au dernier anniversaire (en années) :</label>
                            <input type="number" name="age" min="10" max="100" required>
                            <small class="text-pink-600">* Ne pas collecter l'âge en catégorie</small>
                        </div>
                        <div>
                            <label>État matrimonial :</label>
                            <div class="radio-group flex flex-wrap gap-2">
                                <label><input type="radio" name="etat_matrimonial" value="Célibataire" required> Célibataire</label>
                                <label><input type="radio" name="etat_matrimonial" value="Mariée"> Mariée</label>
                                <label><input type="radio" name="etat_matrimonial" value="Union libre"> Union libre</label>
                                <label><input type="radio" name="etat_matrimonial" value="Veuve / Divorcée"> Veuve / Divorcée</label>
                            </div>
                        </div>
                        <div>
                            <label>Niveau d’instruction :</label>
                            <div class="radio-group flex flex-wrap gap-2">
                                <label><input type="radio" name="niveau_instruction" value="Aucun" required> Aucun</label>
                                <label><input type="radio" name="niveau_instruction" value="Primaire"> Primaire</label>
                                <label><input type="radio" name="niveau_instruction" value="Secondaire"> Secondaire</label>
                                <label><input type="radio" name="niveau_instruction" value="Universitaire"> Universitaire</label>
                            </div>
                        </div>
                        <div>
                            <label>Profession :</label>
                            <div class="radio-group flex flex-wrap gap-2">
                                <label><input type="radio" name="profession" value="Ménagère" required> Ménagère</label>
                                <label><input type="radio" name="profession" value="Commerçante"> Commerçante</label>
                                <label><input type="radio" name="profession" value="Fonctionnaire"> Fonctionnaire</label>
                                <label><input type="radio" name="profession" value="Étudiante"> Étudiante</label>
                                <label><input type="radio" name="profession" value="Autre"> Autre : <input type="text" name="profession_autre" class="ml-2 w-24 border-b border-gray-400 outline-none"></label>
                            </div>
                        </div>
                        <div>
                            <label>Religion :</label>
                            <input type="text" name="religion" required>
                        </div>
                        <div>
                            <label>Nombre de consultations prénatales (CPN) :</label>
                            <div class="radio-group flex flex-wrap gap-2">
                                <label><input type="radio" name="cpn" value="0" required> 0</label>
                                <label><input type="radio" name="cpn" value="1 – 3"> 1 – 3</label>
                                <label><input type="radio" name="cpn" value="≥ 4"> ≥ 4</label>
                            </div>
                        </div>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mt-4 bg-gray-100 p-4 rounded border">
                        <div>
                            <label class="text-pink-700 font-bold">Gestité :</label>
                            <div class="radio-group flex flex-wrap gap-2">
                                <label><input type="radio" name="gestite" value="Primigeste" required> Primigeste</label>
                                <label><input type="radio" name="gestite" value="2 – 4 grossesses"> 2 – 4 grossesses</label>
                                <label><input type="radio" name="gestite" value="≥ 5 grossesses"> ≥ 5 grossesses</label>
                            </div>
                        </div>
                        <div>
                            <label class="text-pink-700 font-bold">Parité :</label>
                            <div class="radio-group flex flex-wrap gap-2">
                                <label><input type="radio" name="parite" value="Primipare" required> Primipare</label>
                                <label><input type="radio" name="parite" value="Paucipare"> Paucipare</label>
                                <label><input type="radio" name="parite" value="Multipare"> Multipare</label>
                                <label><input type="radio" name="parite" value="Grande multipare"> Grande multipare</label>
                            </div>
                        </div>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mt-4">
                        <div>
                            <label>Nombre d’enfant né vivant :</label>
                            <input type="number" name="enfants_vivants" min="0" required>
                        </div>
                        <div>
                            <label>Nombre d’enfant mort né :</label>
                            <input type="number" name="enfants_morts_nes" min="0" required>
                        </div>
                        <div>
                            <label>Antécédent d’avortement :</label>
                            <input type="number" name="atcd_avortement" min="0" placeholder="Nombre" required>
                        </div>
                        <div>
                            <label>Antécédent de fausse couche :</label>
                            <input type="number" name="atcd_fausse_couche" min="0" placeholder="Nombre" required>
                        </div>
                        <div>
                            <label>Date des Dernières Règles (DDR) :</label>
                            <input type="date" name="ddr">
                        </div>
                        <div>
                            <label>Âge de la grossesse (en semaines ou mois) :</label>
                            <input type="text" name="age_grossesse" required>
                        </div>
                    </div>
                </div>

                <div class="form-section">
                    <h3>3. Connaissances des signes de danger</h3>
                    
                    <label>Avez-vous déjà entendu parler des signes de danger pendant la grossesse ?</label>
                    <div class="radio-group">
                        <label><input type="radio" name="connaissance_entendu" value="Oui" required> Oui</label>
                        <label><input type="radio" name="connaissance_entendu" value="Non"> Non</label>
                    </div>

                    <label>Si oui, source d’information :</label>
                    <div class="radio-group flex flex-wrap gap-2">
                        <label><input type="radio" name="source_info" value="Personnel de santé"> Personnel de santé</label>
                        <label><input type="radio" name="source_info" value="Radio / TV"> Radio / TV</label>
                        <label><input type="radio" name="source_info" value="Famille"> Famille</label>
                        <label><input type="radio" name="source_info" value="Amis"> Amis</label>
                        <label><input type="radio" name="source_info" value="Autre"> Autre : <input type="text" name="source_info_autre" class="ml-2 w-32 border-b border-gray-400 outline-none"></label>
                    </div>

                    <label class="mt-4">Citez les signes de danger que vous connaissez (plusieurs réponses possibles) :</label>
                    <div class="checkbox-group flex flex-col gap-1">
                        <label><input type="checkbox" name="signes_connus" value="Saignement vaginal"> Saignement vaginal</label>
                        <label><input type="checkbox" name="signes_connus" value="Convulsions"> Convulsions</label>
                        <label><input type="checkbox" name="signes_connus" value="Céphalées intenses"> Céphalées intenses</label>
                        <label><input type="checkbox" name="signes_connus" value="Vision floue"> Vision floue</label>
                        <label><input type="checkbox" name="signes_connus" value="Douleurs abdominales"> Douleurs abdominales</label>
                        <label><input type="checkbox" name="signes_connus" value="Fièvre"> Fièvre</label>
                        <label><input type="checkbox" name="signes_connus" value="Difficulté respiratoire"> Difficulté respiratoire</label>
                        <label><input type="checkbox" name="signes_connus" value="Diminution des mouvements du fœtus"> Diminution des mouvements du fœtus</label>
                        <label><input type="checkbox" name="signes_connus" value="Œdème important"> Œdème important</label>
                        <label><input type="checkbox" name="signes_connus" value="Autre"> Autre : <input type="text" name="signes_connus_autre" class="ml-2 w-64 border-b border-gray-400 outline-none"></label>
                    </div>
                </div>

                <div class="form-section">
                    <h3>4. Attitudes</h3>
                    
                    <label>Pensez-vous que ces signes sont graves ?</label>
                    <div class="radio-group">
                        <label><input type="radio" name="attitude_gravite" value="Oui" required> Oui</label>
                        <label><input type="radio" name="attitude_gravite" value="Non"> Non</label>
                        <label><input type="radio" name="attitude_gravite" value="Ne sait Pas"> Ne sait Pas</label>
                    </div>

                    <label>Que feriez-vous en cas de signe de danger ?</label>
                    <div class="radio-group flex flex-col gap-1">
                        <label><input type="radio" name="attitude_action" value="Aller immédiatement à l’hôpital" required> Aller immédiatement à l’hôpital</label>
                        <label><input type="radio" name="attitude_action" value="Attendre que ça passe"> Attendre que ça passe</label>
                        <label><input type="radio" name="attitude_action" value="Médecine traditionnelle"> Médecine traditionnelle</label>
                        <label><input type="radio" name="attitude_action" value="Demander conseil à la famille"> Demander conseil à la famille</label>
                    </div>

                    <label>Qui décide d’aller à l’hôpital ?</label>
                    <div class="radio-group flex flex-wrap gap-2">
                        <label><input type="radio" name="attitude_decideur" value="Vous-même" required> Vous-même</label>
                        <label><input type="radio" name="attitude_decideur" value="Mari"> Mari</label>
                        <label><input type="radio" name="attitude_decideur" value="Famille"> Famille</label>
                        <label><input type="radio" name="attitude_decideur" value="Autre"> Autre : <input type="text" name="attitude_decideur_autre" class="ml-2 w-24 border-b border-gray-400 outline-none"></label>
                    </div>
                </div>

                <div class="form-section">
                    <h3>5. Pratiques</h3>
                    
                    <label>Avez-vous déjà présenté un signe de danger pendant cette grossesse ?</label>
                    <div class="radio-group">
                        <label><input type="radio" name="pratique_deja_presente" value="Oui" required> Oui</label>
                        <label><input type="radio" name="pratique_deja_presente" value="Non"> Non</label>
                    </div>

                    <label>Si oui, lequel ?</label>
                    <input type="text" name="pratique_lequel">

                    <label>Qu’avez-vous fait ?</label>
                    <div class="radio-group flex flex-wrap gap-2">
                        <label><input type="radio" name="pratique_action" value="Consultation médicale"> Consultation médicale</label>
                        <label><input type="radio" name="pratique_action" value="Automédication"> Automédication</label>
                        <label><input type="radio" name="pratique_action" value="Traitement traditionnel"> Traitement traditionnel</label>
                        <label><input type="radio" name="pratique_action" value="Rien"> Rien</label>
                    </div>

                    <label>Délai avant de consulter :</label>
                    <div class="radio-group flex flex-col gap-1">
                        <label><input type="radio" name="pratique_delai" value="Immédiatement"> Immédiatement</label>
                        <label><input type="radio" name="pratique_delai" value="Après quelques heures"> Après quelques heures</label>
                        <label><input type="radio" name="pratique_delai" value="Après plusieurs jours"> Après plusieurs jours</label>
                    </div>
                </div>

                <div class="form-section bg-pink-50 border-pink-200">
                    <h3 class="text-pink-800 border-pink-300">6. Évaluation du niveau de connaissance <span class="text-sm font-normal">(À remplir par l’enquêteur)</span></h3>
                    <p class="mb-2 text-xs text-gray-500 italic">* C’est vous qui devez évaluer le niveau de connaissance en fonction du nombre de signes cités plus haut. Ce n'est pas à la femme de s'auto-évaluer.</p>
                    <div class="radio-group flex flex-col gap-2">
                        <label><input type="radio" name="evaluation_niveau" value="Bonne connaissance" required> <strong>Bonne connaissance :</strong> ≥ 3 signes corrects</label>
                        <label><input type="radio" name="evaluation_niveau" value="Connaissance moyenne"> <strong>Connaissance moyenne :</strong> 2 signes</label>
                        <label><input type="radio" name="evaluation_niveau" value="Mauvaise connaissance"> <strong>Mauvaise connaissance :</strong> ≤ 1 signe</label>
                    </div>
                </div>

                <div class="text-center pb-8">
                    <button type="submit" class="bg-green-600 text-white font-bold py-3 px-8 rounded hover:bg-green-700 focus:outline-none focus:ring-4 focus:ring-green-300 transition duration-300">Enregistrer et Soumettre la fiche</button>
                </div>
            </form>
        </div>

        <div id="matrice" class="tab-content">
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-2xl font-bold text-gray-800">MATRICE DES DONNÉES</h2>
                <button id="btnExport" class="bg-green-600 text-white px-4 py-2 rounded shadow hover:bg-green-700 flex items-center transition">
                    <svg class="w-4 h-4 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 10v6m0 0l-3-3m3 3l3-3m2 8H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"></path></svg>
                    Télécharger Excel
                </button>
            </div>
            
            <div class="overflow-x-auto bg-white border border-gray-200 rounded-lg shadow">
                <table class="min-w-full divide-y divide-gray-200">
                    <thead class="bg-gray-50">
                        <tr>
                            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Date Saisie</th>
                            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">N° Fiche</th>
                            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Âge Patient</th>
                            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Gestité / Parité</th>
                            <th class="px-6 py-3 text-center text-xs font-medium text-gray-500 uppercase tracking-wider">Actions</th>
                        </tr>
                    </thead>
                    <tbody id="tableBody" class="bg-white divide-y divide-gray-200">
                        </tbody>
                </table>
            </div>
        </div>
    </div>

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
                    </div>
                <div class="items-center px-4 py-3 mt-4 border-t">
                    <button onclick="closeModal()" class="px-4 py-2 bg-pink-500 text-white text-base font-medium rounded-md w-full shadow-sm hover:bg-pink-600 focus:outline-none focus:ring-2 focus:ring-pink-300">Fermer</button>
                </div>
            </div>
        </div>
    </div>

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

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getDatabase, ref, push, onValue, remove } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-database.js";

        // Configuration Firebase (Restée identique)
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

        const app = initializeApp(firebaseConfig);
        const db = getDatabase(app);
        const recordsRef = ref(db, 'fiches_collecte');

        let allRecordsData = [];

        // GESTION DU FORMULAIRE
        document.getElementById('dataForm').addEventListener('submit', async (e) => {
            e.preventDefault();
            const form = e.target;
            const formData = new FormData(form);
            const dataObj = { date_soumission: new Date().toISOString() };

            for (let [key, value] of formData.entries()) {
                if (dataObj[key]) {
                    if (!Array.isArray(dataObj[key])) {
                        dataObj[key] = [dataObj[key]];
                    }
                    dataObj[key].push(value);
                } else {
                    dataObj[key] = value;
                }
            }

            try {
                await push(recordsRef, dataObj);
                alert("Fiche enregistrée avec succès dans la base de données !");
                form.reset();
                switchTab('matrice');
            } catch (error) {
                console.error("Erreur lors de l'enregistrement : ", error);
                alert("Une erreur est survenue lors de l'enregistrement.");
            }
        });

        // LECTURE EN TEMPS RÉEL
        onValue(recordsRef, (snapshot) => {
            const data = snapshot.val();
            const tableBody = document.getElementById('tableBody');
            tableBody.innerHTML = '';
            allRecordsData = [];

            if (data) {
                for (let key in data) {
                    const record = data[key];
                    record.id = key; 
                    allRecordsData.push(record);

                    const tr = document.createElement('tr');
                    const dateObj = new Date(record.date_soumission);
                    const dateFr = dateObj.toLocaleDateString('fr-FR') + ' ' + dateObj.toLocaleTimeString('fr-FR');

                    // Mise à jour des colonnes pour refléter la nouvelle fiche
                    tr.innerHTML = `
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${dateFr}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">${record.num_fiche || 'N/A'}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${record.age || '-'} ans</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">G: ${record.gestite || '-'} / P: ${record.parite || '-'}</td>
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

        // GESTION DES CLICS
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

        // AFFICHER DANS LE MODAL
        function afficherDetails(record) {
            const modalContent = document.getElementById('modalContent');
            let html = '<ul class="divide-y divide-gray-200">';
            
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
            
            document.getElementById('modalTitle').innerText = "Fiche N° : " + (record.num_fiche || 'Inconnu');
            modalContent.innerHTML = html;
            document.getElementById('viewModal').classList.remove('hidden');
        }

        // EXPORT EXCEL
        document.getElementById('btnExport').addEventListener('click', () => {
            if (allRecordsData.length === 0) {
                alert("Aucune donnée à télécharger.");
                return;
            }
            
            const exportData = allRecordsData.map(record => {
                const { id, ...cleanRecord } = record;
                for (let key in cleanRecord) {
                    if (Array.isArray(cleanRecord[key])) {
                        cleanRecord[key] = cleanRecord[key].join('; ');
                    }
                }
                return cleanRecord;
            });

            const worksheet = XLSX.utils.json_to_sheet(exportData);
            const workbook = XLSX.utils.book_new();
            XLSX.utils.book_append_sheet(workbook, worksheet, "Donnees_Obstetrique");
            XLSX.writeFile(workbook, "Base_Donnees_CAP_Danger_Obstetrique.xlsx");
        });
    </script>
</body>
</html>
