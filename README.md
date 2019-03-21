# CVFX-Team8-HW2

# HW2-Style-Transfer

## Training MUNIT
#### 1. 照原本的參數train
![](https://i.imgur.com/ULOo0uc.png)  
然而我們發現，train到後面(190k iteration)時，圖片的效果反而開始下降了，因此打算試試看用其他參數重新train一個看看
#### 2. batch size = 3
![](https://i.imgur.com/kJdyPqO.png)  
由於記憶體限制，我們只能將batch size調高到3試試看，期望能夠有更好的效果

---
## Inference images in multiple style



---
## Compare with other methods
### 1. Neural-Style Transfer
* 論文參考自這篇：[A Neural Algorithm of Artistic Style](https://arxiv.org/pdf/1508.06576v2.pdf)
* 論文中有說明如何使用CNN進行 **圖片風格轉換 (Style Transfer)**，非常適合進行 **畫家風格轉換**，因為作者模擬 **目標圖的顏色、紋理**，就可以讓人感覺是某種畫風

    - 對原圖(Content Image)取出 **Content**
    - 對風格圖(Style Image)取出 **Style**
    - 兩者加以合成，即可達到 **Style Transfer** 效果

#### 詳細原理如下：
* 使用**VGG19**，分別對原圖及風格圖萃取特徵

    - 16 Convolutional Neural Network
    - 3 Fully Connected Layers
    - 5 Average-Pooling

        - Replace Max-Pooling
        - Improve the gradient flow
        - 架構圖簡易如下：
        ![](https://i.imgur.com/1DBY1V0.png)

* 定義 **原圖 content loss function**: **原圖與合成圖的特徵差距**

    ![](https://i.imgur.com/0Q6zTs3.png)
    - P 是原圖內容的特徵向量
    - F 是合成圖內容的特徵向量

* 定義 **合成圖 style loss function**: **風格圖與合成圖的特徵差距** 

    - 作者找到一個公式，較能接近的反應畫風的差距
    - A 是原圖風格的特徵向量
    - G 是合成圖風格的特徵向量
    ![](https://i.imgur.com/ugSCcd9.png)
    ![](https://i.imgur.com/t8f5hoB.png)
    

* 最後，**計算 Total Loss = Content loss + Style Loss**

    - 最小化Total Loss

        - 採取**梯度下降法**
        - 對 content 偏微分:
        - ![](https://i.imgur.com/I5zoiCl.png)
        - 對 style 偏微分:
        - ![](https://i.imgur.com/UvSHNyB.png)

    - 求出合成圖的的特徵向量
    - 然後再還原影像

* 程式碼參考自 [neural-style](https://github.com/anishathalye/neural-style)
* 補充：

    - 使用 VGG19 與 VGG16 並無太大的差異，如果想要縮短執行時間，可以改用 VGG16 。此法還是有改善空間，比較好的方式是改變它的演算法，因此，後來有很多論文都在討論如何快速產生合成圖


---
## Conclusion



---
