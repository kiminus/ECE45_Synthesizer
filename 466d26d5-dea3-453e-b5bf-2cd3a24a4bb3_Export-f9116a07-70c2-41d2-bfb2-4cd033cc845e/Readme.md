# Sound Synthesizer

Created: March 22, 2024 9:30 PM

### overview and tips

- Participant: zilin liu, A17691286 (I did everything)
- this is a react project using [web audio ap](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)i. Use chrome browser if you encounter compatibility issues

- if you find a bug or a sound does not terminate properly, refresh the page and most of the time it should solve the problem
- due to the react `useEffect` property, it would run when first render the page, so you may hear a sound when open the page, it is expected.
- the side bars are just for decoration, initially I thought more people would join me but I ended up complete everything myself. So you can ignore them

### Main control section

- use the first button `Hold to play envelope` to play a sound with the ADSR envelop
- or you can quickly play pitches by pressing the button on the second row. Note those pitches are NOT using the ADSR envelop properties.
    - this would automatically change the `frequency` property of the wave
- both play methods use all the defined settings (except ADSR, since that one would require a different User interaction logic) in this page

### Waveform

- there are 4 types of waveforms available: `sine`, `square`, `sawtooth`, and `triangle`

![[https://en.wikipedia.org/wiki/Waveform](https://en.wikipedia.org/wiki/Waveform), the x axis represent time, y is ampliude](Sound%20Synthesizer%205892b46114ca4d15866d5bfbea608417/Untitled.png)

[https://en.wikipedia.org/wiki/Waveform](https://en.wikipedia.org/wiki/Waveform), the x axis represent time, y is ampliude

- observed that the square wave is the most buzziest wave in all of the four types of waveforms, this is due to the slope of the waveforms
    - the slope of sine wave is a cosine, so it has the most smooth transition (or harmony)
    - the slope of of triangle is a constant, so transition is also smooth
    - the slope of a sawtooth, however, consists of one infinity (when it goes from +amplitude to -amplitude. This creates a buzzy sound
    - similarly, the square wave has two instantaneous transitions, so it is the buzziest of the four

### Frequency

- move the slider to change the frequency, or use the dropdown to select a note in the 4th octave
- the wave sounds thinner as we move up the frequency, the frequency

### Amplitude

- the amplitude change the overall amplitude of the sound
- note: since there is a filter, the actual amplitude does not actually goes to 0 when the value is 0

- an interesting observation: the sine wave:
    - note, when you set the amplitude greater than 1 (or any value beyond the range of [-1,1], you hear a clipping sound if you set the amplitude greater than 0.5
    - or if you turn on the detune and when the constructive interference occurs.
    - this is because the max amplitude for chrome audio is 1, after passing this value it would clamped to 1.
    
    ![[https://teropa.info/blog/2016/08/30/amplitude-and-loudness](https://teropa.info/blog/2016/08/30/amplitude-and-loudness)](Sound%20Synthesizer%205892b46114ca4d15866d5bfbea608417/Untitled%201.png)
    
    [https://teropa.info/blog/2016/08/30/amplitude-and-loudness](https://teropa.info/blog/2016/08/30/amplitude-and-loudness)
    

### Detune

> **Detune** is a parameter that is used to offset a played note from its base frequency. The unit for detune is **cents**; one cent is equivalent to 1/100 of a **semitone** and there are 12 semitones in an octave.
> 

$$
f_d=f_0 *2^{d/1200}
$$

- where
    - fd is the frequency of the detune
    - f0 is the original frequency
    - d is the detune
- interference pattern and modulation
    - intensity increase
    - as detune increases, or the modulation frequency increases $|f_1 - f_2|$, the interfering result is more apparent
- Note that if we increase the detune, the modulation frequency increases
- connection to class: The Fourier series
    - in the class, we discussed how a sound is composed of a Spectrum of signals
    - in this simulation, we can play with the detune (which is essentially offset from the base frequency)
    - by change the detune, we create a new wave of difference frequency, and we can hear the combined effect of multiple waves.
    - this is the video we watched in the class about complex input signals.

### ADSR

- use the `hold to play` button to enable ADSR
- there is a bug in the release method, I am trying to hot fix it if possible,

### band pass

documentation:

| bandpass | Standard second-order bandpass filter. Frequencies outside the given range of frequencies are attenuated; the frequencies inside it pass through. | frequency value: The center of the range of frequencies. | Q-value - Controls the width of the frequency band. The greater the Q value, the smaller the frequency band. | Not used |
| --- | --- | --- | --- | --- |

![[https://www.electronics-tutorials.ws/filter/filter_4.html#:~:text=Typical Band Pass Filter Circuit&text=Then for widely spread frequencies,%3D ƒH – ƒL](https://www.electronics-tutorials.ws/filter/filter_4.html#:~:text=Typical%20Band%20Pass%20Filter%20Circuit&text=Then%20for%20widely%20spread%20frequencies,%3D%20%C6%92H%20%E2%80%93%20%C6%92L).](Sound%20Synthesizer%205892b46114ca4d15866d5bfbea608417/Untitled%202.png)

[https://www.electronics-tutorials.ws/filter/filter_4.html#:~:text=Typical Band Pass Filter Circuit&text=Then for widely spread frequencies,%3D ƒH – ƒL](https://www.electronics-tutorials.ws/filter/filter_4.html#:~:text=Typical%20Band%20Pass%20Filter%20Circuit&text=Then%20for%20widely%20spread%20frequencies,%3D%20%C6%92H%20%E2%80%93%20%C6%92L).

![[https://www.electronics-tutorials.ws/filter/filter_4.html#:~:text=Typical Band Pass Filter Circuit&text=Then for widely spread frequencies,%3D ƒH – ƒL](https://www.electronics-tutorials.ws/filter/filter_4.html#:~:text=Typical%20Band%20Pass%20Filter%20Circuit&text=Then%20for%20widely%20spread%20frequencies,%3D%20%C6%92H%20%E2%80%93%20%C6%92L).](Sound%20Synthesizer%205892b46114ca4d15866d5bfbea608417/Untitled%203.png)

[https://www.electronics-tutorials.ws/filter/filter_4.html#:~:text=Typical Band Pass Filter Circuit&text=Then for widely spread frequencies,%3D ƒH – ƒL](https://www.electronics-tutorials.ws/filter/filter_4.html#:~:text=Typical%20Band%20Pass%20Filter%20Circuit&text=Then%20for%20widely%20spread%20frequencies,%3D%20%C6%92H%20%E2%80%93%20%C6%92L).

- from these two equations,  the equation for the center frequency is

$$
F_c = \sqrt{f_l + f_h}
$$

- for the Q factor, I found the definition in this material :

![[https://blog.knowlescapacitors.com/blog/filter-basics-part-7-different-approaches-to-q-factor](https://blog.knowlescapacitors.com/blog/filter-basics-part-7-different-approaches-to-q-factor)](Sound%20Synthesizer%205892b46114ca4d15866d5bfbea608417/Untitled%204.png)

[https://blog.knowlescapacitors.com/blog/filter-basics-part-7-different-approaches-to-q-factor](https://blog.knowlescapacitors.com/blog/filter-basics-part-7-different-approaches-to-q-factor)

- so the Q factor for the band pass is

$$
Q = \frac{f_c}{f_h - f_l}
$$

### low pass

- I think I may switch to low pass and i will add band pass if i still got the time
- if you lower the low pass frequency, observe that when the low pass is low, the sound is more deep.
    - connection to the class: you can see the higher frequency is attenuated.
- The Q factor is the damped value of the filter

### In the end

1. This is a hard project. I self learned react (and javascript) development, web audio api and the ant design pattern(a react design pattern) to complete this project. I spent a lot of time in debugging the code, especially for the asynchronous methods, since those mistakes are really hard to locate especially for a beginner. Please consider this is a solo project and the effort I put into this in your grading, I really need this extra credit to make up for the final. thanks!
2. I might upload some videos to explain coding behind it, if you did not see it now, you can go back to this repository on Monday after I upload it (hopefully). 

### reference

[https://github.com/mdn/webaudio-examples](https://github.com/mdn/webaudio-examples)

[https://developer.mozilla.org/en-US/docs/Web/API/](https://developer.mozilla.org/en-US/docs/Web/API)