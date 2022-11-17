# ELECTRONIC : Sonic Pi + VCV Rack

[Download VCV RACK Patch](https://patchstorage.com/electronic/)

Sonic Pi send midi to VCV RACK for speech synthesis with Audible Instruments, Macro Oscillator 2

[Video youtube](https://youtu.be/TSmakawV1Ko)

![](https://patchstorage.com/wp-content/uploads/2022/11/ELECTRONIC-1024x576.png)

```
reset_mixer!; use_cue_logging false; use_debug false; use_osc_logging false; use_midi_logging false
set_mixer_control! lpf: 135.5, lpf_slide: 3 # Global Sonic Pi Low Pass Filter

live_loop :a do; tick # STRUCTURE # PAD , BASS
  if beat > 144; set_mixer_control! amp: 0, amp_slide: 33; end # Fadeout fin
  d = dice(4) # Degrés d'accords (un simple dé 4) variables (d)
  g = (knit :major,5,:minor,1).look # Gammes (5 Majeur,1 mineur) variables (g)
  Ab = (chord_degree d, :c2, g,4) # Degrés d'accords (d), Gammes (g) variables (Ab) / 8 temps
  puts 'C', g, 'degree', d, 'beat', beat # Info Journal
  puts note_info(Ab[0]), 'Ab[0,1,2,3]', Ab # Info Journal
  with_fx :level, amp: 1.9 do
    synth :hollow, note: Ab+12, amp: 9, attack: 0.5, decay: 0.1,
      sustain: 4.4, release: 5, cutoff: range(80,90).choose, res: 0.99, noise: 0 # PAD
  end
  if beat > 15; with_fx :ixi_techno, phase: [4,4.5,5].choose, cutoff_min: rrand(90,95), cutoff_max: rrand(105,110), mix: 0.45 do
      with_fx :slicer, phase: [1/3.0,1/3.0,2/3.0].choose do
        synth :prophet, amp: 1.2, note: Ab[0]-0.1, attack: rrand(0.5,1), sustain: 5, release: 2.5,
          cutoff: range(105,120).choose, cutoff_slide: 8, res: 0.7 # BASS
      end
  end; end
  sleep 8 # 8 Temps
end

live_loop :b, sync: :a do; tick # ARPEGE
  with_fx :ring_mod, freq: Ab[0]+12, mod_amp: 0.6, amp: 1.2 do
    synth :tb303, note: Ab.reflect.drop(1).look+(knit 0,4,12,1,24,1).choose, amp: 1.5+rrand(0.05,-0.05),
      pan: rrand(0.7,-0.7), attack: 0.01, decay: 0.01, sustain: 0.05, release: 0.2+rrand(0.125,0.185),
      env_curve: 6, cutoff_release: 1, cutoff: range(90,120).stretch(6).reflect.drop(1).look,
      res: 0.79+(range(20,0).look/100), wave: 1, pulse_width: 0.33+(range(0,33).stretch(2).reflect.drop(1).look/100) # ARPEGE
  end
  sleep 0.25 # 1/8 Temps
end

live_loop :c, sync: :a, delay: 8 do; tick # KICK , SNARE
  with_fx :compressor,  amp: 1.5, threshold: 0.3, clamp_time: 0.008, slope_below: 0.75 do
    synth :mod_fm, note: :c1+0.2, amp: 8.1, decay: 0.007, sustain: 0.008, release: 0.65, env_curve: 6,
      cutoff: :c9-0.2, divisor: 3, depth: 3, mod_phase: 0.0014, mod_range: 12*2, mod_pulse_width: 0.95,
      mod_phase_offset: 0.9, mod_invert_wave: 0, mod_wave: 1 if (spread 1,16).look ^ (one_in (knit 12,56,1,8,12,96).look) # KICK
  end
  with_fx :rhpf, amp: 1.5, pre_amp: 1.5, pre_mix: 1.5, cutoff: :g6-rrand(0.2,0.4), res: rrand(0.1,0.3), mix: 0.78 do
    synth :pnoise, amp: 4.5, attack: 0, decay: 0.02, release: 0.75+rrand(0.05,-0.05), env_curve: 6, cutoff: :g5+0.2,
      res: 0.72+(range(0,5).stretch(4).reflect.look/100) if (bools 0,0,0,0, 0,0,0,0, 1,0,0,0, 0,0,0,0).look ^ (one_in 19) # SNARE
  end
  sleep 0.125 # 1/16 Temps
end

live_loop :d, sync: :a, delay: 16 do; tick # HITHAT
  t = [0.25,0.25,0.125,0.25,0.125,0.25,0.125,0.25,0.125/2,0.125,0.25,0.25].choose # Temps variables(t)
  8.times do
    synth :cnoise, amp: 0.45, attack: (knit 0,3,rrand(0.018,0.022),1).choose, release: 0.1+rrand(0.05,-0.02), env_curve: 7,
      cutoff: :c9+range(-3,10).stretch(3).reflect.drop(1).look, res: 0.55 # HITHAT
    sleep t # random 8x1/8,8x1/16,8x1/32 Temps
  end
end

live_loop :e, sync: :a, delay: 24 do; tick # ELECTRONIC
  midi Ab[0]+12,sustain: 0.1,vel_f: 0.5,channel: 1 if (bools 0,1,0,0, 0,0,0,0, 0,0,0,0, 0,0,0,0).look # ELECTRONIC
  sleep 0.25 # 1/8 Temps
end
live_loop :f, sync: :a do; tick # WORDS
  v = (knit 0,16*8,0.5,16*6,0,16*2,0.5,16*6,0,16*2).look # Velocité variable(v) (mute)
  midi [Ab[0],Ab[2],Ab[0]+12].choose+12,sustain: 0.1,vel_f: v,channel: 2 if (bools 0,0,0,0, 0,0,1,0, 0,0,0,0, 1,0,0,0).look # WORDS
  sleep 0.25 # 1/8 Temps
end
```
