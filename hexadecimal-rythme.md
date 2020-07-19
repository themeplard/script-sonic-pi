## Schéma rythmique hexadécimal/binaire

L'idée est d'avoir plusieurs instruments dans une `live_loop` avec une écriture rapide des rythmes en hexadécimal qui seront retranscrits en binaire.
Je vais vous expliquer tout le cheminement de mon idée en gardant le même exemple de rythme.

### Spread
A la base, c'est inspiré de l'exemple 3 de la commande `spread` dans la documentation de sonic-pi:

```
live_loop :euclid_beat do
    sample :elec_bong, amp: 1.5 if (spread 3, 8).tick
    sample :perc_snap, amp: 0.8 if (spread 7, 11).look
    sample :bd_haus, amp: 2 if (spread 1, 4).look
    sleep 0.125
  end
```
Il y a bien plusieurs instruments dans une `live_loop` avec une écriture rapide des rythmes mais je trouvais `spread` pas simple à utiliser.

### Bools
Ensuite, on m'a parlé de la commande `bools`:

```
live_loop :beat do
  sample :elec_bong, amp: 1.5 if (bools 1,0,0,1,0,0,1,0).tick
  sample :perc_snap, amp: 0.8 if (bools 1,0,1,0,1,1,0,1,1,0,1).look
  sample :bd_haus, amp: 2 if (bools 1,0,0,0).look
  sleep 0.125
end
``` 
Il y a bien plusieurs instruments dans une `live_loop` avec une écriture simple des rythmes mais je trouvais `bools` pas assez rapide à utiliser.

### Fonction Schéma rythmique binaire
Avant de faire la fonction hexadécimal, il me fallait une fonction Schéma rythmique binaire:

```
# Binary rhythmic scheme
def schr(num)
  num.split(//).collect  { |n| n == "1" }
end

live_loop :beat do
  sample :elec_bong, amp: 1.5 if schr('10010010').ring.tick
  sample :perc_snap, amp: 0.8 if schr('10101101101').ring.look
  sample :bd_haus, amp: 2 if schr('1000').ring.look
  sleep 0.125
end

```
Il y a bien plusieurs instruments dans une `live_loop` avec une écriture simple des rythmes mais ce n'est pas beaucoup plus rapide que `bools` à utiliser.

 ### Fonction Schéma rythmique hexadécimal/binaire
Voila pourquoi j'en suis arrivé à une fonction Schéma rythmique hexadécimal/binaire.
J'ai fait le choix de garder 4 bits pour 1 hexadécimal pour obtenir les 16 schémas suivants:

0=0000, 1=0001, 2=0010, 3=0011 
4=0100, 5=0101, 6=0110, 7=0111
8=1000, 9=1001, a=1010, b=1011
c=1100, d=1101, e=1110, f=1111

```
# hexadecimal/binary rhythmic scheme
def schr2(num)
  num.hex.to_s(2).rjust(num.size*4, '0').split(//).collect  { |n| n == "1" }
end
# 0=0000, 1=0001, 2=0010, 3=0011
# 4=0100, 5=0101, 6=0110, 7=0111
# 8=1000, 9=1001, a=1010, b=1011
# c=1100, d=1101, e=1110, f=1111
# Binary rhythmic scheme
def schr(num)
  num.split(//).collect  { |n| n == "1" }
 end
 
live_loop :beat do
    sample :elec_bong, amp: 1.5 if schr2('92').ring.tick
    sample :perc_snap, amp: 0.8 if schr('10101101101').ring.look
    sample :bd_haus, amp: 2 if schr2('8').ring.look
    sleep 0.125
  end
```
Plus efficace mais moins flexible puisque j'ai fait le choix de garder 4 bits pour 1 hexadécimal.
Dans l'exemple ci-dessus, je n'ai pas pu refaire le `(spread 7, 11)` de 11bits en hexadécimal puisque 11 n'est pas un multiple de 4.

Vous trouverez un exemple concret d'utilisation de cette fonction dans le script [Rhum Virtuel](https://github.com/themeplard/script-sonic-pi/blob/master/Rhum-Virtuel.md).
