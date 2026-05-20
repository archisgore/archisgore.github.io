---
layout: post
title: "Disassembled a Shearwater Petrel 2"
description: "My instincts on building a dive computer from first-principles were more than right."
date: 2026-04-18 01:31:18 +0000
original_url: https://medium.com/@archisgore/disassembled-a-shearwater-petrel-2-b29cf9211133
---

## Disassembled a Shearwater Petrel 2

My instincts on building a dive computer from first-principles were more than right.

[**GitHub - archisgore/BFY\_Bottom\_Timer: A Dive computer based on Arduino + off-the-shelf components in…**  
*A Dive computer based on Arduino + off-the-shelf components in a day. - archisgore/BFY\_Bottom\_Timer*github.com](https://github.com/archisgore/BFY_Bottom_Timer "https://github.com/archisgore/BFY_Bottom_Timer")

Last week my Petrel died after a decade of excellent service. An opportunity to disassemble it.

![](/assets/images/disassembled-a-shearwater-petrel-2/1_36TvzHF5rwnfkRqdV_7ZkA_2x.jpeg)![](/assets/images/disassembled-a-shearwater-petrel-2/1_vTqD7H0Vjr7mDkKxjYEAgw_2x.jpeg)![](/assets/images/disassembled-a-shearwater-petrel-2/1_geSmPyoF-ZfnKTkcKvKzxQ_2x.jpeg)![](/assets/images/disassembled-a-shearwater-petrel-2/1_nn8tKjQB-t1XgypyqgnInQ_2x.jpeg)![](/assets/images/disassembled-a-shearwater-petrel-2/1_Mgc3zjAWIfXBRbrLRVnBLg_2x.jpeg)![](/assets/images/disassembled-a-shearwater-petrel-2/1_y8L67SFweSFV7PoSJ3ZdwQ_2x.jpeg)![](/assets/images/disassembled-a-shearwater-petrel-2/1_Kp7V3zy1kuYxTLYTkjEFNQ_2x.jpeg)![](/assets/images/disassembled-a-shearwater-petrel-2/1_m7EKoY8vuNKE6AA3X3SK6w_2x.jpeg)![](/assets/images/disassembled-a-shearwater-petrel-2/1_cfSJk2e9WuOEOCxcb3NNhA_2x.jpeg)![](/assets/images/disassembled-a-shearwater-petrel-2/1_p_xbQc3uigctA-IWKohSPA_2x.jpeg)

This basically confirms what a dive computer really is.

It’s really a microcontroller (Cortex-M chip in this case), an ISM module, a pressure gauge and a screen.

Intuitively I knew this is all it really could be, but always happy to confirm.

I bet most of the cost really comes from the need for reliability, testing, and especially the deco algorithm. Last thing you want is a computer failing mid way through a dive.
