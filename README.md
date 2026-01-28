<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vocabulaire & Quiz</title>
    <style>
        :root {
            --primary-color: #2c3e50;
            --accent-color: #c0392b; /* Rouge brique */
            --success-color: #27ae60; /* Vert succès */
            --bg-color: #ecf0f1;
            --card-bg: #ffffff;
            --text-color: #2c3e50;
        }

        body {
            font-family: 'Baskerville', 'Georgia', 'Times New Roman', serif;
            background-color: var(--bg-color);
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            color: var(--text-color);
            padding: 20px;
        }

        /* Navigation entre les modes */
        .nav-bar {
            margin-bottom: 20px;
            display: flex;
            gap: 15px;
            z-index: 10;
        }

        .nav-btn {
            background: transparent;
            border: 2px solid var(--primary-color);
            color: var(--primary-color);
            padding: 10px 20px;
            font-family: sans-serif;
            text-transform: uppercase;
            font-weight: bold;
            cursor: pointer;
            border-radius: 30px;
            transition: all 0.3s;
        }

        .nav-btn.active, .nav-btn:hover {
            background: var(--primary-color);
            color: white;
        }

        /* Conteneur principal */
        .card {
            background-color: var(--card-bg);
            width: 100%;
            max-width: 650px;
            padding: 40px;
            border-radius: 8px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.1);
            text-align: center;
            position: relative;
            min-height: 450px;
            border-top: 5px solid var(--primary-color);
            display: flex;
            flex-direction: column;
            justify-content: center;
        }

        /* Styles Mode Apprentissage */
        h1 { font-size: 3rem; margin: 0 0 10px 0; color: var(--primary-color); letter-spacing: -1px; }
        .pronunciation { font-family: sans-serif; font-size: 0.8rem; color: #7f8c8d; margin-bottom: 20px; text-transform: uppercase; letter-spacing: 1px; }
        .definition { font-size: 1.3rem; margin-bottom: 25px; line-height: 1.5; font-weight: 600; }
        .example-box { background: #f8f9fa; border-left: 4px solid var(--accent-color); padding: 15px; text-align: left; margin-bottom: 30px; }
        .example-text { font-style: italic; color: #555; font-size: 1.1rem; }
        
        /* Styles Mode Quiz */
        .quiz-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
            font-family: sans-serif;
            color: #7f8c8d;
            font-weight: bold;
        }
        
        .quiz-word {
            font-size: 2.2rem;
            color: var(--primary-color);
            margin-bottom: 30px;
            font-weight: bold;
        }

        .options-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 10px;
        }

        .option-btn {
            background: white;
            border: 1px solid #bdc3c7;
            padding: 15px;
            font-size: 1rem;
            color: #444;
            cursor: pointer;
            border-radius: 5px;
            transition: all 0.2s;
            text-align: left;
            line-height: 1.4;
            font-family: 'Segoe UI', sans-serif;
        }

        .option-btn:hover {
            background-color: #f1f2f6;
            border-color: var(--primary-color);
        }

        .option-btn.correct {
            background-color: var(--success-color) !important;
            color: white !important;
            border-color: var(--success-color) !important;
        }

        .option-btn.wrong {
            background-color: var(--accent-color) !important;
            color: white !important;
            border-color: var(--accent-color) !important;
        }

        /* Bouton d'action principal */
        .action-btn { 
            background: var(--primary-color); 
            color: #fff; 
            border: none; 
            padding: 15px 40px; 
            font-size: 1rem; 
            border-radius: 3px; 
            cursor: pointer; 
            font-family: sans-serif; 
            text-transform: uppercase; 
            letter-spacing: 1px; 
            transition: 0.2s; 
            margin-top: 20px;
            align-self: center;
        }
        .action-btn:hover { background: var(--accent-color); transform: translateY(-2px); }

        /* Animations */
        .hidden { display: none !important; }
        .fade-in { animation: fadeIn 0.4s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .score-display {
            font-size: 4rem;
            color: var(--accent-color);
            font-weight: bold;
        }
    </style>
</head>
<body>

    <div class="nav-bar">
        <button class="nav-btn active" id="btn-learn" onclick="switchMode('learn')">Apprentissage</button>
        <button class="nav-btn" id="btn-quiz" onclick="switchMode('quiz')">Quiz</button>
    </div>

    <div class="card">
        
        <div id="learn-section" class="fade-in">
            <h1 id="word">Bienvenue</h1>
            <div class="pronunciation" id="type">Mode Apprentissage</div>
            <p class="definition" id="definition">Découvrez des mots rares pour enrichir votre esprit.</p>
            <div class="example-box">
                <span style="font-size:0.7rem; text-transform:uppercase; color:#95a5a6; font-weight:bold;">Contexte</span><br>
                <span class="example-text" id="example">"Le vocabulaire est la clé de la pensée."</span>
            </div>
            <button class="action-btn" onclick="newWord()">Mot Suivant</button>
        </div>

        <div id="quiz-section" class="hidden">
            <div id="quiz-start">
                <h2>Testez vos connaissances</h2>
                <p>10 questions pour évaluer votre maîtrise du vocabulaire.</p>
                <button class="action-btn" onclick="startQuiz()">Commencer le Quiz</button>
            </div>

            <div id="quiz-question" class="hidden">
                <div class="quiz-header">
                    <span id="q-progress">Question 1/10</span>
                    <span id="q-score">Score: 0</span>
                </div>
                <div class="pronunciation">Que signifie ce mot ?</div>
                <div class="quiz-word" id="q-word">Mot</div>
                
                <div class="options-grid" id="q-options">
                    </div>
            </div>

            <div id="quiz-result" class="hidden">
                <h2>Résultat Final</h2>
                <div class="score-display" id="final-score">8/10</div>
                <p id="final-message">Pas mal !</p>
                <button class="action-btn" onclick="startQuiz()">Recommencer</button>
            </div>
        </div>

    </div>

    <script>
        // ==========================================
        // BASE DE DONNÉES (Collez vos listes ici)
        // ==========================================
                const vocabulaire = [
            { word: "Abhorrer", type: "v.", def: "Avoir en horreur, détester au plus haut point.", ex: "Pacifiste convaincu, il abhorre toute forme de violence." },
            { word: "Abnégation", type: "n.f.", def: "Sacrifice total de son intérêt propre.", ex: "Elle a soigné les blessés avec une abnégation admirable." },
            { word: "Abstrus", type: "adj.", def: "Difficile à comprendre, obscur (pour l'esprit).", ex: "Sa théorie est si abstruse que même les experts s'y perdent." },
            { word: "Acariâtre", type: "adj.", def: "D'une humeur grincheuse et difficile.", ex: "Son voisin est un vieillard acariâtre qui crie dès qu'un ballon touche sa pelouse." },
            { word: "Accointance", type: "n.f.", def: "Liaison familière, relation (souvent péjoratif).", ex: "On le soupçonne d'avoir des accointances avec la mafia locale." },
            { word: "Acribie", type: "n.f.", def: "Précision méticuleuse, soin extrême.", ex: "Le texte a été corrigé avec une acribie de moine copiste." },
            { word: "Acrimonie", type: "n.f.", def: "Aigreur, méchanceté dans le langage ou l'humeur.", ex: "La discussion s'est terminée dans l'acrimonie et les reproches mutuels." },
            { word: "Admonester", type: "v.", def: "Réprimander sévèrement.", ex: "Le juge a admonesté le témoin pour son manque de sérieux." },
            { word: "Affable", type: "adj.", def: "Qui accueille les gens avec douceur et bienveillance.", ex: "Malgré son rang, le roi restait affable avec ses sujets." },
            { word: "Agnostique", type: "adj. et n.", def: "Qui pense que l'absolu (Dieu) est inaccessible à l'esprit humain.", ex: "Il se déclare agnostique, ne pouvant prouver ni l'existence ni l'absence de Dieu." },
            { word: "Alambiqué", type: "adj.", def: "Trop compliqué, contourné (raisonnement, style).", ex: "Ses explications alambiquées ne font que nous embrouiller davantage." },
            { word: "Allégation", type: "n.f.", def: "Affirmation, citation que l'on donne pour preuve.", ex: "Ces allégations mensongères visent à détruire sa réputation." },
            { word: "Altruisme", type: "n.m.", def: "Souci désintéressé du bien d'autrui.", ex: "Son altruisme le pousse à donner la moitié de son salaire aux œuvres." },
            { word: "Amène", type: "adj.", def: "Agréable, doux, poli.", ex: "Le directeur, d'habitude si froid, s'est montré d'une humeur amène ce matin." },
            { word: "Amphigouri", type: "n.m.", def: "Discours ou écrit confus et inintelligible.", ex: "Son rapport n'est qu'un amphigouri sans queue ni tête." },
            { word: "Anachorète", type: "n.m.", def: "Religieux vivant dans la solitude (ermite).", ex: "Il vit comme un anachorète au fond des bois, loin de la civilisation." },
            { word: "Anathème", type: "n.m.", def: "Condamnation religieuse, exclusion. Fig: réprobation publique.", ex: "Il a jeté l'anathème sur tous ceux qui osaient le contredire." },
            { word: "Ancillaire", type: "adj.", def: "Relatif aux servantes. (Amours ancillaires : avec des domestiques).", ex: "Les romans du XIXe siècle sont pleins d'histoires d'amours ancillaires." },
            { word: "Annihiler", type: "v.", def: "Réduire à rien, détruire complètement.", ex: "Cette mauvaise nouvelle a annihilé tous ses espoirs de réussite." },
            { word: "Antédiluvien", type: "adj.", def: "D'avant le déluge. Fig: Très vieux, démodé.", ex: "Il travaille encore avec un ordinateur antédiluvien qui prend dix minutes à démarrer." },
            { word: "Apanage", type: "n.m.", def: "Privilège exclusif, propre à quelqu'un ou quelque chose.", ex: "La raison est, dit-on, l'apanage de l'homme." },
            { word: "Apathie", type: "n.f.", def: "Indolence, incapacité de réagir, indifférence.", ex: "Il a accueilli la catastrophe avec une apathie inquiétante." },
            { word: "Apocryphe", type: "adj.", def: "Dont l'authenticité est douteuse (texte).", ex: "Cette citation de Napoléon est probablement apocryphe." },
            { word: "Apologie", type: "n.f.", def: "Discours visant à défendre ou justifier une personne ou une doctrine.", ex: "Il a fait l'apologie de la paresse dans son dernier essai." },
            { word: "Aporie", type: "n.f.", def: "Contradiction insoluble dans un raisonnement.", ex: "Le débat s'est enlisé dans une aporie sans issue." },
            { word: "Apostasie", type: "n.f.", def: "Abjuration, abandon public d'une religion ou d'un parti.", ex: "Son passage à l'ennemi fut considéré comme une impardonnable apostasie." },
            { word: "Ardu", type: "adj.", def: "Difficile, pénible (tâche, chemin).", ex: "L'apprentissage du mandarin est une tâche ardue mais gratifiante." },
            { word: "Aréopage", type: "n.m.", def: "Assemblée de juges ou d'hommes de lettres compétents.", ex: "Il a dû présenter sa thèse devant un aréopage de scientifiques sévères." },
            { word: "Argutie", type: "n.f.", def: "Raisonnement d'une subtilité excessive, chicanaillerie.", ex: "Ne nous perdons pas dans des arguties juridiques, allons à l'essentiel." },
            { word: "Arroger (s')", type: "v.", def: "S'attribuer un pouvoir ou un droit sans y être autorisé.", ex: "Il s'est arrogé le droit de parler au nom de tous ses collègues." },
            { word: "Ascèse", type: "n.f.", def: "Discipline de vie stricte (privations) pour tendre à la perfection.", ex: "L'écriture de ce roman lui a demandé une véritable ascèse intellectuelle." },
            { word: "Ataraxie", type: "n.f.", def: "Tranquillité de l'âme, absence de trouble (philosophie stoïcienne).", ex: "Le sage vise l'ataraxie, restant indifférent aux aléas de la fortune." },
            { word: "Atavisme", type: "n.m.", def: "Hérédité biologique ou psychologique.", ex: "Par un curieux atavisme, il a les mêmes manies que son grand-père." },
            { word: "Atermoiement", type: "n.m.", def: "Action de repousser ce qu'on doit faire, hésitation.", ex: "Après des mois d'atermoiement, le gouvernement a enfin voté la loi." },
            { word: "Auguste", type: "adj.", def: "Qui inspire un grand respect, majestueux.", ex: "Le vieil homme avait une allure auguste avec sa barbe blanche." },
            { word: "Aulique", type: "adj.", def: "Relatif à la cour d'un roi.", ex: "Il a utilisé un style aulique et précieux pour flatter la reine." },
            { word: "Autarcie", type: "n.f.", def: "État d'un pays ou groupe qui se suffit à lui-même.", ex: "Cette communauté vit en autarcie totale, produisant tout ce qu'elle consomme." },
            { word: "Avatar", type: "n.m.", def: "Métamorphose, transformation. Mésaventure (sens courant déformé).", ex: "Ce projet a connu de nombreux avatars avant d'être enfin réalisé." },
            { word: "Baguenauder", type: "v.", def: "Se promener sans but, flâner.", ex: "Nous avons passé l'après-midi à baguenauder dans les ruelles de Venise." },
            { word: "Baliverne", type: "n.f.", def: "Propos futile, croyance erronée, sornette.", ex: "Ne croyez pas ces balivernes, c'est de la pure superstition." },
            { word: "Béatitude", type: "n.f.", def: "Bonheur parfait, félicité céleste.", ex: "Il affichait un sourire de béatitude après avoir réussi son examen." },
            { word: "Belliqueux", type: "adj.", def: "Qui aime la guerre, agressif.", ex: "Son ton belliqueux a immédiatement tendu l'atmosphère de la réunion." },
            { word: "Biaiser", type: "v.", def: "Utiliser des moyens détournés, ne pas être franc.", ex: "Arrêtez de biaiser et répondez franchement à ma question !" },
            { word: "Bienséance", type: "n.f.", def: "Conduite sociale conforme aux usages, politesse.", ex: "La bienséance veut que l'on ne coupe pas la parole à ses aînés." },
            { word: "Bileux", type: "adj.", def: "Qui se fait du souci pour rien ; inquiet, pessimiste.", ex: "Ne sois pas si bileux, tout va très bien se passer." },
            { word: "Blafard", type: "adj.", def: "D'une pâleur terne et livide.", ex: "Sous la lumière blafarde des néons, il paraissait malade." },
            { word: "Bonhomie", type: "n.f.", def: "Simplicité dans les manières, bonté naïve.", ex: "Il a accepté les excuses avec sa bonhomie habituelle." },
            { word: "Borborygme", type: "n.m.", def: "Bruit de gaz dans l'abdomen. Fig: Propos incompréhensible.", ex: "L'ivrogne n'émettait plus que de vagues borborygmes." },
            { word: "Bucolique", type: "adj.", def: "Relatif à la vie de berger, champêtre, poétique.", ex: "Ils ont fait un pique-nique dans un cadre bucolique, au milieu des prés." },
            { word: "Burlesque", type: "adj.", def: "D'un comique extravagant, qui frise le ridicule.", ex: "La situation était si burlesque que personne n'a pu garder son sérieux." },
            { word: "Cabale", type: "n.f.", def: "Complot secret, intrigue menée par une faction.", ex: "Il se dit victime d'une cabale destinée à l'écarter du pouvoir." },
            { word: "Cacochyme", type: "adj.", def: "D'une santé déficiente, maladif, vieux et faible.", ex: "Ce gouvernement cacochyme ne tiendra pas l'hiver." },
            { word: "Cacophonie", type: "n.f.", def: "Mélange de sons discordants.", ex: "La répétition de l'orchestre débutant était une véritable cacophonie." },
            { word: "Caduc", type: "adj.", def: "Qui est périmé, qui n'a plus cours (loi, usage).", ex: "Ce règlement est désormais caduc, une nouvelle loi l'a remplacé." },
            { word: "Cafardeux", type: "adj.", def: "Qui a le cafard, triste, mélancolique.", ex: "Le temps gris me rend toujours un peu cafardeux le dimanche." },
            { word: "Calembredaine", type: "n.f.", def: "Propos extravagant, plaisanterie futile.", ex: "Il passe son temps à raconter des calembredaines pour amuser la galerie." },
            { word: "Callipyge", type: "adj.", def: "Qui a de belles fesses (statues grecques).", ex: "La Vénus callipyge est une célèbre statue antique." },
            { word: "Cancan", type: "n.m.", def: "Bavardage malveillant, médisance.", ex: "Le village vit au rythme des cancans et des rumeurs." },
            { word: "Capricant", type: "adj.", def: "Inégal, qui fait des bonds (comme une chèvre). Pouls capricant.", ex: "Son style capricant passe du coq à l'âne sans prévenir." },
            { word: "Captieux", type: "adj.", def: "Qui tend à tromper sous des apparences de vérité.", ex: "C'est un argument captieux qui ne résiste pas à l'analyse." },
            { word: "Cardinal", type: "adj.", def: "Principal, essentiel, fondamental.", ex: "L'honnêteté est pour lui une vertu cardinale." },
            { word: "Carence", type: "n.f.", def: "Manque, insuffisance (vitamine, ou incapacité juridique).", ex: "La carence de l'État dans ce dossier est flagrante." },
            { word: "Casanier", type: "adj.", def: "Qui aime rester chez soi, qui n'aime pas voyager.", ex: "C'est un casanier qui préfère son fauteuil aux aventures lointaines." },
            { word: "Casuistique", type: "n.f.", def: "Art de trancher les cas de conscience (souvent avec subtilité excessive).", ex: "Il a usé de toute sa casuistique pour justifier sa trahison." },
            { word: "Catachrèse", type: "n.f.", def: "Détournement d'un mot de son sens propre (ex: les pieds d'une chaise).", ex: "'À cheval sur un mur' est une catachrèse courante." },
            { word: "Catalyseur", type: "n.m.", def: "Élément qui provoque ou accélère une réaction.", ex: "Cet incident a servi de catalyseur à la révolte populaire." },
            { word: "Cathartique", type: "adj.", def: "Qui purifie, qui libère des passions (théâtre, psychanalyse).", ex: "Écrire ce livre fut une expérience cathartique qui l'a délivré de ses démons." },
            { word: "Caustique", type: "adj.", def: "Qui attaque, qui blesse par la moquerie. Mordant.", ex: "Son humour caustique ne plaît pas à tout le monde." },
            { word: "Cautèle", type: "n.f.", def: "Prudence mêlée de ruse, défiance.", ex: "Il a répondu avec une cautèle de vieux diplomate." },
            { word: "Céleste", type: "adj.", def: "Qui appartient au ciel. Fig: Divin, merveilleux.", ex: "Une musique céleste descendait des voûtes de la cathédrale." },
            { word: "Cénobite", type: "n.m.", def: "Moine vivant en communauté (opposé à anachorète).", ex: "La vie de cénobite exige le respect d'une règle commune stricte." },
            { word: "Céruléen", type: "adj.", def: "D'une couleur bleu ciel, azuré.", ex: "Ses yeux d'un bleu céruléen fascinaient tous ses prétendants." },
            { word: "Chimère", type: "n.f.", def: "Idée folle, projet irréalisable, illusion.", ex: "La paix universelle est-elle une chimère ?" },
            { word: "Circonlocution", type: "n.f.", def: "Manière d'exprimer sa pensée par des détours (périphrase).", ex: "Au lieu de dire 'non', il s'est perdu dans d'interminables circonlocutions." },
            { word: "Circonspect", type: "adj.", def: "Qui agit avec réflexion et prudence.", ex: "Il est resté circonspect face à cette offre trop belle pour être vraie." },
            { word: "Claustrer", type: "v.", def: "Enfermer dans un lieu clos (se cloîtrer).", ex: "Il vit claustré chez lui depuis la mort de sa femme." },
            { word: "Clémence", type: "n.f.", def: "Vertu qui consiste à pardonner, à adoucir les punitions.", ex: "Le condamné a imploré la clémence du roi." },
            { word: "Clivage", type: "n.m.", def: "Séparation, fracture (sociale, politique).", ex: "Le clivage entre riches et pauvres s'est accentué ces dernières années." },
            { word: "Coercition", type: "n.f.", def: "Action de contraindre quelqu'un.", ex: "L'usage de la coercition est parfois nécessaire pour maintenir l'ordre." },
            { word: "Cohorte", type: "n.f.", def: "Troupe, groupe nombreux (souvent péjoratif).", ex: "Une cohorte de touristes a envahi la place du village." },
            { word: "Collusion", type: "n.f.", def: "Entente secrète au préjudice d'un tiers.", ex: "Il y a une collusion évidente entre ces deux entreprises pour fixer les prix." },
            { word: "Commisération", type: "n.f.", def: "Sentiment de pitié pour les malheurs d'autrui.", ex: "Il ne demande pas votre commisération, mais votre respect." },
            { word: "Compendieux", type: "adj.", def: "Qui est bref et concis (abrégé).", ex: "Il a fait un résumé compendieux de la situation en trois phrases." },
            { word: "Complaisance", type: "n.f.", def: "Désir de plaire, indulgence excessive.", ex: "Il s'écoute parler avec une complaisance agaçante." },
            { word: "Concomitant", type: "adj.", def: "Qui se produit en même temps.", ex: "La hausse des prix fut concomitante à la pénurie de pétrole." },
            { word: "Concupiscence", type: "n.f.", def: "Désir vif des biens terrestres et des plaisirs sensuels.", ex: "Il regardait le buffet avec une concupiscence non dissimulée." },
            { word: "Condescendance", type: "n.f.", def: "Attitude hautaine teintée d'une bienveillance fausse.", ex: "Il m'a expliqué mon métier avec une insupportable condescendance." },
            { word: "Confiner", type: "v.", def: "Être très proche de, toucher à (limite). Enformer.", ex: "Son génie confine parfois à la folie." },
            { word: "Conflagration", type: "n.f.", def: "Incendie vaste et violent. Fig: Guerre généralisée.", ex: "L'assassinat de l'archiduc a déclenché une conflagration mondiale." },
            { word: "Congru", type: "adj.", def: "Qui convient exactement. Portion congrue : très petite part.", ex: "Les artistes ne reçoivent souvent qu'une portion congrue des bénéfices." },
            { word: "Conjecturer", type: "v.", def: "Supposer, présumer sur des probabilités.", ex: "On ne peut que conjecturer sur les raisons de sa disparition." },
            { word: "Connivence", type: "n.f.", def: "Entente secrète, complicité tacite.", ex: "Ils ont échangé un regard de connivence qui en disait long." },
            { word: "Conspuer", type: "v.", def: "Huer, manifester son mépris publiquement.", ex: "L'orateur a été conspué par une foule en colère." },
            { word: "Consternation", type: "n.f.", def: "Abattement causé par une très mauvaise surprise.", ex: "La nouvelle de l'accident a jeté la famille dans la consternation." },
            { word: "Contingence", type: "n.f.", def: "Caractère de ce qui peut arriver ou non (hasard).", ex: "Nous devons nous préparer aux contingences de l'avenir." },
            { word: "Contumace", type: "n.f.", def: "Refus de comparaître devant un tribunal.", ex: "Il a été jugé et condamné par contumace car il était en fuite." },
            { word: "Convivial", type: "adj.", def: "Qui favorise les relations humaines agréables.", ex: "L'ambiance de ce petit restaurant est très conviviale." },
            { word: "Cooptation", type: "n.f.", def: "Nomination d'un nouveau membre par les membres actuels.", ex: "Les membres de l'Académie sont élus par cooptation." },
            { word: "Coquecigrue", type: "n.f.", def: "Oiseau imaginaire. Fig: Chimère, absurdité.", ex: "Il croit à toutes sortes de coquecigrues et de légendes urbaines." },
            { word: "Cordial", type: "adj.", def: "Qui vient du cœur, chaleureux.", ex: "Nous entretenons des relations cordiales avec nos voisins." },
            { word: "Corollaire", type: "n.m.", def: "Conséquence immédiate, suite naturelle.", ex: "La liberté a pour corollaire la responsabilité." },
            { word: "Corporatisme", type: "n.m.", def: "Défense exclusive des intérêts d'une catégorie professionnelle.", ex: "Le syndicat a été accusé de corporatisme aveugle." },
            { word: "Corrosif", type: "adj.", def: "Qui ronge, qui attaque. Fig: Critique mordante.", ex: "Son esprit corrosif n'épargne personne." },
            { word: "Cosmopolite", type: "adj.", def: "Qui comprend des personnes de tous les pays.", ex: "Londres est une ville cosmopolite et vibrante." },
            { word: "Courroux", type: "n.m.", def: "Colère vive et emportée (terme littéraire).", ex: "Il redoutait le courroux de son père plus que tout." },
            { word: "Crédule", type: "adj.", def: "Qui croit trop facilement ce qu'on lui dit.", ex: "Il est assez crédule pour avoir acheté ce terrain marécageux." },
            { word: "Crépusculaire", type: "adj.", def: "Relatif au crépuscule. Fig: Qui décline, qui finit.", ex: "C'est la fin d'une époque, une ambiance crépusculaire règne sur le pays." },
            { word: "Cryptique", type: "adj.", def: "Caché, secret, obscur.", ex: "Il m'a envoyé un message cryptique que je n'arrive pas à décoder." },
            { word: "Cupidité", type: "n.f.", def: "Désir immodéré de l'argent.", ex: "Sa cupidité l'a poussé à trahir ses meilleurs amis pour une prime." },
            { word: "Cure", type: "n.f.", def: "Soin, traitement. (N'avoir cure de : se moquer de).", ex: "Il n'a cure des critiques et continue son chemin." },
            { word: "Cyclopéen", type: "adj.", def: "Gigantesque, énorme (comme les murs bâtis par les Cyclopes).", ex: "Ce barrage est un ouvrage cyclopéen." },
            { word: "Cynisme", type: "n.m.", def: "Attitude qui consiste à braver les convenances et la morale.", ex: "Il parle de la corruption avec un cynisme désarmant." },
            { word: "Damner", type: "v.", def: "Condamner à l'enfer. (Faire damner : faire enrager).", ex: "Cet enfant est insupportable, il ferait damner un saint !" },
            { word: "Debbagouler", type: "v.", def: "Vomir. Fig: Débiter des paroles grossières (très familier).", ex: "Il a débagoulé des injures pendant dix minutes." },
            { word: "Débilitant", type: "adj.", def: "Qui affaiblit physiquement ou moralement.", ex: "Cette chaleur humide est débilitante." },
            { word: "Débonnaire", type: "adj.", def: "Bon jusqu'à la faiblesse, facile à vivre.", ex: "C'est un géant débonnaire qui ne ferait pas de mal à une mouche." },
            { word: "Décadence", type: "n.f.", def: "Acheminement vers la ruine, déclin.", ex: "L'Empire romain a connu une longue période de décadence." },
            { word: "Décent", type: "adj.", def: "Convenable, conforme aux bonnes mœurs.", ex: "Ayez la décence de ne pas rire lors des funérailles." },
            { word: "Déchus", type: "adj.", def: "Qui a perdu sa place, son rang.", ex: "C'est un roi déchu qui vit désormais en exil." },
            { word: "Dédale", type: "n.m.", def: "Labyrinthe. Fig: Ensemble embrouillé.", ex: "Je me suis perdu dans le dédale des procédures administratives." },
            { word: "Défection", type: "n.f.", def: "Action d'abandonner un parti, une cause.", ex: "La défection de ses alliés l'a laissé seul face à l'ennemi." },
            { word: "Défience", type: "n.f.", def: "Méfiance, manque de confiance.", ex: "Il y a une défiance croissante envers les médias." },
            { word: "Déicide", type: "n.m.", def: "Meurtre d'un dieu.", ex: "Nietzsche a proclamé le déicide moderne en disant 'Dieu est mort'." },
            { word: "Délectable", type: "adj.", def: "Qui procure un grand plaisir (goût).", ex: "Ce vin est tout simplement délectable." },
            { word: "Délétère", type: "adj.", def: "Nuisible à la santé, toxique.", ex: "L'ambiance délétère du bureau pousse les gens à la dépression." },
            { word: "Démiurge", type: "n.m.", def: "Dieu créateur (chez Platon). Fig: Créateur d'une œuvre vaste.", ex: "Balzac est le démiurge de la Comédie Humaine." },
            { word: "Démagogie", type: "n.f.", def: "Politique par laquelle on flatte les masses pour gagner leur adhésion.", ex: "Promettre la lune est une forme classique de démagogie." },
            { word: "Dément", type: "adj.", def: "Fou, insensé.", ex: "C'est un projet dément qui va nous coûter une fortune." },
            { word: "Dénégation", type: "n.f.", def: "Action de nier.", ex: "Malgré ses dénégations, tout le monde sait qu'il est coupable." },
            { word: "Déprédation", type: "n.f.", def: "Vol, pillage, dégât commis sur des biens.", ex: "Les troupes ont commis de nombreuses déprédations dans la ville occupée." },
            { word: "Déréliction", type: "n.f.", def: "État d'abandon, de solitude morale (soutenu).", ex: "Il a fini sa vie dans une déréliction totale, oublié de tous." },
            { word: "Dérisoire", type: "adj.", def: "Si petit ou insignifiant qu'il en est risible.", ex: "Il m'a offert une somme dérisoire pour ma voiture." },
            { word: "Despotisme", type: "n.m.", def: "Pouvoir absolu et arbitraire.", ex: "Le peuple s'est révolté contre le despotisme du tyran." },
            { word: "Destituer", type: "v.", def: "Priver quelqu'un de sa charge, de sa fonction.", ex: "Le président a destitué le général après son échec." },
            { word: "Désuet", type: "adj.", def: "Qui n'est plus en usage, démodé.", ex: "Il emploie des termes désuets que plus personne ne comprend." },
            { word: "Déterminisme", type: "n.m.", def: "Doctrine selon laquelle les événements sont liés par une cause nécessaire.", ex: "Il refuse le déterminisme social et croit au libre arbitre." },
            { word: "Détracteur", type: "n.m.", def: "Personne qui dénigre, qui critique la valeur de quelqu'un.", ex: "Même ses plus farouches détracteurs reconnaissent son talent." },
            { word: "Dévoyé", type: "adj.", def: "Qui est sorti du droit chemin (moral).", ex: "Une jeunesse dévoyée par l'oisiveté et le vice." },
            { word: "Dextérité", type: "n.f.", def: "Adresse de la main, habileté.", ex: "Le magicien manipule les cartes avec une dextérité incroyable." },
            { word: "Diabolique", type: "adj.", def: "Digne du diable, très méchant ou très rusé.", ex: "Il a mis au point un plan diabolique pour se venger." },
            { word: "Diaphane", type: "adj.", def: "Translucide, très pâle.", ex: "Sa peau diaphane laissait voir ses veines bleues." },
            { word: "Diatribe", type: "n.f.", def: "Critique virulente et amère.", ex: "Il a lancé une longue diatribe contre la société de consommation." },
            { word: "Dichotomie", type: "n.f.", def: "Division en deux éléments opposés.", ex: "Il y a une dichotomie entre ses paroles publiques et ses actes privés." },
            { word: "Didactique", type: "adj.", def: "Qui vise à instruire, à enseigner.", ex: "Ce musée a une vocation didactique pour les enfants." },
    { word: "Dilatoire", type: "adj.", def: "Qui vise à retarder une décision ou un jugement, à gagner du temps.", ex: "L'avocat a multiplié les manœuvres dilatoires pour repousser le procès." },
    { word: "Dilettante", type: "n. et adj.", def: "Personne qui s'occupe d'une chose par plaisir, sans contrainte et de manière superficielle.", ex: "Il fait de la peinture en dilettante, sans prétendre devenir un grand artiste." },
    { word: "Disette", type: "n.f.", def: "Manque de vivres, pénurie (souvent alimentaire).", ex: "Après la mauvaise récolte, le pays a connu une terrible disette." },
    { word: "Dissension", type: "n.f.", def: "Opposition d'intérêts, de sentiments ; désaccord profond.", ex: "Les dissensions internes ont fini par faire éclater le parti politique." },
    { word: "Dithyrambe", type: "n.m.", def: "Éloge enthousiaste, souvent excessif.", ex: "Il a écrit un dithyrambe à la gloire du nouveau directeur." },
    { word: "Docile", type: "adj.", def: "Qui obéit facilement, qui se laisse conduire.", ex: "C'est un enfant docile qui ne fait jamais de caprices." },
    { word: "Dogmatique", type: "adj.", def: "Qui exprime ses opinions de manière péremptoire, comme des vérités absolues.", ex: "Je déteste son ton dogmatique, il refuse tout débat." },
    { word: "Draconien", type: "adj.", def: "D'une sévérité excessive (loi, mesure).", ex: "L'entreprise a imposé des économies draconiennes pour éviter la faillite." },
    { word: "Dubitatif", type: "adj.", def: "Qui exprime le doute.", ex: "Devant mes explications, il est resté dubitatif, arquant un sourcil sceptique." },
    { word: "Dupe", type: "n.f.", def: "Personne qu'on trompe facilement.", ex: "Je ne serai pas la dupe de vos mensonges une fois de plus." },
    { word: "Éclectisme", type: "n.m.", def: "Attitude de quelqu'un qui s'intéresse à des domaines très divers.", ex: "Son éclectisme le pousse à lire aussi bien des mangas que de la philosophie." },
    { word: "Éden", type: "n.m.", def: "Paradis terrestre. Lieu de délices.", ex: "Cette île tropicale est un véritable éden pour les vacanciers." },
    { word: "Égérie", type: "n.f.", def: "Femme qui inspire un artiste, un créateur ou un homme politique.", ex: "Elle fut l'égérie du peintre pendant sa période bleue." },
    { word: "Égocentrique", type: "adj.", def: "Qui rapporte tout à soi.", ex: "C'est un personnage égocentrique, incapable de s'intéresser aux problèmes des autres." },
    { word: "Égrillard", type: "adj.", def: "D'une gaieté libre et un peu osée (grivois).", ex: "Il a raconté une anecdote égrillarde qui a fait rougir la maîtresse de maison." },
    { word: "Élucubration", type: "n.f.", def: "Théorie laborieuse et extravagante.", ex: "Personne n'a pris au sérieux ses élucubrations sur la fin du monde." },
    { word: "Émacié", type: "adj.", def: "Très maigre (visage), où les os saillent.", ex: "Son visage émacié trahissait une longue maladie." },
    { word: "Émulation", type: "n.f.", def: "Sentiment qui pousse à égaler ou surpasser quelqu'un (concurrence saine).", ex: "Il y a une bonne émulation entre les élèves de cette classe d'élite." },
    { word: "Endémique", type: "adj.", def: "Qui sévit en permanence dans une région (maladie). Propre à un territoire.", ex: "Le chômage est devenu un problème endémique dans cette région industrielle." },
    { word: "Engeance", type: "n.f.", def: "Catégorie de personnes méprisables.", ex: "Méfiez-vous de cette engeance de profiteurs !" },
    { word: "Énigmatique", type: "adj.", def: "Mystérieux, difficile à comprendre.", ex: "La Joconde est célèbre pour son sourire énigmatique." },
    { word: "Entité", type: "n.f.", def: "Chose réelle mais abstraite, essence.", ex: "L'État est une entité juridique distincte des citoyens." },
    { word: "Épicurien", type: "n. et adj.", def: "Qui recherche le plaisir et sait en jouir (surtout de la table).", ex: "C'est un bon vivant, un épicurien qui ne refuse jamais un bon repas." },
    { word: "Épique", type: "adj.", def: "Qui raconte des exploits héroïques. Fig: Mémorable, grandiose.", ex: "Notre voyage de retour sous la tempête fut une aventure épique." },
    { word: "Épistolaire", type: "adj.", def: "Relatif à la correspondance par lettres.", ex: "Ils ont entretenu une relation épistolaire passionnée sans jamais se voir." },
    { word: "Équanimité", type: "n.f.", def: "Égalité d'humeur, sérénité face aux événements.", ex: "Elle a accueilli la mauvaise nouvelle avec une équanimité impressionnante." },
    { word: "Équivoque", type: "adj.", def: "Qui peut s'interpréter de plusieurs manières, ambigu. Louche.", ex: "Sa réponse équivoque nous laisse dans l'incertitude sur ses intentions." },
    { word: "Ergoter", type: "v.", def: "Contester sur des riens, chicaner.", ex: "Il passe son temps à ergoter sur des détails insignifiants du règlement." },
    { word: "Érudit", type: "adj. et n.", def: "Qui a un savoir approfondi.", ex: "C'est un érudit qui connaît l'histoire de la Mésopotamie sur le bout des doigts." },
    { word: "Ésotérique", type: "adj.", def: "Qui ne peut être compris que par des initiés. Obscur.", ex: "Ce poème est trop ésotérique pour le grand public." },
    { word: "Esthète", type: "n.", def: "Personne qui a le culte de la beauté.", ex: "En véritable esthète, il ne supporte pas la laideur des zones industrielles." },
    { word: "Éthéré", type: "adj.", def: "Qui est de la nature de l'éther. Fig: Pur, céleste, immatériel.", ex: "Elle a une beauté éthérée, comme si elle ne touchait pas terre." },
    { word: "Évanescent", type: "adj.", def: "Qui s'amoindrit et disparaît peu à peu.", ex: "Un souvenir évanescent de son enfance lui revint en mémoire." },
    { word: "Exacerber", type: "v.", def: "Rendre plus vif, plus violent (douleur, sentiment).", ex: "Cette remarque n'a fait qu'exacerber sa colère." },
    { word: "Exsangue", type: "adj.", def: "Qui a perdu beaucoup de sang. Fig: Très pâle, sans force.", ex: "L'économie du pays ressort exsangue de cette longue guerre." },
    { word: "Extatique", type: "adj.", def: "Qui est en extase, ravi, émerveillé.", ex: "Il restait là, le regard extatique, devant la beauté du paysage." },
    { word: "Exutoire", type: "n.m.", def: "Moyen de se débarrasser de ce qui gêne (surtout psychologique).", ex: "Le sport est pour lui un exutoire indispensable pour évacuer son stress." },
    { word: "Factice", type: "adj.", def: "Artificiel, faux, pas naturel.", ex: "Son enthousiasme semblait factice, comme s'il jouait un rôle." },
    { word: "Fallacieux", type: "adj.", def: "Qui cherche à tromper, mensonger.", ex: "Il a utilisé un argument fallacieux pour nous convaincre d'investir." },
    { word: "Famélique", type: "adj.", def: "Qui ne mange pas à sa faim, maigre.", ex: "Un chien famélique errait dans les rues à la recherche de nourriture." },
    { word: "Fantasmagorie", type: "n.f.", def: "Abondance d'images bizarres ou illusoires (rêve, hallucination).", ex: "La fièvre lui a donné des visions dignes d'une fantasmagorie." },
    { word: "Fatuité", type: "n.f.", def: "Satisfaction de soi-même qui s'étale avec insolence.", ex: "Il a répondu avec une fatuité insupportable, persuadé d'être le meilleur." },
    { word: "Fébrilité", type: "n.f.", def: "Agitation nerveuse, excitation.", ex: "Une certaine fébrilité régnait dans la salle avant l'annonce des résultats." },
    { word: "Félicité", type: "n.f.", def: "Bonheur calme et complet.", ex: "Ils vivent dans une parfaite félicité depuis leur mariage." },
    { word: "Férule", type: "n.f.", def: "Autorité sévère (être sous la férule de).", ex: "L'école était dirigée sous la férule d'un directeur tyrannique." },
    { word: "Flegme", type: "n.m.", def: "Calme, maîtrise de soi (caractère imperturbable).", ex: "Il a continué à boire son thé avec un flegme tout britannique pendant l'incendie." },
    { word: "Florilège", type: "n.m.", def: "Recueil de pièces choisies (anthologie).", ex: "Ce livre est un florilège des plus beaux poèmes d'amour." },
    { word: "Fomenter", type: "v.", def: "Préparer secrètement (un complot, un trouble).", ex: "Il est accusé de fomenter une rébellion contre le gouvernement." },
    { word: "Fortuit", type: "adj.", def: "Qui arrive par hasard, imprévu.", ex: "Une rencontre fortuite dans le train a changé le cours de sa vie." },
    { word: "Fourvoyer", type: "v.", def: "Égarer, mettre hors du bon chemin. (Se fourvoyer : se tromper).", ex: "Je me suis complètement fourvoyé en lui faisant confiance." },
    { word: "Frasque", type: "n.f.", def: "Écart de conduite, folie de jeunesse.", ex: "Ses frasques nocturnes font régulièrement la une des journaux à scandale." },
    { word: "Frelaté", type: "adj.", def: "Qui a été altéré, falsifié (alcool). Fig: Impur, pas sincère.", ex: "L'air de cette pièce est vicié, et l'ambiance morale y est frelatée." },
    { word: "Frivole", type: "adj.", def: "Qui a du goût pour les choses futiles, léger.", ex: "On le juge frivole parce qu'il ne pense qu'à s'amuser." },
    { word: "Frugal", type: "adj.", def: "Simple et peu abondant (repas, vie).", ex: "Il se contente d'un repas frugal composé de pain et de fromage." },
    { word: "Fugace", type: "adj.", def: "Qui dure très peu, qui disparaît vite.", ex: "J'ai eu une impression fugace de l'avoir déjà vu quelque part." },
    { word: "Fuligineux", type: "adj.", def: "Qui ressemble à la suie, noir. Fig: Obscur, confus.", ex: "Son style littéraire est fuligineux et difficile à déchiffrer." },
    { word: "Fustiger", type: "v.", def: "Battre à coups de bâton. Fig: Critiquer violemment.", ex: "L'éditorialiste a fustigé l'incompétence des services publics." },
    { word: "Gabegie", type: "n.f.", def: "Désordre provenant d'une mauvaise gestion financière.", ex: "Cette entreprise est une véritable gabegie, l'argent est jeté par les fenêtres." },
    { word: "Gageure", type: "n.f.", def: "Action ou projet si difficile qu'il semble impossible (pari risqué).", ex: "Réformer ce système en un mois est une véritable gageure." },
    { word: "Galimatias", type: "n.m.", def: "Discours embrouillé et confus.", ex: "Je n'ai rien compris à son galimatias technique." },
    { word: "Gallicisme", type: "n.m.", def: "Tournure propre à la langue française.", ex: "'C'est moi qui' est un gallicisme courant pour insister." },
    { word: "Gargantuesque", type: "adj.", def: "Gigantesque, digne de Gargantua (repas).", ex: "Le banquet était gargantuesque, il y avait de la nourriture pour une armée." },
    { word: "Gémellité", type: "n.f.", def: "État de jumeaux.", ex: "Leur gémellité est si parfaite qu'on ne peut les distinguer." },
    { word: "Genèse", type: "n.f.", def: "Origine, formation et développement d'une œuvre ou d'une chose.", ex: "Il nous a raconté la genèse de son roman, de la première idée à la publication." },
    { word: "Geôle", type: "n.f.", def: "Prison, cachot.", ex: "Le prisonnier a croupi dix ans dans les geôles du château." },
    { word: "Gérontocratie", type: "n.f.", def: "Gouvernement où le pouvoir appartient aux vieillards.", ex: "Ce parti politique ressemble à une gérontocratie coupée de la jeunesse." },
    { word: "Gloriole", type: "n.f.", def: "Vanité tirée de petites choses.", ex: "Il ne cherche que la gloriole et les applaudissements faciles." },
    { word: "Glose", type: "n.f.", def: "Note explicative, commentaire. (Gloser : critiquer).", ex: "Ce texte est accompagné de nombreuses gloses pour en éclairer le sens." },
    { word: "Gourmé", type: "adj.", def: "Qui est affecté de gravité, raide, guindé.", ex: "Il est toujours très gourmé dans ses réceptions mondaines." },
    { word: "Grandiloquent", type: "adj.", def: "Qui parle avec une emphase excessive.", ex: "Son discours grandiloquent a fini par ennuyer l'auditoire." },
    { word: "Grégaire", type: "adj.", def: "Qui vit en troupeau. Fig: Qui suit docilement le groupe.", ex: "L'instinct grégaire pousse les gens à acheter tous la même chose." },
    { word: "Grief", type: "n.m.", def: "Sujet de plainte contre quelqu'un.", ex: "Il a exposé ses griefs contre la direction lors de la réunion." },
    { word: "Grivois", type: "adj.", def: "Qui est un peu osé, libre (chanson, propos).", ex: "Il a chanté une chanson grivoise à la fin du banquet." },
    { word: "Hâve", type: "adj.", def: "Amaigri et pâli par la faim, la fatigue ou la maladie.", ex: "Il est revenu du front avec le visage hâve et les yeux cernés." },
    { word: "Hébétude", type: "n.f.", def: "Engourdissement des facultés intellectuelles, stupeur.", ex: "Il est resté dans un état d'hébétude après le choc de l'accident." },
    { word: "Hédonisme", type: "n.m.", def: "Doctrine qui fait du plaisir le but de la vie.", ex: "Notre société de consommation prône un hédonisme sans limites." },
    { word: "Hégémonie", type: "n.f.", def: "Suprématie, domination d'une puissance sur les autres.", ex: "Les États-Unis ont exercé une hégémonie culturelle sur l'Occident au XXe siècle." },
    { word: "Hérésie", type: "n.f.", def: "Opinion contraire à la doctrine établie. Absurdité.", ex: "Boire du champagne avec des glaçons est une hérésie pour un puriste." },
    { word: "Hermétique", type: "adj.", def: "Parfaitement clos. Fig: Impénétrable, difficile à comprendre.", ex: "Sa poésie est hermétique, seuls les spécialistes l'apprécient." },
    { word: "Hétéroclite", type: "adj.", def: "Fait de pièces et de morceaux disparates.", ex: "Sa maison est meublée d'un assemblage hétéroclite d'objets trouvés." },
    { word: "Hétérodoxe", type: "adj.", def: "Qui n'est pas conforme à la doctrine officielle (opposé à orthodoxe).", ex: "Ses idées économiques hétérodoxes dérangent l'establishment." },
    { word: "Hiératique", type: "adj.", def: "Qui concerne les choses sacrées. Fig: Raide, figé, majestueux.", ex: "Elle se tenait droite avec une pose hiératique digne d'une statue égyptienne." },
    { word: "Hirsute", type: "adj.", def: "Qui a les cheveux/poils hérissés et désordonnés.", ex: "Il est sorti du lit l'air hirsute et mal réveillé." },
    { word: "Histrion", type: "n.m.", def: "Comédien qui jouait des farces grossières. Fig: M'as-tu-vu.", ex: "Cesse de faire l'histrion pour attirer l'attention !" },
    { word: "Holistique", type: "adj.", def: "Qui s'intéresse à son objet dans sa globalité.", ex: "La médecine holistique soigne le patient entier, pas seulement le symptôme." },
    { word: "Homélie", type: "n.f.", def: "Sermon, instruction religieuse. Fig: Discours moralisateur ennuyeux.", ex: "Il nous a servi une longue homélie sur les dangers de l'alcool." },
    { word: "Honi", type: "adj.", def: "Couverts de honte (dans l'expression 'Honi soit qui mal y pense').", ex: "Honi soit qui mal y pense !" },
    { word: "Huppé", type: "adj.", def: "Qui a une huppe. Fig: Riche, chic, de la haute société.", ex: "Ils habitent dans un quartier huppé de la capitale." },
    { word: "Hybride", type: "adj.", def: "Qui provient du croisement de deux espèces/genres différents.", ex: "C'est un véhicule hybride qui fonctionne à l'essence et à l'électricité." },
    { word: "Hyménée", type: "n.m.", def: "Mariage (langue littéraire).", ex: "Ils ont célébré leur hyménée dans la plus stricte intimité." },
    { word: "Hypocondriaque", type: "adj.", def: "Qui a une peur obsédante des maladies.", ex: "Le moindre rhume persuade cet hypocondriaque qu'il va mourir." },
    { word: "Iconoclaste", type: "adj.", def: "Qui détruit les images saintes. Fig: Qui s'attaque aux traditions.", ex: "Son style iconoclaste bouscule les codes du cinéma traditionnel." },
    { word: "Idylle", type: "n.f.", def: "Petite aventure amoureuse, tendre et naïve.", ex: "Leur idylle d'été n'a duré que le temps des vacances." },
    { word: "Ignare", type: "adj.", def: "Complètement ignorant, inculte.", ex: "Il est ignare en matière d'art et confond Monet et Manet." },
    { word: "Illicite", type: "adj.", def: "Interdit par la loi ou la morale.", ex: "Le commerce illicite de l'ivoire est sévèrement puni." },
    { word: "Illustre", type: "adj.", def: "Très célèbre et glorieux.", ex: "Un illustre inconnu a remporté le premier prix." },
    { word: "Immanent", type: "adj.", def: "Qui est contenu dans la nature d'un être, qui ne vient pas de l'extérieur.", ex: "La justice est-elle immanente à l'homme ou une construction sociale ?" },
    { word: "Immiscer (s')", type: "v.", def: "S'ingérer, intervenir mal à propos dans les affaires d'autrui.", ex: "Je ne veux pas m'immiscer dans votre vie privée, mais je m'inquiète." },
    { word: "Immuable", type: "adj.", def: "Qui ne change pas.", ex: "Les lois de la physique sont immuables." },
    { word: "Impavide", type: "adj.", def: "Qui ne connaît pas la peur, calme face au danger.", ex: "Il est resté impavide face aux menaces de ses agresseurs." },
    { word: "Impérieux", type: "adj.", def: "Auxquel on ne peut résister (besoin). Autoritaire (ton).", ex: "Il a ressenti un besoin impérieux de quitter la salle." },
    { word: "Impétueux", type: "adj.", def: "Dont l'impulsion est violente et rapide.", ex: "C'est un torrent impétueux qui emporte tout sur son passage." },
    { word: "Implacable", type: "adj.", def: "Qu'on ne peut apaiser, acharné.", ex: "C'est une logique implacable contre laquelle on ne peut rien dire." },
    { word: "Implicite", type: "adj.", def: "Qui est contenu dans une proposition sans être formulé.", ex: "Il y a une clause implicite de confidentialité dans ce contrat." },
    { word: "Impondérable", type: "n.m.", def: "Élément imprévisible qu'on ne peut peser ou mesurer.", ex: "Nous avons échoué à cause des impondérables de la météo." },
    { word: "Importun", type: "adj.", def: "Qui dérange, qui arrive mal à propos (fâcheux).", ex: "Un visiteur importun a interrompu notre réunion." },
    { word: "Imposture", type: "n.f.", def: "Tromperie d'une personne qui se fait passer pour ce qu'elle n'est pas.", ex: "Son titre de docteur n'est qu'une imposture, il n'a jamais étudié." },
    { word: "Imprécation", type: "n.f.", def: "Malédiction lancée contre quelqu'un.", ex: "La sorcière lançait des imprécations contre les villageois." },
    { word: "Impromptu", type: "adj.", def: "Qui n'est pas préparé, improvisé.", ex: "Il a prononcé un discours impromptu très émouvant." },
    { word: "Impudent", type: "adj.", def: "Effronté, qui manque de pudeur ou de respect.", ex: "Ce jeune homme est impudent de parler ainsi à ses aînés." },
    { word: "Impuni", type: "adj.", def: "Qui n'a pas été puni.", ex: "Ce crime ne restera pas impuni." },
    { word: "Imputrescible", type: "adj.", def: "Qui ne peut pas pourrir.", ex: "Le bois de teck est imputrescible, idéal pour les bateaux." },
    { word: "Inanité", type: "n.f.", def: "Caractère de ce qui est vide, vain, inutile.", ex: "Il a réalisé l'inanité de ses efforts pour la faire changer d'avis." },
    { word: "Inaudible", type: "adj.", def: "Qu'on ne peut pas entendre.", ex: "Sa voix était inaudible à cause du bruit de la circulation." },
    { word: "Incantatoire", type: "adj.", def: "Qui a le caractère d'une incantation, envoûtant.", ex: "Son discours avait un rythme incantatoire qui fascinait la foule." },
    { word: "Incartade", type: "n.f.", def: "Léger écart de conduite, petite offense.", ex: "On lui a pardonné cette incartade car c'est sa première erreur." },
    { word: "Incoercible", type: "adj.", def: "Qu'on ne peut contenir ou arrêter.", ex: "Il fut pris d'un fou rire incoercible en plein enterrement." },
    { word: "Incommensurable", type: "adj.", def: "Qu'on ne peut mesurer, immense.", ex: "L'univers est d'une grandeur incommensurable." },
    { word: "Incongru", type: "adj.", def: "Qui n'est pas convenable, déplacé, insolite.", ex: "Sa tenue de clown était incongrue dans cette soirée de gala." },
    { word: "Indécent", type: "adj.", def: "Qui choque la pudeur ou les convenances.", ex: "Il gagne des sommes indécentes alors que ses employés sont mal payés." },
    { word: "Indigent", type: "adj.", def: "Qui manque du nécessaire, pauvre. Fig: Pauvre en esprit.", ex: "Son raisonnement est indigent et manque de culture." },
    { word: "Indolent", type: "adj.", def: "Qui évite l'effort, mou, apathique.", ex: "Il mène une vie indolente sous les tropiques." },
    { word: "Ineffable", type: "adj.", def: "Qu'on ne peut exprimer par des mots.", ex: "Une joie ineffable l'a envahi à la naissance de son fils." },
    { word: "Inéluctable", type: "adj.", def: "Contre quoi on ne peut lutter, inévitable.", ex: "Le vieillissement est un processus inéluctable." },
    { word: "Inepte", type: "adj.", def: "Absurde, stupide.", ex: "C'est une remarque inepte qui ne fait pas avancer le débat." },
    { word: "Inerte", type: "adj.", def: "Sans mouvement, sans activité, sans vie.", ex: "Le corps est resté inerte malgré les tentatives de réanimation." },
    { word: "Inexorable", type: "adj.", def: "Qu'on ne peut fléchir (prière) ; fatal.", ex: "Le temps est un juge inexorable." },
    { word: "Infamie", type: "n.f.", def: "Action déshonorante, honteuse.", ex: "Trahir son pays est la pire des infamies." },
    { word: "Infatuer", type: "v.", def: "Entêter quelqu'un d'une idée, lui donner une trop haute opinion de lui-même.", ex: "Le succès l'a complètement infatué, il est devenu arrogant." },
    { word: "Infime", type: "adj.", def: "Très petit, minime.", ex: "Il n'y a qu'une chance infime que cela fonctionne." },
    { word: "Inflexible", type: "adj.", def: "Qui ne se courbe pas. Fig: Qui ne se laisse pas émouvoir.", ex: "Il est resté inflexible sur le prix malgré nos supplications." },
    { word: "Inhérent", type: "adj.", def: "Qui appartient essentiellement à un être, inséparable.", ex: "Le risque est inhérent au métier de pompier." },
    { word: "Inique", type: "adj.", def: "Très injuste.", ex: "Ce verdict inique a révolté l'opinion publique." },
    { word: "Initier", type: "v.", def: "Admettre à la connaissance de mystères. Commencer.", ex: "Mon père m'a initié à la musique classique dès mon plus jeune âge." },
    { word: "Injonction", type: "n.f.", def: "Ordre formel et impératif.", ex: "Il a obéi à l'injonction du tribunal de payer ses dettes." },
    { word: "Inné", type: "adj.", def: "Que l'on possède en naissant (naturel).", ex: "Il a un sens inné du rythme." },
    { word: "Inoculer", type: "v.", def: "Introduire un germe (vaccin). Fig: Transmettre (idée, vice).", ex: "Il a tenté d'inoculer le doute dans l'esprit des jurés." },
    { word: "Inopiné", type: "adj.", def: "Qui arrive sans qu'on s'y soit attendu.", ex: "Une visite inopinée de l'inspecteur a semé la panique." },
    { word: "Insatiable", type: "adj.", def: "Qui ne peut être rassasié (faim, désir).", ex: "Il a une soif insatiable de connaissances." },
    { word: "Insidieux", type: "adj.", def: "Qui cherche à faire tomber dans un piège. (Maladie: qui progresse sournoisement).", ex: "La propagande est un poison insidieux qui s'infiltre partout." },
    { word: "Insipide", type: "adj.", def: "Qui n'a pas de goût/saveur. Fig: Ennuyeux, sans esprit.", ex: "La conversation était aussi insipide que le repas." },
    { word: "Insolite", type: "adj.", def: "Qui étonne par son caractère inhabituel.", ex: "C'est un objet insolite dont on ignore l'usage." },
    { word: "Insulaire", type: "adj.", def: "Relatif aux îles.", ex: "Les habitants ont gardé une mentalité insulaire très fermée." },
    { word: "Intangible", type: "adj.", def: "Qu'on ne doit pas toucher, porter atteinte (sacré).", ex: "Le droit de propriété est considéré comme intangible." },
    { word: "Intègre", type: "adj.", def: "D'une honnêteté incorruptible.", ex: "C'est un juge intègre qu'on ne peut pas acheter." },
    { word: "Intempestif", type: "adj.", def: "Qui se produit à contretemps, mal à propos.", ex: "Ce bruit intempestif m'empêche de me concentrer." },
    { word: "Interlope", type: "adj.", def: "Louche, suspect (trafic, milieu).", ex: "Il fréquente les bars interlopes du port." },
    { word: "Intransigeant", type: "adj.", def: "Qui ne fait aucune concession.", ex: "Il est intransigeant sur les principes de sécurité." },
    { word: "Intrinsèque", type: "adj.", def: "Qui est intérieur et propre à l'objet, indépendant des facteurs externes.", ex: "La valeur intrinsèque de ce bijou est faible, mais sa valeur sentimentale est grande." },
    { word: "Invectiver", type: "v.", def: "Insulter, accabler d'injures.", ex: "Les conducteurs ont commencé à s'invectiver après l'accrochage." },
    { word: "Invétéré", type: "adj.", def: "Qui est ancré par l'habitude (fumeur, joueur).", ex: "C'est un célibataire invétéré qui refuse de se marier." },
    { word: "Irascible", type: "adj.", def: "Qui se met facilement en colère.", ex: "Ne le dérangez pas, il est très irascible le matin." },
    { word: "Irrévérencieux", type: "adj.", def: "Qui manque de respect.", ex: "Il a fait une blague irrévérencieuse sur le pape." },
    { word: "Jactance", type: "n.f.", def: "Vantardise bruyante, arrogance verbale.", ex: "Sa jactance agace tout le monde, il ne fait que parler de lui." },
    { word: "Jalonner", type: "v.", def: "Marquer la direction, l'itinéraire.", ex: "Sa carrière est jalonnée de succès retentissants." },
    { word: "Jargon", type: "n.m.", def: "Langage technique incompréhensible pour les autres.", ex: "Le jargon médical est souvent obscur pour les patients." },
    { word: "Jérémiade", type: "n.f.", def: "Plainte sans fin et importune.", ex: "Arrête tes jérémiades et agis !" },
    { word: "Joug", type: "n.m.", def: "Pièce de bois pour les bœufs. Fig: Contrainte matérielle ou morale, servitude.", ex: "Le peuple a fini par secouer le joug du tyran." },
    { word: "Jubiler", type: "v.", def: "Éprouver une joie vive.", ex: "Il jubilait à l'idée de partir en vacances." },
    { word: "Judicieux", type: "adj.", def: "Qui a du bon sens, pertinent.", ex: "C'est une remarque judicieuse qui nous fait gagner du temps." },
    { word: "Juguler", type: "v.", def: "Arrêter le développement de quelque chose (épidémie, crise).", ex: "Le gouvernement tente de juguler l'inflation." },
    { word: "Jurer", type: "v.", def: "Affirmer par serment. Ne pas aller ensemble (couleurs).", ex: "Ce vert jure affreusement avec ce bleu." }
    { word: "Kafkaïen", type: "adj.", def: "Qui rappelle l'atmosphère absurde et cauchemardesque des œuvres de Kafka.", ex: "L'administration m'a plongé dans un dossier kafkaïen où chaque document en annulait un autre." },
    { word: "Laconique", type: "adj.", def: "Qui s'exprime en peu de mots ; bref et concis.", ex: "Il m'a envoyé un message laconique pour dire qu'il ne viendrait pas." },
    { word: "Ladre", type: "adj. et n.", def: "D'une avarice sordide.", ex: "Ce vieux ladre refuse même de chauffer sa maison en plein hiver." },
    { word: "Lambique", type: "adj.", def: "Alambiqué, compliqué, qui manque de simplicité.", ex: "Son raisonnement lambiqué a fini par perdre tout l'auditoire." },
    { word: "Languide", type: "adj.", def: "Qui est dans un état de langueur, de faiblesse ou de mélancolie.", ex: "Elle jetait des regards languides vers l'horizon, perdue dans ses pensées." },
    { word: "Lapidaire", type: "adj.", def: "Relatif à la taille des pierres. Fig: Bref et frappant (style).", ex: "Il a clos le débat d'une sentence lapidaire qui n'appelait aucune réponse." },
    { word: "Larcins", type: "n.m. pl.", def: "Petits vols commis sans violence.", ex: "Il a commencé par quelques larcins à l'étalage avant de devenir un vrai malfrat." },
    { word: "Lascif", type: "adj.", def: "Qui incite à la volupté, qui exprime le désir sexuel.", ex: "Elle s'étirait sur le canapé d'une manière lascive." },
    { word: "Latent", type: "adj.", def: "Qui reste caché mais peut apparaître à tout moment.", ex: "Il existe un conflit latent entre les deux départements de l'entreprise." },
    { word: "Latitude", type: "n.f.", def: "Liberté d'action, possibilité d'agir à sa guise.", ex: "Le directeur m'a laissé toute latitude pour organiser ce projet." },
    { word: "Léonin", type: "adj.", def: "Qui appartient au lion. Fig: Se dit d'un partage où l'un prend la plus grosse part.", ex: "Ils ont signé un contrat léonin qui ne profite qu'à la multinationale." },
    { word: "Léthargie", type: "n.f.", def: "Sommeil profond et prolongé. Fig: État d'inaction, d'apathie.", ex: "L'économie du pays semble plongée dans une profonde léthargie." },
    { word: "Lévitation", type: "n.f.", def: "Fait de s'élever au-dessus du sol sans appui.", ex: "Le fakir prétendait être en état de lévitation pendant sa méditation." },
    { word: "Libidineux", type: "adj.", def: "Qui est porté sur les plaisirs sexuels de manière grossière.", ex: "Ce vieillard libidineux ne pouvait s'empêcher de dévisager les jeunes femmes." },
    { word: "Licencieux", type: "adj.", def: "Qui choque la pudeur, qui est contraire à la décence.", ex: "Certaines scènes de ce film ont été jugées trop licencieuses pour la télévision." },
    { word: "Lige", type: "adj.", def: "Féodal: dévoué sans réserve (homme lige).", ex: "Il est l'homme lige du président, prêt à tout pour le servir." },
    { word: "Liminaire", type: "adj.", def: "Placé au début d'un ouvrage, d'un discours (en guise d'introduction).", ex: "Il a fait quelques remarques liminaires avant d'entrer dans le vif du sujet." },
    { word: "Limpide", type: "adj.", def: "Clair et transparent. Fig: Facile à comprendre.", ex: "Ses explications sont limpides, même pour un néophyte." },
    { word: "Litanie", type: "n.f.", def: "Prière longue et répétitive. Fig: Énumération longue et ennuyeuse.", ex: "Il nous a servi la litanie habituelle de ses malheurs." },
    { word: "Litige", type: "n.m.", def: "Contestation donnant lieu à un procès.", ex: "L'avocat tente de régler le litige à l'amiable pour éviter le tribunal." },
    { word: "Lobby", type: "n.m.", def: "Groupe de pression cherchant à influencer les décisions politiques.", ex: "Le lobby du tabac est très puissant auprès du Parlement." },
    { word: "Locution", type: "n.f.", def: "Groupe de mots figé ayant une fonction grammaticale.", ex: "'Tout à coup' est une locution adverbiale." },
    { word: "Logorrhée", type: "n.f.", def: "Flux de paroles inutiles, besoin irrésistible de parler.", ex: "Sa logorrhée est épuisante, il ne s'arrête jamais pour respirer." },
    { word: "Loisir", type: "n.m.", def: "Temps libre. (À loisir : tout le temps nécessaire).", ex: "Vous pourrez examiner ce document à loisir ce week-end." },
    { word: "Lubie", type: "n.f.", def: "Caprice extravagant, idée bizarre et passagère.", ex: "Il s'est mis en tête d'élever des lamas, c'est sa dernière lubie." },
    { word: "Lucratif", type: "adj.", def: "Qui rapporte de l'argent, du profit.", ex: "Elle a quitté son poste pour une activité bien plus lucrative." },
    { word: "Ludique", type: "adj.", def: "Qui relève du jeu.", ex: "Cette méthode d'apprentissage ludique plaît beaucoup aux enfants." },
    { word: "Lugubre", type: "adj.", def: "Qui évoque la mort, qui est d'une tristesse profonde.", ex: "Le vent hurlait dans les ruines, créant une ambiance lugubre." },
    { word: "Lustre", type: "n.m.", def: "Éclat, gloire. Période de cinq ans.", ex: "Ce vieux palais a perdu son lustre d'antan." },
    { word: "Laconisme", type: "n.m.", def: "Manière de s'exprimer avec concision.", ex: "Le laconisme de sa réponse m'a un peu déconcerté." },
    { word: "Lésiner", type: "v.", def: "Épargner avec avarice.", ex: "Il ne faut pas lésiner sur la qualité des matériaux." },
    { word: "Léthifère", type: "adj.", def: "Qui cause la mort (poétique ou médical).", ex: "Le serpent lui a infligé une morsure léthifère." },
    { word: "Lippu", type: "adj.", def: "Qui a une grosse lèvre inférieure.", ex: "Il affichait un air boudeur, le visage lippu." },
    { word: "Lustrer", type: "v.", def: "Rendre luisant.", ex: "Il faut lustrer les chaussures pour la cérémonie." },
    { word: "Machiavélique", type: "adj.", def: "Qui utilise la ruse et la perfidie pour arriver à ses fins.", ex: "Il a élaboré un plan machiavélique pour évincer son rival." },
    { word: "Magnanimité", type: "n.f.", def: "Grandeur d'âme, générosité (surtout envers un ennemi).", ex: "Le vainqueur a fait preuve de magnanimité en épargnant les prisonniers." },
    { word: "Malicieux", type: "adj.", def: "Qui aime s'amuser aux dépens d'autrui, mais sans méchanceté.", ex: "Il avait un petit sourire malicieux au coin des lèvres." },
    { word: "Malséant", type: "adj.", def: "Qui n'est pas convenable, qui choque les usages.", ex: "Il est malséant de couper la parole à quelqu'un qui vous conseille." },
    { word: "Mandement", type: "n.m.", def: "Instruction écrite d'un évêque à ses fidèles.", ex: "L'évêque a publié un mandement pour le carême." },
    { word: "Manichéen", type: "adj.", def: "Qui voit le monde de façon binaire (le bien contre le mal), sans nuances.", ex: "Son analyse de la situation est trop manichéenne pour être juste." },
    { word: "Mansuétude", type: "n.f.", def: "Disposition à pardonner, douceur, indulgence.", ex: "Le professeur a fait preuve de mansuétude en ne notant pas ce devoir." },
    { word: "Marasme", type: "n.f.", def: "Affaiblissement extrême. Fig: Stagnation, arrêt de l'activité.", ex: "Le commerce local est plongé dans le marasme depuis la crise." },
    { word: "Marginal", type: "adj.", def: "Qui est situé en bordure. Fig: Qui vit en dehors des normes sociales.", ex: "C'est un artiste marginal qui refuse tout contact avec les galeries." },
    { word: "Matois", type: "adj.", def: "Ruzé, fin, qui cache sa finesse sous des dehors simples.", ex: "Ce paysan matois n'allait pas se laisser tromper par le marchand." },
    { word: "Maudire", type: "v.", def: "Vouer au malheur, détester profondément.", ex: "Je maudis le jour où j'ai accepté cette proposition." },
    { word: "Méandre", type: "n.m.", def: "Sinuosité d'un fleuve. Fig: Détours compliqués d'une affaire ou de la pensée.", ex: "Il s'est perdu dans les méandres de l'administration fiscale." },
    { word: "Mécène", type: "n.m.", def: "Personne riche qui aide les artistes ou les savants.", ex: "Grâce à son mécène, le jeune pianiste a pu enregistrer son premier album." },
    { word: "Médiéviste", type: "n.", def: "Spécialiste de l'histoire ou de la littérature du Moyen Âge.", ex: "Ce professeur est un médiéviste reconnu dans le monde entier." },
    { word: "Méduser", type: "v.", def: "Pétrifier de stupeur, étonner au plus haut point.", ex: "Sa réponse brutale a médusé toute l'assistance." },
    { word: "Mégalomane", type: "adj. et n.", def: "Qui a un désir excessif de gloire, de puissance.", ex: "C'est un projet mégalomane qui coûtera des milliards à la ville." },
    { word: "Mélopée", type: "n.f.", def: "Chant monotone et triste.", ex: "On entendait au loin la mélopée des pêcheurs sur le fleuve." },
    { word: "Mémorable", type: "adj.", def: "Digne d'être conservé dans la mémoire.", ex: "Ce fut une soirée mémorable dont nous parlerons encore longtemps." },
    { word: "Mendicité", type: "n.f.", def: "Fait de demander l'aumône.", ex: "La mendicité est interdite dans les couloirs du métro." },
    { word: "Ménager", type: "v.", def: "Traiter avec soin, utiliser avec économie. Ne pas froisser.", ex: "Il faut ménager sa santé si l'on veut tenir sur la durée." },
    { word: "Mercantile", type: "adj.", def: "Relatif au commerce. Fig: Qui n'est inspiré que par le gain.", ex: "Il a une mentalité mercantile, tout se vend et tout s'achète pour lui." },
    { word: "Mercure", type: "n.m.", def: "Métal liquide. Fig: Messager (d'après le dieu romain).", ex: "Le Mercure de la bande courut porter la nouvelle au chef." },
    { word: "Méridional", type: "adj.", def: "Qui est au sud.", ex: "Il a conservé son accent méridional bien qu'il vive à Paris." },
    { word: "Métamorphose", type: "n.f.", def: "Changement de forme ou de nature.", ex: "La chenille subit une métamorphose pour devenir un papillon." },
    { word: "Méticuleux", type: "adj.", def: "Qui apporte un soin extrême aux détails.", ex: "C'est un horloger méticuleux qui ne laisse rien au hasard." },
    { word: "Miasme", type: "n.m.", def: "Émanation fétide provenant de matières en décomposition.", ex: "Des miasmes s'élevaient des marécages environnants." },
    { word: "Mielleux", type: "adj.", def: "Qui a la douceur du miel. Fig: Hypocrite, doucereux.", ex: "Je n'aime pas son ton mielleux, il cherche sûrement à obtenir quelque chose." },
    { word: "Misanthrope", type: "n.", def: "Personne qui déteste le genre humain.", ex: "Ce vieil ermite est un misanthrope qui fuit toute compagnie." },
    { word: "Misogyne", type: "adj. et n.", def: "Qui méprise ou déteste les femmes.", ex: "Ses plaisanteries misogynes ont choqué ses collègues féminines." },
    { word: "Mitigé", type: "adj.", def: "Qui est atténué, modéré. Qui comporte des réserves.", ex: "L'accueil de son nouveau film a été plutôt mitigé par la critique." },
    { word: "Mnémotechnique", type: "adj.", def: "Qui aide la mémoire.", ex: "Utiliser une phrase pour retenir les planètes est un moyen mnémotechnique." },
    { word: "Modicité", type: "n.f.", def: "Caractère de ce qui est médiocre, peu élevé (prix).", ex: "La modicité de son salaire ne lui permet pas de voyager." },
    { word: "Modus vivendi", type: "loc. m.", def: "Accord transactionnel permettant à deux parties en conflit de cohabiter.", ex: "Ils ont trouvé un modus vivendi pour continuer à travailler ensemble." },
    { word: "Moite", type: "adj.", def: "Légèrement humide (mains, air).", ex: "L'air de la jungle était chaud et moite." },
    { word: "Monacal", type: "adj.", def: "Propre aux moines (austère, silencieux).", ex: "Il mène une vie monacale, entièrement dévouée à l'étude." },
    { word: "Monolithe", type: "n.m.", def: "Bloc de pierre d'une seule pièce. Fig: Groupe très soudé et uniforme.", ex: "Ce parti politique n'est plus le monolithe qu'il était autrefois." },
    { word: "Morgue", type: "n.f.", def: "Lieu où l'on dépose les morts. Fig: Attitude hautaine et méprisante.", ex: "Il nous a reçus avec une morgue insupportable." },
    { word: "Morose", type: "adj.", def: "D'une humeur triste et chagrinée.", ex: "Le temps gris me rend morose." },
    { word: "Mortifier", type: "v.", def: "Imposer des souffrances physiques/morales. Humilier.", ex: "Il a été mortifié par les critiques publiques de son supérieur." },
    { word: "Muable", type: "adj.", def: "Sujet au changement.", ex: "L'opinion publique est muable au gré des événements." },
    { word: "Muer", type: "v.", def: "Changer de peau ou de voix. Se transformer.", ex: "La voix de l'adolescent commence à muer." },
    { word: "Multiforme", type: "adj.", def: "Qui présente des formes variées.", ex: "La menace est multiforme et difficile à cerner." },
    { word: "Munificence", type: "n.f.", def: "Générosité magnifique et royale.", ex: "Le roi a étalé sa munificence lors du banquet." },
    { word: "Muse", type: "n.f.", def: "Divinité de l'inspiration. Fig: Femme qui inspire un artiste.", ex: "Elle est la muse qui a inspiré tous ses plus beaux poèmes." },
    { word: "Mutisme", type: "n.m.", def: "État de celui qui refuse de parler.", ex: "Le suspect s'est enfermé dans un mutisme total pendant l'interrogatoire." },
    { word: "Mysticisme", type: "n.m.", def: "Croyance en la possibilité d'une union directe de l'âme avec Dieu.", ex: "Il s'est tourné vers le mysticisme à la fin de sa vie." },
    { word: "Mythomane", type: "n. et adj.", def: "Qui ne peut s'empêcher de mentir et d'inventer des histoires.", ex: "Ne croyez pas ce qu'il dit, c'est un mythomane célèbre dans le quartier." },
    { word: "Nadir", type: "n.m.", def: "Point de la sphère céleste opposé au zénith. Fig: Le point le plus bas.", ex: "Cette défaite marque le nadir de sa carrière politique." },
    { word: "Narcissisme", type: "n.m.", def: "Admiration excessive de soi-même.", ex: "Le narcissisme de certains influenceurs est flagrant sur les réseaux sociaux." },
    { word: "Nébuleux", type: "adj.", def: "Plein de nuages. Fig: Obscur, confus, vague.", ex: "Ses projets pour l'avenir sont encore très nébuleux." },
    { word: "Néophyte", type: "n.", def: "Nouveau converti. Débutant dans un domaine.", ex: "En tant que néophyte en cuisine, j'ai commencé par des recettes simples." },
    { word: "Népotisme", type: "n.m.", def: "Faveur accordée par un homme au pouvoir à sa famille ou ses amis.", ex: "Le maire a été accusé de népotisme après avoir embauché son fils." },
    { word: "Névrose", type: "n.f.", def: "Trouble psychique dont le sujet a conscience et qui n'altère pas la personnalité.", ex: "Il souffre d'une névrose d'angoisse qui l'empêche de sortir." },
    { word: "Nihilisme", type: "n.m.", def: "Idéologie niant toute vérité morale ou valeur absolue.", ex: "Sa philosophie est teintée d'un nihilisme désespérant." },
    { word: "Nobiliaire", type: "adj.", def: "Qui appartient à la noblesse.", ex: "Il est très fier de sa particule et de ses titres nobiliaires." },
    { word: "Noctambule", type: "n. et adj.", def: "Qui aime se promener ou s'amuser la nuit.", ex: "C'est un noctambule qui ne rentre jamais chez lui avant l'aube." },
    { word: "Nostalgie", type: "n.f.", def: "Regret mélancolique du passé ou du pays natal.", ex: "En revoyant ces vieilles photos, une pointe de nostalgie m'a envahi." },
    { word: "Notoire", type: "adj.", def: "Connu de tous de manière évidente.", ex: "C'est un menteur notoire, personne ne le croit plus." },
    { word: "Novateur", type: "adj. et n.", def: "Qui introduit des nouveautés.", ex: "C'est un artiste novateur qui a révolutionné la sculpture moderne." },
    { word: "Nuance", type: "n.f.", def: "Différence légère entre deux choses voisines.", ex: "Il y a une nuance subtile entre ces deux synonymes." },
    { word: "Nullité", type: "n.f.", def: "Caractère de ce qui est nul. Personne sans talent.", ex: "Ce film est d'une nullité affligeante." },
    { word: "Obédience", type: "n.f.", def: "Soumission à un supérieur. Fig: Appartenance à un courant (politique, religieux).", ex: "Il appartient à une obédience maçonnique très ancienne." },
    { word: "Obérer", type: "v.", def: "Accabler de dettes, compromettre gravement.", ex: "Ces dépenses inutiles vont obérer le budget de la commune pour des années." },
    { word: "Objectif", type: "adj.", def: "Qui repose sur des faits, sans parti pris.", ex: "Un juge doit rester objectif malgré ses sentiments personnels." },
    { word: "Objurgation", type: "n.f.", def: "Prière pressante, reproche vif pour détourner quelqu'un d'agir.", ex: "Malgré les objurgations de ses amis, il a décidé de partir seul." },
    { word: "Oblat", type: "n.m.", def: "Personne qui s'est donnée à un monastère.", ex: "Il vit en tant qu'oblat sans avoir prononcé de vœux monastiques." },
    { word: "Obliquer", type: "v.", def: "Changer de direction.", ex: "Le sentier oblique soudainement vers la droite." },
    { word: "Oblitérer", type: "v.", def: "Effacer, boucher. Timbrer.", ex: "Le temps a fini par oblitérer ses souvenirs de jeunesse." },
    { word: "Obnubiler", type: "v.", def: "Obséder au point de troubler la vue ou l'esprit.", ex: "Il est obnubilé par l'idée de retrouver son trésor perdu." },
    { word: "Obséquieux", type: "adj.", def: "Qui exagère les marques de respect par servilité ou hypocrisie.", ex: "Ce serveur obséquieux espère sans doute un gros pourboire." },
    { word: "Obsolescence", type: "n.f.", def: "Fait de devenir périmé.", ex: "L'obsolescence programmée pousse les consommateurs à racheter du matériel neuf." },
    { word: "Obstination", type: "n.f.", def: "Attachement opiniâtre à une idée, une résolution.", ex: "Son obstination à vouloir finir ce puzzle de 5000 pièces est admirable." },
    { word: "Obtempérer", type: "v.", def: "Obéir à un ordre, se soumettre.", ex: "Le conducteur a fini par obtempérer aux ordres de la police." },
    { word: "Obtus", type: "adj.", def: "Dont l'angle est supérieur à 90°. Fig: Lent à comprendre.", ex: "Il est trop obtus pour comprendre cette blague au second degré." },
    { word: "Obvier", type: "v.", def: "Prendre les mesures nécessaires pour faire obstacle à quelque chose.", ex: "Il faut obvier à ces difficultés avant qu'elles ne s'aggravent." },
    { word: "Occulte", type: "adj.", def: "Caché, secret, mystérieux.", ex: "Il semble être influencé par des forces occultes." },
    { word: "Odieux", type: "adj.", def: "Qui excite la haine, le dégoût, l'indignation.", ex: "Il a eu un comportement odieux envers sa pauvre mère." },
    { word: "Oisif", type: "adj. et n.", def: "Qui n'a pas d'occupation, qui ne travaille pas.", ex: "Il mène une existence oisive grâce à sa fortune personnelle." },
    { word: "Oligarchie", type: "n.f.", def: "Gouvernement aux mains de quelques familles ou d'une classe privilégiée.", ex: "Le pays est dirigé par une oligarchie financière très puissante." },
    { word: "Omettra", type: "v.", def: "Oublier de faire, négliger.", ex: "Il ne faut pas omettre de signer le registre à l'entrée." },
    { word: "Omnipotence", type: "n.f.", def: "Puissance absolue et sans limite.", ex: "Le dictateur rêve d'une omnipotence totale sur son peuple." },
    { word: "Omniscient", type: "adj.", def: "Qui sait tout.", ex: "Dans ce roman, le narrateur est omniscient et connaît les pensées de tous." },
    { word: "Onde", type: "n.f.", def: "Vibration. Fig: L'eau, la mer (poétique).", ex: "Le reflet de la lune tremblait sur l'onde calme du lac." },
    { word: "Onéreux", type: "adj.", def: "Qui coûte cher.", ex: "L'entretien de cette voiture ancienne est particulièrement onéreux." },
    { word: "Onirique", type: "adj.", def: "Relatif aux rêves.", ex: "Le peintre nous transporte dans un univers onirique et coloré." },
    { word: "Opacité", type: "n.f.", def: "Qui ne laisse pas passer la lumière. Fig: Manque de transparence.", ex: "L'opacité de ses comptes bancaires a éveillé les soupçons." },
    { word: "Opiniâtre", type: "adj.", def: "Tenace, persévérant, qui ne cède pas facilement.", ex: "C'est un chercheur opiniâtre qui a passé dix ans sur ce problème." },
    { word: "Opportunisme", type: "n.m.", def: "Attitude consistant à tirer parti des circonstances au mieux de ses intérêts.", ex: "Son ralliement au vainqueur est un pur acte d'opportunisme." },
    { word: "Opprobre", type: "n.m.", def: "Honte publique, déshonneur extrême.", ex: "Il a jeté l'opprobre sur sa famille par sa conduite scandaleuse." },
    { word: "Opulence", type: "n.f.", def: "Grande richesse, abondance de biens.", ex: "Ils vivent dans l'opulence la plus totale depuis leur gain au loto." },
    { word: "Oracle", type: "n.m.", def: "Réponse d'une divinité. Fig: Personne dont les avis sont très écoutés.", ex: "On attendait la décision du vieux sage comme s'il s'agissait d'un oracle." },
    { word: "Orageux", type: "adj.", def: "Relatif à l'orage. Fig: Agité, violent.", ex: "Leur discussion fut orageuse et s'est terminée en cris." },
    { word: "Oraison", type: "n.f.", def: "Prière. Discours solennel (oraison funèbre).", ex: "Le ministre a prononcé une oraison funèbre émouvante pour le héros." },
    { word: "Orbite", type: "n.f.", def: "Trajectoire d'un astre. Cavité de l'œil.", ex: "Ses yeux semblaient sortir de leurs orbites sous l'effet de la peur." },
    { word: "Ordinaire", type: "adj.", def: "Qui arrive d'ordinaire, habituel. Commun.", ex: "C'est un homme tout à fait ordinaire, sans signe distinctif." },
    { word: "Orfèvre", type: "n.", def: "Artisan qui fabrique des objets en métaux précieux. Fig: Expert.", ex: "En matière de diplomatie, c'est un véritable orfèvre." },
    { word: "Orgueil", type: "n.m.", def: "Fierté excessive, haute opinion de soi-même.", ex: "Son orgueil l'empêche d'admettre qu'il a eu tort." },
    { word: "Original", type: "adj.", def: "Qui provient de la source. Nouveau, singulier.", ex: "Elle a un style très original qui ne ressemble à aucun autre." },
    { word: "Orner", type: "v.", def: "Embellir par des ornements.", ex: "Des fresques magnifiques ornent les murs de la chapelle." },
    { word: "Orthodoxe", type: "adj.", def: "Conforme à la doctrine officielle ou aux usages.", ex: "Sa méthode de travail n'est pas très orthodoxe, mais elle fonctionne." },
    { word: "Ostensible", type: "adj.", def: "Fait avec l'intention d'être vu.", ex: "Il a consulté sa montre d'une manière ostensible pour montrer son ennui." },
    { word: "Ostentatoire", type: "adj.", def: "Qui cherche à attirer l'attention par son étalage (souvent de richesse).", ex: "Il porte des bijoux en or d'une manière très ostentatoire." },
    { word: "Ostracisme", type: "n.m.", def: "Action d'exclure quelqu'un d'un groupe ou de la société.", ex: "Il a été victime d'un véritable ostracisme après ses révélations." },
    { word: "Outrancier", type: "adj.", def: "Qui pousse les choses à l'excès.", ex: "Ses propos outranciers ont fini par choquer même ses partisans." },
    { word: "Outrecuidance", type: "n.f.", def: "Confiance excessive en soi, impertinence.", ex: "Il a eu l'outrecuidance de me demander de faire son travail à sa place." },
    { word: "Ouvertement", type: "adv.", def: "Sans se cacher, franchement.", ex: "Il a ouvertement critiqué la décision du gouvernement." },
    { word: "Ouvrage", type: "n.m.", def: "Travail, produit d'un travail (livre, construction).", ex: "C'est un bel ouvrage qui a demandé des années de recherche." },
    { word: "Ovations", type: "n.f. pl.", def: "Applaudissements enthousiastes.", ex: "L'acteur a reçu des ovations debout à la fin de la pièce." },
    { word: "Ovin", type: "adj.", def: "Relatif au mouton.", ex: "L'élevage ovin est la principale ressource de cette région." },
    { word: "Pacifique", type: "adj.", def: "Qui aime la paix, sans violence.", ex: "Ils ont organisé une manifestation pacifique dans les rues." },
    { word: "Pacte", type: "n.m.", def: "Accord entre deux ou plusieurs parties.", ex: "Ils ont conclu un pacte secret pour se soutenir mutuellement." },
    { word: "Païen", type: "adj. et n.", def: "Qui ne croit en aucune des religions du Livre (souvent polythéiste).", ex: "Certaines fêtes chrétiennes ont des origines païennes." },
    { word: "Palabre", type: "n.f.", def: "Discussion longue et vaine.", ex: "Cessez ces palabres inutiles et mettez-vous au travail !" },
    { word: "Palingénésie", type: "n.f.", def: "Renaissance, retour à la vie.", ex: "Le printemps est une sorte de palingénésie de la nature." },
    { word: "Palme", type: "n.f.", def: "Feuille de palmier. Fig: Symbole de victoire.", ex: "Il a remporté la palme du meilleur orateur lors du concours." },
    { word: "Palpable", type: "adj.", def: "Que l'on peut toucher. Fig: Évident.", ex: "La tension dans la pièce était presque palpable." },
    { word: "Panacée", type: "n.f.", def: "Remède universel contre tous les maux.", ex: "Le sport n'est pas une panacée, mais il aide à rester en bonne santé." },
    { word: "Panégyrique", type: "n.m.", def: "Discours ou écrit à la louange de quelqu'un.", ex: "L'orateur a prononcé un long panégyrique en l'honneur du défunt." },
    { word: "Paradigme", type: "n.m.", def: "Modèle théorique de pensée qui oriente la recherche et la réflexion.", ex: "L'arrivée d'Internet a provoqué un changement de paradigme dans la communication." },
    { word: "Paradoxal", type: "adj.", def: "Qui va contre l'opinion commune, qui contient une contradiction.", ex: "Il est paradoxal de vouloir protéger la nature tout en utilisant autant de plastique." },
    { word: "Parangon", type: "n.m.", def: "Modèle, exemple parfait.", ex: "Il est considéré comme un parangon de vertu dans sa communauté." },
    { word: "Parité", type: "n.f.", def: "Égalité parfaite (de valeur, de nombre, de sexe).", ex: "La loi impose désormais la parité sur les listes électorales." },
    { word: "Paroxysme", type: "n.m.", def: "Le plus haut degré d'un sentiment ou d'une douleur.", ex: "L'enthousiasme de la foule a atteint son paroxysme à l'arrivée des joueurs." },
    { word: "Paria", type: "n.", def: "Personne exclue d'un groupe, méprisée par la société.", ex: "Après son scandale financier, il est devenu un paria dans le monde des affaires." },
    { word: "Parnassien", type: "adj. et n.", def: "Relatif au Parnasse. Poète d'une école du XIXe prônant l'art pour l'art.", ex: "Sa poésie froide et parfaite est typiquement parnassienne." },
    { word: "Parodie", type: "n.f.", def: "Imitation burlesque d'une œuvre sérieuse. Fig: Imitation grotesque.", ex: "Ce procès n'était qu'une triste parodie de justice." },
    { word: "Paroxysme", type: "n.m.", def: "Le plus haut degré d'une sensation, d'un sentiment.", ex: "La douleur a atteint son paroxysme avant que le médicament ne fasse effet." },
    { word: "Partisan", type: "n. et adj.", def: "Personne attachée à une opinion, un parti. Qui n'est pas neutre.", ex: "Il a rendu un jugement très partisan qui avantage ses amis." },
    { word: "Patibulaire", type: "adj.", def: "Qui donne l'impression qu'on mérite la potence ; inquiétant.", ex: "Nous avons été accueillis par deux individus à la mine patibulaire." },
    { word: "Pâtir", type: "v.", def: "Souffrir d'une situation, en supporter les conséquences négatives.", ex: "Les petits commerces vont pâtir de l'ouverture de ce centre commercial." },
    { word: "Pécuniaire", type: "adj.", def: "Relatif à l'argent.", ex: "Il a reçu une aide pécuniaire pour terminer ses études." },
    { word: "Pédant", type: "adj. et n.", def: "Qui fait étalage de son savoir de manière vaine et déplacée.", ex: "Je ne supporte pas son ton pédant quand il parle de vin." },
    { word: "Peine", type: "n.f.", def: "Souffrance morale. Sanction. (À peine : avec difficulté).", ex: "Il a fait son travail avec beaucoup de peine." },
    { word: "Péremptoire", type: "adj.", def: "Auquel on ne peut rien répliquer, tranchant.", ex: "Il a affirmé cela d'un ton péremptoire qui excluait tout débat." },
    { word: "Pérennité", type: "n.f.", def: "Caractère de ce qui dure toujours ou très longtemps.", ex: "Le chef d'entreprise s'inquiète de la pérennité de son activité." },
    { word: "Perfide", type: "adj.", def: "Qui manque à sa parole, qui trahit celui qui lui faisait confiance.", ex: "Elle m'a lancé un sourire perfide avant de révéler mon secret." },
    { word: "Péricliter", type: "v.", def: "Aller vers la ruine, dépérir.", ex: "Sans investissements, l'usine va rapidement péricliter." },
    { word: "Périphrase", type: "n.f.", def: "Exprimer en plusieurs mots ce que l'on pourrait dire en un seul.", ex: "'Le roi des animaux' est une périphrase pour désigner le lion." },
    { word: "Péritel", type: "n.f.", def: "Ancien système de connexion audio-vidéo.", ex: "Il me faut un adaptateur péritel pour brancher ma vieille console." },
    { word: "Pernicieux", type: "adj.", def: "Dangereux pour la santé ou l'esprit, nuisible.", ex: "La lecture de ces magazines a une influence pernicieuse sur les jeunes." },
    { word: "Perpétrer", type: "v.", def: "Commettre un acte criminel.", ex: "Le tyran a perpétré de nombreux massacres pendant son règne." },
    { word: "Perspicace", type: "adj.", def: "Qui a l'esprit pénétrant, qui comprend vite les choses cachées.", ex: "Grâce à son esprit perspicace, l'enquêteur a vite démasqué le coupable." },
    { word: "Pertinent", type: "adj.", def: "Qui convient exactement au sujet, approprié.", ex: "Vous avez posé une question très pertinente." },
    { word: "Pervenche", type: "n.f.", def: "Petite fleur bleue. Fig: Contractuelle (à cause de la couleur de l'uniforme).", ex: "Il a reçu une amende de la part d'une pervenche." },
    { word: "Pessimisme", type: "n.m.", def: "Disposition à voir le mauvais côté des choses.", ex: "Son pessimisme l'empêche de tenter de nouvelles expériences." },
    { word: "Pestilentiel", type: "adj.", def: "Qui a une odeur infecte (de peste).", ex: "Une odeur pestilentielle s'échappait des égouts." },
    { word: "Pétulant", type: "adj.", def: "Qui a une ardeur vive et joyeuse.", ex: "C'est une enfant pétulante qui ne tient pas en place." },
    { word: "Phallocrate", type: "n. et adj.", def: "Partisan de la domination des hommes sur les femmes.", ex: "C'est un vieux phallocrate qui refuse d'être dirigé par une femme." },
    { word: "Pharisaïque", type: "adj.", def: "D'une hypocrisie méticuleuse et orgueilleuse.", ex: "Je déteste sa morale pharisaïque, il ne s'applique pas ses propres principes." },
    { word: "Philanthrope", type: "n.", def: "Personne qui cherche à faire du bien à l'humanité.", ex: "Ce milliardaire philanthrope a légué sa fortune à des œuvres caritatives." },
    { word: "Philistin", type: "n.m.", def: "Personne d'esprit fermé, vulgaire, insensible aux arts.", ex: "Seul un philistin pourrait rester de marbre devant une telle peinture." },
    { word: "Phobie", type: "n.f.", def: "Peur irrationnelle et obsessionnelle.", ex: "Elle a une phobie des araignées qui l'empêche d'aller au grenier." },
    { word: "Phtisie", type: "n.f.", def: "Ancien nom de la tuberculose pulmonaire.", ex: "Le poète est mort de la phtisie à l'âge de vingt ans." },
    { word: "Pictural", type: "adj.", def: "Relatif à la peinture.", ex: "Son œuvre picturale est influencée par le surréalisme." },
    { word: "Pieux", type: "adj.", def: "Animé d'une ferveur religieuse. (Mensonge pieux : fait pour ne pas blesser).", ex: "C'est une femme très pieuse qui va à la messe chaque matin." },
    { word: "Pillage", type: "n.m.", def: "Action de voler et de saccager.", ex: "La ville a été livrée au pillage après la défaite." },
    { word: "Pinailler", type: "v.", def: "S'arrêter à des détails insignifiants pour critiquer.", ex: "Arrête de pinailler sur la ponctuation et regarde le fond du texte !" },
    { word: "Pionnier", type: "n.m.", def: "Celui qui ouvre la voie, qui est le premier dans un domaine.", ex: "Marie Curie fut une pionnière dans l'étude de la radioactivité." },
    { word: "Pittoresque", type: "adj.", def: "Qui attire l'attention par son aspect original ou coloré.", ex: "Nous avons visité un petit village pittoresque dans les Alpes." },
    { word: "Placide", type: "adj.", def: "Doux et calme, difficile à émouvoir.", ex: "Malgré l'agression, il est resté placide et n'a pas élevé la voix." },
    { word: "Plagiat", type: "n.m.", def: "Copie d'une œuvre littéraire ou artistique en la faisant passer pour sienne.", ex: "Ce romancier a été accusé de plagiat par un auteur inconnu." },
    { word: "Platonique", type: "adj.", def: "Relatif à Platon. Fig: Pur, sans lien charnel.", ex: "Ils entretiennent une relation platonique depuis des années." },
    { word: "Plébiscite", type: "n.m.", def: "Vote par lequel un peuple se prononce sur un homme ou une mesure.", ex: "Son élection fut un véritable plébiscite, il a obtenu 90% des voix." },
    { word: "Pléthore", type: "n.f.", def: "Abondance excessive.", ex: "Il y a une pléthore de candidats pour ce poste très convoité." },
    { word: "Ploutocratie", type: "n.f.", def: "Gouvernement par les plus riches.", ex: "Certains critiques dénoncent une dérive de la démocratie vers la ploutocratie." },
    { word: "Pochelée", type: "n.f.", def: "Contenu d'une poche.", ex: "Il a sorti une pochelée de bonbons pour les enfants." },
    { word: "Podium", type: "n.m.", def: "Plate-forme pour les vainqueurs ou les orateurs.", ex: "L'athlète est monté fièrement sur la plus haute marche du podium." },
    { word: "Poindre", type: "v.", def: "Commencer à paraître (jour, sentiment).", ex: "Le jour commençait à poindre sur la vallée." },
    { word: "Polémique", type: "n.f.", def: "Débat violent par écrit ou par oral.", ex: "Ses propos ont déclenché une vive polémique dans les médias." },
    { word: "Polisson", type: "adj. et n.", def: "Enfant espiègle. Propos un peu érotique.", ex: "C'est un petit polisson qui adore faire des farces." },
    { word: "Poliomyélite", type: "n.f.", def: "Maladie infectieuse pouvant entraîner des paralysies.", ex: "Le vaccin a permis d'éradiquer la poliomyélite dans de nombreux pays." },
    { word: "Polir", type: "v.", def: "Rendre lisse par frottement. Fig: Rendre plus civilisé.", ex: "Les années passées à la ville ont fini par polir ses manières rudes." },
    { word: "Politicien", type: "n.m.", def: "Personne qui fait de la politique son métier (parfois péjoratif).", ex: "C'est un politicien habile qui sait toujours d'où vient le vent." },
    { word: "Polyglotte", type: "adj. et n.", def: "Qui parle plusieurs langues.", ex: "Cette secrétaire polyglotte parle couramment six langues." },
    { word: "Polythéisme", type: "n.m.", def: "Croyance en plusieurs dieux.", ex: "L'hindouisme est une religion fondée sur le polythéisme." },
    { word: "Pompeux", type: "adj.", def: "Qui a une solennité excessive et ridicule.", ex: "Il a prononcé un discours pompeux qui a fait sourire l'auditoire." },
    { word: "Pondéré", type: "adj.", def: "Équilibré, mesuré dans ses jugements.", ex: "C'est un homme pondéré qui réfléchit toujours avant d'agir." },
    { word: "Pontifier", type: "v.", def: "Parler avec une autorité dogmatique et prétentieuse.", ex: "Il adore pontifier sur des sujets qu'il ne maîtrise pas." },
    { word: "Populace", type: "n.f.", def: "Le peuple, considéré avec mépris.", ex: "Le noble regardait la populace s'agiter depuis son balcon." },
    { word: "Portentueux", type: "adj.", def: "Qui tient du prodige, extraordinaire.", ex: "Il possède une mémoire portentueuse, il retient tout." },
    { word: "Postérité", type: "n.f.", def: "Descendance d'une personne. Fig: Générations futures.", ex: "Ce grand peintre n'a connu la gloire qu'à la postérité." },
    { word: "Posthume", type: "adj.", def: "Qui arrive ou qui est publié après la mort.", ex: "Ce roman posthume est considéré comme son chef-d'œuvre." },
    { word: "Postulat", type: "n.m.", def: "Principe que l'on demande d'admettre avant une démonstration.", ex: "Sa théorie repose sur un postulat que beaucoup contestent." },
    { word: "Potentiel", type: "adj. et n.", def: "Qui existe en puissance, possible.", ex: "Cette entreprise a un fort potentiel de croissance." },
    { word: "Pourpre", type: "n.m. ou f.", def: "Couleur rouge foncé. Fig: Symbole de la dignité royale ou cardinalice.", ex: "Le cardinal portait la pourpre avec beaucoup de prestance." },
    { word: "Pragmatique", type: "adj.", def: "Qui privilégie l'action et l'efficacité sur la théorie.", ex: "C'est une femme pragmatique qui cherche des solutions concrètes." },
    { word: "Préalable", type: "adj. et n.", def: "Qui doit être fait avant autre chose.", ex: "Une enquête préalable est nécessaire avant de prendre une décision." },
    { word: "Préambule", type: "n.m.", def: "Ce qui précède le corps d'un discours ou d'un écrit.", ex: "Le préambule de la Constitution définit les valeurs de la République." },
    { word: "Précaire", type: "adj.", def: "Dont l'existence n'est pas assurée, fragile.", ex: "Il vit dans une situation précaire, sans emploi stable." },
    { word: "Précaution", type: "n.f.", def: "Mesure prise pour éviter un mal ou un danger.", ex: "Par précaution, j'ai pris mon parapluie malgré le soleil." },
    { word: "Précédent", type: "n.m.", def: "Acte antérieur qui peut servir de modèle ou d'exemple.", ex: "Cette décision de justice va créer un précédent important." },
    { word: "Précepte", type: "n.m.", def: "Règle de conduite, enseignement moral.", ex: "Il suit rigoureusement les préceptes de sa religion." },
    { word: "Prêchi-prêcha", type: "n.m.", def: "Discours moralisateur et ennuyeux.", ex: "Je n'ai pas besoin de ton prêchi-prêcha sur ma façon de vivre." },
    { word: "Préciput", type: "n.m.", def: "Avantage que l'on donne à l'un des héritiers.", ex: "Il a reçu cette maison en préciput lors du partage." },
    { word: "Préconiser", type: "v.", def: "Conseiller vivement, recommander.", ex: "Le médecin préconise un repos total pendant une semaine." },
    { word: "Prédilection", type: "n.f.", def: "Préférence marquée pour quelqu'un ou quelque chose.", ex: "Il a une prédilection pour la musique baroque." },
    { word: "Prééminence", type: "n.f.", def: "Supériorité absolue de rang ou de dignité.", ex: "La prééminence du droit sur la force est un principe démocratique." },
    { word: "Prélude", type: "n.m.", def: "Ce qui précède et annonce quelque chose.", ex: "Ces quelques notes étaient le prélude d'une symphonie magnifique." },
    { word: "Préméditation", type: "n.f.", def: "Dessein formé d'avance de commettre un acte (crime).", ex: "Le juge a retenu la préméditation, ce qui alourdit la peine." },
    { word: "Prémices", type: "n.f. pl.", def: "Premiers fruits récoltés. Fig: Commencement, début.", ex: "Ce livre contient les prémices de sa future philosophie." },
    { word: "Prémunir", type: "v.", def: "Garantir, protéger d'avance (contre un mal).", ex: "Il faut se prémunir contre les risques de piratage informatique." },
    { word: "Prépondérant", type: "adj.", def: "Qui a plus de poids, d'importance.", ex: "Il a joué un rôle prépondérant dans la réussite de cette affaire." },
    { word: "Prérogative", type: "n.f.", def: "Avantage exclusif attaché à une fonction, un titre.", ex: "Gracier les prisonniers est une prérogative du président." },
    { word: "Présage", type: "n.m.", def: "Signe par lequel on croit prévoir l'avenir.", ex: "Le vol des oiseaux était considéré comme un présage par les Romains." },
    { word: "Présomptueux", type: "adj.", def: "Qui a une trop haute opinion de soi (arrogant).", ex: "Il est bien présomptueux de croire qu'il peut gagner sans s'entraîner." },
    { word: "Présure", type: "n.f.", def: "Substance pour faire cailler le lait.", ex: "On ajoute de la présure pour fabriquer le fromage." },
    { word: "Prétention", type: "n.f.", def: "Affirmation d'un mérite que l'on n'a pas. Ambition.", ex: "Il a la prétention de parler chinois après seulement trois cours." },
    { word: "Prétorien", type: "adj. et n.m.", def: "Relatif à la garde de l'empereur romain. Fig: Soldat dévoué au pouvoir.", ex: "Le dictateur s'est entouré d'une garde de prétoriens fidèles." },
    { word: "Prévaloir", type: "v.", def: "Avoir l'avantage, l'emporter.", ex: "La raison doit prévaloir sur la colère." },
    { word: "Prévarication", type: "n.f.", def: "Manquement grave d'un fonctionnaire à ses devoirs.", ex: "Le ministre a été démis pour prévarication et détournement de fonds." },
    { word: "Prévenant", type: "adj.", def: "Qui devance les désirs d'autrui par gentillesse.", ex: "C'est un hôte très prévenant qui s'occupe de tout." },
    { word: "Priver", type: "v.", def: "Retirer à quelqu'un l'usage de quelque chose.", ex: "On ne peut pas priver un enfant de son éducation." },
    { word: "Probe", type: "adj.", def: "D'une honnêteté rigoureuse.", ex: "Tout le monde s'accorde à dire que c'est un homme probe." },
    { word: "Probité", type: "n.f.", def: "Honnêteté scrupuleuse.", ex: "Sa probité n'a jamais été mise en doute durant sa longue carrière." },
    { word: "Procrastination", type: "n.f.", def: "Tendance à remettre au lendemain ce que l'on peut faire le jour même.", ex: "La procrastination est le pire ennemi de l'étudiant en période d'examens." },
    { word: "Prodige", type: "n.m.", def: "Événement extraordinaire. Enfant au talent exceptionnel.", ex: "Ce jeune pianiste est un véritable prodige." },
    { word: "Prodigue", type: "adj. et n.", def: "Qui dépense sans compter.", ex: "Il est très prodigue et n'hésite pas à offrir des cadeaux coûteux." },
    { word: "Profane", type: "adj. et n.", def: "Qui n'est pas religieux. Qui n'est pas initié à un art.", ex: "Pour un profane, ce tableau ne semble être qu'un mélange de couleurs." },
    { word: "Proférer", type: "v.", def: "Prononcer à haute voix (souvent des insultes).", ex: "Il a commencé à proférer des menaces contre ses voisins." },
    { word: "Profil", type: "n.m.", def: "Vue de côté. Fig: Ensemble des traits caractéristiques.", ex: "Nous recherchons un candidat avec un profil technique." },
    { word: "Profusion", type: "n.f.", def: "Très grande abondance.", ex: "Les fleurs poussaient en profusion dans le jardin abandonné." },
    { word: "Prohiber", type: "v.", def: "Interdire par la loi ou le règlement.", ex: "La loi prohibe la vente d'alcool aux mineurs." },
    { word: "Prolixe", type: "adj.", def: "Qui est trop long dans ses discours ou ses écrits.", ex: "Il est si prolixe qu'on finit par perdre le fil de son histoire." },
    { word: "Prolonger", type: "v.", def: "Faire durer plus longtemps.", ex: "Nous avons décidé de prolonger notre séjour d'une semaine." },
    { word: "Promiscuité", type: "n.f.", def: "Voisinage désagréable imposé par le manque d'espace.", ex: "La promiscuité dans les prisons est un problème majeur." },
    { word: "Promulguer", type: "v.", def: "Publier officiellement une loi pour la rendre exécutoire.", ex: "Le président a promulgué la loi dès le lendemain du vote." },
    { word: "Prôner", type: "v.", def: "Recommander avec insistance (une idée, une méthode).", ex: "Il prône un retour à une vie plus simple et proche de la nature." },
    { word: "Propension", type: "n.f.", def: "Penchant naturel, tendance.", ex: "Il a une propension naturelle à l'exagération." },
    { word: "Propice", type: "adj.", def: "Qui favorise le succès, favorable.", ex: "Le calme du soir est propice à la réflexion." },
    { word: "Propos", type: "n.m. pl.", def: "Paroles dites. (À propos : opportun).", ex: "Ses propos ont été mal interprétés par la presse." },
    { word: "Prosaïque", type: "adj.", def: "Qui manque de poésie, qui est plat et vulgaire.", ex: "Il a une vision très prosaïque de l'amour, uniquement basée sur l'argent." },
    { word: "Proscrire", type: "v.", def: "Interdire, rejeter.", ex: "Il faut proscrire le sucre de votre alimentation." },
    { word: "Prosterné", type: "adj.", def: "Étendu le visage contre terre (respect). Fig: Soumis.", ex: "Le peuple restait prosterné au passage du souverain." },
    { word: "Protagoniste", type: "n.", def: "Personnage principal d'une pièce, d'une affaire.", ex: "Les deux protagonistes du drame refusent de se parler." },
    { word: "Protéiforme", type: "adj.", def: "Qui change de forme, qui prend des aspects très variés.", ex: "C'est un artiste protéiforme, à la fois peintre, sculpteur et musicien." },
    { word: "Prouesse", type: "n.f.", def: "Acte de bravoure, exploit.", ex: "Le gymnaste a réalisé une véritable prouesse technique." },
    { word: "Providenciel", type: "adj.", def: "Qui arrive par un heureux hasard (comme par miracle).", ex: "Son arrivée fut providencielle, il nous a sauvé la vie." },
    { word: "Proxy", type: "n.m.", def: "Intermédiaire informatique. Mandataire.", ex: "Vous devez configurer votre proxy pour accéder à Internet." },
    { word: "Prude", type: "adj. et n.f.", def: "Qui affecte une pudeur excessive.", ex: "Elle fait la prude, mais elle adore les histoires grivoises." },
    { word: "Prudence", type: "n.f.", def: "Attitude qui consiste à éviter les risques inutiles.", ex: "La prudence est de mise sur ces routes enneigées." },
    { word: "Puant", type: "adj.", def: "Qui sent mauvais. Fig: Arrogant (fier).", ex: "Il est d'un orgueil puant." },
    { word: "Puéril", type: "adj.", def: "Qui ressemble à un enfant, qui manque de sérieux.", ex: "Arrêtez ces disputes puériles et comportez-vous en adultes !" },
    { word: "Pugnacité", type: "n.f.", def: "Tendance à combattre, à lutter.", ex: "L'avocat a fait preuve d'une pugnacité remarquable pour défendre son client." },
    { word: "Pulluler", type: "v.", def: "Se multiplier très rapidement. Être en grand nombre.", ex: "Les insectes pullulent dans cette zone humide." },
    { word: "Pusillanime", type: "adj.", def: "Qui manque d'audace, qui craint les responsabilités.", ex: "Le ministre a pris une décision pusillanime pour ne fâcher personne." },
    { word: "Putatif", type: "adj.", def: "Supposé (mariage, père).", ex: "Il a été reconnu comme son père putatif." },
    { word: "Putride", type: "adj.", def: "Qui est en état de putréfaction.", ex: "Une odeur putride s'élevait du cadavre de l'animal." },
    { word: "Quiconque", type: "pron. indéf.", def: "Toute personne qui.", ex: "Quiconque sera trouvé ici sera puni." },
    { word: "Quiddité", type: "n.f.", def: "Ce qu'une chose est en soi (essence).", ex: "Le philosophe cherche la quiddité de l'âme humaine." },
    { word: "Quiétude", type: "n.f.", def: "Tranquillité de l'âme, calme paisible.", ex: "J'aime la quiétude de la campagne au petit matin." },
    { word: "Quinine", type: "n.f.", def: "Médicament contre le paludisme.", ex: "On utilisait autrefois la quinine pour faire tomber la fièvre." },
    { word: "Quintessence", type: "n.f.", def: "Ce qu'il y a de plus pur et de plus raffiné dans une chose.", ex: "Ce parfum est la quintessence du luxe français." },
    { word: "Quorum", type: "n.m.", def: "Nombre minimum de membres présents pour qu'un vote soit valable.", ex: "Le quorum n'étant pas atteint, la séance a été reportée." },
    { word: "Rabrouer", type: "v.", def: "Repousser quelqu'un avec brusquerie et mépris.", ex: "Il s'est fait rabrouer par son patron pour avoir posé une question." },
    { word: "Rachat", type: "n.m.", def: "Action de racheter. Fig: Action de réparer une faute.", ex: "Il cherche le rachat de ses péchés par de bonnes actions." },
    { word: "Radical", type: "adj.", def: "Qui va jusqu'à la racine. Drastique.", ex: "Il faut un changement radical de politique pour sauver l'économie." },
    { word: "Radoter", type: "v.", def: "Répéter inutilement les mêmes choses (vieillesse).", ex: "Le vieil homme commence à radoter sur ses souvenirs de guerre." },
    { word: "Raffinement", type: "n.m.", def: "Caractère de ce qui est très délicat et choisi.", ex: "Cette maison est décorée avec un raffinement extrême." },
    { word: "Rancœur", type: "n.f.", def: "Ressentiment tenace lié à une blessure morale.", ex: "Il garde une vive rancœur contre ceux qui l'ont trahi." },
    { word: "Rançon", type: "n.f.", def: "Prix exigé pour la liberté d'un captif. Fig: Prix à payer.", ex: "La fatigue est souvent la rançon du succès." },
    { word: "Rapt", type: "n.m.", def: "Enlèvement illégal par la force.", ex: "Le rapt de l'enfant a bouleversé tout le pays." },
    { word: "Raréfaction", type: "n.f.", def: "Fait de devenir rare.", ex: "La raréfaction des ressources naturelles inquiète les experts." },
    { word: "Rassasier", type: "v.", def: "Satisfaire pleinement la faim ou un désir.", ex: "Rien ne semble pouvoir rassasier son ambition." },
    { word: "Ratifier", type: "v.", def: "Approuver officiellement un acte (traité, contrat).", ex: "Le Parlement doit ratifier cet accord international." },
    { word: "Rauque", type: "adj.", def: "D'une sonorité rude et basse (voix).", ex: "Il a une voix rauque de gros fumeur." },
    { word: "Réactionnaire", type: "adj. et n.", def: "Qui s'oppose au progrès social et veut revenir au passé.", ex: "Ses idées réactionnaires choquent ses collègues plus libéraux." },
      { word: "Réaliste", type: "adj.", def: "Qui a le sens des réalités. Relatif au réalisme.", ex: "Il faut être réaliste, nous n'aurons jamais fini à temps." },
    { word: "Rebuffade", type: "n.f.", def: "Accueil brusque et humiliant, refus méprisant.", ex: "Il a essuyé une nouvelle rebuffade en demandant une augmentation." },
    { word: "Récalcitrant", type: "adj.", def: "Qui résiste avec opiniâtreté.", ex: "L'enfant récalcitrant refusait d'avaler son médicament." },
    { word: "Récuser", type: "v.", def: "Refuser d'accepter un juge, un expert ou un témoignage.", ex: "L'avocat a décidé de récuser cet expert pour partialité." },
    { word: "Rédhibitoire", type: "adj.", def: "Qui constitue un obstacle absolu, un défaut empêchant une vente ou une action.", ex: "Son manque d'expérience est un défaut rédhibitoire pour ce poste." },
    { word: "Réfection", type: "n.f.", def: "Action de réparer, de remettre à neuf.", ex: "La réfection de la toiture coûtera très cher à la copropriété." },
    { word: "Réfectoire", type: "n.m.", def: "Salle où l'on prend les repas en commun dans une collectivité.", ex: "Les élèves se dirigent vers le réfectoire pour le déjeuner." },
    { word: "Réfréner", type: "v.", def: "Retenir, réprimer un sentiment, une pulsion.", ex: "Il a dû réfréner sa colère pour ne pas aggraver la situation." },
    { word: "Réfuter", type: "v.", def: "Démontrer la fausseté d'une affirmation par des preuves contraires.", ex: "Le savant a réussi à réfuter la théorie de son prédécesseur." },
    { word: "Régalien", type: "adj.", def: "Qui appartient en propre au souverain (puis à l'État).", ex: "La justice et la police sont des fonctions régaliennes." },
    { word: "Reginglard", type: "adj.", def: "Se dit d'un vin aigrelet et de mauvaise qualité.", ex: "On nous a servi un petit vin reginglard imbuvable." },
    { word: "Réitérer", type: "v.", def: "Faire de nouveau, répéter.", ex: "Je réitère ma demande de remboursement sans délai." },
    { word: "Relaps", type: "adj. et n.", def: "Chrétien retombé dans l'hérésie après l'avoir abjurée.", ex: "Il fut condamné comme relaps par le tribunal de l'Inquisition." },
    { word: "Reléguer", type: "v.", def: "Exiler, envoyer dans un lieu éloigné. Écarter.", ex: "Il a été relégué au second plan depuis l'arrivée du nouveau directeur." },
    { word: "Réminiscence", type: "n.f.", def: "Souvenir vague d'une chose presque oubliée.", ex: "Ce paysage éveilla en lui une réminiscence d'enfance." },
    { word: "Rémunération", type: "n.f.", def: "Prix d'un travail, d'un service rendu.", ex: "La rémunération doit être proportionnelle à l'effort fourni." },
    { word: "Renégat", type: "n.", def: "Personne qui a renié sa religion, son parti, sa patrie.", ex: "Il est considéré comme un renégat par ses anciens compagnons d'armes." },
    { word: "Renoncement", type: "n.m.", def: "Action de renoncer à quelque chose par abnégation.", ex: "Sa vie est faite de renoncement et de dévouement aux autres." },
    { word: "Rente", type: "n.f.", def: "Revenu périodique tiré d'un capital ou d'un bien.", ex: "Il vit confortablement grâce à ses rentes immobilières." },
    { word: "Répudier", type: "v.", def: "Renvoyer son épouse (dans certaines sociétés). Rejeter.", ex: "Il a décidé de répudier ses anciennes croyances." },
    { word: "Requiem", type: "n.m.", def: "Messe pour les morts.", ex: "Le Requiem de Mozart est l'une de ses œuvres les plus célèbres." },
    { word: "Réquisitoire", type: "n.m.", def: "Discours du procureur contre l'accusé. Fig: Critique violente.", ex: "Il a prononcé un véritable réquisitoire contre la politique actuelle." },
    { word: "Résilience", type: "n.f.", def: "Capacité à surmonter les chocs traumatiques et à se reconstruire.", ex: "Les psychologues admirent la résilience de cet enfant orphelin." },
    { word: "Résipiscence", type: "n.f.", def: "Reconnaissance de sa faute avec regret (venir à résipiscence).", ex: "Le coupable a fini par venir à résipiscence après des jours de silence." },
    { word: "Résolu", type: "adj.", def: "Qui a pris une décision et s'y tient avec fermeté.", ex: "C'est un homme résolu qui ne se laisse pas influencer." },
    { word: "Résonance", type: "n.f.", def: "Fait de résonner. Fig: Écho, retentissement.", ex: "Ses paroles ont trouvé une résonance profonde dans le cœur du public." },
    { word: "Résoudre", type: "v.", def: "Trouver une solution. (Se résoudre à : se décider à faire une chose pénible).", ex: "Il a dû se résoudre à vendre sa maison familiale." },
    { word: "Respectif", type: "adj.", def: "Qui appartient à chacun d'eux.", ex: "Ils sont rentrés dans leurs domiciles respectifs après la fête." },
    { word: "Ressentiment", type: "n.m.", def: "Souvenir d'une injure avec désir de vengeance.", ex: "Il n'éprouve aucun ressentiment malgré l'injustice qu'il a subie." },
    { word: "Restituer", type: "v.", def: "Rendre ce qui a été pris ou perdu.", ex: "Le musée doit restituer ces œuvres d'art à leur pays d'origine." },
    { word: "Restrictif", type: "adj.", def: "Qui restreint, qui limite.", ex: "Cette loi impose des conditions très restrictives pour l'immigration." },
    { word: "Rétif", type: "adj.", def: "Qui s'arrête et refuse d'avancer (cheval). Fig: Rebelle à toute influence.", ex: "C'est un esprit rétif à toute forme d'autorité." },
    { word: "Réticence", type: "n.f.", def: "Omission volontaire. Hésitation à dire ou à faire.", ex: "J'ai quelques réticences à lui confier mon véhicule." },
    { word: "Rétorsion", type: "n.f.", def: "Action de répondre à un procédé par un procédé identique (souvent hostile).", ex: "Le gouvernement a pris des mesures de rétorsion commerciale." },
    { word: "Rétroactif", type: "adj.", def: "Qui exerce son effet sur le passé.", ex: "Cette augmentation de salaire est rétroactive au premier janvier." },
    { word: "Rétrograde", type: "adj.", def: "Qui va en arrière. Fig: Opposé au progrès.", ex: "Il a une vision rétrograde de l'éducation." },
    { word: "Retors", type: "adj.", def: "Tordu. Fig: Rusé et perfide.", ex: "C'est un avocat retors qui connaît toutes les failles de la loi." },
    { word: "Révéler", type: "v.", def: "Faire connaître ce qui était caché.", ex: "L'enquête a révélé l'existence d'un réseau de corruption." },
    { word: "Révérence", type: "n.f.", def: "Respect profond. Salut cérémonieux.", ex: "L'ambassadeur a fait une profonde révérence au roi." },
    { word: "Réviser", type: "v.", def: "Examiner de nouveau pour corriger ou améliorer.", ex: "Le gouvernement doit réviser sa stratégie économique." },
    { word: "Révolu", type: "adj.", def: "Qui est passé, terminé.", ex: "L'époque de la marine à voile est désormais révolue." },
    { word: "Rhapsodie", type: "n.f.", def: "Pièce musicale inspirée de thèmes populaires.", ex: "La Rhapsodie hongroise de Liszt est très célèbre." },
    { word: "Rhétorique", type: "n.f.", def: "Art de bien parler, de persuader.", ex: "Il utilise une rhétorique habile pour convaincre les électeurs." },
    { word: "Ribe", type: "n.f.", def: "Ancien instrument pour broyer le chanvre. Fig: Femme de mauvaise vie.", ex: "Il traînait avec toutes les ribes du quartier." },
    { word: "Ribote", type: "n.f.", def: "Excès de table et de boisson (faire ribote).", ex: "Ils ont fait ribote jusqu'au milieu de la nuit." },
    { word: "Rictus", type: "n.m.", def: "Contraction des muscles du visage simulant un sourire grimaçant.", ex: "Un rictus de douleur déformait ses traits." },
    { word: "Ridicule", type: "adj. et n.", def: "Qui prête à rire, qui est indigne de sérieux.", ex: "Il s'est mis dans une situation ridicule en voulant trop en faire." },
    { word: "Rigide", type: "adj.", def: "Qui ne plie pas. Fig: D'une sévérité inflexible.", ex: "Le règlement intérieur de cette école est très rigide." },
    { word: "Rigoureux", type: "adj.", def: "D'une exactitude ou d'une sévérité extrême.", ex: "Ce travail demande un esprit rigoureux et méthodique." },
    { word: "Rimaille", type: "n.f.", def: "Mauvais vers, poésie sans valeur.", ex: "Son recueil n'est qu'une suite de rimailles sans intérêt." },
    { word: "Rixe", type: "n.f.", def: "Querelle violente accompagnée de coups.", ex: "Une rixe a éclaté à la sortie de la boîte de nuit." },
    { word: "Robuste", type: "adj.", def: "Fort, vigoureux, qui résiste bien.", ex: "C'est un homme robuste qui n'est jamais malade." },
    { word: "Rocambolesque", type: "adj.", def: "Plein d'aventures invraisemblables et extraordinaires.", ex: "Il nous a raconté son évasion rocambolesque de prison." },
    { word: "Rodomontade", type: "n.f.", def: "Propos fanfaron, vantardise.", ex: "On ne compte plus ses rodomontades sur ses exploits sportifs." },
    { word: "Rogue", type: "adj.", def: "Arrogant, méprisant et rude.", ex: "Il a répondu d'un ton rogue qui a mis fin à la conversation." },
    { word: "Romance", type: "n.f.", def: "Poème ou chanson au ton sentimental.", ex: "Il chantait une vieille romance sous sa fenêtre." },
    { word: "Rompre", type: "v.", def: "Casser. Cesser une relation. Interrompre.", ex: "Le pays a décidé de rompre ses relations diplomatiques." },
    { word: "Ronce", type: "n.f.", def: "Arbrisseau épineux.", ex: "Le jardin était envahi par les ronces et les mauvaises herbes." },
    { word: "Rosse", type: "adj. et n.f.", def: "Mauvais cheval. Fig: Personne méchante, dure.", ex: "Quel prof de maths, c'est une vraie rosse !" },
    { word: "Rostre", type: "n.m.", def: "Éperon d'un navire antique. Prolongement de la tête de certains animaux.", ex: "L'espadon possède un rostre impressionnant." },
    { word: "Roture", type: "n.f.", def: "Condition de celui qui n'est pas noble.", ex: "À l'époque, un noble ne pouvait épouser une femme de la roture." },
    { word: "Rouerie", type: "n.f.", def: "Ruse d'une personne maligne et sans scrupules.", ex: "Il a utilisé toutes ses roueries pour obtenir ce contrat." },
    { word: "Rouiller", type: "v.", def: "Se couvrir de rouille. Fig: Perdre de sa souplesse ou de son talent par manque d'exercice.", ex: "Mon anglais commence à rouiller, je devrais pratiquer davantage." },
    { word: "Rousseur", type: "n.f.", def: "Couleur rousse. (Taches de rousseur).", ex: "Le visage de l'enfant était couvert de petites taches de rousseur." },
    { word: "Rubicond", type: "adj.", def: "Très rouge de visage.", ex: "Ce bon vivant avait un visage rubicond qui respirait la santé." },
    { word: "Rude", type: "adj.", def: "Rugueux. Difficile, pénible. Peu civilisé.", ex: "L'hiver a été particulièrement rude cette année." },
    { word: "Rudiment", type: "n.m.", def: "Premières notions d'une science ou d'un art.", ex: "Il possède les rudiments du japonais." },
    { word: "Ruer", type: "v.", def: "Lancer les pieds de derrière (cheval). Fig: Se jeter avec violence.", ex: "Il s'est rué sur la nourriture dès qu'on l'a servi." },
    { word: "Rugir", type: "v.", def: "Pousser son cri (lion). Fig: Crier de colère.", ex: "Le patron a rugi de colère en voyant les résultats." },
    { word: "Ruine", type: "n.f.", def: "Destruction d'un édifice. Perte de la fortune ou de la santé.", ex: "Sa passion pour le jeu l'a mené à la ruine." },
    { word: "Ruminer", type: "v.", def: "Remâcher les aliments. Fig: Retourner longuement une pensée dans son esprit.", ex: "Il rumine sa vengeance depuis des mois." },
    { word: "Rupture", type: "n.f.", def: "Fait de se rompre. Séparation.", ex: "La rupture entre les deux associés était devenue inévitable." },
    { word: "Rural", type: "adj.", def: "Qui concerne la campagne.", ex: "Il préfère le calme du monde rural à l'agitation des villes." },
    { word: "Rustique", type: "adj.", def: "Qui appartient à la campagne. Simple et robuste.", ex: "Nous avons logé dans une auberge rustique au charme fou." },
    { word: "Sabbat", type: "n.m.", def: "Repos hebdomadaire des juifs. Assemblée nocturne de sorcières. Grand vacarme.", ex: "Les voisins ont fait un sabbat infernal toute la nuit." },
    { word: "Sacerdoce", type: "n.m.", def: "Dignité du prêtre. Fig: Fonction qui demande un grand dévouement.", ex: "Pour elle, l'enseignement est un véritable sacerdoce." },
    { word: "Sacrilège", type: "n.m.", def: "Profanation d'une chose sacrée.", ex: "Utiliser ce calice comme vase est un pur sacrilège." },
    { word: "Saillie", type: "n.f.", def: "Partie qui dépasse. Fig: Trait d'esprit vif et brillant.", ex: "Il nous a régalés de ses saillies humoristiques pendant le dîner." },
    { word: "Salace", type: "adj.", def: "Qui incite à la luxure, grivois.", ex: "Il raconte toujours des histoires salaces qui mettent mal à l'aise." },
    { word: "Salubre", type: "adj.", def: "Sain, favorable à la santé (air, lieu).", ex: "L'air de la montagne est bien plus salubre que celui de la ville." },
    { word: "Sanctuaire", type: "n.m.", def: "Lieu saint. Fig: Refuge inviolable.", ex: "Ce parc naturel est un sanctuaire pour les espèces menacées." },
    { word: "Sanguinaire", type: "adj.", def: "Qui aime répandre le sang, cruel.", ex: "Ce dictateur sanguinaire a éliminé tous ses opposants." },
    { word: "Sardonique", type: "adj.", def: "D'une ironie amère et méchante.", ex: "Il a laissé échapper un rire sardonique en apprenant mon échec." },
    { word: "Satiété", type: "n.f.", def: "État d'une personne dont la faim est pleinement satisfaite.", ex: "J'ai mangé jusqu'à satiété." },
    { word: "Satire", type: "n.f.", def: "Écrit où l'on attaque les vices et les ridicules.", ex: "Cette pièce de théâtre est une satire féroce de la bourgeoisie." },
    { word: "Satrape", type: "n.m.", def: "Gouverneur de province chez les anciens Perses. Fig: Homme puissant et fastueux.", ex: "Il mène une vie de satrape dans son palais de marbre." },
    { word: "Saturer", type: "v.", def: "Combler au maximum. Fig: Fatiguer par excès.", ex: "Le marché est saturé de produits de basse qualité." },
    { word: "Saumâtre", type: "adj.", def: "Qui a le goût du sel. Fig: Amer, déplaisant.", ex: "Cette remarque lui est restée saumâtre." },
    { word: "Sauvegarder", type: "v.", def: "Protéger, préserver.", ex: "Il faut sauvegarder notre patrimoine historique." },
    { word: "Savoir-vivre", type: "n.m.", def: "Connaissance et pratique des usages du monde.", ex: "Il manque singulièrement de savoir-vivre, il n'enlève même pas son chapeau." },
    { word: "Scabreux", type: "adj.", def: "Qui présente des risques. Fig: Qui choque la décence.", ex: "Il s'est lancé dans une affaire scabreuse qui pourrait mal finir." },
    { word: "Scandaleux", type: "adj.", def: "Qui cause un scandale, indécent, honteux.", ex: "Il est scandaleux de laisser ces gens sans abri." },
    { word: "Sceptique", type: "adj. et n.", def: "Qui doute, qui refuse de croire sans preuves.", ex: "Je reste sceptique quant à l'efficacité de ce nouveau remède." },
    { word: "Schisme", type: "n.m.", def: "Séparation au sein d'une église ou d'un groupe.", ex: "La question du budget a provoqué un schisme au sein du parti." },
    { word: "Sciemment", type: "adv.", def: "En pleine connaissance de cause, volontairement.", ex: "Il a sciemment caché la vérité à ses associés." },
    { word: "Scinder", type: "v.", def: "Diviser une chose en deux ou plusieurs parties.", ex: "Nous allons scinder le projet en trois étapes distinctes." },
    { word: "Scrupuleux", type: "adj.", def: "Qui a des scrupules. Très précis et minutieux.", ex: "Il est scrupuleux sur le respect des horaires." },
    { word: "Sébile", type: "n.f.", def: "Petite coupe de bois pour recueillir l'aumône.", ex: "Le mendiant tendait sa sébile aux passants." },
    { word: "Séculaire", type: "adj.", def: "Qui date d'un ou de plusieurs siècles.", ex: "Cet arbre séculaire est le plus vieux de la forêt." },
    { word: "Sédentaire", type: "adj.", def: "Qui n'a pas de domicile fixe. Qui ne bouge pas beaucoup.", ex: "Un mode de vie trop sédentaire est mauvais pour le cœur." },
    { word: "Séditieux", type: "adj.", def: "Qui incite à la révolte contre l'autorité.", ex: "Il a été arrêté pour avoir distribué des tracts séditieux." },
    { word: "Sémillant", type: "adj.", def: "D'une vivacité pleine d'agrément.", ex: "C'est un jeune homme sémillant qui plaît beaucoup aux dames." },
    { word: "Séminal", type: "adj.", def: "Relatif à la semence. Fig: Qui contient les germes d'un développement futur.", ex: "Cet article est considéré comme l'acte séminal de cette nouvelle science." },
    { word: "Sénile", type: "adj.", def: "Relatif à la vieillesse (dégradation physique ou mentale).", ex: "Il commence à montrer des signes de démence sénile." },
    { word: "Sensuel", type: "adj.", def: "Qui procure du plaisir aux sens.", ex: "Une musique sensuelle emplissait la pièce." },
    { word: "Sentencieux", type: "adj.", def: "Qui s'exprime par sentences, avec une solennité affectée.", ex: "Il parle d'un ton sentencieux qui finit par lasser son entourage." },
    { word: "Séquelle", type: "n.f.", def: "Conséquence fâcheuse d'une maladie ou d'un accident.", ex: "Il garde de graves séquelles de son accident de voiture." },
    { word: "Serein", type: "adj.", def: "Pur et calme (ciel). Paisible et sans trouble (âme).", ex: "Malgré les problèmes, il reste serein et confiant." },
    { word: "Sévir", type: "v.", def: "Traiter avec rigueur. Fig: Faire rage (épidémie, crime).", ex: "La grippe continue de sévir dans toute la région." },
    { word: "Sibyllin", type: "adj.", def: "Dont le sens est obscur et mystérieux.", ex: "Il m'a envoyé un message sibyllin que je n'ai toujours pas décodé." },
    { word: "Sicaire", type: "n.m.", def: "Tueur à gages.", ex: "Le parrain a envoyé un sicaire pour liquider son rival." },
    { word: "Sidérer", type: "v.", def: "Frapper de stupeur.", ex: "L'annonce de son départ soudain a sidéré tout le monde." },
    { word: "Simonie", type: "n.f.", def: "Commerce des choses sacrées (charges ecclésiastiques).", ex: "L'évêque fut accusé de simonie pour avoir vendu des indulgences." },
    { word: "Simulacre", type: "n.m.", def: "Image, apparence vaine. Fausse apparence.", ex: "Leur réconciliation n'était qu'un simulacre pour les journalistes." },
    { word: "Sinécure", type: "n.f.", def: "Emploi où l'on n'a presque rien à faire tout en étant payé.", ex: "Ce poste de conseiller est une véritable sinécure." },
    { word: "Singulier", type: "adj.", def: "Unique. Particulier, bizarre.", ex: "Il a une façon très singulière de s'habiller." },
    { word: "Sinistre", type: "adj. et n.m.", def: "Qui annonce un malheur. Accident grave.", ex: "Le bâtiment a subi un sinistre important lors de l'incendie." },
    { word: "Sinuosité", type: "n.f.", def: "Caractère de ce qui est sinueux. Détour.", ex: "La route suit les sinuosités de la côte." },
    { word: "Sirocco", type: "n.m.", def: "Vent chaud et sec venant du Sahara.", ex: "Le sirocco a apporté une chaleur étouffante sur la Sicile." },
    { word: "Sobriquet", type: "n.m.", def: "Surnom familier ou moqueur.", ex: "Tout le monde l'appelle 'Le Grand', c'est son sobriquet depuis l'école." },
    { word: "Soliloque", type: "n.m.", def: "Discours d'une personne qui se parle à elle-même.", ex: "Il s'est lancé dans un long soliloque devant son miroir." },
    { word: "Sollicitude", type: "n.f.", def: "Attention affectueuse et dévouée.", ex: "Il a entouré sa femme malade d'une grande sollicitude." },
    { word: "Solvable", type: "adj.", def: "Qui a de quoi payer ses dettes.", ex: "La banque vérifie que le client est bien solvable avant de lui prêter." },
    { word: "Somatique", type: "adj.", def: "Qui concerne le corps (par opposition au psychique).", ex: "Ses maux de ventre ont une origine somatique bien réelle." },
    { word: "Sommaire", type: "adj. et n.m.", def: "Abrégé, succinct. Table des matières.", ex: "Il nous a fait un compte-rendu très sommaire de la réunion." },
    { word: "Somptueux", type: "adj.", def: "Qui a coûté beaucoup d'argent, magnifique.", ex: "Ils ont organisé une fête somptueuse dans leur château." },
    { word: "Soporifique", type: "adj.", def: "Qui provoque le sommeil. Fig: Très ennuyeux.", ex: "Son discours était tellement soporifique que je me suis endormi." },
    { word: "Sordide", type: "adj.", def: "D'une saleté repoussante. Fig: Bas, ignoble.", ex: "Il vit dans un appartement sordide au fond d'une impasse." },
    { word: "Sottise", type: "n.f.", def: "Manque d'intelligence. Chose bête dite ou faite.", ex: "Il a fait la sottise de laisser ses clés à l'intérieur." },
    { word: "Souffreteux", type: "adj.", def: "De santé fragile, chétif.", ex: "C'est un enfant souffreteux qui attrape tous les microbes." },
    { word: "Souveraineté", type: "n.f.", def: "Pouvoir suprême.", ex: "Le peuple exerce sa souveraineté par le vote." },
    { word: "Spacieux", type: "adj.", def: "Qui offre beaucoup d'espace, vaste.", ex: "Ils ont emménagé dans un appartement spacieux et lumineux." },
    { word: "Spasmodique", type: "adj.", def: "Relatif aux spasmes. Fig: Qui se produit par accès brusques.", ex: "On entendait le bruit spasmodique d'une vieille machine." },
    { word: "Spécieux", type: "adj.", def: "Qui a une belle apparence mais manque de vérité (argument).", ex: "Son raisonnement est spécieux : il semble logique mais il est faux." },
    { word: "Spoliation", type: "n.f.", def: "Action de dépouiller quelqu'un par violence ou fraude.", ex: "Il a été victime d'une spoliation de ses biens pendant la guerre." },
    { word: "Sporadique", type: "adj.", def: "Qui se produit de temps en temps, de façon irrégulière.", ex: "On entend encore des tirs sporadiques dans les faubourgs de la ville." },
    { word: "Spumeux", type: "adj.", def: "Qui est couvert d'écume.", ex: "Le cheval avait les flancs spumeux après sa course folle." },
    { word: "Squalide", type: "adj.", def: "D'une malpropreté dégoûtante.", ex: "Les prisonniers vivaient dans des conditions squalides." },
    { word: "Stagnation", type: "n.f.", def: "État de ce qui ne bouge pas. Arrêt de l'activité.", ex: "L'économie du pays connaît une période de stagnation." },
    { word: "Statique", type: "adj.", def: "Qui reste au repos, sans mouvement.", ex: "C'est une analyse statique qui ne tient pas compte des évolutions." },
    { word: "Stature", type: "n.f.", def: "Taille d'une personne. Fig: Importance morale ou intellectuelle.", ex: "C'est un homme d'État d'une stature internationale." },
    { word: "Stéréotype", type: "n.m.", def: "Opinion toute faite, cliché.", ex: "Il faut dépasser les stéréotypes sur les cultures étrangères." },
    { word: "Stérile", type: "adj.", def: "Qui ne produit rien. Fig: Inutile.", ex: "Leurs discussions sont restées stériles et n'ont abouti à rien." },
    { word: "Stigmatiser", type: "v.", def: "Marquer au fer rouge. Fig: Blâmer publiquement et sévèrement.", ex: "Le Premier ministre a stigmatisé le comportement irresponsable de certains." },
    { word: "Stoïque", type: "adj.", def: "Qui supporte la douleur ou le malheur avec fermeté.", ex: "Il est resté stoïque malgré les insultes de la foule." },
    { word: "Stupéfiant", type: "adj. et n.m.", def: "Qui étonne au plus haut point. Drogue.", ex: "Les résultats de cette étude sont tout à fait stupéfiants." },
    { word: "Suave", type: "adj.", def: "D'une douceur délicieuse (odeur, voix).", ex: "Un parfum suave se dégageait des gardénias." },
    { word: "Subalterne", type: "adj. et n.", def: "Qui occupe un rang inférieur.", ex: "Il traite ses subalternes avec beaucoup de mépris." },
    { word: "Subir", type: "v.", def: "Supporter une chose pénible.", ex: "Il a dû subir une opération de plusieurs heures." },
    { word: "Subjuguer", type: "v.", def: "Soumettre par la force. Fig: Charmer, captiver.", ex: "L'orateur a réussi à subjuguer tout l'auditoire." }
    { word: "Suborner", type: "v.", def: "Corrompre quelqu'un, l'inciter à faire le mal (témoin).", ex: "Il a tenté de suborner le témoin pour qu'il mente au procès." },
    { word: "Subreptice", type: "adj.", def: "Fait furtivement, de façon cachée et déloyale.", ex: "Il a jeté un coup d'œil subreptice vers la sortie, cherchant à s'échapper." },
    { word: "Subsidiaire", type: "adj.", def: "Qui vient en second lieu, qui renforce l'élément principal.", ex: "C'est une question subsidiaire qui départagera les candidats." },
    { word: "Substantiel", type: "adj.", def: "Important, considérable. Nourrissant.", ex: "Il a reçu une augmentation substantielle de son salaire." },
    { word: "Subtil", type: "adj.", def: "Fin, délié, qui a de la pénétration d'esprit.", ex: "Il a fait une analyse subtile de la situation politique." },
    { word: "Succédané", type: "n.m.", def: "Produit de remplacement (souvent de moins bonne qualité).", ex: "La chicorée est un succédané du café." },
    { word: "Succinct", type: "adj.", def: "Qui est dit en peu de mots, bref.", ex: "Il m'a fait un résumé succinct de l'affaire." },
    { word: "Suffisance", type: "n.f.", def: "Orgueil, vanité de celui qui est content de lui.", ex: "Il a répondu avec une suffisance qui a agacé tout le monde." },
    { word: "Sujétion", type: "n.f.", def: "État de soumission, contrainte.", ex: "Il vit dans une sujétion totale envers son employeur." },
    { word: "Superfétatoire", type: "adj.", def: "Qui s'ajoute inutilement, superflu.", ex: "Ce rappel au règlement est superfétatoire, tout le monde le connaît." },
    { word: "Supplanter", type: "v.", def: "Prendre la place de quelqu'un (en le renversant).", ex: "Le jeune loup a fini par supplanter le vieux chef." },
    { word: "Supplique", type: "n.f.", def: "Demande écrite adressée avec humilité à une autorité.", ex: "Il a envoyé une supplique au roi pour demander sa grâce." },
    { word: "Suranné", type: "adj.", def: "Qui appartient à une époque révolue, démodé (charme).", ex: "Cette expression a un charme suranné que j'aime beaucoup." },
    { word: "Surseoir", type: "v.", def: "Suspendre, remettre à plus tard (une décision).", ex: "Le tribunal a décidé de surseoir au jugement en attendant le rapport." },
    { word: "Susurrer", type: "v.", def: "Murmurer doucement.", ex: "Elle lui a susurré des mots doux à l'oreille." },
    { word: "Sycophante", type: "n.m.", def: "Délateur, espion, fourbe.", ex: "Ne te confie pas à lui, c'est un sycophante qui répète tout." },
    { word: "Taciturne", type: "adj.", def: "Qui parle peu, d'humeur sombre.", ex: "Depuis son échec, il est devenu taciturne et renfermé." },
    { word: "Tact", type: "n.m.", def: "Délicatesse dans les rapports avec autrui.", ex: "Il a annoncé la mauvaise nouvelle avec beaucoup de tact." },
    { word: "Talion", type: "n.m.", def: "Loi qui inflige au coupable la même peine qu'il a fait subir (œil pour œil).", ex: "Il a appliqué la loi du talion en se vengeant de la même manière." },
    { word: "Tancer", type: "v.", def: "Réprimander vertement.", ex: "La mère a tancé son enfant pour son insolence." },
    { word: "Tangible", type: "adj.", def: "Que l'on peut toucher. Fig: Évident, concret.", ex: "Nous attendons des preuves tangibles avant d'accuser quelqu'un." },
    { word: "Tarauder", type: "v.", def: "Percer. Fig: Tourmenter moralement.", ex: "Le remords le taraude depuis des années." },
    { word: "Targuer (se)", type: "v.", def: "Se vanter, se prévaloir de quelque chose.", ex: "Il se targue d'avoir lu tous les classiques, mais c'est faux." },
    { word: "Tartufe", type: "n.m.", def: "Hypocrite, faux dévot (personnage de Molière).", ex: "C'est un tartufe qui prêche la vertu mais vit dans le vice." },
    { word: "Tautologie", type: "n.f.", def: "Répétition d'une même idée en termes différents (ex: monter en haut).", ex: "'Un sou est un sou' est une tautologie courante." },
    { word: "Téméraire", type: "adj.", def: "Hardi jusqu'à l'imprudence.", ex: "Il a fait une tentative téméraire pour traverser la rivière en crue." },
    { word: "Tempérance", type: "n.f.", def: "Modération dans les plaisirs, sobriété.", ex: "Il fait preuve de tempérance en ne buvant jamais d'alcool." },
    { word: "Ténacité", type: "n.f.", def: "Persistance, obstination.", ex: "Grâce à sa ténacité, il a fini par obtenir ce qu'il voulait." },
    { word: "Ténébreux", type: "adj.", def: "Obscur. Mystérieux et inquiétant.", ex: "C'est un personnage ténébreux dont on ignore les véritables motivations." },
    { word: "Ténu", type: "adj.", def: "Très fin, très mince. Fig: Fragile.", ex: "La différence entre ces deux concepts est très ténue." },
    { word: "Tergiverser", type: "v.", def: "User de détours pour retarder une décision.", ex: "Arrête de tergiverser et dis-moi oui ou non !" },
    { word: "Thaumaturge", type: "n.m.", def: "Faiseur de miracles.", ex: "Certains le considèrent comme un thaumaturge capable de guérir par apposition des mains." },
    { word: "Thésauriser", type: "v.", def: "Amasser de l'argent sans le dépenser ni le faire fructifier.", ex: "L'avare passe son temps à thésauriser des pièces d'or." },
    { word: "Thuriféraire", type: "n.m.", def: "Flatteur excessif.", ex: "Il est entouré de thuriféraires qui applaudissent à toutes ses bêtises." },
    { word: "Timoré", type: "adj.", def: "Qui a peur de tout, trop prudent.", ex: "Il est trop timoré pour oser entreprendre quoi que ce soit." },
    { word: "Tirade", type: "n.f.", def: "Longue suite de phrases déclamatoires.", ex: "Il nous a fait une longue tirade sur l'ingratitude des enfants." },
    { word: "Titanesque", type: "adj.", def: "Gigantesque, démesuré.", ex: "La construction de ce barrage fut un travail titanesque." },
    { word: "Tonitruant", type: "adj.", def: "Qui fait un bruit de tonnerre.", ex: "Il a une voix tonitruante qui fait trembler les murs." },
    { word: "Topique", type: "adj.", def: "Qui se rapporte exactement au sujet (argument).", ex: "Il a cité un exemple topique qui a convaincu tout le monde." },
    { word: "Torpeur", type: "n.f.", def: "Engourdissement, inertie.", ex: "La chaleur de l'après-midi nous a plongés dans une douce torpeur." },
    { word: "Torride", type: "adj.", def: "Très chaud, brûlant.", ex: "Ils ont vécu une passion torride sous les tropiques." },
    { word: "Tortueux", type: "adj.", def: "Qui fait des détours (chemin). Fig: Hypocrite, compliqué.", ex: "Il a un esprit tortueux, il complique toujours tout." },
    { word: "Tractation", type: "n.f.", def: "Négociation (souvent secrète ou louche).", ex: "Des tractations secrètes ont eu lieu avant la signature du contrat." },
    { word: "Transcender", type: "v.", def: "Dépasser, être au-dessus de.", ex: "L'amour peut transcender les barrières culturelles." },
    { word: "Transfuge", type: "n.m.", def: "Personne qui abandonne son parti pour passer à l'ennemi.", ex: "Ce député est un transfuge qui a changé de camp trois fois." },
    { word: "Transiger", type: "v.", def: "Faire des concessions pour s'accorder. (Ne pas transiger : être ferme).", ex: "Je ne transigerai pas sur la qualité du service." },
    { word: "Translucide", type: "adj.", def: "Qui laisse passer la lumière sans permettre de distinguer les objets.", ex: "Le verre dépoli est translucide mais pas transparent." },
    { word: "Transmuer", type: "v.", def: "Changer la nature d'une chose.", ex: "L'alchimiste cherchait à transmuer le plomb en or." },
    { word: "Traquenard", type: "n.m.", def: "Piège.", ex: "Il est tombé dans un traquenard tendu par ses rivaux." },
    { word: "Travestir", type: "v.", def: "Déguiser. Fig: Déformer la vérité.", ex: "Il a travesti les faits pour se donner le beau rôle." },
    { word: "Trépidant", type: "adj.", def: "Qui s'agite, qui vibre. (Vie trépidante).", ex: "Il mène une vie trépidante à New York, sans jamais s'arrêter." },
    { word: "Trépas", type: "n.m.", def: "La mort (littéraire).", ex: "Il est passé de vie à trépas paisiblement dans son sommeil." },
    { word: "Tributaire", type: "adj.", def: "Qui dépend de quelqu'un ou de quelque chose.", ex: "L'agriculture est tributaire des conditions météorologiques." },
    { word: "Trivial", type: "adj.", def: "Vulgaire, grossier. Banal.", ex: "Évitez ces plaisanteries triviales en société." },
    { word: "Tronquer", type: "v.", def: "Retrancher une partie (texte). Mutiler.", ex: "Il a cité mon discours en le tronquant pour en changer le sens." },
    { word: "Truculent", type: "adj.", def: "Haut en couleur, pittoresque, plein de verve.", ex: "C'est un personnage truculent qui raconte des histoires incroyables." },
    { word: "Truisme", type: "n.m.", def: "Vérité banale et évidente.", ex: "Dire qu'il faut manger pour vivre est un truisme." },
    { word: "Tumulte", type: "n.m.", def: "Grand bruit de foule, désordre.", ex: "Le tumulte de la manifestation s'entendait de loin." },
    { word: "Turpitude", type: "n.f.", def: "Indignité, action honteuse.", ex: "Ce livre révèle les turpitudes cachées de la haute société." },
    { word: "Tutélaire", type: "adj.", def: "Qui assure une protection (comme un tuteur).", ex: "Il est la figure tutélaire de ce mouvement artistique." },
    { word: "Ubiquité", type: "n.f.", def: "Capacité d'être partout à la fois.", ex: "Il faudrait avoir le don d'ubiquité pour assister à toutes ces réunions." },
    { word: "Uchronie", type: "n.f.", def: "Récit fictif basé sur une réécriture de l'Histoire.", ex: "Ce roman est une uchronie où Napoléon aurait gagné à Waterloo." },
    { word: "Ulcéré", type: "adj.", def: "Profondément blessé, indigné.", ex: "Il est ulcéré par les propos mensongers de son adversaire." },
    { word: "Ultimatum", type: "n.m.", def: "Dernière proposition avant rupture.", ex: "Le syndicat a lancé un ultimatum à la direction." },
    { word: "Unanime", type: "adj.", def: "Qui est du même avis (tous ensemble).", ex: "Le jury a été unanime pour lui décerner le premier prix." },
    { word: "Urbanité", type: "n.f.", def: "Politesse, usage du monde.", ex: "Il nous a reçus avec une grande urbanité." },
    { word: "Usurpateur", type: "n.m.", def: "Celui qui s'empare d'un pouvoir ou d'un bien sans droit.", ex: "Le roi légitime a chassé l'usurpateur du trône." },
    { word: "Vaciller", type: "v.", def: "Être sur le point de tomber, trembler. Hésiter.", ex: "La flamme de la bougie vacillait dans le courant d'air." },
    { word: "Vacuité", type: "n.f.", def: "Vide (moral ou intellectuel).", ex: "Il a pris conscience de la vacuité de son existence mondaine." },
    { word: "Vaillance", type: "n.f.", def: "Courage, bravoure.", ex: "Les soldats ont combattu avec vaillance." },
    { word: "Vanité", type: "n.f.", def: "Caractère de ce qui est vain. Orgueil.", ex: "Tout est vanité, disait l'Ecclésiaste." },
    { word: "Vaticiner", type: "v.", def: "Prophétiser (souvent avec emphase).", ex: "Il vaticine sans cesse sur la fin du monde." },
    { word: "Véhément", type: "adj.", def: "Qui a une grande force expressive, emporté.", ex: "Il a fait une protestation véhémente contre cette injustice." },
    { word: "Velléitaire", type: "adj.", def: "Qui n'a que des intentions faibles, sans volonté.", ex: "C'est un velléitaire incapable de mener un projet à terme." },
    { word: "Véloce", type: "adj.", def: "Rapide, agile.", ex: "Le cycliste véloce a distancé le peloton." },
    { word: "Vénal", type: "adj.", def: "Qui se vend, qui agit par intérêt financier.", ex: "L'amour ne devrait pas être vénal." },
    { word: "Vénérable", type: "adj.", def: "Digne de respect (souvent par l'âge).", ex: "Le vénérable vieillard a pris la parole." },
    { word: "Ventripotent", type: "adj.", def: "Qui a un gros ventre.", ex: "Un homme ventripotent était assis au comptoir." },
    { word: "Véracité", type: "n.f.", def: "Caractère de ce qui est vrai, exactitude.", ex: "On peut douter de la véracité de son témoignage." },
    { word: "Verbeux", type: "adj.", def: "Qui utilise trop de mots, bavard.", ex: "Son rapport est verbeux et difficile à lire." },
    { word: "Véreux", type: "adj.", def: "Qui contient un ver. Fig: Malhonnête, louche.", ex: "Il a fait affaire avec un notaire véreux qui l'a escroqué." },
    { word: "Verve", type: "n.f.", def: "Inspiration vive et brillante (parole).", ex: "Il a raconté l'histoire avec une verve comique irrésistible." },
    { word: "Vespéral", type: "adj.", def: "Du soir.", ex: "La clarté vespérale descendait sur la ville." },
    { word: "Vestige", type: "n.m.", def: "Trace de ce qui a disparu, ruine.", ex: "Ce mur est le dernier vestige du château médiéval." },
    { word: "Vétille", type: "n.f.", def: "Chose insignifiante.", ex: "Ils se sont disputés pour une vétille." },
    { word: "Vétuste", type: "adj.", def: "Vieux et en mauvais état (bâtiment).", ex: "L'école est vétuste et a besoin de rénovations urgentes." },
    { word: "Viager", type: "adj. et n.m.", def: "Qui dure autant que la vie.", ex: "Il a acheté cet appartement en viager." },
    { word: "Vicissitude", type: "n.f.", def: "Changement (souvent malheureux) dans le cours des choses.", ex: "Il a traversé les vicissitudes de la vie avec courage." },
    { word: "Viduité", type: "n.f.", def: "État de veuvage. (Droit).", ex: "Elle a respecté le délai de viduité avant de se remarier." },
    { word: "Vilipender", type: "v.", def: "Traiter avec mépris, dénigrer.", ex: "La presse a vilipendé le film dès sa sortie." },
    { word: "Vindicatif", type: "adj.", def: "Porté à la vengeance, rancunier.", ex: "Il est très vindicatif et n'oublie jamais une offense." },
    { word: "Virago", type: "n.f.", def: "Femme d'allure masculine et autoritaire.", ex: "Sa tante est une virago qui terrifie toute la famille." },
    { word: "Virtuose", type: "n. et adj.", def: "Personne qui excelle dans un art.", ex: "C'est un virtuose du violon." },
    { word: "Virulent", type: "adj.", def: "Violent, nocif. (Microbe ou critique).", ex: "Il a publié une critique virulente contre le gouvernement." },
    { word: "Viscéral", type: "adj.", def: "Qui vient des tripes, profond et irraisonné.", ex: "Il a une haine viscérale de l'injustice." },
    { word: "Vitupérer", type: "v.", def: "Blâmer avec violence.", ex: "Il passe son temps à vitupérer contre la jeunesse actuelle." },
    { word: "Vivace", type: "adj.", def: "Qui a la vie dure, qui dure longtemps.", ex: "C'est une plante vivace qui repousse chaque année." },
    { word: "Vocable", type: "n.m.", def: "Mot, terme.", ex: "Il utilise des vocables savants pour impressionner." },
    { word: "Vociférer", type: "v.", def: "Parler en criant avec colère.", ex: "Il s'est mis à vociférer des injures contre l'arbitre." },
    { word: "Volage", type: "adj.", def: "Qui change facilement d'amoureux, inconstant.", ex: "Il a la réputation d'être un cœur volage." },
    { word: "Volubile", type: "adj.", def: "Qui parle beaucoup et vite.", ex: "Cette vendeuse volubile a réussi à me convaincre d'acheter." },
    { word: "Volupté", type: "n.f.", def: "Plaisir vif des sens.", ex: "Il savourait ce chocolat avec volupté." },
    { word: "Vorace", type: "adj.", def: "Qui mange avec avidité. Fig: Qui détruit tout.", ex: "L'incendie vorace a détruit toute la forêt." },
    { word: "Votif", type: "adj.", def: "Offert en vertu d'un vœu.", ex: "On a retrouvé des offrandes votives dans le temple." },
    { word: "Vulnérable", type: "adj.", def: "Qui peut être blessé, fragile.", ex: "Les enfants sont les plus vulnérables en temps de guerre." },
    { word: "Xénophile", type: "adj.", def: "Qui aime les étrangers.", ex: "C'est un peuple xénophile et accueillant." },
    { word: "Zélé", type: "adj.", def: "Qui montre beaucoup d'ardeur et de dévouement.", ex: "Un employé trop zélé a jeté des documents importants." },
    { word: "Zénith", type: "n.m.", def: "Point du ciel juste au-dessus. Fig: Apogée.", ex: "Sa carrière est au zénith." },
    { word: "Zizanie", type: "n.f.", def: "Discorde, désunion.", ex: "Il a semé la zizanie dans le groupe avec ses mensonges." },
    { word: "Gorge-de-pigeon", type: "n.m. inv.", def: "Couleur changeante (irisée) mêlant le gris, le rose et le vert, comme le cou d'un pigeon.", ex: "La soie de sa robe avait des reflets gorge-de-pigeon fascinants." },
    { word: "Incarnadin", type: "adj.", def: "D'un rouge pâle ou rose chair (couleur de la chair vive).", ex: "L'émotion fit monter une teinte incarnadine à ses joues." },
    { word: "Smaragdin", type: "adj.", def: "D'un vert émeraude éclatant (littéraire).", ex: "Le dragon fixait le chevalier de ses yeux smaragdins." },
    { word: "Fuligineux", type: "adj.", def: "Qui a la couleur ou l'aspect de la suie (noirâtre). Fig: Obscur.", ex: "Le ciel fuligineux annonçait un orage terrible." },
    { word: "Alezan", type: "adj. et n.m.", def: "Couleur fauve, brun roussâtre (se dit surtout pour les chevaux).", ex: "Il montait un superbe étalon alezan qui brillait au soleil." },
    { word: "Garance", type: "n.f. et adj.", def: "Rouge vif tiré de la racine d'une plante (couleur des anciens pantalons militaires).", ex: "Les soldats portaient autrefois des pantalons couleur garance très voyants." },
    { word: "Pers", type: "adj.", def: "D'une couleur indéfinissable entre le bleu, le vert et le gris (souvent pour les yeux).", ex: "La déesse Athéna est souvent décrite avec des yeux pers." },
    { word: "Mirliflore", type: "n.m.", def: "Jeune élégant, prétentieux et très soucieux de sa toilette (Dandy ridicule).", ex: "Ce jeune mirliflore passe plus de temps devant son miroir que ses amies." },
    { word: "Diapré", type: "adj.", def: "De couleurs variées et changeantes, scintillant.", ex: "Au printemps, les prairies sont diaprées de mille fleurs." },
    { word: "Amarante", type: "n.f. et adj.", def: "Rouge pourpre velouté (couleur d'une fleur qui ne fane pas).", ex: "Les rideaux de velours amarante donnaient au salon un air théâtral." },
    { word: "Jais", type: "n.m.", def: "Pierre d'un noir brillant et dur. (Noir de jais).", ex: "Elle avait une chevelure noir de jais qui contrastait avec sa peau pâle." },
    { word: "Ocelé", type: "adj.", def: "Marqué de taches rondes bordées d'une autre couleur (comme le léopard).", ex: "Le papillon avait des ailes ocelées pour effrayer les prédateurs." },
    { word: "Nankin", type: "n.m. et adj.", def: "Jaune clair ou beige (couleur d'une toile de coton de Nankin).", ex: "Il portait un gilet nankin très à la mode au siècle dernier." },
    { word: "Glauque", type: "adj.", def: "Vert pâle tirant sur le bleu (couleur de l'eau de mer). Fig: Sordide.", ex: "Ses yeux étaient d'un glauque transparent, comme l'eau du lagon." },
    { word: "Céruléen", type: "adj.", def: "D'un bleu ciel profond.", ex: "La voûte céruléenne s'étendait à l'infini au-dessus de l'océan." },
    { word: "Mordoré", type: "adj.", def: "D'un brun chaud avec des reflets dorés.", ex: "Les feuilles d'automne formaient un tapis mordoré dans la forêt." },
    { word: "Opalin", type: "adj.", def: "Qui a les reflets laiteux et irisés de l'opale.", ex: "La lumière opaline du petit matin baignait la chambre." },
    { word: "Bis", type: "adj.", def: "D'un gris beige un peu sale (pain bis).", ex: "Le paysan rompit le pain bis et le partagea." },
    { word: "Vermeil", type: "adj. et n.m.", def: "D'un rouge éclatant. Argent plaqué d'or.", ex: "Elle avait les lèvres vermeilles comme la rose." },
    { word: "Zibeline", type: "adj. et n.f.", def: "Fourrure précieuse. Couleur brun foncé chaud.", ex: "Ses sourcils zibeline soulignaient son regard intense." }
    { word: "Coquecigrue", type: "n.f.", def: "Animal imaginaire, absurde. Fig: Illusion, baliverne.", ex: "Il m'a raconté des coquecigrues pour justifier son retard." },
    { word: "Pétrichor", type: "n.m.", def: "Odeur particulière de la terre après la pluie.", ex: "J'aime respirer le pétrichor qui monte du jardin après l'orage." },
    { word: "Brimborion", type: "n.m.", def: "Petit objet de peu de valeur, babiole.", ex: "Son bureau est encombré de brimborions et de souvenirs inutiles." },
    { word: "Tintinnabuler", type: "v.", def: "Produire un son de clochette (cristallin).", ex: "Les glaçons tintinnabulent dans les verres en cristal." },
    { word: "Palimpseste", type: "n.m.", def: "Parchemin dont on a effacé la première écriture pour écrire dessus. Fig: Chose qui garde des traces du passé.", ex: "La mémoire de cette ville est un palimpseste, chaque siècle recouvrant le précédent." },
    { word: "Coruscant", type: "adj.", def: "Qui brille, qui scintille (soutenu).", ex: "Il est apparu dans une armure coruscante sous les projecteurs." },
    { word: "Marcescent", type: "adj.", def: "Se dit d'une feuille qui fane mais reste accrochée à l'arbre tout l'hiver.", ex: "Les chênes gardent leur feuillage marcescent jusqu'au printemps." },
    { word: "Nyctalope", type: "adj. et n.", def: "Qui a la faculté de voir dans la pénombre.", ex: "Le chat est un animal nyctalope, roi de la nuit." },
    { word: "Effluve", type: "n.m.", def: "Émanation (odeur) qui se dégage des corps ou des substances.", ex: "Un effluve de jasmin flottait dans l'air du soir." },
    { word: "Iridescence", type: "n.f.", def: "Propriété de certaines surfaces qui changent de couleur selon l'angle (arc-en-ciel).", ex: "J'admirais l'iridescence d'une bulle de savon au soleil." },
    { word: "Vaporeux", type: "adj.", def: "Qui a la légèreté de la vapeur, flou, impalpable.", ex: "Elle portait une robe en tulle vaporeux." },
    { word: "Déliquescence", type: "n.f.", def: "Fait de tomber en eau (chimie). Fig: Décadence, perte de force et de morale.", ex: "Ce poète décrit la déliquescence d'une société en fin de règne." },
    { word: "Empyrée", type: "n.m.", def: "La plus haute partie du ciel (mythologie). Fig: Monde idéal.", ex: "Il vit dans l'empyrée des grands artistes, loin des soucis matériels." },
    { word: "Cygne", type: "n.m.", def: "Grand oiseau blanc. (Chant du cygne : dernière œuvre avant la mort).", ex: "Ce magnifique concert fut son chant du cygne." },
    { word: "Lépidoptère", type: "n.m.", def: "Nom savant des papillons.", ex: "Ce collectionneur possède des lépidoptères rares aux couleurs éclatantes." },
    { word: "Fulgurance", type: "n.f.", def: "Éclat passager et très vif (comme l'éclair).", ex: "Il a eu une fulgurance de génie qui a résolu le problème." },
    { word: "Nacré", type: "adj.", def: "Qui a les reflets irisés de la nacre (intérieur des coquillages).", ex: "Le ciel avait une teinte nacrée avant le lever du soleil." },
    { word: "Spleen", type: "n.m.", def: "Mélancolie sans cause apparente, ennui profond (Baudelaire).", ex: "Les jours de pluie réveillent mon spleen habituel." },
    { word: "Volute", type: "n.f.", def: "Forme en spirale (fumée, architecture).", ex: "Il suivait des yeux les volutes de fumée de son cigare." },
    { word: "Zéphire", type: "n.m.", def: "Vent doux et agréable (aussi écrit Zéphyr).", ex: "Un léger zéphire agitait les feuilles du tremble." }
    { word: "Éphélides", type: "n.f. pl.", def: "Nom poétique et médical des taches de rousseur.", ex: "Le soleil d'été avait semé une constellation d'éphélides sur son nez." },
    { word: "Hyalin", type: "adj.", def: "Qui a la transparence du verre ou du cristal. (Quartz hyalin).", ex: "L'air du matin était d'une pureté hyaline." },
    { word: "Céladon", type: "n.m. et adj.", def: "Vert pâle et tendre (céramique). Fig: Amoureux sentimental et platonique.", ex: "Il lui a offert un vase céladon en lui récitant des vers." },
    { word: "Incunable", type: "n.m.", def: "Livre imprimé aux débuts de l'imprimerie (avant 1500). Très rare.", ex: "Ce collectionneur a déniché un incunable poussiéreux dans un grenier." },
    { word: "Ichor", type: "n.m.", def: "Sang incolore des dieux dans la mythologie grecque.", ex: "Dans ses veines ne coule pas du sang, mais de l'ichor divin." },
    { word: "Mandragore", type: "n.f.", def: "Plante aux racines fourchues à laquelle les légendes prêtent des vertus magiques.", ex: "La sorcière préparait une potion à base de bave de crapaud et de mandragore." },
    { word: "Clepsydre", type: "n.f.", def: "Horloge à eau utilisée dans l'Antiquité.", ex: "Le temps s'écoulait goutte à goutte, comme dans une clepsydre invisible." },
    { word: "Boustrophédon", type: "n.m. et adj.", def: "Écriture archaïque dont les lignes se lisent alternativement de gauche à droite puis de droite à gauche (comme le bœuf au labour).", ex: "Cette inscription antique est gravée en boustrophédon." },
    { word: "Albâtre", type: "n.m.", def: "Pierre blanche et translucide. Fig: Blancheur éclatante (peau).", ex: "Elle avait un teint d'albâtre que le soleil n'avait jamais touché." },
    { word: "Népenthès", type: "n.m.", def: "Breuvage magique censé dissiper la tristesse et l'oubli.", ex: "Il cherchait dans le vin le népenthès qui lui ferait oublier ses chagrins." },
    { word: "Firmament", type: "n.m.", def: "La voûte céleste, le ciel étoilé.", ex: "Les étoiles scintillaient au firmament comme des diamants sur du velours." },
    { word: "Cariatide", type: "n.f.", def: "Statue de femme soutenant une corniche (servant de colonne).", ex: "L'entrée du temple était gardée par quatre cariatides de marbre." },
    { word: "Aquilin", type: "adj.", def: "Recourbé comme le bec de l'aigle (nez).", ex: "Son profil sévère était marqué par un nez aquilin." },
    { word: "Simoun", type: "n.m.", def: "Vent brûlant et violent du désert.", ex: "Le souffle du simoun soulevait des tourbillons de sable rouge." },
    { word: "Ambroisie", type: "n.f.", def: "Nourriture des dieux de l'Olympe (qui donne l'immortalité).", ex: "Ce gâteau était si délicieux qu'on aurait dit de l'ambroisie." },
    { word: "Obsidienne", type: "n.f.", def: "Roche volcanique vitreuse d'un noir profond et coupant.", ex: "Son regard était noir et dur comme de l'obsidienne." },
    { word: "Filigrane", type: "n.m.", def: "Ouvrage d'orfèvrerie très fin. (En filigrane : de manière implicite).", ex: "La tristesse de l'auteur se lit en filigrane tout au long du roman." },
    { word: "Mausolée", type: "n.m.", def: "Monument funéraire somptueux et grandiose.", ex: "Il s'est fait construire un véritable mausolée de son vivant." },
    { word: "Opuscule", type: "n.m.", def: "Petit livre, petit ouvrage (souvent scientifique ou littéraire).", ex: "Il a publié un opuscule sur la vie des fourmis rouges." },
    { word: "Astre", type: "n.m.", def: "Corps céleste (étoile, planète). Fig: Personne brillante.", ex: "C'est l'astre montant de la nouvelle scène musicale." },
    { word: "Basilic", type: "n.m.", def: "Reptile légendaire dont le regard tuait. (Aussi une plante).", ex: "Il me foudroya d'un regard de basilic." },
    { word: "Incipit", type: "n.m.", def: "Premiers mots d'un livre ou d'un manuscrit.", ex: "L'incipit de ce roman ('Longtemps, je me suis couché de bonne heure') est mondialement connu." },
    { word: "Grimoire", type: "n.m.", def: "Livre de magie indéchiffrable. Fig: Écrit incompréhensible.", ex: "Ses notes de cours sont un véritable grimoire que personne ne peut relire." },
    { word: "Philtre", type: "n.m.", def: "Breuvage magique propre à inspirer l'amour.", ex: "Tristan et Iseult ont bu le philtre d'amour par erreur." },
    { word: "Zébrure", type: "n.f.", def: "Rayure semblable à celle du zèbre (souvent marque de fouet ou éclair).", ex: "Une zébrure d'éclair déchira le ciel nocturne." },
    { word: "Volute", type: "n.f.", def: "Enroulement en spirale (fumée, chapiteau ionique).", ex: "Les volutes de son cigare montaient lentement vers le plafond." },
    { word: "Sélénite", type: "adj. et n.", def: "Relatif à la Lune. Habitant imaginaire de la Lune.", ex: "Jules Verne a imaginé la rencontre avec les Sélénites." },
    { word: "Porphyre", type: "n.m.", def: "Roche rouge foncé très dure, symbole impérial.", ex: "Le sarcophage de l'empereur était taillé dans un bloc de porphyre." },
    { word: "Chimérique", type: "adj.", def: "Qui n'est qu'une illusion, impossible à réaliser (utopique).", ex: "La poursuite de l'éternelle jeunesse est une quête chimérique." },
    { word: "Dédale", type: "n.m.", def: "Labyrinthe. Lieu où l'on se perd.", ex: "Les vieilles rues de la ville forment un dédale inextricable." }
    { word: "Adamantin", type: "adj.", def: "Qui a l'éclat ou la dureté du diamant.", ex: "Les sommets enneigés brillaient d'un éclat adamantin sous le soleil." },
    { word: "Éburnéen", type: "adj.", def: "Qui a la blancheur et l'aspect de l'ivoire.", ex: "Elle passait sa main éburnéenne dans ses cheveux sombres." },
    { word: "Purpurin", type: "adj.", def: "De couleur pourpre (souvent pour les lèvres ou le visage).", ex: "L'émotion colora ses joues d'une teinte purpurine." },
    { word: "Matutinal", type: "adj.", def: "Qui appartient au matin (littéraire).", ex: "Il aime se promener dans la brume matutinale." },
    { word: "Sépulcre", type: "n.m.", def: "Tombeau. Fig: Lieu silencieux et sombre.", ex: "Cette maison vide et froide ressemble à un sépulcre." },
    { word: "Catafalque", type: "n.m.", def: "Estrade décorée sur laquelle on place le cercueil lors d'une cérémonie funèbre.", ex: "Le cercueil du roi reposait sur un immense catafalque de velours noir." },
    { word: "Aquilon", type: "n.m.", def: "Vent du nord, froid et violent (poétique).", ex: "L'aquilon soufflait en tempête, faisant gémir les vieux arbres." },
    { word: "Frimas", type: "n.m.", def: "Brouillard épais qui gèle en tombant (givre).", ex: "Il s'enveloppa dans son manteau pour affronter les frimas de l'hiver." },
    { word: "Ondine", type: "n.f.", def: "Génie des eaux dans la mythologie nordique.", ex: "Elle nageait avec la grâce d'une ondine." },
    { word: "Sylphide", type: "n.f.", def: "Génie de l'air (femme). Fig: Femme mince et gracieuse.", ex: "C'est une véritable sylphide, elle semble ne pas toucher terre quand elle marche." },
    { word: "Séraphique", type: "adj.", def: "Qui appartient aux anges (séraphins). Fig: D'une pureté angélique.", ex: "Elle affichait un sourire séraphique et innocent." },
    { word: "Fiel", type: "n.m.", def: "Liquide amer (bile). Fig: Haine, méchanceté amère.", ex: "Il a déversé tout son fiel dans une lettre d'insultes." },
    { word: "Cinabre", type: "n.m.", def: "Rouge vermillon très vif (pigment).", ex: "Les colonnes du temple étaient peintes en cinabre éclatant." },
    { word: "Opalescent", type: "adj.", def: "Qui a des reflets laiteux et bleutés comme l'opale.", ex: "La lune diffusait une lumière opalescente sur le lac." },
    { word: "Sourdre", type: "v.", def: "Jaillir silencieusement (eau). Fig: Naître (sentiment).", ex: "Une sourde inquiétude commençait à sourdre en lui." },
    { word: "Exhaler", type: "v.", def: "Dégager, répandre (une odeur, un souffle).", ex: "La rose exhale son parfum le plus fort le soir." },
    { word: "Vénusté", type: "n.f.", def: "Beauté gracieuse et élégante (comme Vénus).", ex: "Le peintre a tenté de capturer la vénusté de son modèle." },
    { word: "Pandémonium", type: "n.m.", def: "Capitale imaginaire de l'Enfer. Fig: Lieu de désordre et de vacarme.", ex: "La salle de classe est devenue un véritable pandémonium en l'absence du professeur." },
    { word: "Égide", type: "n.f.", def: "Bouclier de Zeus/Athéna. (Sous l'égide de : sous la protection de).", ex: "La conférence s'est tenue sous l'égide de l'UNESCO." },
    { word: "Linceul", type: "n.m.", def: "Drap dans lequel on ensevelit un mort. Fig: Ce qui couvre comme un voile.", ex: "Un linceul de neige recouvrait toute la plaine." },
    { word: "Cime", type: "n.f.", def: "Le sommet pointu d'une montagne ou d'un arbre.", ex: "L'aigle planait au-dessus des cimes enneigées." },
    { word: "Abîme", type: "n.m.", def: "Gouffre très profond. Fig: Situation désespérée ou mystère insondable.", ex: "Il a plongé son regard dans l'abîme de ses yeux noirs." },
    { word: "Suave", type: "adj.", def: "D'une douceur exquise et délicate (goût, odeur, son).", ex: "Une mélodie suave nous parvenait du jardin." },
    { word: "Miasme", type: "n.m.", def: "Émanation malsaine provenant de matières décomposées.", ex: "Loin des miasmes de la ville, il revit à la campagne." },
    { word: "Météore", type: "n.m.", def: "Phénomène atmosphérique. Fig: Personne qui brille peu de temps mais intensément.", ex: "Ce poète fut un météore dans le ciel littéraire du XIXe siècle." },
    { word: "Zénith", type: "n.m.", def: "Point du ciel à la verticale. Fig: Le point culminant.", ex: "Le soleil était au zénith et la chaleur était écrasante." },
    { word: "Nadir", type: "n.m.", def: "Point du ciel opposé au zénith (sous nos pieds). Fig: Le point le plus bas.", ex: "Son moral a atteint le nadir après cette terrible nouvelle." },
    { word: "Atrabile", type: "n.f.", def: "Bile noire (selon les anciens). Fig: Mélancolie, mauvaise humeur.", ex: "Il est sujet à des accès d'atrabile qui le rendent insupportable." },
    { word: "Cygne", type: "n.m.", def: "Oiseau blanc. (Le chant du cygne : la dernière œuvre avant la mort).", ex: "Ce dernier roman fut son chant du cygne, il mourut peu après." },
    { word: "Apostille", type: "n.f.", def: "Note ajoutée en marge d'un écrit.", ex: "Il a ajouté une apostille ironique en bas de la lettre." }
   ];
     

        // ==========================================
        // LOGIQUE APPRENTISSAGE
        // ==========================================
        function newWord() {
            const section = document.getElementById('learn-section');
            
            // Petit effet de fade
            section.style.opacity = 0;
            section.style.transform = 'translateY(10px)';

            setTimeout(() => {
                const randomItem = vocabulaire[Math.floor(Math.random() * vocabulaire.length)];
                document.getElementById('word').innerText = randomItem.word;
                document.getElementById('type').innerText = randomItem.type;
                document.getElementById('definition').innerText = randomItem.def;
                document.getElementById('example').innerText = `"${randomItem.ex}"`;
                
                section.style.opacity = 1;
                section.style.transform = 'translateY(0)';
            }, 200);
        }

        // ==========================================
        // LOGIQUE QUIZ
        // ==========================================
        let quizState = {
            questions: [],
            currentIndex: 0,
            score: 0,
            waiting: false
        };

        function startQuiz() {
            // 1. Sélectionner 10 mots au hasard
            let pool = [...vocabulaire]; // Copie pour ne pas modifier l'original
            let selectedWords = [];
            
            for(let i=0; i<10; i++) {
                if(pool.length === 0) break;
                const randomIndex = Math.floor(Math.random() * pool.length);
                selectedWords.push(pool[randomIndex]);
                pool.splice(randomIndex, 1); // Retirer pour ne pas avoir de doublons
            }

            quizState.questions = selectedWords;
            quizState.currentIndex = 0;
            quizState.score = 0;
            quizState.waiting = false;

            // UI Updates
            document.getElementById('quiz-start').classList.add('hidden');
            document.getElementById('quiz-result').classList.add('hidden');
            document.getElementById('quiz-question').classList.remove('hidden');
            
            loadQuestion();
        }

        function loadQuestion() {
            const currentItem = quizState.questions[quizState.currentIndex];
            const optionsContainer = document.getElementById('q-options');
            
            // Mise à jour textes
            document.getElementById('q-progress').innerText = `Question ${quizState.currentIndex + 1}/10`;
            document.getElementById('q-score').innerText = `Score: ${quizState.score}`;
            document.getElementById('q-word').innerText = currentItem.word;
            
            // Générer les réponses (1 bonne + 3 mauvaises)
            let answers = [];
            answers.push({ text: currentItem.def, correct: true });

            // Trouver 3 mauvaises réponses
            let wrongPool = vocabulaire.filter(w => w.word !== currentItem.word);
            for(let i=0; i<3; i++) {
                const rnd = Math.floor(Math.random() * wrongPool.length);
                answers.push({ text: wrongPool[rnd].def, correct: false });
                wrongPool.splice(rnd, 1); // Éviter doublons de mauvaises réponses
            }

            // Mélanger les réponses
            answers.sort(() => Math.random() - 0.5);

            // Créer les boutons HTML
            optionsContainer.innerHTML = '';
            answers.forEach(ans => {
                const btn = document.createElement('button');
                btn.className = 'option-btn';
                btn.innerText = ans.text;
                btn.onclick = () => handleAnswer(btn, ans.correct);
                optionsContainer.appendChild(btn);
            });
        }

        function handleAnswer(btn, isCorrect) {
            if(quizState.waiting) return; // Empêcher le double clic
            quizState.waiting = true;

            const allBtns = document.querySelectorAll('.option-btn');
            
            if(isCorrect) {
                btn.classList.add('correct');
                quizState.score++;
            } else {
                btn.classList.add('wrong');
                // Montrer la bonne réponse
                allBtns.forEach(b => {
                    if(vocabulaire.find(v => v.def === b.innerText && v.word === quizState.questions[quizState.currentIndex].word)) {
                       // Logique complexe pour retrouver le texte exact, 
                       // plus simple : on marque celle qui était correcte dans l'objet JS
                       // mais ici on n'a pas gardé la ref.
                       // Astuce: On sait que seule 1 est correcte.
                    }
                });
                // Méthode simple pour révéler la vraie : on a besoin de passer l'info.
                // On va tricher : on sait que le texte du bouton correspond à la définition
                let correctDef = quizState.questions[quizState.currentIndex].def;
                allBtns.forEach(b => {
                    if(b.innerText === correctDef) b.classList.add('correct');
                });
            }

            // Attendre 1.5 seconde avant la suite
            setTimeout(() => {
                quizState.currentIndex++;
                quizState.waiting = false;
                
                if(quizState.currentIndex < 10) {
                    loadQuestion();
                } else {
                    showResults();
                }
            }, 1500);
        }

        function showResults() {
            document.getElementById('quiz-question').classList.add('hidden');
            document.getElementById('quiz-result').classList.remove('hidden');
            document.getElementById('quiz-result').classList.add('fade-in');
            
            document.getElementById('final-score').innerText = `${quizState.score}/10`;
            
            let msg = "";
            if(quizState.score === 10) msg = "Parfait ! Une maîtrise digne de l'Académie.";
            else if(quizState.score >= 8) msg = "Excellent niveau !";
            else if(quizState.score >= 5) msg = "Pas mal, mais peut mieux faire.";
            else msg = "Il faut retourner en mode Apprentissage !";
            
            document.getElementById('final-message').innerText = msg;
        }

        // ==========================================
        // GESTION DES MODES
        // ==========================================
        function switchMode(mode) {
            const learnSec = document.getElementById('learn-section');
            const quizSec = document.getElementById('quiz-section');
            const btnLearn = document.getElementById('btn-learn');
            const btnQuiz = document.getElementById('btn-quiz');

            if(mode === 'learn') {
                learnSec.classList.remove('hidden');
                quizSec.classList.add('hidden');
                btnLearn.classList.add('active');
                btnQuiz.classList.remove('active');
                newWord(); // Rafraîchir
            } else {
                learnSec.classList.add('hidden');
                quizSec.classList.remove('hidden');
                btnLearn.classList.remove('active');
                btnQuiz.classList.add('active');
                
                // Réinitialiser l'écran de démarrage du quiz
                document.getElementById('quiz-start').classList.remove('hidden');
                document.getElementById('quiz-question').classList.add('hidden');
                document.getElementById('quiz-result').classList.add('hidden');
            }
        }

        // Init au chargement
        window.onload = () => {
            document.getElementById('learn-section').style.transition = "all 0.3s ease-out";
            newWord();
        };

    </script>
</body>
</html>
