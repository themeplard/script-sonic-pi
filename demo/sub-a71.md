## sub-a71

### Music sonic-pi

```
reset_mixer!
use_bpm 55
live_loop :a do; tick
  set_mixer_control! amp: 0, amp_slide: 16 if beat>150
  Ac=(chord_degree dice(4), :c1,(knit :major,3,:minor,1).look, 4)
  with_fx [:slicer, :none,:panslicer,:none,:wobble,:none].choose, 
  phase: [0.25,0.5,0.125,0.25,1/3.0].choose, probalility: 0.75 do
    2.times do
      synth :rodeo, note: Ac+24, attack: 0.1, decay: 1, sustain: 2.8, 
      release: 2, cutoff: 72, amp: (knit 0.4,8,0,1).look
      sleep 6
    end
  end
end
live_loop :b, sync: :a do; tick
  sample :bd_klub, rate: [0.9,1,1].choose, amp: 1.9 if (spread 1,12).look ^ (one_in 18)
  with_fx [:echo,:none,:none].choose, phase: [0.5,0.25,0.125,0.75,0.375].choose do
    sample :sn_dolf, rate: (knit 1.1,4,1,1,-1.15,1).choose, 
    amp: 0.6 if (bools (knit 0,6,1,1,0,5).look).look ^ (one_in 24)
  end
  synth :cnoise, release: rrand(0.1,0.25), cutoff: rrand_i(110,130), 
  amp: 0.15*(knit [1,0].look,48,[1,0,1].look,48,[0,1,1,1,1].choose,96).look
  synth :fm, note: (knit Ac[0],1,Ac.look,2).look+12, release: rrand(0.1,0.3), 
  amp: 1.9 if (spread 5,12).look ^ (one_in 9)
  sleep 0.125
end
live_loop :c, sync: :a, delay: 12 do; tick
  synth :tech_saws, note: Ac.choose+48, release: rrand(0.35,0.55), 
  amp: (knit 0.7,74,0,70).look if (spread 1,12).look ^ (one_in 10)
  synth :supersaw , note: (chord_invert Ac, [0,1,0,2].choose)+36, 
  release: (knit rrand(0.3,0.75),144,0.3,24).look,
    amp: [0,0.7].look  if (bools (knit 0,9,1,1,0,2).look).look ^ (one_in (knit 15,144,1,24).look)
  sleep 0.125
end
```

### Image Hydra

```
// s0.initVideo("video-0.mp4")
// s1.initVideo("video-1.mp4")

a.show()
a.setBins(4)
a.fft[0]
a.setCutoff(6)
a.setSmooth(0.5)
src(o0)
.blend(src(s0)
.blend(src(s1),()=>Math.sin(time/11))
,()=>Math.sin(time/2)*0.1+0.06)
.modulate(src(o0)
.mult(osc(6,0.1,1.5))
.brightness(()=>Math.sin(time/3)*0.8-0.3)
,0.001)
.layer(shape(2,0.001)
.mult(osc(0.4,0.5,1))
.modulate(noise(() => (a.fft[0])*8))
.luma())
.layer(
  voronoi(50).add(osc(6,0.2,1.5)
  .brightness(-1.2))
  .scale(() => (a.fft[3])*-0.3+1)
  .blend(src(s0),0.8)
  .mask(
    src(s0)
    .blend(src(s1),()=>Math.sin(time/11))
    .invert()
    .thresh(0.83,0)
  )).out()
```
