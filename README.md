GPIO EX1 BOUTON ET LED :

Pour se travail en mycropython j'utilise un PICO PI pour contrôler le clignotement d'un module LED grâce à un module bouton.

BRANCHEMENT NECESSAIRES :
Ici j'utilise un PICO PI possèdant un shield (représentation image ci-dessous).
<img width="258" height="258" alt="image" src="https://github.com/user-attachments/assets/dd28b4e7-6121-44a0-bd8a-4ff0c186486c" />

Le module Bouton est brancher sur la broche GP16 et le module LED est brancher sur la broche GP18 (représentation dans l'image ci-dessous).

<img width="413" height="401" alt="image" src="https://github.com/user-attachments/assets/8da42ac3-a866-4b2e-adff-19457d622442" />
<img width="832" height="562" alt="image" src="https://github.com/user-attachments/assets/66f3165d-3550-4c88-acef-d0518306d7ea" />

OBJECTIFS DU PROGRAMME :
1. Quand on appuie une fois sur le module bouton, le module LED s'allume 5 secondes fixes puis commence à clignoter à une vitesse de 0.5Hz (1s allumée, 1s éteinte).
2. Quand on appuie une deuxième fois sur le bouton, la LED s'allume denouveau 5 secondes fixes  puis commence à clignoter plus rapidement à une vitesse de 2Hz (250ms allumée, 250ms éteinte).
3. Quand on appuie une troisième fois sur le bouton, la LED s'allume encore pour 5 secondes fixes puis s'arrête de clignoter.
4. Si on réapuie alors sur le bouton, le cycle recommence.

COMMENT EST CONSTRUIT LE CODE :

Librairies utilisée :
Import machine et import utime.

Les entrées / sorties :
On commence par importer les bibliothèques machine et utime.
On initialise la broche 16 pour le bouton en entrée avec une résistance "PULL_DOWN" pour éviter les faux signaux électriques, et la broche 18 en sortie pour piloter la LED.
<img width="636" height="46" alt="image" src="https://github.com/user-attachments/assets/359362af-d64a-4cf7-8ae3-0a723d343d48" />

La fonction d'interruption :
Au lieu de surveiller le bouton en boucle, on utilise une interruption avec "BOUTON.irq(...)".
<img width="725" height="30" alt="image" src="https://github.com/user-attachments/assets/f0bef349-44c1-428d-804a-82ebf25e241d" />

Dès qu'on appuie sur le bouton :
On utilise "ticks_diff" pour filtrer les faux rebonds mécaniques (délai de sécurité de 200 ms).
On change la variable "etat" pour passer à l'étape suivante (0, 1 ou 2).
On active la variable "en_transition = True" et on allume immédiatement la LED pour lancer le délai de 5 secondes. Pendant ces 5 secondes, les nouveaux appuis sont bloqués pour éviter les bugs.
<img width="855" height="165" alt="image" src="https://github.com/user-attachments/assets/ffecba2b-5ba4-4558-82f0-2dc4ae95f145" />

Dans la boucle principale while True, on n'utilise aucun "utime.sleep()" long pour ne jamais geler le microcontrôleur.
<img width="210" height="45" alt="image" src="https://github.com/user-attachments/assets/8f98360d-129a-4f7b-8a14-73624663df42" />

Si on est en transition "(en_transition == True)", la LED reste allumée. Dès que les 5000 ms sont passées, la transition s'arrête et on bascule dans le mode demandé.
Si on est en mode normal, on regarde la variable etat :
1. etat == 0 : LED éteinte.
2. etat == 1 : On inverse l'état de la LED toutes les 1000 ms avec ticks_diff pour avoir un clignotement lent (0.5 Hz).
3. etat == 2 : On inverse l'état de la LED toutes les 250 ms pour le clignotement rapide (2Hz).

<img width="772" height="373" alt="image" src="https://github.com/user-attachments/assets/f2df860e-ed54-44a4-97d3-377906c3a47c" />


