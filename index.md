---
title: Pocket Orchestra
---

# **Midterm project for CIM433 Augmented Reality**

## Description

Pocket Orchestra is an interactive musical experience, that allows the user to shape the soundtrack by choosing different instruments that perform it in real time.

User is given four playing cards - one for each track, and uses them to cover chosen parts of a printed marker.

## Storyboard

![Cards](/images/cards.jpg) ![Marker](/images/marker.jpg) ![Screenshot](/images/screenshot.png)

## AR
### To button or not to button?

My initial idea for the interaction did not include the playing cards. I planned for the user to simply touch the specified areas on the marker to play their respective tracks. That did not feel very engaging though.

Making the user need to put a physical object on the marker makes the actual interaction more meaningful, and more suitable for an AR application. If you think about it, simply pressing buttons is a very artificial task, completely detached from reality. Introducing the element of persistence (marker needs to remain covered for the music to play) adds a physical representation of the state of the application, therefore better connecting the physical and the virtual world.

### 🚀 Space!

I really wanted to make the app feel like the sound is coming from the "orchestra". Adding sound spacialization allows just for that.

The spatial effect is very subtle though. The limitations of marker based AR do not allow the user to get their device close enough to the marker, without losing tracking. I imagine this problem could be solved by using multiple markers in a very well lit room in addition to positional tracking of the user's device. I leave this for future iterations.

## Implementation issues...

### Marker

Designing a good marker is the key to a successful AR experience. It took a few iterations to arrive at the final solution. Making the card slots have the highest contrast and detailed patterns printed solved all of the issues I had with the markers I initially envisioned.

### Synchronization

Initially, my plan of making the tracks play in sync included using the MIDI format. MIDI, unlike MP3 or WAV files, do not contain any audio information themselves. Rather than that, they contain very specific instructions for a host application, which provides the virtual instruments to perform them *in real time*. That makes them easier to synchronize than the standard audio files. Unfortunately, MIDI comes with lots of limitations, when used outside of regular DAW (Digital Audio Workstation) environment, since in my case such host environment would need to be substituted inside Unity. That comes with the complexity of dealing with external plugins, digital instruments, special export formats, and so on.

Luckily, for the scope of this project utilizing MIDI was unnecesary, as the song I put together used only 8 audio tracks at most. Unity's sound optimization can handle such low amount of audio sources with ease, and did not require me to come up with workarounds. With the good old MP3's, making the music sound together was as simple as starting all the 8 tracks at the same time.

**But...**

...how together is together *enough*? Unity's `AudioSource` component has a very convenient public property "Play on awake". At first, right after setting up a simple scene with just 8 AudioSources and a simple `MusicController` script, the time of processing all the initialization events was pretty short. But as soon as I added some more elements, that grab their dependencies on Awake, searching through the scene for specific components, the audio tracks began to play in a noticeable offset. I assume, that Unity's "Play on awake" flag starts the audio playback as soon as the audio file is loaded in memory, which does not happen at the same time for all of them.

A solution was a simple as starting each one of the tracks individually in their Start methods, as at that point they are all allocated in memory.

### Tracking jitter

A really annoying issue I had with my marker is that after covering it with all 4 cards, the camera loses the tracking information from close to half of the marker. That caused the playback to rapidly mute and unmute, resulting in a very annoying stutter.

To solve that, I added a short delay between registering a change in button's `Pressed` status, and actually muting its track. Here's the Update method in the `CardSlot` class that made my music flow continuously:

```c#
private async void Update() {
  if (_button.Pressed == _status) return;

  await Task.Delay(_button.Pressed ? 500 : 1000);
  SlotEventManager.Instance.OnSlotPoseChanged.Invoke(this, _button.Pressed);
  _status = _button.Pressed;
}
```
