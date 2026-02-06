---
tags: AI
---

*for back-of-the-envelope datacenter stat estimates*

I noticed that I'd hear news headlines like "OpenAI completes $X billion datacenter" or "Anthropic completes X megawatt datacenter" and not really have a felt sense of what that actually means.

Here's a collection of numbers that'll get you a lot closer to doing [back-of-the-envelope Fermi estimates](https://en.wikipedia.org/wiki/Fermi_problem) when you hear a datacenter stat. This aims to be a rough tldr reference point without going too deep into the technicalities.

---

### Compute

- **Units:**
  - It can be handy to measure compute in H100 equivalents. Here are the more important NVIDIA GPUs and how they compare to an H100 (flop/s):
    - A100 (~0.3x H100) and H20 (~0.3x H100) < H100 < B30A (~2x H100) < B300 (~4x H100)
  - 10k H100 equivalents = GPT-4

- **GPT-4 Training:** Was trained for 3 months (You can either 2x the GPUs or 2x the time. So 10k H100s for 3 months = 5k H100s for 6 months)

- **Largest Current AI Datacenter:** Around 275k H100 equivalents (xAI Colossus Memphis Phase 3) ([source](https://epoch.ai/data/gpu-clusters?view=table))
  - So the frontier is currently at around 30x what GPT-4 was trained on

---

### Energy

- **Units:**
  - Joules = unit for energy
  - Watts = unit for power = energy over time (e.g., joules per second)
  - So a watt is a unit for power, watt-hours is a unit for energy (like joules)
  - 1 kW = 1 thousand W (10^3 W)
  - 1 MW = 1 million W (10^6 W)
  - 1 GW = 1 billion W (10^9 W)
    - The current largest datacenter is about 1/3 this size
  - 1 TW = 1 trillion W (10^12 W)
  - The city of Seattle consumes about 1 GW on average ([source](https://epoch.ai/blog/what-you-need-to-know-about-ai-data-centers?__readwiseLocation=))
  - Washington state [consumes about 92 TWh annually](https://www.energy.gov/sites/prod/files/2016/09/f33/WA_Energy%20Sector%20Risk%20Profile.pdf) ([source](https://situational-awareness.ai/racing-to-the-trillion-dollar-cluster/)), so about 10 GW on average
  - Total annual US electricity production is about 4,250 TWh, so about 485 GW on average

- **Energy per GPU:** Semianalysis [estimates](https://www.semianalysis.com/p/ai-datacenter-energy-dilemma-race) ~1,400W energy per H100 in a datacenter (including facility power overhead, like cooling and networking)
  - So 1 H100 = 1 American household
  - This holds up surprisingly well for the current largest datacenter. Take xAI Memphis Phase 3, which has 275,796 H100 equivalents. This would predict 386 MW, where the actual power is 352.35 MW ([source](https://epoch.ai/data/gpu-clusters?view=table))
  - So you can usually use this to predict datacenter power pretty well, given H100 equivalence. For rough estimates, you can assume power scales pretty linearly with compute

- **Datacenter Power Consumption:**
  - The GPT-4 datacenter used ~0.03 GW (28.9 MW)
  - The largest confirmed AI datacenter is currently 0.35 GW (xAI Colossus Memphis Phase 3) ([source](https://epoch.ai/data/gpu-clusters?view=table))
  - A 10k H100 datacenter (remember, this is GPT-4) uses ~15MW (0.015 GW)
    - So a 100x GPT-4 datacenter (1,000,000 H100) would be around 1.5 GW (so 1.5x Seattle)
  - A 10GW datacenter would be ~30x the current largest datacenter (xAI colossus Memphis Phase 3), and similar to all of Washington state's total power consumption

---

### FLOPs

- **Units**
  - A flop is one floating-point arithmetic operation (like addition or multiplication of decimal numbers)

- **Training Compute Estimates:**
  - ~10^24 flops = GPT-3.5
  - ~10^25 flops = GPT-4
    - Once again, anchoring on GPT-4 is nice. So if you hear a model was trained with 10^27 flops, just subtract 27-25 = 2. Now you know that it was trained with 10^2 = 100x as many flops as GPT-4
  - ~10^26 flops = Grok-4

---

### Datacenter Spending

- **Cost per H100 Equivalent:** Dividing the total datacenter cost by the H100e for the frontier datacenters [here](https://epoch.ai/data/data-centers) gives a ballpark of ~$40k per H100 equivalent (for the whole datacenter, not just chips)
  - The Anthropic-Amazon New Carlisle datacenter purportedly cost $15B and has 300k H100e
    - This puts it at $50,000 per H100e (including facility overhead)
  - The xAI Colossus 2 datacenter purportedly cost $9B and has 280k H100e
    - This is a bit less, at $32,000 per H100e
  - The OpenAI Stargate Abilene datacenter purportedly cost $8B and has 250k H100e
    - Also $32,000 per H100e

- **Rough Estimation:** So if a datacenter costs $10B, it's reasonable to guess that it has around 250k H100e. If a datacenter costs $4B, you can guess it has around 100K H100e
  - It seems like there's a bit more variance here compared to energy requirements per H100e, but it still seems like it gets you in the right ballpark

- **Cost Breakdown** of datacenters (excluding labor) ([source](https://arxiv.org/pdf/2405.21015))
  - Chips ~52%
  - Server Components ~24%
  - Networking ~18%
  - Energy ~6%

- **Additional Spending Data:**
  - OpenAI spends 6x on compute as on labor ([source](https://blog.ai-futures.org/p/why-america-wins))
  - US annual wages for work that can [in principle be done remotely](https://www.nber.org/papers/w26948) exceeds $5T ([source](https://adamkhoja.substack.com/p/8c09bce9-d044-47a4-a45d-1fa09327839e?__readwiseLocation=))

---

### Geopolitics

- **US vs China Compute:**
  - The US currently has around 6x as many H100 equivalents as China
    - The US has 1.395 million H100 equivalents (from [Epoch's dataset](https://epoch.ai/data/gpu-clusters#data-insights))
      - The largest US datacenter is 275k H100 equivalents
    - China has 231,651 H100 equivalents (from [Epoch's dataset](https://epoch.ai/data/gpu-clusters#data-insights))
      - The largest Chinese datacenter is 30k H100 equivalents (roughly 10x smaller than the largest US datacenter)
      - So the state of Tennessee alone (which has >583,079 H-100e) has 2.5x the compute of China

- **Global Distribution:** The US has around 75% of global compute, China has around 15%, the EU has around 5%, and the rest of the world combined has around 5% (from [Epoch's dataset](https://epoch.ai/data/gpu-clusters#data-insights))

- **Export Restrictions:** The most advanced chip that is currently being legally exported to China is the H20 (which, remember, is ~0.3x the performance of an H100)

---

### Acknowledgements

Please let me know if you spot any errors. Realistically, there are likely a few.

*Most of this is a hodgepodge of stats I stole from [Epoch](https://epoch.ai/), [Semianalysis](https://newsletter.semianalysis.com/), [AI Futures](https://ai-futures.org/), and [Situational Awareness](https://situational-awareness.ai/). Thanks to Adam Khoja and Arunim Agarwal for discussions, and Konstantin Pilz for answering a clarifying question.*

*PS: If someone turns these into a nice Anki deck ([with pretty good prompts](https://andymatuschak.org/prompts/)), I'll Zelle you $10.*
