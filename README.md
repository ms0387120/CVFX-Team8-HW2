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
#### Winter to Summer
![](https://i.imgur.com/gWGEzbB.jpg)

#### Summer to Winter
![](https://i.imgur.com/chQ6AJS.jpg)


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
        - ![](https://i.imgur.com/1DBY1V0.png)

* 定義 **原圖 content loss function**: **原圖與合成圖的特徵差距**

    - ![](https://i.imgur.com/0Q6zTs3.png)
    - P 是原圖內容的特徵向量
    - F 是合成圖內容的特徵向量

* 定義 **合成圖 style loss function**: **風格圖與合成圖的特徵差距** 

    - 作者找到一個公式，較能接近的反應畫風的差距
    - ![](https://i.imgur.com/ugSCcd9.png)
    - ![](https://i.imgur.com/t8f5hoB.png)
    - A 是原圖風格的特徵向量
    - G 是合成圖風格的特徵向量

* 最後，**計算 Total Loss = Content loss + Style Loss**

    - 最小化Total Loss

        - 採取**梯度下降法**
        - 對 content 偏微分:
        - ![](https://i.imgur.com/I5zoiCl.png)
        - 對 style 偏微分:
        - ![](https://i.imgur.com/UvSHNyB.png)

    - 求出合成圖的的特徵向量
    - 然後再還原影像

>補充：使用 VGG19 與 VGG16 並無太大的差異，如果想要縮短執行時間，可以改用 VGG16 。此法還是有改善空間，比較好的方式是改變它的演算法，因此，後來有很多論文都在討論如何快速產生合成圖

* 程式碼參考自 [neural-style](https://github.com/anishathalye/neural-style)

* 測試如下：

#### summer2winter
![](https://i.imgur.com/0VWy3q1.jpg)

* Neural-Style 的效果不是很好，除了第二張比較好，其餘的某些物件（比方說：天空、樹林）都會不自然。但畫風特色的確明顯展現出來
* 由於 MUNIT效果也沒說好到哪裡去，只有content2的所有風格轉換的皆恰當。但兩者無法斷定哪個比較好看

#### winter2summer
![](https://i.imgur.com/uyv92NX.jpg)

* Neural-Style 的效果不是很好，除了第三張比較好，其餘的某些物件（比方說：天空、樹林）都會不自然。但畫風特色的確明顯展現出來。**但 winter2summer的效果比summer2winter還要差**
* MUNIT的 Style1、3 轉換的都還算恰當
* **在winter2summer，MUNIT 比 Neural-Style 效果好很多**


---
### 2. FastPhotoStyle
* 論文參考自這篇：[A Closed-form Solution to Photorealistic Image Stylization](https://arxiv.org/pdf/1802.06474.pdf)
* 主要分成兩大步驟：(如圖所示)
![](https://imgur.com/5s71Mnk.png)


* 程式碼參考自 [FastPhotoStyle](https://github.com/NVIDIA/FastPhotoStyle)

* 測試如下：

#### summer2winter
![](https://imgur.com/FIcbxeW.jpg)

* 不同於 Neural-Style 的將圖片轉化為"繪畫"風格，FastPhotoStyle在風格轉換上比較有真實感。
* 從上圖的比較也可以發現，適當的style圖片選擇(整體的色調挑選)，能大大增加轉化出的真實性。
* 雖然FastPhotoStyle的效果沒有到很好，但如果嚴格要跟MUNIT相比，FastPhotoStyle在成像的細緻度上，來得高一些。

#### winter2summer
![](https://imgur.com/gOOQcDP.jpg)

* FastPhotoStyle在做風格轉換時，好像更強調色彩的捕捉，所以再轉換為夏日時，抓取天空、樹木的色彩為主，不過仍有相當大的進步空間。
* **winter2summer，FastPhotoStyle比MUNIT整體上更有夏日的感覺**

---
## Conclusion

* **MUNIT**

    - 優點

        - 可以**生成一對多風格**，有別於以往的方法
        - winter2summer的表現比Neural-Style方法好

    - 缺點

        - 或許是因為paper著重於產生不同風格的style，並沒有使用太多技巧來訓練GAN和修改細節，因此圖片效果和訓練的穩定性都不是很好

* **Neural-Style**

    - 優點

        - **適合畫風轉換**

    - 缺點

        - 某些物件（比方說：天空、樹林）表現的不自然
        - 由於使用VGG，合成的時間略久

* **FastPhotoStyle**

    - 優點

        - A

    - 缺點

        - B


---
