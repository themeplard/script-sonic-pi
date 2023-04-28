## INTELLIGENCE HARMONIFICIEL

Ceci n'est pas et n'a pas été fait avec une intelligence artificiel,
C'est juste un jeu de mot de mauvaise graine dans l'aire du temps.

```
reset_mixer!; use_cue_logging false; use_random_seed 444493 ### Reset Mixer, Journal d'Info et seed (la mauvaise graine ^^)

### Info: Départ direct de "Intelligence Harmonificiel" avec Boucle a
live_loop :a do # début bloc live_loop :a
  if beat > 195; stop; end ### Stop live_loop :a à partir de time 195
  Fb= dice(4) ### Lancer de Dé 4 pour Dégré d'accord
  Ab= (chord_degree Fb, :c2, :major, 5) ### Accord de degré Fb en do majeur
  rv=[0,1,0,2,0].choose ### Définition du renversement
  Arb=(chord_invert (Ab),rv).reflect.take(8) ### Accord renversé
  Ax=(note_range :c3,:c5, pitches: (scale :c3, :major)).drop((Fb)-1).take(8).mirror.shuffle ### Mode du degré Fb en do majeur
  puts note_info(Ab[0]),"Dégré d\'accord",Fb,"en do majeur","Renversement",rv ### Info (ligne 1)
  puts "Main Gauche",Arb; puts "Main Droite",Ax ### Info des notes du piano et autres instruments (ligne 2 et 3)
  32.times do; tick ### 32 x 1/8 Temps
    synth :prophet, note: Ax.look, sustain: rrand(0.07,0.1), release: rrand(0.1,0.07), hard: rrand(0.5,0.7), pan: rrand(0,0.5),
      amp: 1*(knit [0,0,1,1].look,256,[0,1,1].choose,128,1,128,[0,1,1].choose,128).look ### Piano Main Droite (Notes Ax)
    sample :bd_klub, pan: rrand(0.1,-0.1), amp: 3.8 if (spread 1,4).look ^ (one_in 24) ### Kick sur les Temps 1 + Random 1/24
    synth :hollow, note: Ab.choose+36,release: rrand(0.4,1.5), attack: 0, cutoff: 100, pan: rrand(0.5,-0.5),
      amp: (knit 6,256,0,128).look if (spread 1,16).look ^ (one_in 9) ### Synth Solo (Notes Ab)
    sleep 0.125 ### 1/8 Temps
  end
end # fin du bloc live_loop :a

### Info: Départ Boucle b à partir de time 11 (delay: 11)
live_loop :b, sync: :a, delay: 11 do; tick # début bloc live_loop :b
  if beat > 192; stop; end ### Stop live_loop :b à partir de time 192
  with_fx (knit :whammy,1,:none,3).look, transpose: 0, mix: 0.5, deltime: 0, max_delay_time: 0.75, pre_amp: 1.4 do
    synth :blade, note: Arb.look, attack: 0, sustain: 0.15,
      amp: 1,hard: rrand(0.4,0.6), pan: rrand(-0.3,-0.6) ### Piano Main Gauche (Notes Arb), FX Whammy 1/4 Notes
  end
  synth :chipnoise, amp: 0.3, env_curve: 3, sustain: 0, release: rrand(0.2,0.4),
    freq_band: 5 if (bools 0,0,0,0,1,0,0,0).look ^ (one_in 12) ### Snare sur les Temps 3 + Random 1/12
  synth :chipbass, note: [Ab[0],Arb.look].look,release: rrand(0.25,0.4),
    amp: 2 if (spread 3,8).look ^ (one_in 12) ### Bass (Notes Ab[0], Arb)
  sleep 0.25 ### 1/4 Temps
end # fin du bloc live_loop :b

### Info: Départ Boucle c à partir de time 23 (delay: 23)
live_loop :c, sync: :a, delay: 23 do; tick  # début bloc live_loop :c
  if beat > 197; stop; end ### Stop live_loop :c à partir de time 197
  a=rrand_i(1000,8000); b=0; if beat > 80; b=0.4; end ### Bitcrusher sample_rate et mix à partir de time 80 (if beat > 80)
  with_fx [:none,:bitcrusher,:bitcrusher].choose, bits: rrand_i(4,8), mix: b, sample_rate: a do ### FX Bitcrusher
    sample :loop_safari, onset: pick, sustain: 1, release: 1, pan: rrand(-0.6,0.6),
      cutoff: rrand_i(110,125), amp: 3*[1,0,1,1].choose ### Loop Percu random slice
    sample :loop_tabla, onset: pick, sustain: 1, release: 1, pan: rrand(-0.5,0.5),
      cutoff: rrand_i(110,125), amp: 8*[0,1,1].choose ### Loop Tabla random slice
  end
  sleep [0.125,0.25,0.125].choose ### 1/8, 1/4, 1/8 Temps Random
end # fin du bloc live_loop :c
```
