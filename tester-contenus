## Itération des synthétiseur
Itérer chaque nom de synthétiseur et les tester rapidement.

```
syd=synth_names

i=0
while i<syd.length
  8.times do
    puts "synth number"
    puts syd[i], i
    synth syd[i],note: chord(:fs3, :m7)
    sleep (ring 0.125,0.75,0.125,1,0.25,0.5,0.25,1).tick
  end
  i+=1
end
```

## Itération des effets
Itérer chaque nom d'effet et les tester rapidement.

```
fxd=fx_names

i=0
while i<fxd.length
  demofx = fxd[i]
  if fxd[i] == :record
    demofx = :none
  end
  with_fx demofx do
    8.times do
      puts "Fx number"
      puts demofx, i
      synth :saw,note: chord(:fs3, :m7),release: 0.6
      sleep (ring 0.125,0.75,0.125,1,0.25,0.5,0.25,1).tick
    end
  end
  i+=1
end
```
## Itération des Samples
Itérer dans tous les groupes d'échantillons Sonic Pi chaque nom d'échantillon et les lire.

```
sg=sample_groups
i=0
while i<sg.length
  sn=sample_names(sg[i])
  j=0
  while j<sn.length
    sd=sample_duration(sn[j])
    puts "group, number, sample, number, duration"
    puts sg[i], i, sn[j], j, sd
    sample(sn[j])
    sleep sd+0.5
    j+=1
  end
  i+=1
end
```

## Effet et synthé random
Tester un effet et un synthé au hasard toutes les mesures.

```
use_random_seed 4

fxd=fx_names
syd=synth_names

live_loop :foo do
  k=rrand_i(0,fxd.length)
  i=rrand_i(0,syd.length)
  demofx = fxd[k]
  if fxd[k] == :record
    demofx = :none
  end
  demosyd = syd[i]
  with_fx demofx do
    8.times do
      puts "Demo Fx number, synth number"
      puts demofx, k, demosyd, i
      synth demosyd,note: chord(:fs3, :m7),release: 0.6
      sleep (ring 0.125,0.75,0.125,1,0.25,0.5,0.25,1).tick
    end
  end
end
```
