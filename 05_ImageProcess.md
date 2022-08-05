本章節紀錄影像處理手法: 模糊、銳化、臨界值、邊緣偵測、輪廓、侵蝕、膨脹與open-closing 這些手法(好多啊...)  

# 模糊(Blur)
模糊是指將圖像內每一個像素與相鄰的像素混合。 
類似CNN的stride window，我們可以定義kxk的滑動視窗，每種方法在這個kxk的window上都有自己的計算方式。 

## 平均模糊(Averaging Blur)
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv30.png)  
- 使用內建的cv2.blur(img , (k,k))  
- 平均模糊使用window中央像素周圍的所有像素平均。   
- 很顯然，當window越大，圖像處理過後會越模糊。  

平均模糊其實就是用以下的transformation:  
<img src="https://latex.codecogs.com/svg.image?\frac{1}{K_{weight}}\frac{1}{K_{height}}\begin{bmatrix}1&space;&&space;1&space;&&space;&space;1\\1&space;&&space;1&space;&&space;&space;1\\1&space;&&space;1&space;&&space;&space;1\\\end{bmatrix}" title="\frac{1}{K_{weight}}\frac{1}{K_{height}}\begin{bmatrix}1 & 1 & 1\\1 & 1 & 1\\1 & 1 & 1\\\end{bmatrix}" /> 
K_weight 和 K_height是window的寬和高


以下是執行結果(用hstack水平串接起來):  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv31.png)

## 中位數模糊(Median Blur)
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv32.png)  
- 使用cv2.medianBlur(img , k)  
- 中位數模糊使用window中央像素周圍的所有像素之中位數。   
- 這邊window不是給tuple，而是指定長度。  

以下是執行結果(用hstack水平串接起來):  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv33.png)

## 高斯模糊(Gaussian Blur)
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv34.png)  
- 使用cv2.GaussianBlur(img , (k,k) , SigmaX)  
- 高斯模糊使用加權平均，越靠近中央像素的權重越高。   
- SigmaX參數設定為0代表OpenCV將依照window width/height計算標準差，否則會依bivariate normal各自計算margin X與Y的標準差。  

以下是執行結果(用hstack水平串接起來):  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv35.png)  

## 雙邊濾波模糊(Bilateral Blur)
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv36.png)   
- 使用cv2.bilateralFilter(src, d, sigmaColor, sigmaSpace[, dst[, borderType]]	) ->	dst(這個dst就是過濾後的圖片變數)  
- 根據官方API文件解釋: d為過濾使用的每個像素鄰域的直徑。如果它是非正數，則從sigmaSpace計算。  
- 大過濾器（d > 5）會影響運算速度，因此建議實務中使用 d=5，對於需要大量噪聲過濾的離線應用，則建議使用d=9。  
- 雙邊濾波器可以很好地減少不需要的噪聲，同時保持邊緣相當清晰。 但與大多數過濾器相比運算速度非常慢。  
- 通常會將2個sigma值設置為相同。 如果設定很小（< 10），則濾鏡不會有太大的效果，如果很大（> 150），則產生的圖檔會很像卡通。  
- 第一個sigma值將像素相鄰區域內的顏色混合，使區域內的圖片均一色，越大代表能混合的像素值越寬。  
- 第二個sigma值影響相鄰的像素距離，越大代表越遠的"相似像素"會彼此影響。  

以下為執行結果:
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv37.png)  

# 銳化(Sharpening)
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv38.png)  
銳化的目的是將模糊的照片變得清晰。  
- 使用cv2.filter2D(img , depth , kernel)
- img為原圖，depth為設定處理結果的色彩深度(維持原圖色彩深度則設為-1)，kernel為銳化使用的2Darray。  

以下為執行結果:  
(原圖 vs 一般銳化sharp1)  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv39.png)  
(過度銳化sharp2 & sharp3)  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv40.png)  
(邊緣加強sharp4)  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv41.png)  

# 臨界值(Thresholding) 
臨界值就是對圖像進行二值化，像素值比閥值小就是0，反之則為255。  
透過臨界值可以將圖像焦點放在特定物件或區域上。  

## 簡單臨界值(Simple Threshold)
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv43.png) 
- 透過人為設定閥值來達成二值化。  
- 使用cv2.threshold(src, thresh, maxval, type[, dst]	) ->	retval, dst  
- 根據官方API文件定義，thresh為自行定義的閥值，maxval為像素強度最大值，type為二值化的算法。  
- type可使用的參數為以下:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv42.png)  

使用簡單臨界值，則選 cv2.THRESH_BINARY。  
注意該函數有2個回傳值，第一個retval是演算法得到的閥值，dst則為回傳圖像。  

執行結果如下:
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv44.png)  
須注意cv2.THRESH_BINARY_INV代表將原本的threshold結果當作mask，也就是原圖黑白互換。  

## 自適應臨界值(Adaptive Threshold)
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv45.png) 
- 根據每個像素的相鄰區域找出最佳二值化閥值。  
- 使用cv2.adaptiveThreshold(src, maxValue, adaptiveMethod, thresholdType, blockSize, C[, dst]	) ->	dst  
-  maxValue為像素最大強度值，adaptiveMethod為使用的演算法，thresholdType為二值化的算法，blocksize為選取計算neighbo的範圍。  
-  blocksize必須為奇數。  
-  臨界值種類只能是 cv2.THRESH_BINARY 或 cv2.THRESH_BINARY_INV 其中一個。  
-  adaptiveMethod可使用的參數為以下:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv47.png)  
平均法使用block的整體平均扣除常數C，而高斯法則是block內取加權平均後扣除常數C!  

執行結果如下:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv46.png) 
## 大津臨界值(Otsu Threshold)
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv48.png)  
該演算法假設圖像轉灰階後的像素直方圖有兩個peak，該方法主要在尋找分開兩peak的最佳臨界值T。  
- 使用cv2.threshold(img , thresh , maxval , cv2.THRESH_BINARY_INV+cv2.THRESH_OTSU)  
- thresh的位置可以隨意填值，因為我們已經指定otsu演算法了，所以人工閥值的位置給多少都不會影響結果!  

執行結果如下:
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv49.png)  

Otsu’s threshold: 96.0  
Otsu’s threshold after Gaussian blur: 97.0  

## Riddler-Calvard演算法
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv50.png)  
- 類似otsu，但需要安裝mahotas套件才能跑。  

執行結果如下:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv51.png)

Otsu’s threshold: 165  
Riddler-Calvard: 165.87138975004711  

# 邊緣偵測

## Sobel
## Laplacian
## Canny

# 輪廓

# 侵蝕

# 膨脹

# Opening-Closing
