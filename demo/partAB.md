## PartAB

```
use_sched_ahead_time 2
use_random_seed 4
use_bpm 50

def schr2(num)
  num.hex.to_s(2).rjust(num.size*4, '0').split(//).collect  { |n| n == "1" }.ring
end
reset_mixer!

live_loop :a do #sequences
  6.times do; X = tick; sleep 15; end
  tick_reset
end

live_loop :b do #chords
  5.times do; I = tick; sleep 3; end
  tick_reset
end

live_loop :foo do #samples/notes
  if X<4
    chords = (ring 1, 7, 6, 2, 5)
  else
    chords = (ring 4, 4, 5, 6, 2)
  end
  abc=(chord_degree chords[I], :a2,:major,4)
  puts X
  puts I
  with_fx :slicer, phase: [0.5,0.25,0.75,0.125,0.25][I] do
    use_synth :blade
    play abc+12, amp: 1.7,attack: 0.2,release: 3 if schr2('800').look      # chord
  end
  use_synth :saw
  play abc+12, amp: 0.4,attack: 0.2,release: 3 if schr2('800').look      # chord
  with_fx :echo, phase: [0.5,0.25,0.75,0.25,0.5][I]  do
    use_synth :chiplead
    play abc, amp: 1.2 ,release: 0.2 if schr2('8cb').look ^ (one_in 32)       # chord
  end
  with_fx :reverb do
    use_synth :hollow
    play abc.reflect.look+36, amp: 2,release: 1, attack: 0, cutoff: rrand(90,110) if schr2('800').look ^ (one_in 9)                  # solo
    use_synth :pulse
    play abc[0]-12,attack: 0.05, amp: 0.7,release: 0.35, cutoff: rrand(110,120) if schr2('a22').look ^ (one_in 24) # bass
  end
  with_swing -0.125,pulse: 3 do
    sample :drum_snare_soft, amp: 1.5 if schr2('924').tick ^ (one_in 12)     # drum
    sample :drum_cymbal_closed, amp: 1 if schr2('aaa').look ^ (one_in 24)  # drum
    sample :bd_haus, amp: 2 if schr2('888').look ^ (one_in 32)    # drum
    with_fx :bitcrusher,amp: 1.3,cutoff: rrand(110,130) do
      sample :bd_tek, amp: 2 if schr2('800').look ^ (one_in 31)    # drum
      sample :elec_hi_snare, amp: 1 if schr2('020').look ^ (one_in 30)    # drum
    end
  end
  sleep 0.125
end

```
