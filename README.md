Code d'affichage de la carte « Pour Clément » (CSS et JavaScript, aucune donnée). Chargé par jsDelivr dans les cartes de fin de tour.

## Limite connue : pas de bouton micro dans la carte (constat du 24/11/2026)

Demande étudiée : un bouton de dictée vocale (Web Speech API, `webkitSpeechRecognition`, `lang fr-FR`) à côté des champs texte de la carte. Ce n'est pas faisable tant que la carte est rendue par `show_widget` dans l'app Claude desktop (vérifié sur la version 6.28899.6, macOS) :

1. Le widget est un cadre sans origine (`about:blank`, sous-cadre) dans la fenêtre de l'app. L'app journalise ses demandes de permission comme « origin-less frame » (`~/Library/Logs/Claude/main.log`, déjà vu pour le presse-papiers du bouton Copier).
2. Un widget de test qui appelait `getUserMedia({audio:true})` et `webkitSpeechRecognition.start()` n'a laissé aucune demande de micro dans ce journal : la demande est arrêtée dans le cadre lui-même, avant d'atteindre l'app (aucune autorisation `microphone` sur ce cadre, probable).
3. Même avec le micro, la reconnaissance vocale de Chromium a besoin des serveurs de Google, que les applications Electron comme Claude desktop n'ont pas : elle répond en général par une erreur `network` (probable).
4. Seuls les Artifacts publiés ont une capacité micro et dictée déclarable, et ce n'est pas le support de la carte.

Ne rien forcer dans `card.js` : un bouton qui ne marche pas serait pire qu'aucun bouton. À revoir si l'app donne un jour l'accès au micro aux widgets.

## Solution de secours : la Dictée de macOS

Elle marche dans tout champ texte, carte comprise. Réglages Système, Clavier, Dictée : l'activer, langue Français (France), raccourci « Appuyer deux fois sur la touche 🌐 ». Clique dans le champ, appuie deux fois sur 🌐, parle, puis appuie de nouveau sur 🌐 pour arrêter. Le texte s'ajoute là où est le curseur.
