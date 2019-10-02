# Multiple-Style-Image-Transfer-System-Based-on-Conditional-CycleGAN
points of my master’s thesis

# Abstract
本篇論文提出一個多種影像風格轉換系統。主要的想法是結合Conditional GAN [1]和CycleGAN [2]，建立出一個條件式循環生成對抗網路（Conditional CycleGAN），期望能夠將一張圖片從一種風格轉換到另一種指定風格的逼真圖片。系統的輸入除了一張圖片外還附加了一個指定轉換風格的標籤作為條件，因而比在兩個圖像域之間轉換的CycleGAN有更多的轉換圖像域。期望能建立一個更有彈性的圖像轉換系統。此架構的概念可以延伸至其他的轉換應用，比如人臉表情或各種特徵的合成等。

# Related work
[Generative Adversarial Nets (GAN)](http://papers.nips.cc/paper/5423-generative-adversarial-nets.pdf)

[Conditional GAN (CGAN)](https://arxiv.org/abs/1411.1784)

[Cycle-Consistent Adversarial Networks (CycleGAN)](https://github.com/junyanz/CycleGAN)

[PatchGAN](https://phillipi.github.io/pix2pix/)

# Conditional CycleGAN

CCGAN 系統架構圖
![image](https://github.com/36179825/Multiple-Style-Image-Transfer-System-Based-on-Conditional-CycleGAN/blob/master/CCGAN%E7%B3%BB%E7%B5%B1%E6%9E%B6%E6%A7%8B%E5%9C%96.png)

前向流程架構圖
![image](https://github.com/36179825/Multiple-Style-Image-Transfer-System-Based-on-Conditional-CycleGAN/blob/master/%E5%89%8D%E5%90%91%E6%B5%81%E7%A8%8B%E6%9E%B6%E6%A7%8B%E5%9C%96.png)

反向流程架構圖
![image](https://github.com/36179825/Multiple-Style-Image-Transfer-System-Based-on-Conditional-CycleGAN/blob/master/%E5%8F%8D%E5%90%91%E6%B5%81%E7%A8%8B%E6%9E%B6%E6%A7%8B%E5%9C%96.png)

# CCGAN 系統架構

CCGAN的生成器架構類似CycleGAN，如圖十二所示，包含六層卷積層（Convolutional layer）和位於中間的五個殘差區塊（residual block）[28]（CycleGAN共使用九個殘差區塊），除最後一層以外，每個卷積層之後都有一層實例正規化（Instance Normalization）和線性整流函數（ReLU）當作激活函數（activation function），最後一層的激活函數是使用雙曲正切函數（tanh）。其中殘差區塊的目的是為了讓後面的層數也能學習到前面的特徵，並能縮短訓練的時間。
判別器架構如圖十三所示，包含五層卷積層，除第一層及最後一層以外，每個卷積層之後都有一層批量正規化（Batch Normalization, BN）和線性整流函數（Leaky ReLU）當作激活函數，最後輸出為30*30大小的圖像塊（patch），利用多個數值來表示圖片的真偽。

generator
![image](https://github.com/36179825/Multiple-Style-Image-Transfer-System-Based-on-Conditional-CycleGAN/blob/master/generator.png)

discriminator
![image](https://github.com/36179825/Multiple-Style-Image-Transfer-System-Based-on-Conditional-CycleGAN/blob/master/discriminator.png)

