# Multiple-Style-Image-Transfer-System-Based-on-Conditional-CycleGAN
points of my master’s thesis

# Abstract
本篇論文提出一個多種影像風格轉換系統。主要的想法是結合Conditional GAN [1]和CycleGAN [2]，建立出一個條件式循環生成對抗網路（Conditional CycleGAN），期望能夠將一張圖片從一種風格轉換到另一種指定風格的逼真圖片。系統的輸入除了一張圖片外還附加了一個指定轉換風格的標籤作為條件，因而比在兩個圖像域之間轉換的CycleGAN有更多的轉換圖像域。期望能建立一個更有彈性的圖像轉換系統。此架構的概念可以延伸至其他的轉換應用，比如人臉表情或各種特徵的合成等。

# Related work
Generative Adversarial Nets (GAN)

Conditional GAN (CGAN)

Cycle-Consistent Adversarial Networks (CycleGAN)

PatchGAN

# Conditional CycleGAN