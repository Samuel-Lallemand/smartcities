EXERCICE 2 : VOLUME D'UNE MÉLODIE (ADC & PWM)

Pour ce travail en MicroPython, j'utilise un Raspberry Pi Pico pour jouer une mélodie en boucle sur un module Buzzer (thème de Megalovania) tout en contrôlant son volume en temps réel grâce à un module potentiomètre.

BRANCHEMENTS NÉCESSAIRES :

rappelle des broches sur Pico (Sans Shield)

<img width="416" height="256" alt="image" src="https://github.com/user-attachments/assets/215034e2-52dd-4df5-ac50-39a58703059b" />


Le module potentiomètre est branché sur la broche analogique GP26 (ADC0) et le module Buzzer est branché sur la broche PWM GP27.

<img width="243" height="231" alt="image" src="https://github.com/user-attachments/assets/8ba75a43-e7ff-4ab1-9c1a-f10eabad669b" />

Voici le module potentiomètre utiliser :

<img width="412" height="111" alt="image" src="https://github.com/user-attachments/assets/251f881d-1bba-4d6b-8f82-7d0d3b447b89" />

Voici le module buzzer utiliser (passif) :

<img width="423" height="85" alt="image" src="https://github.com/user-attachments/assets/367bc15e-d275-463a-9a1f-e9c6e1a62d16" />



OBJECTIFS DU PROGRAMME :

1.Une mélodie (jouée sur un buzzer) est jouée en boucle indéfiniment.

2.Le fait de tourner le potentiomètre modifie directement le volume de la mélodie, y compris pendant qu'une note est en train de jouer.



ANALYSE DU PROGRAMME :

Librairies utilisées :
Import Pin, PWM, ADC depuis la bibliothèque machine et sleep depuis time.


Les entrées / sorties :
On initialise la broche analogique 26 (pot = ADC(Pin(26))) pour lire la position du potentiomètre.
On initialise la broche 27 en PWM (buzzer = PWM(Pin(27))) pour pouvoir générer les fréquences sonores et ajuster le volume via le rapport cyclique (duty cycle).

La fonction jouer_note(frequence, duree) :
Au lieu d'attendre passivement la fin d'une note avec un simple sleep(), cette fonction découpe la durée de la note en petits intervalles de 0.01 seconde (10 ms).
À chaque boucle de 10 ms :

On lit la valeur analogique du potentiomètre avec pot.read_u16() (valeur entre 0 et 65535).

On applique directement cette valeur au buzzer avec buzzer.duty_u16(volume).
Cela permet de changer le volume de manière ultra-fluide et instantanée pendant la note. À la fin de la note, le volume est remis à 0 pour couper le son (buzzer.duty_u16(0)).

Les notes de musique :
Chaque note correspond à sa fréquence exacte en Hertz (par exemple D4 = 294, A4 = 440, D5 = 587).

La boucle principale while True :
On enchaîne les appels à la fonction jouer_note() pour jouer la partition musicale note par note. Une fois la séquence terminée, la boucle recommence depuis le début.


