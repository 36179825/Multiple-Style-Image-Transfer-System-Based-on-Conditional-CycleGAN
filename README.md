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

# 訓練概述

一個可以在n種不同風格的圖片之間做轉換的CCGAN，我們稱為n-label CCGAN，包含兩個生成器G1、G2和n個判別器。n種風格之間的轉換，若也考慮同一種風格的轉換則總共有C(n, 2) + n = n(n+1)/2對風格的轉換，然而我們有更簡化的訓練程序，只要訓練n-1對風格的轉換就可以讓系統做所有n(n+1)/2對風格的轉換。假若n = 5總共就有5(5+1)/2 = 15對風格的轉換，而我們只要訓練5-1 = 4對風格的轉換就讓5-label CCGAN做15對風格的轉換了。

n種風格其一為照片。為了方便敘述令照片為第0種風格，其餘為第1至n-1種風格。我們只訓練CCGAN的生成器G1將照片（第0種風格）轉換成另外n-1種不同風格（第1至n-1種風格）的圖片。因為CCGAN具有CycleGAN的循環特性，在訓練G1的同時也會訓練生成器G2將另外n-1種不同風格的圖片轉換成照片。

若一開始就將照片（風格0）和其它n-1種風格（風格1至n-1）之間的轉換一起訓練，會比較沒效率。因此我們首先對照片和其它每種風格的轉換分別用一個CCGAN網路單獨訓練，再將訓練好的n-1個網路權重值取平均。如式（三十三）所示，wi為單獨訓練照片和第i種風格之間的轉換得到的網路權重值，W為n-1對風格轉換分別單獨訓練後的權重平均值。接著再以W為初始權重值的CCGAN網路，一起循環訓練n-1對風格的轉換。如此不只能加速訓練，而且可以得到較好的轉換結果。

