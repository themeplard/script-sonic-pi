## PartAB
L'idée développée ici etait de faire un script permettant de jouer 2 séquences d'accords différentes pour un ensemble d'instruments.
### live_loop :a
Le `live_loop :a` représente les séquences d'accords. 
- `6.times`= nombre de séquences
- `sleep 15`= durée d'une séquences

### live_loop :b
Le `live_loop :b` représente les suites d'accords. 
- `5.times`= nombre d'accords
- `sleep 3`= durée d'un accord

### live_loop :foo
Le `live_loop :foo` représente l'orchestration.

le `if X<4...else...end` permet de définir les accords des 2 séquences, le `X<4` permet le changement entre les 2.

Le `if beat > 270` permet de définir la fin du morceau avec un fade out `set_mixer_control! amp: 0, amp_slide: 14`.

Le `stop if beat > 290` permet de stopper le `live_loop`.

### A noter
- L'utilisation de `with_swing` qui comme indique le nom, ajoute du swing sur les drums
- L'utilisation de Fonction [hexadecimal/binary rhythmic scheme](https://github.com/themeplard/script-sonic-pi/blob/master/hexadecimal-rythme.md)

## [![Soundcloud](https://icon-icons.com/icons2/159/PNG/64/soundcloud_22554.png)](https://soundcloud.com/themeplard/partab) Ecouter sur Soundcloud

```
use_sched_ahead_time 3
use_random_seed 4
use_bpm 50
reset_mixer!

def schr2(num)
  num.hex.to_s(2).rjust(num.size*4, '0').split(//).collect  { |n| n == "1" }.ring
end

live_loop :a do |k|#sequences
  6.times do; X = tick; sleep 15; end
  tick_reset
end

live_loop :b do #chords
  5.times do; I = tick; sleep 3; end
  tick_reset
end

live_loop :foo do  #samples/notes
  if beat > 270
    set_mixer_control! amp: 0, amp_slide: 14
  end
  stop if beat > 290
  puts X
  puts I
  if X<4
    chords = (ring 1, 7, 6, 2, 5)
    abc=(chord_degree chords[I], :a2,:major,4)
  else
    chords = (ring 4, 4, 5, 6, 2)
    abc=(chord_degree chords[I], :a2,:major,4)
  end
  with_fx :slicer, phase: [0.5,0.25,0.75,0.125,0.25][I] do
    use_synth :blade
    play abc+12, amp: 1.7,attack: 0.2,release: 3 if schr2('800').look # chord
  end
  use_synth :saw
  play abc+12, amp: 0.4,attack: 0.2,release: 3 if schr2('800').look # chord
  with_fx :echo, phase: [0.5,0.25,0.75,0.25,0.5][I]  do
    use_synth :chiplead
    play abc, amp: 1.2 ,release: 0.2 if schr2('8cb').look ^ (one_in 32) # chord
  end
  use_synth :hollow
  play abc.reflect.look+36, amp: 5,release: 1, attack: 0, cutoff: rrand(90,110) if schr2('800').look ^ (one_in 9)  # solo
  use_synth :pulse
  play abc[0]-12,attack: 0.05, amp: 0.7,release: 0.35, cutoff: rrand(110,120) if schr2('a22').look ^ (one_in 24) # bass
  with_swing -0.125,pulse: 3 do
    sample :drum_snare_soft, amp: 1.5 if schr2('924').tick ^ (one_in 12) # drum
    sample :drum_cymbal_closed, amp: 1 if schr2('aaa').look ^ (one_in 24) # drum
    sample :bd_haus, amp: 2 if schr2('888').look ^ (one_in 32) # drum
    with_fx :bitcrusher,amp: 1.3,cutoff: rrand(110,130) do
      sample :bd_tek, amp: 1.5 if schr2('800').look ^ (one_in 31) # drum
      sample :elec_hi_snare, amp: 1 if schr2('020').look ^ (one_in 30) # drum
    end
  end
  sleep 0.125
end
```
## [![Soundcloud](https://icon-icons.com/icons2/159/PNG/64/soundcloud_22554.png)](https://soundcloud.com/themeplard/partab) Ecouter sur Soundcloud
