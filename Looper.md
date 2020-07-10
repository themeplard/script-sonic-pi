# Looper 

 Un looper permet l’enregistrement de pistes sonores afin de les faire rejouer ensuite en boucle.

```
use_real_time

# beat
live_loop :a do
  sample :bd_tek
  sleep 1
end#

# enregistrement audio dans foo
live_loop :b, sync: :a do
  buffer(:foo,16)
  with_fx :record, buffer: :foo do
    synth :sound_in, sustain: 16 # entrée audio
  end
  sleep 16
  stop
end

# lecture de foo
live_loop :c, sync: :a do
  sample buffer(:foo)
  sleep 8
end

```
