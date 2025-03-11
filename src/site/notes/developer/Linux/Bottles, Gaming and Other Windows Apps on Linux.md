---
{"dg-publish":true,"tags":["linux","gaming"],"permalink":"/developer/linux/bottles-gaming-and-other-windows-apps-on-linux/","dgPassFrontmatter":true}
---

Gaming on Linux can either be a cake walk or walking on glass. I've experienced both sides of the coin.

## The Right Distro
If you search "Best Linux distro for gaming" you'll get a handful of ones that tout "works out of the box" such as...
- [Garuda Linux](https://garudalinux.org/)
- [Nobara Linux](https://nobaraproject.org/download-nobara/)
- [Pop!_OS by System76](https://pop.system76.com/)

I did give **Garuda** a try and was impressed by the n00b friendly installation of OS and apps, but there was still basic GUI issues when I would wake the computer from sleep (black/white static screen). Instead of trying to work out the bugs, I wanted to return to a **Debian** based distro

**PopOS** seemed like clear winner with Nvidia drivers baked. Everything was peachy the first week or 2 of use, but afterwards It would be super slow to cold boot, simple apps such as the folder explorer and software store would take 2-3 min to load each time. Updating software sometime took hours (not sure if just GUI bug or literal slow downloads). I just had it after troubleshooting on and off.

Surprise surprise I come crawling back to [Linux Mint](https://www.linuxmint.com/)as my go-to distro. 

> [!note] Nvidia Driver
> As of writing this I installed the `nvidia-driver-535` recommended for my **GTX 1070**. There is a higher `nvidia-driver-545` but everytime I attempted to reinstall this it would fail and revert to the open source `nouveau` driver 

> [!warning] update flatpak after driver
> After you update the gpu driver, also run `flatpak update` to make sure the sandbox drivers are also up to date. I would get `openGL missing` and or `(ERROR) Unable to load libGLX_nvidia.so.0` issues 

---
## Credits
- [NVIDIA: Issues with OpenGL support · Issue #138 · flatpak/flatpak · GitHub](https://github.com/flatpak/flatpak/issues/138)
- [Unable to load libGLX_nvidia - General - Flathub Discourse](https://discourse.flathub.org/t/unable-to-load-libglx-nvidia/6471)