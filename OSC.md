# OSC

Une façon d’obtenir de l’information entrante et sortante dans Sonic Pi est via un réseau en utilisant un protocole simple appelé OSC (Open Sound Control). 
Cela va vous permet d'envoyer et recevoir des messages de programmes ou périphériques externes.
Par défaut, lorsque Sonic Pi est lancé, il écoute sur le port 4560 pour des messages OSC entrant de programmes sur le même ordinateur. 

Pour des raisons de sécurité, par défaut Sonic Pi ne laisse pas les machines distantes lui envoyer des messages OSC. 
Cependant, vous pouvez activer le support des machines distantes dans Préférences->Entrée/Sortie->Recevoir des messages OSC distants. 
Une fois que vous l’avez activé, vous pouvez recevoir les messages OSC de n’importe quel ordinateur sur votre réseau.



## App Android

Renseigner simplement l'adresse IP et le port de l'hôte sonic-pi dans les paramètres des applications Android.

Vous trouverez ces informations dans les préférences (Entrée/Sortie) de sonic-pi.

### OscHook

![](https://lh3.ggpht.com/GEO66_LCl6wlrD5nvKbVlCfgHq151v80bbUx5re-DX9KocPFvbSfrRZQcwgfkXXRsA=s180-rw)

[OscHook](https://play.google.com/store/apps/details?id=com.hollyhook.oscHook&hl=fr)

Controler Sonic-pi avec les capteurs d'un smartphone Android.

Dans l'exemple suivant, on utilise le capteur de proximité pour déclencher le son d'une vague genre océan.
Dans le script sonic-pi, remplacer `[VOTRE_IP:PORT]` par L'IP et le port OSC de votre smartphone.
Ces informations devraient s'afficher dans la fenêtre Marqueurs de sonic-pi.


```
with_fx :reverb, mix: 0.5 do
  live_loop :foo do
    use_real_time
    a = sync "/osc:[VOTRE_IP:PORT]/proximity"
    b = a[0]
    if b<4
      s = synth [:bnoise, :cnoise, :gnoise].choose, amp: rrand(0.5, 1.5), attack: rrand(0, 4), sustain: rrand(0, 2), release: rrand(1, 5), cutoff_slide: rrand(0, 5), cutoff: rrand(60, 100), pan: rrand(-1, 1), pan_slide: rrand(1, 5), amp: rrand(0.5, 1)
      control s, pan: rrand(-1, 1), cutoff: rrand(60, 110)
    end
  end
end

```

### OSC Controller

![](https://lh3.googleusercontent.com/DK9taBmBlRmJV7u2F-tQnhaVYGOQ2TyFoyna_b-BkIbU6MVuOxFy0P-ksMASYiEt_7Y=s180-rw) 

[OSC Controller](https://play.google.com/store/apps/details?id=com.ffsmultimedia.osccontroller&hl=fr)

Jouer un sample sur sonic-pi par un bouton OSC Controller et contrôle l'amplitude avec un slider OSC Controller.
Dans le script sonic-pi, remplacer `[VOTRE_IP:PORT]` par L'IP et le port OSC de votre smartphone.
Ces informations devraient s'afficher dans la fenêtre Marqueurs de sonic-pi.


````
    live_loop :foo do
      use_real_time
          a = sync "/osc:[VOTRE_IP:PORT]/oscControl/button1"
      b = get "/osc:[VOTRE_IP:PORT]/oscControl/slider1"
      if  a[0] == 1
        sample :bd_boom, amp: b[0]
      end
    end
```
