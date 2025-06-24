# code-quiz-challenge
Code Quiz Challenge : apprenez HTML, CSS et JS avec 176 questions interactives ! Quiz et Associer le Code, inspiré de Touche les Animaux ! et plus.
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Code Quiz Challenge</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            background-color: #e8f1f2;
        }
        #gameArea {
            text-align: center;
            max-width: 600px;
            padding: 20px;
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        #question, #codeQuestion {
            font-size: 20px;
            margin-bottom: 20px;
            white-space: pre-wrap;
        }
        .answer, .code-answer {
            display: block;
            padding: 10px;
            margin: 5px;
            font-size: 16px;
            background-color: #2a9d8f;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.2s;
        }
        .answer:hover, .code-answer:hover {
            background-color: #264653;
        }
        .answer:disabled, .code-answer:disabled {
            background-color: #ccc;
            cursor: not-allowed;
        }
        #startButton, #switchModeButton {
            padding: 10px 20px;
            font-size: 16px;
            background-color: #e76f51;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin: 10px;
        }
        #result, #timer, #highScore, #feedback {
            margin-top: 20px;
            font-size: 18px;
            color: #264653;
        }
        #timer {
            color: #e76f51;
        }
        #feedback {
            color: #e76f51;
            font-size: 16px;
        }
        #codeArea {
            display: none;
        }
    </style>
