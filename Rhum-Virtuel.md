## Rhum Virtuel

Pour faire un Rhum Virtuel, il faut mettre:

- Une dose d'océan parce que si il y a du rhum, l'océan n'est pas loin
- Un rythme ou une musique spécifique pour donner une origine géographique
- Des sons FX pour l'effet de l'alcool
- Et sonic-pi bien sûr

Ici c'est un Maloya, un rythme traditionnel de l'Ile de la Réunion.
Alors, je vous ai fait un Rhum Virtuel de l'Ile de la Réunion.

[![Soundcloud](https://download.seaicons.com/download/i80491/uiconstock/socialmedia/uiconstock-socialmedia-soundcloud.ico)](https://soundcloud.com/themeplard/rhum-virtuel)

To make a Virtual Rum, you have to put:

- A dose of ocean because if there is rum, the ocean is not far
- A specific rhythm or music to give a geographical origin
- FX sounds for the effect of alcohol
- And sonic-pi of course

Here is Maloya, a rhythm of Reunion Island.
So, I made for you a Virtual Rum from Reunion Island.

```
# eviter Error: Timing Exception: thread got too far behind time
use_sched_ahead_time 2
# tempo
use_bpm 50
# fonction hexadecimal to binary rhythmic scheme
def schr2(num)
  num.hex.to_s(2).rjust(num.size*4, '0').split(//).collect  { |n| n == "1" }.ring
end
# 0=0000, 1=0001, 2=0010, 3=0011
# 4=0100, 5=0101, 6=0110, 7=0111
# 8=1000, 9=1001, a=1010, b=1011
# c=1100, d=1101, e=1110, f=1111

live_loop :foo, delay: 6 do |n|
  10.times do
    b = [1,0,0,0,0,1,0,0,0,1][n]
    puts n
    puts b
    case b
    when 0
      96.times do # rythme
        with_fx :bpf,amp: 1.2 do
          synth :pnoise, note: 72,amp: 1, attack: rrand(0.005,0.01), release: [0.1,0.2,0.12].ring.look, env_curve: 6, cutoff: rrand(110, 130) if schr2('fff').tick # kayamb
          synth :dull_bell, note: 122,amp: 0.8, attack: rrand(0.005,0.01), release: [0.04,0.08,0.01].ring.look, env_curve: 6, cutoff: rrand(110, 130) if schr2('fff').look # triangle
          with_fx :bitcrusher do
            synth :bnoise, note: 72,amp: 2, attack: 0, release: [0.05,0.1,0.07].ring.look, env_curve: 6, cutoff: rrand(90, 110) if schr2('fff').look # sati
          end
        end
        sample :bd_tek,amp: 3.2, cutoff: rrand(100, 110), release: [0.1,0.2,0.05].ring.look if schr2('b6db6f').look ^ (one_in 18) # rouler
        with_fx :reverb ,room: 0.5, mix: 0.4 do
          sample [:tabla_ghe1,:tabla_ghe2,:tabla_ghe3,:tabla_ghe4].choose, cutoff: rrand(90, 130), amp: 2.3 if schr2('a02').look ^ (one_in 24) # tabla
          sample [:tabla_na_o,:tabla_tas1,:tabla_na_s,:tabla_tas2,:tabla_na,:tabla_tas3,:tabla_tun1].choose, cutoff: rrand(90, 130), amp: 1.3 if schr2('924').look ^ (one_in 6) # tabla
          with_fx :ixi_techno do
            use_synth :blade
            play (scale :e5, :minor_pentatonic, num_octaves: 2).choose, amp: 2.3,release: 0.7, attack: 0.1, cutoff: rrand(100,130) if schr2('808080').look ^ (one_in 15) # fx1
          end
        end
        sample :ambi_glass_hum,amp: 2.2, rate: 0.5 if schr2('800000000000').look # fx2
        sleep 0.125
      end
    when 1
      48.times do # break
        with_fx :bpf,amp: 1.2 do
          synth :pnoise, note: 72,amp: 1, attack: rrand(0.005,0.01), release: [0.1,0.2,0.12].ring.look, env_curve: 6, cutoff: rrand(110, 130) if schr2('e0').tick # kayamb
          synth :dull_bell, note: 122,amp: 0.8, attack: rrand(0.005,0.01), release: [0.08,0.08,0.08].ring.look, env_curve: 6, cutoff: rrand(110, 130) if schr2('aaa').look # triangle
          with_fx :bitcrusher do
            synth :bnoise, note: 72,amp: 2, attack: 0, release: [0.05,0.1,0.07].ring.look, env_curve: 6, cutoff: rrand(90, 110) if schr2('0e').look # sati
          end
        end
        sample :bd_tek,amp: 3.2, cutoff: rrand(100, 110), release: [0.1,0.2,0.05].ring.look if schr2('e0').look # rouler
        with_fx :reverb ,room: 0.5, mix: 0.5 do
          sample [:tabla_ghe1,:tabla_ghe2,:tabla_ghe3,:tabla_ghe4].choose, cutoff: rrand(90, 130), amp: 2.3 if schr2('0e').look # tabla
          sample [:tabla_na_o,:tabla_tas1,:tabla_na_s,:tabla_tas2,:tabla_na,:tabla_tas3,:tabla_tun1].choose, cutoff: rrand(90, 130), amp: 1.3 if schr2('0e').look # tabla
        end
        sleep 0.125
      end
    end
    n=n+1
  end
  stop
end

# Ocean Coded by Sam Aaron

with_fx :reverb, mix: 0.5 do
  live_loop :oceans do
    with_fx :level, amp: 0.2 do
      s = synth [:bnoise, :cnoise, :gnoise].choose, amp: rrand(0.01, 0.05), attack: rrand(0, 4), sustain: rrand(0, 2), release: rrand(1, 5), cutoff_slide: rrand(0, 5), cutoff: rrand(60, 100), pan: rrand(-1, 1), pan_slide: rrand(1, 5), amp: rrand(0.5, 1)
      control s, pan: rrand(-1, 1), cutoff: rrand(60, 110)
      sleep rrand(2, 4)
    end
  end
end
```


