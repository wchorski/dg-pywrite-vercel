---
{"dg-publish":true,"permalink":"/developer/mac-os/i-tunes-audio-output-not-working/","dgPassFrontmatter":true}
---

On a Windows machine, iTunes likes to have a separate audio routing *feature*, independent of the System Audio. 

Example, I'm playing music out of iTunes with my laptop on my lap (audio coming out of laptop speakers). I go to plug in my headphones via aux chord but the music continues to come out of the built in speakers

One "fix" is to close iTunes and (with the headphones plugged in) start up iTunes again remedying the issue. But that is annoying to do every time you dock, headphone, or unplug your laptop. 

## The Real Fix
> settings (ctrl + ,) was set to "Windows Audio Session". Turns out, the one you probably want is "Direct Sound", the default Windows Audio Driver.

---
## Credits
- [Switching Audio Output on iTunes (Windows… - Apple Community](https://discussions.apple.com/thread/254553277?sortBy=best)
- [[developer/MacOs/index\|index]]