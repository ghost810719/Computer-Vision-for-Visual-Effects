# Computer-Vision-for-Visual-Effects
HW2
# Homework 2 Report

1. MUNIT
MUNIT利用UNIT的架構來學習兩個dataset的共同特徵，經由隨機給一個數值來達到不同風格的輸出，每個class都會有content code和style code，content code是不同dataset之間的特徵，而style code是每個class各自的特徵。
![](https://i.imgur.com/26ArSgf.png)
* Model overview
下圖是MUNIT大致的整體架構。左圖代表作訓練的時候，圖片經過encoder得到content和style code，再經由decoder將兩個特徵結合，得到重建的圖片，這兩張圖片的差距越小，代表訓練的結果越好。右圖則是經過交換content code和style code之後，重建出一張不同風格的圖片，在經過encoder得到的content code和style code仍然要與原本的content code和style code相近。
![](https://i.imgur.com/6Ohwxnz.png)

2. Training MUNIT
* 訓練過程
我們花了約三四天的時間訓練到75萬左右個iteration，中間有和50萬左右的訓練結果做比較，發現兩個訓練出來的效果差不多，50萬的甚至還比較清晰、雜訊較少，可能在50萬之前就達到converge了。
![](https://i.imgur.com/RJZD0tA.png)
* 圖片
50萬iteration：
![](https://i.imgur.com/CXRSxul.png)
75萬iteration：
![](https://i.imgur.com/3aYpPBG.png)



2. Inferece in multiple style
* Summer to winter
第一張在溪流和山的部分有轉換成雪的感覺，雖然還是有些雜訊，但整體的感覺還不錯。
第二張在樹木的部分加上了很多雪，但在河川的部分還保留了樹木的倒影，而不是整條變成冰，有點可惜。
![](https://i.imgur.com/kCPFlK2.png)
![](https://i.imgur.com/56JHyjW.png)
* Winter to summer
相對summer-to-winter，winter-to-summer的表現比較差，只把樹木稍微變成綠色，雪的部分都還留著，並沒有成功轉換成夏天的樣子。
![](https://i.imgur.com/9a35kHw.png)


3. Compare with other method
* neural-style(https://github.com/anishathalye/neural-style)
* ![](https://i.imgur.com/aucmRL7.png)

這方法的CNN模型是採用pre-trained的VGG19模型

1.內容重構：
內容指的是圖片中像是山水天空等部分。
把圖片放入CNN模型，使用不同layer提取的feature map對圖片進行重構。使用高層(如d,e)的特徵會使的細節流失，而high level content則會保留下來。

2.風格重構：
風格指的是作畫的手法。
在原始的CNN模型表示之上，建構一個新的可以捕捉輸入圖片風格的特徵空間。style representations計算CNN不同層中的不同特徵之間的相關性，藉由style representations重構輸入圖片的風格。使用多層的feature correlations重構風格會更加匹配圖像本身的風格，但卻也會忽略全景。(This creates images that match the style of a given image on an increasing scale while discarding information of the global arrangement of the scene.)

1層的content loss是 ![](https://i.imgur.com/OuJvshM.png)


1層的Style loss是 ![](https://i.imgur.com/IIJU3kT.png)

多層的Total loss是 ![](https://i.imgur.com/3EylcLd.png)


優化content loss +style loss ![](https://i.imgur.com/pm9qzDb.png)

 
通過迭代最小化loss得到best ![](https://i.imgur.com/dodDzgm.png)
官方建議迭代次數在500到2000之間最好

使用的原圖為![](https://i.imgur.com/GO6cHbq.png)
下面是進行style transfer的結果，最左邊是作為style的原圖，接下來做style transfer的迭代次數依序分別是200 500 1000 2000
![](https://i.imgur.com/4HLPZlq.jpg)
由上面的風格轉換圖可以發現，由於是只有受單一風格圖片的影響，所以顏色的部分會有些不自然(如下面天空的藍色被當成湖泊的顏色)。從200到500之間確實可以感受到圖片變得更自然了，雖然看起來還是有雜質。而1000和2000看起來並沒有如此劇烈的差別。





* Deep dream generator(https://deepdreamgenerator.com/)
與上面的方法相比圖片更漂亮更smooth，沒有那麼多雜質，但沒有找到他詳細的架構說明。
![](https://i.imgur.com/C2cHQVc.png) ![](https://i.imgur.com/8aUM8HR.png)

## Conclusion

### MUNIT

MUNIT是一個結合兩種圖片學習的方式，所綜合而成的圖片風格轉換。

首先是利用Multimodal讓給定一張的圖片可以生成多個圖片，再來把兩個不同dataset的圖片重複一起訓練，
透過pair instances學到共通特徵，然後經由隨機的一個數值來改變輸出，來達到一對多的結果。

這樣的方法比起HW1的cycleGAN，兩張對換圖的位置必須與最初要轉換的形象完全相同，而MUNIT的優點是不需要這些數據也可以完成任務。


