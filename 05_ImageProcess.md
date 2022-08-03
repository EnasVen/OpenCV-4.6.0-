本章節紀錄影像處理手法: 模糊、銳化、臨界值、邊緣偵測、輪廓、侵蝕、膨脹與open-closing 這些手法(好多啊...)  

# 模糊(Blur)
模糊是指將圖像內每一個像素與相鄰的像素混合。 
類似CNN的stride window，我們可以定義kxk的滑動視窗，每種方法在這個kxk的window上都有自己的計算方式。 

## 平均模糊(Averaging Blur)
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/cv30.png)  
- 使用內建的cv2.blur(img , (k,k))  
- 平均模糊使用window中央像素周圍的所有像素平均。   
- 很顯然，當window越大，圖像處理過後會越模糊。  

以下是執行結果(用hstack水平串接起來):  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/cv31.png)

## 中位數模糊(Median Blur)
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/cv32.png)  
- 使用cv2.medianBlur(img , k)  
- 中位數模糊使用window中央像素周圍的所有像素之中位數。   
- 這邊window不是給tuple，而是指定長度。  

以下是執行結果(用hstack水平串接起來):  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/cv33.png)

## 高斯模糊(Gaussian Blur)
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/cv34.png)  
- 使用cv2.GaussianBlur(img , (k,k) , flag)  
- 高斯模糊使用加權平均，越靠近中央像素的權重越高。   
- flag參數代表。  

以下是執行結果(用hstack水平串接起來):  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/cv35.png)

## 雙邊濾波模糊(Bilateral Blur)

# 銳化(Sharpening)

# 臨界值(Thresholding)

## 簡單臨界值(Simple Threshold)
## 自適應臨界值(Adaptive Threshold)
## 大津臨界值(Otsu Threshold)
## Riddler-Calvard演算法


# 邊緣偵測
## Sobel
## Laplacian
## Canny

# 輪廓

# 侵蝕

# 膨脹

# Opening-Closing
