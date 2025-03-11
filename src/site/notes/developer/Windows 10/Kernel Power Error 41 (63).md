---
{"dg-publish":true,"permalink":"/developer/windows-10/kernel-power-error-41-63/","dgPassFrontmatter":true}
---

I've recently had problems with my **Windows 10 Machine** suddenly powering off as if someone ripped the cord from the wall. Looking into the logs via

- `Windows Key + r`
- Open `eventvwr`

The solution was to go into my **Aorus BIOS** and reset to default. I assumed there was some type of 'overclock' with the **CPU** or **RAM** that was happening and so far resetting the bios settings did the trick. The only thing I customize in the BIOs is my fan speeds to `silent` as I like to run my computer as quiet as possible 

---
## Backlinks
- [[developer/Windows 10/Microsoft Windows\|Microsoft Windows]]