</head>
<body>
    <div id="gameArea">
        <h1>Code Quiz Challenge</h1>
        <button id="switchModeButton">Passer au mode Associer le Code</button>
        <p id="timer">Temps : 10s</p>
        <p id="question"></p>
        <div id="answers"></div>
        <p id="codeQuestion"></p>
        <div id="codeAnswers"></div>
        <p id="feedback"></p>
        <button id="startButton">Démarrer</button>
        <p id="result">Score : 0</p>
        <p id="highScore">Meilleur score : 0</p>
    </div>

    <script>
        const quizQuestions = [
            {"question":"Dans ton jeu 'Touche les Animaux !', quelle propriété CSS utiliserais-tu pour animer un animal qui bouge ?","answers":["transform","color","font-size","border"],"correct":0,"feedback":"La propriété `transform` permet de déplacer, tourner ou redimensionner un élément."},
            {"question":"Dans ta plateforme islamique, quelle balise HTML utiliserais-tu pour structurer un menu de navigation ?","answers":["<nav>","<footer>","<section>","<article>"],"correct":0,"feedback":"La balise `<nav>` est utilisée pour définir un bloc de navigation."},
            {"question":"En JavaScript, quelle méthode ajoute un élément à la fin d'un tableau ?","answers":["push()","pop()","shift()","unshift()"],"correct":0,"feedback":"`push()` ajoute un élément à la fin d'un tableau."},
            {"question":"En CSS, quelle propriété définit la couleur du texte ?","answers":["background-color","color","font-size","text-align"],"correct":1,"feedback":"La propriété `color` définit la couleur du texte."},
            {"question":"Dans 'Touche les Animaux !', quel événement JavaScript détecte un clic sur un animal ?","answers":["onhover","onclick","onchange","onload"],"correct":1,"feedback":"`onclick` détecte un clic sur un élément."},
            {"question":"Dans ta plateforme de paris, quelle méthode JavaScript utiliserais-tu pour générer un coupon aléatoire ?","answers":["Math.random()","setInterval()","querySelector()","addEventListener()"],"correct":0,"feedback":"`Math.random()` génère un nombre aléatoire, utile pour un coupon."},
            {"question":"Dans ta plateforme islamique, quelle propriété CSS utiliserais-tu pour centrer un quiz interactif ?","answers":["float","margin: auto","position: fixed","display: inline"],"correct":1,"feedback":"`margin: auto` centre un élément avec une largeur définie."},
            {"question":"Dans 'Touche les Animaux !', quelle méthode JavaScript permet de mettre à jour le score affiché ?","answers":["innerHTML","appendChild()","classList.add()","setTimeout()"],"correct":0,"feedback":"`innerHTML` modifie le contenu d’un élément, comme le score."},
            {"question":"Dans ta plateforme de paris, quelle balise HTML encapsulerait une liste de matchs ?","answers":["<ul>","<div>","<p>","<span>"],"correct":0,"feedback":"`<ul>` est utilisé pour une liste non ordonnée, comme des matchs."},
            {"question":"Dans 'Touche les Animaux !', quelle propriété CSS rend un animal semi-transparent pendant une animation ?","answers":["opacity","visibility","display","z-index"],"correct":0,"feedback":"`opacity` contrôle la transparence d’un élément."},
            {"question":"Dans ta plateforme islamique, quelle balise HTML utiliserais-tu pour un titre principal ?","answers":["<h1>","<p>","<div>","<span>"],"correct":0,"feedback":"`<h1>` est utilisé pour un titre principal."},
            {"question":"En JavaScript, quelle méthode supprime le premier élément d’un tableau ?","answers":["pop()","shift()","slice()","splice()"],"correct":1,"feedback":"`shift()` supprime le premier élément d’un tableau."},
            {"question":"Dans ta plateforme de paris, quelle propriété CSS alignerait les cotes à droite ?","answers":["text-align: right","float: left","margin: auto","display: block"],"correct":0,"feedback":"`text-align: right` aligne le texte à droite."},
            {"question":"Dans 'Touche les Animaux !', quel attribut HTML associe un événement à un élément ?","answers":["class","id","onclick","style"],"correct":2,"feedback":"`onclick` est un attribut pour associer un événement de clic."},
            {"question":"En CSS, quelle propriété définit l’espacement entre les éléments flex ?","answers":["gap","margin","padding","border"],"correct":0,"feedback":"`gap` définit l’espacement entre les éléments dans un conteneur flex."},
            {"question":"Dans ta plateforme islamique, quelle balise HTML utiliserais-tu pour un paragraphe de texte ?","answers":["<p>","<div>","<span>","<section>"],"correct":0,"feedback":"`<p>` est utilisé pour un paragraphe."},
            {"question":"En JavaScript, quelle méthode vérifie si un élément existe dans un tableau ?","answers":["includes()","find()","filter()","map()"],"correct":0,"feedback":"`includes()` vérifie la présence d’un élément dans un tableau."},
            {"question":"Dans 'Touche les Animaux !', quelle propriété CSS contrôle la vitesse d’une animation ?","answers":["animation-duration","animation-delay","transition","transform"],"correct":0,"feedback":"`animation-duration` définit la durée d’une animation."},
            {"question":"Dans ta plateforme de paris, quelle méthode JavaScript boucle sur un tableau de matchs ?","answers":["forEach()","push()","pop()","slice()"],"correct":0,"feedback":"`forEach()` exécute une fonction pour chaque élément d’un tableau."},
            {"question":"En CSS, quelle propriété centre verticalement un élément dans un conteneur flex ?","answers":["align-items","justify-content","flex-direction","gap"],"correct":0,"feedback":"`align-items` centre les éléments sur l’axe vertical dans un conteneur flex."},
            {"question":"Dans ta plateforme islamique, quelle balise HTML encapsulerait une image ?","answers":["<img>","<picture>","<div>","<span>"],"correct":0,"feedback":"`<img>` est utilisé pour afficher une image."},
            {"question":"En JavaScript, quelle méthode convertit une chaîne en nombre ?","answers":["parseInt()","toString()","split()","join()"],"correct":0,"feedback":"`parseInt()` convertit une chaîne en entier."},
            {"question":"Dans 'Touche les Animaux !', quelle propriété CSS rend un animal cliquable uniquement ?","answers":["cursor: pointer","display: none","opacity: 0","z-index: 1"],"correct":0,"feedback":"`cursor: pointer` indique qu’un élément est cliquable."},
            {"question":"Dans ta plateforme de paris, quelle balise HTML structurerait un formulaire de pari ?","answers":["<form>","<div>","<section>","<article>"],"correct":0,"feedback":"`<form>` est utilisé pour créer un formulaire interactif."},
            {"question":"Dans 'Touche les Animaux !', quelle méthode JavaScript permettrait de déclencher une alerte lorsqu'un animal est cliqué ?","answers":["alert()","confirm()","prompt()","console.log()"],"correct":0,"feedback":"`alert()` affiche une boîte de dialogue avec un message."},
            {"question":"Dans ta plateforme islamique, quelle propriété CSS utiliserais-tu pour espacer les éléments d’un menu ?","answers":["padding","margin","gap","border-spacing"],"correct":2,"feedback":"`gap` définit l’espacement entre les éléments dans un conteneur flex ou grid."},
            {"question":"En JavaScript, quelle méthode retourne une nouvelle version d’un tableau avec les éléments filtrés ?","answers":["map()","filter()","reduce()","forEach()"],"correct":1,"feedback":"`filter()` crée un nouveau tableau avec les éléments qui passent un test."},
            {"question":"Dans ta plateforme de paris, quelle balise HTML utiliserais-tu pour un champ de saisie de pari ?","answers":["<input>","<textarea>","<button>","<label>"],"correct":0,"feedback":"`<input>` est utilisé pour les champs de saisie."},
            {"question":"En CSS, quelle propriété définit la police d’un texte ?","answers":["font-family","font-size","font-weight","text-transform"],"correct":0,"feedback":"`font-family` définit la police utilisée pour le texte."},
            {"question":"Dans 'Touche les Animaux !', quelle propriété CSS utiliserais-tu pour fixer un animal à un endroit précis ?","answers":["position: absolute","display: block","float: left","z-index: 10"],"correct":0,"feedback":"`position: absolute` positionne un élément par rapport à son parent positionné."},
            {"question":"En JavaScript, quelle méthode joint tous les éléments d’un tableau en une chaîne ?","answers":["join()","concat()","split()","slice()"],"correct":0,"feedback":"`join()` combine les éléments d’un tableau en une chaîne."},
            {"question":"Dans ta plateforme islamique, quelle propriété CSS rendrait un bouton plus visible avec une ombre ?","answers":["box-shadow","text-shadow","border","outline"],"correct":0,"feedback":"`box-shadow` ajoute une ombre autour d’un élément."},
            {"question":"Dans 'Touche les Animaux !', quel événement JavaScript détecte le survol d’un animal ?","answers":["onmouseover","onclick","onchange","onload"],"correct":0,"feedback":"`onmouseover` détecte le survol d’un élément."},
            {"question":"En CSS, quelle propriété définit l’espacement interne d’un élément ?","answers":["margin","padding","border","gap"],"correct":1,"feedback":"`padding` définit l’espacement interne d’un élément."},
            {"question":"Dans ta plateforme de paris, quelle méthode JavaScript permettrait de rafraîchir les cotes toutes les 5 secondes ?","answers":["setInterval()","setTimeout()","requestAnimationFrame()","addEventListener()"],"correct":0,"feedback":"`setInterval()` exécute une fonction à intervalles réguliers."},
            {"question":"Dans ta plateforme islamique, quelle balise HTML encapsulerait un groupe de boutons radio ?","answers":["<form>","<div>","<fieldset>","<section>"],"correct":2,"feedback":"`<fieldset>` regroupe des contrôles de formulaire comme des boutons radio."},
            {"question":"En JavaScript, quelle méthode retourne le premier élément d’un tableau ?","answers":["shift()","pop()","find()","slice()"],"correct":2,"feedback":"`find()` retourne le premier élément qui satisfait une condition."},
            {"question":"Dans 'Touche les Animaux !', quelle propriété CSS rendrait un animal invisible temporairement ?","answers":["visibility: hidden","display: none","opacity: 0","z-index: -1"],"correct":0,"feedback":"`visibility: hidden` cache un élément sans supprimer son espace."},
            {"question":"Dans ta plateforme de paris, quelle méthode JavaScript permettrait de vider un tableau de matchs ?","answers":["splice(0, arr.length)","pop()","shift()","push()"],"correct":0,"feedback":"`splice(0, arr.length)` supprime tous les éléments d’un tableau."},
            {"question":"Dans 'Touche les Animaux !', quelle méthode JavaScript récupère la valeur d’un champ de saisie ?","answers":["value","innerText","textContent","innerHTML"],"correct":0,"feedback":"`value` récupère la valeur d’un champ de saisie."},
            {"question":"Dans ta plateforme islamique, quelle propriété CSS appliquerais-tu pour un effet de survol sur un lien ?","answers":["hover","transition","cursor","filter"],"correct":1,"feedback":"`transition` permet des effets fluides au survol avec `:hover`."},
            {"question":"En JavaScript, quelle méthode arrête un intervalle défini par `setInterval()` ?","answers":["clearInterval()","clearTimeout()","stop()","removeEventListener()"],"correct":0,"feedback":"`clearInterval()` arrête un intervalle."},
            {"question":"Dans ta plateforme de paris, quelle balise HTML utiliserais-tu pour un texte d’étiquette de formulaire ?","answers":["<label>","<span>","<div>","<p>"],"correct":0,"feedback":"`<label>` associe un texte à un champ de formulaire."},
            {"question":"En CSS, quelle propriété définit la taille de la bordure d’un élément ?","answers":["border-width","border-style","border-color","border-radius"],"correct":0,"feedback":"`border-width` définit l’épaisseur de la bordure."},
            {"question":"Dans 'Touche les Animaux !', quelle méthode JavaScript ajoute un écouteur d’événement ?","answers":["addEventListener()","setAttribute()","getAttribute()","removeEventListener()"],"correct":0,"feedback":"`addEventListener()` attache un événement à un élément."},
            {"question":"Dans ta plateforme islamique, quelle propriété CSS alignerait un texte au centre ?","answers":["text-align: center","float: center","margin: auto","display: inline"],"correct":0,"feedback":"`text-align: center` centre le texte."},
            {"question":"En JavaScript, quelle méthode divise une chaîne en un tableau ?","answers":["split()","join()","slice()","concat()"],"correct":0,"feedback":"`split()` divise une chaîne en un tableau selon un séparateur."},
            {"question":"Dans ta plateforme de paris, quelle propriété CSS appliquerais-tu pour un fond dégradé sur un bouton ?","answers":["background: linear-gradient","background-color","box-shadow","filter"],"correct":0,"feedback":"`background: linear-gradient` crée un fond dégradé."},
            {"question":"Dans 'Touche les Animaux !', quelle méthode JavaScript vérifie si un animal est dans un tableau ?","answers":["includes()","indexOf()","findIndex()","some()"],"correct":0,"feedback":"`includes()` vérifie si un élément est dans un tableau."},
            // 25 nouvelles questions pour le mode Quiz
            {"question":"Dans 'Touche les Animaux !', quelle propriété CSS appliquerais-tu pour arrondir les coins d’un élément animal ?","answers":["border-radius","border","box-shadow","outline"],"correct":0,"feedback":"`border-radius` arrondit les coins d’un élément."},
            {"question":"Dans ta plateforme islamique, quelle balise HTML utiliserais-tu pour inclure une vidéo ?","answers":["<video>","<iframe>","<embed>","<object>"],"correct":0,"feedback":"`<video>` est utilisé pour inclure une vidéo."},
            {"question":"En JavaScript, quelle méthode retourne l’index d’un élément dans un tableau ?","answers":["indexOf()","find()","includes()","map()"],"correct":0,"feedback":"`indexOf()` retourne l’index d’un élément ou -1 s’il n’existe pas."},
            {"question":"Dans ta plateforme de paris, quelle propriété CSS rendrait un coupon cliquable visuellement ?","answers":["cursor: pointer","opacity: 0","display: none","z-index: -1"],"correct":0,"feedback":"`cursor: pointer` indique qu’un élément est cliquable."},
            {"question":"Dans 'Touche les Animaux !', quelle méthode JavaScript supprime un écouteur d’événement ?","answers":["removeEventListener()","addEventListener()","setAttribute()","clearInterval()"],"correct":0,"feedback":"`removeEventListener()` supprime un écouteur d’événement."},
            {"question":"En CSS, quelle propriété définit la hauteur d’un élément ?","answers":["height","width","max-height","min-height"],"correct":0,"feedback":"`height` définit la hauteur d’un élément."},
            {"question":"Dans ta plateforme islamique, quelle balise HTML utiliserais-tu pour une liste ordonnée de sections ?","answers":["<ol>","<ul>","<div>","<nav>"],"correct":0,"feedback":"`<ol>` crée une liste ordonnée."},
            {"question":"En JavaScript, quelle méthode exécute une fonction après un délai ?","answers":["setTimeout()","setInterval()","requestAnimationFrame()","clearTimeout()"],"correct":0,"feedback":"`setTimeout()` exécute une fonction après un délai spécifié."},
            {"question":"Dans ta plateforme de paris, quelle propriété CSS appliquerais-tu pour fixer un bandeau en haut ?","answers":["position: fixed","position: absolute","position: relative","display: block"],"correct":0,"feedback":"`position: fixed` fixe un élément par rapport à la fenêtre."},
            {"question":"Dans 'Touche les Animaux !', quelle propriété CSS définirait un effet de fondu pour un animal ?","answers":["animation","transition","opacity","filter"],"correct":0,"feedback":"`animation` définit un effet de fondu avec des keyframes."},
            {"question":"En JavaScript, quelle méthode retourne un nouveau tableau avec les éléments transformés ?","answers":["map()","filter()","reduce()","forEach()"],"correct":0,"feedback":"`map()` crée un nouveau tableau avec les résultats d’une fonction."},
            {"question":"Dans ta plateforme islamique, quelle propriété CSS rendrait un texte en italique ?","answers":["font-style","font-weight","text-decoration","text-transform"],"correct":0,"feedback":"`font-style: italic` rend le texte en italique."},
            {"question":"Dans ta plateforme de paris, quelle balise HTML utiliserais-tu pour un bouton de soumission ?","answers":["<button>","<input>","<a>","<span>"],"correct":0,"feedback":"`<button>` est utilisé pour un bouton de formulaire."},
            {"question":"En CSS, quelle propriété définit l’ordre de superposition des éléments ?","answers":["z-index","position","opacity","display"],"correct":0,"feedback":"`z-index` contrôle l’ordre de superposition des éléments."},
            {"question":"Dans 'Touche les Animaux !', quelle méthode JavaScript récupère tous les éléments avec une classe ?","answers":["querySelectorAll()","getElementById()","getElementsByClassName()","querySelector()"],"correct":0,"feedback":"`querySelectorAll()` sélectionne tous les éléments correspondant à un sélecteur."},
            {"question":"Dans ta plateforme islamique, quelle propriété CSS appliquerais-tu pour un effet de surbrillance ?","answers":["filter","box-shadow","border","outline"],"correct":0,"feedback":"`filter` peut appliquer des effets comme une surbrillance."},
            {"question":"En JavaScript, quelle méthode réduit un tableau à une seule valeur ?","answers":["reduce()","map()","filter()","forEach()"],"correct":0,"feedback":"`reduce()` réduit un tableau à une valeur unique."},
            {"question":"Dans ta plateforme de paris, quelle propriété CSS centrerait un coupon horizontalement et verticalement ?","answers":["display: flex; align-items: center; justify-content: center","margin: auto","text-align: center","position: absolute"],"correct":0,"feedback":"`display: flex` avec `align-items` et `justify-content` centre un élément."},
            {"question":"Dans 'Touche les Animaux !', quelle propriété CSS définirait une bordure en pointillés ?","answers":["border-style: dotted","border: solid","border-width: 2px","border-radius: 5px"],"correct":0,"feedback":"`border-style: dotted` crée une bordure en pointillés."},
            {"question":"En JavaScript, quelle méthode retourne la longueur d’une chaîne ?","answers":["length","size()","count()","toString()"],"correct":0,"feedback":"`length` retourne la longueur d’une chaîne."},
            {"question":"Dans ta plateforme islamique, quelle balise HTML utiliserais-tu pour un lien hypertexte ?","answers":["<a>","<link>","<span>","<div>"],"correct":0,"feedback":"`<a>` crée un lien hypertexte."},
            {"question":"Dans ta plateforme de paris, quelle propriété CSS rendrait un texte barré pour un pari annulé ?","answers":["text-decoration: line-through","font-style: italic","text-transform: uppercase","color: red"],"correct":0,"feedback":"`text-decoration: line-through` barre le texte."},
            {"question":"Dans 'Touche les Animaux !', quelle méthode JavaScript vérifierait si un score est un nombre ?","answers":["isNaN()","parseInt()","toString()","Number()"],"correct":0,"feedback":"`isNaN()` vérifie si une valeur n’est pas un nombre."},
            {"question":"En CSS, quelle propriété définit l’alignement horizontal des éléments inline ?","answers":["text-align","float","margin","padding"],"correct":0,"feedback":"`text-align` aligne les éléments inline horizontalement."},
            {"question":"Dans ta plateforme islamique, quelle propriété CSS appliquerais-tu pour un effet de transition sur un bouton ?","answers":["transition","animation","transform","filter"],"correct":0,"feedback":"`transition` permet des changements fluides sur les propriétés."}
        ];

        const codeQuestions = [
            {"question":"Quel est le résultat de ce code ?\n\nlet x = 5;\nx += 3;\nconsole.log(x);","answers":["5","3","8","undefined"],"correct":2,"feedback":"`x += 3` ajoute 3 à `x`, donc `x` vaut 8."},
            {"question":"Quel effet produit ce CSS ?\n\ndiv { transform: rotate(45deg); }","answers":["Change la couleur","Tourne l’élément de 45 degrés","Réduit la taille","Ajoute une bordure"],"correct":1,"feedback":"`transform: rotate(45deg)` fait pivoter l’élément de 45 degrés."},
            {"question":"Quel est le résultat de ce code ?\n\ndocument.getElementById('animal').style.left = '100px';","answers":["Change la couleur","Déplace l’élément à 100px du bord gauche","Cache l’élément","Agrandit l’élément"],"correct":1,"feedback":"`style.left` déplace un élément positionné à 100px du bord gauche."},
            {"question":"Quel est le résultat de ce code ?\n\nlet matches = ['Match1', 'Match2'];\nmatches.push('Match3');\nconsole.log(matches.length);","answers":["2","3","1","0"],"correct":1,"feedback":"`push()` ajoute un élément, donc la longueur du tableau est 3."},
            {"question":"Quel effet produit ce CSS ?\n\n.nav { display: flex; justify-content: space-between; }","answers":["Cache la navigation","Aligne les éléments horizontalement avec espacement","Centre le texte","Applique une bordure"],"correct":1,"feedback":"`display: flex` et `justify-content: space-between` alignent les éléments avec espacement."},
            {"question":"Quel est le résultat de ce code ?\n\nlet score = 0;\nscore++;\ndocument.getElementById('score').textContent = score;","answers":["Affiche 0","Affiche 1","Affiche undefined","Erreur"],"correct":1,"feedback":"`score++` incrémente `score` à 1, puis `textContent` l’affiche."},
            {"question":"Quel effet produit ce CSS ?\n\n.quiz { animation: fadeIn 1s ease-in; }","answers":["Change la couleur","Applique une animation de fondu","Réduit la taille","Fixe l’élément"],"correct":1,"feedback":"`animation: fadeIn` applique un effet de fondu sur 1 seconde."},
            {"question":"Quel est le résultat de ce code ?\n\ndocument.getElementById('score').innerHTML = '10';","answers":["Affiche 10","Ajoute une classe","Supprime l’élément","Change la couleur"],"correct":0,"feedback":"`innerHTML` définit le contenu HTML d’un élément à '10'."},
            {"question":"Quel effet produit ce CSS ?\n\n.animal { transition: all 0.5s; }","answers":["Cache l’élément","Applique une transition fluide","Change la taille","Ajoute une bordure"],"correct":1,"feedback":"`transition: all 0.5s` applique une transition fluide à toutes les propriétés."},
            {"question":"Quel est le résultat de ce code ?\n\nlet arr = [1, 2, 3];\narr.shift();\nconsole.log(arr);","answers":["[1, 2, 3]","[2, 3]","[1, 2]","[3]"],"correct":1,"feedback":"`shift()` supprime le premier élément, donc il reste `[2, 3]`."},
            {"question":"Quel effet produit ce CSS ?\n\n.nav-item:hover { background-color: #f0f0f0; }","answers":["Change la couleur au clic","Change la couleur au survol","Masque l’élément","Agrandit l’élément"],"correct":1,"feedback":"`:hover` applique un style au survol de l’élément."},
            {"question":"Quel est le résultat de ce code ?\n\ndocument.querySelector('.btn').addEventListener('click', () => alert('Clic !'));","answers":["Affiche une alerte au clic","Change la couleur","Supprime le bouton","Ajoute un texte"],"correct":0,"feedback":"`addEventListener('click')` déclenche une alerte au clic."},
            {"question":"Quel effet produit ce CSS ?\n\n.container { max-width: 1200px; margin: auto; }","answers":["Fixe l’élément","Centre le conteneur","Réduit la taille","Ajoute une ombre"],"correct":1,"feedback":"`margin: auto` centre un conteneur avec une largeur définie."},
            {"question":"Quel est le résultat de ce code ?\n\nlet x = '10';\nconsole.log(parseInt(x) + 5);","answers":["10","15","105","Erreur"],"correct":1,"feedback":"`parseInt('10')` donne 10, puis 10 + 5 = 15."},
            {"question":"Quel effet produit ce CSS ?\n\n.animal { position: absolute; top: 50px; }","answers":["Cache l’élément","Positionne l’élément à 50px du haut","Change la couleur","Agrandit l’élément"],"correct":1,"feedback":"`position: absolute` et `top: 50px` placent l’élément à 50px du haut."},
            {"question":"Quel est le résultat de ce code ?\n\nlet arr = ['chat', 'chien'];\narr[0] = 'lion';\nconsole.log(arr);","answers":["['chat', 'chien']","['lion', 'chien']","['chat', 'lion']","['lion']"],"correct":1,"feedback":"`arr[0] = 'lion'` remplace le premier élément par 'lion'."},
            {"question":"Quel effet produit ce CSS ?\n\n.quiz { box-shadow: 0 0 10px rgba(0,0,0,0.2); }","answers":["Ajoute une ombre","Change la couleur","Centre l’élément","Réduit la taille"],"correct":0,"feedback":"`box-shadow` ajoute une ombre autour de l’élément."},
            {"question":"Quel est le résultat de ce code ?\n\nlet num = 2;\nnum *= 3;\nconsole.log(num);","answers":["2","3","6","Erreur"],"correct":2,"feedback":"`num *= 3` multiplie `num` par 3, donc 2 * 3 = 6."},
            {"question":"Quel effet produit ce CSS ?\n\n.btn { border-radius: 5px; }","answers":["Ajoute des coins arrondis","Change la couleur","Cache le bouton","Agrandit le bouton"],"correct":0,"feedback":"`border-radius: 5px` arrondit les coins de l’élément."},
            {"question":"Quel est le résultat de ce code ?\n\ndocument.getElementById('title').classList.add('highlight');","answers":["Ajoute une classe","Supprime une classe","Change le texte","Cache l’élément"],"correct":0,"feedback":"`classList.add('highlight')` ajoute la classe `highlight` à l’élément."},
            {"question":"Quel effet produit ce CSS ?\n\n.nav { background: linear-gradient(to right, #fff, #ccc); }","answers":["Ajoute un dégradé","Change la police","Centre la navigation","Masque la navigation"],"correct":0,"feedback":"`background: linear-gradient` crée un dégradé de couleur."},
            {"question":"Quel est le résultat de ce code ?\n\nlet arr = [1, 2, 3];\nconsole.log(arr.includes(2));","answers":["true","false","2","Erreur"],"correct":0,"feedback":"`includes(2)` retourne `true` si 2 est dans le tableau."},
            {"question":"Quel est le résultat de ce code ?\n\nlet text = 'Bonjour';\nconsole.log(text.toUpperCase());","answers":["bonjour","BONJOUR","Bonjour","Erreur"],"correct":1,"feedback":"`toUpperCase()` convertit une chaîne en majuscules."},
            {"question":"Quel effet produit ce CSS ?\n\n.animal { z-index: 10; }","answers":["Change la couleur","Place l’élément au-dessus d’autres","Cache l’élément","Réduit la taille"],"correct":1,"feedback":"`z-index: 10` contrôle l’ordre de superposition des éléments."},
            {"question":"Quel est le résultat de ce code ?\n\ndocument.querySelector('.score').textContent = 'Gagné !';","answers":["Change la couleur","Affiche 'Gagné !'","Supprime l’élément","Ajoute une classe"],"correct":1,"feedback":"`textContent` définit le texte d’un élément à 'Gagné !'."},
            {"question":"Quel effet produit ce CSS ?\n\n.btn:hover { transform: scale(1.1); }","answers":["Change la couleur","Agrandit légèrement au survol","Cache le bouton","Ajoute une bordure"],"correct":1,"feedback":"`transform: scale(1.1)` agrandit l’élément de 10% au survol."},
            {"question":"Quel est le résultat de ce code ?\n\nlet arr = [1, 2, 3];\narr.splice(1, 1);\nconsole.log(arr);","answers":["[1, 2, 3]","[1, 3]","[2, 3]","[1, 2]"],"correct":1,"feedback":"`splice(1, 1)` supprime 1 élément à l’index 1, donc `[1, 3]`."},
            {"question":"Quel effet produit ce CSS ?\n\n.nav { flex-direction: column; }","answers":["Aligne les éléments verticalement","Centre le texte","Ajoute une ombre","Cache la navigation"],"correct":0,"feedback":"`flex-direction: column` aligne les éléments flex en colonne."},
            {"question":"Quel est le résultat de ce code ?\n\nlet x = 10;\nconsole.log(x > 5 ? 'Oui' : 'Non');","answers":["Oui","Non","10","Erreur"],"correct":0,"feedback":"L’opérateur ternaire retourne 'Oui' si `x > 5` est vrai."},
            {"question":"Quel effet produit ce CSS ?\n\n.quiz { opacity: 0.5; }","answers":["Change la couleur","Rend l’élément semi-transparent","Cache l’élément","Agrandit l’élément"],"correct":1,"feedback":"`opacity: 0.5` rend l’élément semi-transparent."},
            {"question":"Quel est le résultat de ce code ?\n\nlet arr = ['a', 'b', 'c'];\nconsole.log(arr.join('-'));","answers":["a-b-c","abc","[a, b, c]","Erreur"],"correct":0,"feedback":"`join('-')` combine les éléments avec un séparateur `-`."},
            {"question":"Quel effet produit ce CSS ?\n\n.container { display: grid; grid-template-columns: 1fr 1fr; }","answers":["Crée une grille à deux colonnes","Centre le conteneur","Ajoute une bordure","Cache l’élément"],"correct":0,"feedback":"`display: grid` et `grid-template-columns` créent une grille à deux colonnes."},
            {"question":"Quel est le résultat de ce code ?\n\nlet x = 3;\nconsole.log(x ** 2);","answers":["3","6","9","Erreur"],"correct":2,"feedback":"`x ** 2` calcule x à la puissance 2, donc 3 * 3 = 9."},
            {"question":"Quel effet produit ce CSS ?\n\n.btn { cursor: not-allowed; }","answers":["Change la couleur","Indique que le bouton est désactivé","Agrandit le bouton","Cache le bouton"],"correct":1,"feedback":"`cursor: not-allowed` montre que le bouton n’est pas cliquable."},
            {"question":"Quel est le résultat de ce code ?\n\ndocument.getElementById('animal').style.display = 'none';","answers":["Affiche l’élément","Cache l’élément","Change la couleur","Déplace l’élément"],"correct":1,"feedback":"`style.display = 'none'` cache l’élément."},
            {"question":"Quel effet produit ce CSS ?\n\n.nav { padding: 10px 20px; }","answers":["Ajoute un espacement interne","Centre la navigation","Ajoute une bordure","Change la police"],"correct":0,"feedback":"`padding` ajoute un espacement interne à l’élément."},
            {"question":"Quel est le résultat de ce code ?\n\nlet arr = [1, 2, 3];\nconsole.log(arr[1]);","answers":["1","2","3","Erreur"],"correct":1,"feedback":"`arr[1]` accède au deuxième élément du tableau, soit 2."},
            {"question":"Quel est le résultat de ce code ?\n\nlet x = '5';\nconsole.log(Number(x) * 2);","answers":["5","10","52","Erreur"],"correct":1,"feedback":"`Number(x)` convertit '5' en 5, puis 5 * 2 = 10."},
            {"question":"Quel effet produit ce CSS ?\n\n.animal { animation: bounce 2s infinite; }","answers":["Change la couleur","Applique une animation de rebond","Cache l’élément","Déplace l’élément"],"correct":1,"feedback":"`animation: bounce` applique une animation nommée 'bounce'."},
            {"question":"Quel est le résultat de ce code ?\n\ndocument.querySelector('#quiz').style.backgroundColor = 'blue';","answers":["Change la couleur de fond","Affiche un texte","Supprime l’élément","Ajoute une classe"],"correct":0,"feedback":"`style.backgroundColor` change la couleur de fond à bleu."},
            {"question":"Quel effet produit ce CSS ?\n\n.btn { font-weight: bold; }","answers":["Change la taille","Rend le texte gras","Ajoute une bordure","Cache le bouton"],"correct":1,"feedback":"`font-weight: bold` rend le texte gras."},
            {"question":"Quel est le résultat de ce code ?\n\nlet arr = [1, 2, 3];\nconsole.log(arr.reverse());","answers":["[1, 2, 3]","[3, 2, 1]","[2, 3, 1]","Erreur"],"correct":1,"feedback":"`reverse()` inverse l’ordre des éléments du tableau."},
            {"question":"Quel effet produit ce CSS ?\n\n.nav { position: sticky; top: 0; }","answers":["Fixe la navigation en haut","Centre la navigation","Ajoute une ombre","Change la couleur"],"correct":0,"feedback":"`position: sticky` fixe la navigation en haut lors du défilement."},
            {"question":"Quel est le résultat de ce code ?\n\nlet x = 4;\nconsole.log(x % 2);","answers":["0","1","2","Erreur"],"correct":0,"feedback":"`x % 2` retourne le reste de la division, soit 0."},
            {"question":"Quel effet produit ce CSS ?\n\n.quiz { border: 1px solid black; }","answers":["Ajoute une bordure","Change la couleur de fond","Centre l’élément","Cache l’élément"],"correct":0,"feedback":"`border` ajoute une bordure autour de l’élément."},
            {"question":"Quel est le résultat de ce code ?\n\ndocument.querySelector('.animal').classList.toggle('active');","answers":["Ajoute ou supprime une classe","Change le texte","Cache l’élément","Déplace l’élément"],"correct":0,"feedback":"`classList.toggle('active')` ajoute ou supprime la classe 'active'."},
            {"question":"Quel effet produit ce CSS ?\n\n.container { width: 100%; }","answers":["Fixe la largeur à 100%","Centre le conteneur","Ajoute une ombre","Cache l’élément"],"correct":0,"feedback":"`width: 100%` définit la largeur à 100% du parent."},
            {"question":"Quel est le résultat de ce code ?\n\nlet arr = ['a', 'b'];\nconsole.log(arr.concat(['c']));","answers":["['a', 'b']","['a', 'b', 'c']","['c', 'a', 'b']","Erreur"],"correct":1,"feedback":"`concat(['c'])` ajoute 'c' au tableau, donnant ['a', 'b', 'c']."},
            {"question":"Quel effet produit ce CSS ?\n\n.btn { text-transform: uppercase; }","answers":["Change la couleur","Met le texte en majuscules","Centre le texte","Ajoute une bordure"],"correct":1,"feedback":"`text-transform: uppercase` met le texte en majuscules."},
            {"question":"Quel est le résultat de ce code ?\n\nlet x = true;\nconsole.log(!x);","answers":["true","false","1","Erreur"],"correct":1,"feedback":"`!x` inverse la valeur booléenne, donc `true` devient `false`."},
            {"question":"Quel effet produit ce CSS ?\n\n.nav { display: none; }","answers":["Affiche la navigation","Cache la navigation","Centre la navigation","Ajoute une bordure"],"correct":1,"feedback":"`display: none` cache l’élément."},
            {"question":"Quel est le résultat de ce code ?\n\nlet arr = [1, 2, 3];\nconsole.log(arr.slice(1, 2));","answers":["[1]","[2]","[1, 2]","Erreur"],"correct":1,"feedback":"`slice(1, 2)` extrait l’élément à l’index 1, soit `[2]`."},
            // 25 nouvelles questions pour le mode Associer le Code
            {"question":"Quel est le résultat de ce code ?\n\nlet x = 8;\nconsole.log(x / 2);","answers":["2","4","8","Erreur"],"correct":1,"feedback":"`x / 2` divise 8 par 2, donc 4."},
            {"question":"Quel effet produit ce CSS ?\n\n.animal { filter: grayscale(100%); }","answers":["Change la couleur","Rend l’élément en niveaux de gris","Cache l’élément","Agrandit l’élément"],"correct":1,"feedback":"`filter: grayscale(100%)` rend l’élément en niveaux de gris."},
            {"question":"Quel est le résultat de ce code ?\n\ndocument.getElementById('coupon').style.color = 'green';","answers":["Change la couleur du texte","Affiche un texte","Supprime l’élément","Ajoute une classe"],"correct":0,"feedback":"`style.color` change la couleur du texte à vert."},
            {"question":"Quel effet produit ce CSS ?\n\n.nav { justify-content: center; }","answers":["Aligne les éléments au centre","Cache la navigation","Ajoute une bordure","Change la police"],"correct":0,"feedback":"`justify-content: center` centre les éléments dans un conteneur flex."},
            {"question":"Quel est le résultat de ce code ?\n\nlet arr = [1, 2, 3];\nconsole.log(arr.length);","answers":["1","2","3","Erreur"],"correct":2,"feedback":"`length` retourne la longueur du tableau, soit 3."},
            {"question":"Quel effet produit ce CSS ?\n\n.btn { outline: 2px solid blue; }","answers":["Ajoute une bordure extérieure","Change la couleur de fond","Cache le bouton","Agrandit le bouton"],"correct":0,"feedback":"`outline` ajoute une bordure extérieure autour de l’élément."},
            {"question":"Quel est le résultat de ce code ?\n\nlet x = 'Hello';\nconsole.log(x.substring(0, 2));","answers":["He","Hello","lo","Erreur"],"correct":0,"feedback":"`substring(0, 2)` extrait les caractères de l’index 0 à 1, soit 'He'."},
            {"question":"Quel effet produit ce CSS ?\n\n.quiz { transform: translateX(20px); }","answers":["Déplace l’élément horizontalement","Change la couleur","Cache l’élément","Réduit la taille"],"correct":0,"feedback":"`transform: translateX(20px)` déplace l’élément de 20px à droite."},
            {"question":"Quel est le résultat de ce code ?\n\nlet arr = [1, 2];\narr.unshift(0);\nconsole.log(arr);","answers":["[1, 2]","[0, 1, 2]","[2, 1, 0]","Erreur"],"correct":1,"feedback":"`unshift(0)` ajoute 0 au début du tableau."},
            {"question":"Quel effet produit ce CSS ?\n\n.animal { animation-duration: 1.5s; }","answers":["Change la couleur","Définit la durée d’une animation","Cache l’élément","Agrandit l’élément"],"correct":1,"feedback":"`animation-duration: 1.5s` définit la durée d’une animation à 1,5 seconde."},
            {"question":"Quel est le résultat de ce code ?\n\ndocument.getElementById('nav').style.display = 'block';","answers":["Affiche l’élément","Cache l’élément","Change la couleur","Déplace l’élément"],"correct":0,"feedback":"`style.display = 'block'` affiche l’élément."},
            {"question":"Quel effet produit ce CSS ?\n\n.btn { background-color: red; }","answers":["Change la couleur de fond","Ajoute une bordure","Centre le bouton","Cache le bouton"],"correct":0,"feedback":"`background-color: red` change la couleur de fond à rouge."},
            {"question":"Quel est le résultat de ce code ?\n\nlet x = 5;\nconsole.log(x === '5');","answers":["true","false","5","Erreur"],"correct":1,"feedback":"`===` vérifie l’égalité stricte, donc 5 (nombre) n’est pas égal à '5' (chaîne)."},
            {"question":"Quel effet produit ce CSS ?\n\n.nav { flex-wrap: wrap; }","answers":["Permet le retour à la ligne des éléments flex","Centre la navigation","Ajoute une ombre","Cache la navigation"],"correct":0,"feedback":"`flex-wrap: wrap` permet aux éléments flex de passer à la ligne."},
            {"question":"Quel est le résultat de ce code ?\n\nlet arr = ['a', 'b', 'c'];\nconsole.log(arr.pop());","answers":["a","b","c","Erreur"],"correct":2,"feedback":"`pop()` supprime et retourne le dernier élément, soit 'c'."},
            {"question":"Quel effet produit ce CSS ?\n\n.quiz { margin-top: 20px; }","answers":["Ajoute un espacement en haut","Centre l’élément","Ajoute une bordure","Change la couleur"],"correct":0,"feedback":"`margin-top: 20px` ajoute un espacement de 20px en haut."},
            {"question":"Quel est le résultat de ce code ?\n\nlet x = 10;\nconsole.log(x.toString());","answers":["10","'10'","true","Erreur"],"correct":1,"feedback":"`toString()` convertit le nombre 10 en chaîne '10'."},
            {"question":"Quel effet produit ce CSS ?\n\n.animal { transform: rotateY(180deg); }","answers":["Change la couleur","Tourne l’élément sur l’axe Y","Cache l’élément","Agrandit l’élément"],"correct":1,"feedback":"`transform: rotateY(180deg)` fait pivoter l’élément de 180 degrés sur l’axe Y."},
            {"question":"Quel est le résultat de ce code ?\n\ndocument.querySelector('.btn').style.padding = '10px';","answers":["Ajoute un espacement interne","Change la couleur","Supprime le bouton","Affiche un texte"],"correct":0,"feedback":"`style.padding = '10px'` ajoute un espacement interne de 10px."},
            {"question":"Quel effet produit ce CSS ?\n\n.nav { align-items: center; }","answers":["Centre les éléments verticalement","Change la police","Ajoute une bordure","Cache la navigation"],"correct":0,"feedback":"`align-items: center` centre les éléments flex verticalement."},
            {"question":"Quel est le résultat de ce code ?\n\nlet arr = [1, 2, 3];\nconsole.log(arr.map(x => x * 2));","answers":["[1, 2, 3]","[2, 4, 6]","[1, 4, 9]","Erreur"],"correct":1,"feedback":"`map(x => x * 2)` multiplie chaque élément par 2, donnant `[2, 4, 6]`."},
            {"question":"Quel effet produit ce CSS ?\n\n.btn { text-align: center; }","answers":["Centre le texte","Change la couleur","Ajoute une bordure","Cache le bouton"],"correct":0,"feedback":"`text-align: center` centre le texte à l’intérieur du bouton."},
            {"question":"Quel est le résultat de ce code ?\n\nlet x = 'abc';\nconsole.log(x.length);","answers":["2","3","4","Erreur"],"correct":1,"feedback":"`length` retourne la longueur de la chaîne, soit 3."},
            {"question":"Quel effet produit ce CSS ?\n\n.quiz { animation-delay: 0.5s; }","answers":["Change la couleur","Retarde l’animation de 0,5s","Cache l’élément","Agrandit l’élément"],"correct":1,"feedback":"`animation-delay: 0.5s` retarde le début de l’animation."},
            {"question":"Quel est le résultat de ce code ?\n\nlet arr = [1, 2, 3];\nconsole.log(arr.filter(x => x > 1));","answers":["[1]","[2, 3]","[1, 2, 3]","Erreur"],"correct":1,"feedback":"`filter(x => x > 1)` retourne les éléments supérieurs à 1, soit `[2, 3]`."}
        ];

        const questionEl = document.getElementById('question');
        const answersEl = document.getElementById('answers');
        const codeQuestionEl = document.getElementById('codeQuestion');
        const codeAnswersEl = document.getElementById('codeAnswers');
        const startButton = document.getElementById('startButton');
        const switchModeButton = document.getElementById('switchModeButton');
        const resultEl = document.getElementById('result');
        const timerEl = document.getElementById('timer');
        const highScoreEl = document.getElementById('highScore');
        const feedbackEl = document.getElementById('feedback');
        let currentQuestion = 0;
        let score = 0;
        let timeLeft = 10;
        let isGameRunning = false;
        let isQuizMode = true;
        let timer;
        let highScore = localStorage.getItem('highScore') || 0;
        highScoreEl.textContent = `Meilleur score : ${highScore}`;

        startButton.addEventListener('click', startGame);
        switchModeButton.addEventListener('click', switchMode);

        function switchMode() {
            isQuizMode = !isQuizMode;
            switchModeButton.textContent = isQuizMode ? "Passer au mode Associer le Code" : "Passer au mode Quiz";
            questionEl.style.display = isQuizMode ? "block" : "none";
            answersEl.style.display = isQuizMode ? "block" : "none";
            codeQuestionEl.style.display = isQuizMode ? "none" : "block";
            codeAnswersEl.style.display = isQuizMode ? "none" : "block";
            endGame();
        }

        function startGame() {
            if (!isGameRunning) {
                currentQuestion = 0;
                score = 0;
                isGameRunning = true;
                startButton.disabled = true;
                resultEl.textContent = `Score : ${score}`;
                feedbackEl.textContent = "";
                nextQuestion();
            }
        }

        function nextQuestion() {
            const questions = isQuizMode ? quizQuestions : codeQuestions;
            if (currentQuestion >= questions.length) {
                endGame();
                return;
            }

            timeLeft = 10;
            timerEl.textContent = `Temps : ${timeLeft}s`;
            feedbackEl.textContent = "";
            if (isQuizMode) {
                questionEl.textContent = questions[currentQuestion].question;
                answersEl.innerHTML = '';
                questions[currentQuestion].answers.forEach((answer, index) => {
                    const button = document.createElement('button');
                    button.className = 'answer';
                    button.textContent = answer;
                    button.addEventListener('click', () => checkAnswer(index));
                    answersEl.appendChild(button);
                });
            } else {
                codeQuestionEl.textContent = questions[currentQuestion].question;
                codeAnswersEl.innerHTML = '';
                questions[currentQuestion].answers.forEach((answer, index) => {
                    const button = document.createElement('button');
                    button.className = 'code-answer';
                    button.textContent = answer;
                    button.addEventListener('click', () => checkAnswer(index));
                    codeAnswersEl.appendChild(button);
                });
            }

            timer = setInterval(() => {
                timeLeft--;
                timerEl.textContent = `Temps : ${timeLeft}s`;
                if (timeLeft <= 0) {
                    clearInterval(timer);
                    feedbackEl.textContent = questions[currentQuestion].feedback;
                    currentQuestion++;
                    setTimeout(nextQuestion, 1000);
                }
            }, 1000);
        }

        function checkAnswer(index) {
            clearInterval(timer);
            const questions = isQuizMode ? quizQuestions : codeQuestions;
            const buttons = isQuizMode ? answersEl.querySelectorAll('.answer') : codeAnswersEl.querySelectorAll('.code-answer');
            buttons.forEach(button => button.disabled = true);
            if (index === questions[currentQuestion].correct) {
                score += 10;
                resultEl.textContent = `Score : ${score}`;
                buttons[index].style.backgroundColor = '#2ecc71';
            } else {
                buttons[index].style.backgroundColor = '#e74c3c';
                buttons[questions[currentQuestion].correct].style.backgroundColor = '#2ecc71';
                feedbackEl.textContent = questions[currentQuestion].feedback;
            }
            setTimeout(() => {
                currentQuestion++;
                nextQuestion();
            }, 1000);
        }

        function endGame() {
            isGameRunning = false;
            clearInterval(timer);
            questionEl.textContent = '';
            codeQuestionEl.textContent = '';
            answersEl.innerHTML = '';
            codeAnswersEl.innerHTML = '';
            startButton.disabled = false;
            startButton.textContent = 'Rejouer';
            resultEl.textContent = `Score final : ${score}/${(isQuizMode ? quizQuestions.length : codeQuestions.length) * 10} !`;
            timerEl.textContent = '';
            feedbackEl.textContent = '';
            if (score > highScore) {
                highScore = score;
                localStorage.setItem('highScore', highScore);
                highScoreEl.textContent = `Meilleur score : ${highScore}`;
            }
        }
    </script>
</body>
</html>
