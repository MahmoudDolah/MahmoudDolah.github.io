---
layout: post
title:  "I built a simple Linux hardware monitoring tool"
date:   2025-05-18 06:39:32 -0400

categories: linux open-source monitoring
---

Recently, I upgraded my GPU from an AMD Radeon RX 5500 XT to an AMD Radeon RX 6600. While this is only an incremental upgrade, it's about twice as fast and I can really feel the difference

![Image from Tom's Hardware GPU performance](https://cdn.mos.cms.futurecdn.net/odX4dmxSVcAKwfs6pcqvJL.png)

I was looking for some tooling around benchmarking the performance and found some great tools[1], but I was intrigued enough about this to build one myself.    
There's a tool called [HWINFO64](https://www.hwinfo.com/download/), but it's only available on Windows - so I decided to try my hand at building a Linux version of the tool that I have very creatively dubbed `linux_hwinfo64`

The repository can be found [here](https://github.com/MahmoudDolah/linux_hwinfo64)     
I even published it on [PyPI](https://pypi.org/project/linux-hwinfo64/)


[1] List of GPU monitoring tools for AMD:
- [radeontop](https://github.com/clbr/radeontop)
- [amdgpu_top](https://github.com/Umio-Yasuno/amdgpu_top?tab=readme-ov-file)
- [ROCm](https://rocm.docs.amd.com/projects/install-on-linux/en/latest/install/quick-start.html)