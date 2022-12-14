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
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv54.png)  
用來尋找圖像中像素亮度發生明顯變化的地方。  

## Sobel  
- 利用偏微分尋找圖像亮度的梯度近似值。  
- 使用cv2.Sobel(img , ddepth , dx ,dy)  
- ddepth為色彩深度，例如:cv2.CV_64F(注意給-1則代表與原圖相同色彩深度!)  
- dx, dy 則代表要對X或Y方向做幾階偏微分(注意圖像X軸是第2維度，也就是column方向! axis=1)  

以下為執行結果:
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv53.png)  

注意:  
1) 可使用cv2.addWeighted()來對X,Y方向偏微分結果作加權平均!  
2) 轉換圖像色彩深度可透過 cv2.convertScaleAbs()函數完成!  

## Laplacian
- 利用二階微分運算尋找圖像邊緣  
- 使用cv2.Laplacian(img , ddepth)  

以下為執行結果:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv52.png)  

## Canny
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv55.png)  
- 透過"邊緣檢測計算理論"手法完成  
- 將圖像模糊以降低雜訊  
- 沿X和Y軸計算Sobel圖像梯度  
- 套用NMS(Non-Maximum Suppression)，刪除非邊緣的像素，僅保留candidate邊緣細線  
- 透過threshold確定像素是否為邊緣  
- 使用cv2.Canny(img , threshold1, threshold2)  
- 必須提供兩個梯度值，任何梯度大於threshold2的像素都被視為邊緣 ; 而梯度小於threshold1的像素將被視為非邊緣  
- 介於中間的則根據像素強度來決定是否為邊緣!  

以下為執行結果:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv56.png)  

# 輪廓
輪廓可解釋為連接具有相同顏色或像素強度的所有連續點之曲線。  
由於使用二元圖像會得到更佳的準確性，因此尋找輪廓之前會先套用Canny邊緣偵測技術做遇處理。  
從OpenCV3.2版之後，findContours()函數不會再變更原始圖像了!  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv57.png)  

- 使用cv2.findContours(img , mode , method)尋找輪廓  
- mode為輪廓的取得模式
  > cv2.RETR_EXTERNAL為擷取最外面的輪廓  
  > cv2.RETR_LIST為取得所有輪廓  
- method為求取輪廓的方法
  > cv2.CHAIN_APPROX_SIMPLE 只會保留端點  
  > cv2.CHAIN_APPROX_NONE 則會取得所有座標，比較吃資源  
- findContours()函數回傳list型別物件!  


- 使用cv2.drawContours(img , contours , contourldx  ,color)繪製輪廓  
- contours參數為上面回傳的list，contourldx代表要繪製第幾個輪廓(給-1就是全部)，color則為輪廓線條顏色  

以下為執行結果:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv58.png)  

# 侵蝕與膨脹
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv59.png)  
- 縮減前景物件邊界。  
- 使用kernel(類似CNN的filter)在圖像中滑動，當kernel內所有像素都為1時，原始圖像的像素才會被視為1(255，白色)，否則為0(0，黑色)。  
- 通常使用侵蝕來消除雜訊，對於分離兩個連接的物件非常有用!  
- 使用cv2.erode(img , kernel , iterations=1)  

- 與侵蝕相反的操作，用來膨脹物件邊界。  
- 當kernel內有任何像素為1時，原始圖像的像素就為1(255，白色)，否則為0(0，黑色)。  
- 對於連接物件的斷裂部分有奇效!  
- 使用cv2.dilation(img , kernel , iterations=1)  

以下為執行結果:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv60.png)  

# Opening-Closing
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv61.png)  
- opening = 先侵蝕再擴張，對於消除背景雜訊很有幫助!  
- 使用cv2.morphologyEx(img , cv.MORPH_OPEN , kernel)  
- closing = 先擴張再侵蝕，對於清除物件上的雜訊很有幫助!  
- 使用cv2.morphologyEx(img , cv.MORPH_CLOSE , kernel)  

以下為執行結果:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv62.png)  
