記錄圖像的一些基本操作，如:縮放、平移、旋轉、翻轉、擷取、位元運算與遮罩。  

# 圖像縮放 + 圖像平移
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv09.png)
- 使用cv2.warpAffine(src , M ,dsize [, dst [, flags [, borderMode[, borderValue]]]] )  
- 前三個必要參數分別為: 圖檔路徑、平移矩陣與圖像大小  
- 注意圖像大小的順序是(cols , rows)，而這兩個參數分別為原圖的第一維度(opencv內圖片的y方向)、第二維度(opencv內圖片x方向)長度  
- 平移矩陣是2 by 3 的理由:  
我們在2為平面上要做點座標的變換，以矩陣型態表示就會試以下的形式:
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\begin{bmatrix}m&space;&&space;0&space;&space;\\0&space;&&space;n&space;&space;\\\end{bmatrix}\begin{bmatrix}x&space;\\y\end{bmatrix}&plus;\begin{bmatrix}p&space;\\q\end{bmatrix}=\begin{bmatrix}mx&plus;p&space;\\ny&plus;q\end{bmatrix}" title="\begin{bmatrix}m & 0 \\0 & n \\\end{bmatrix}\begin{bmatrix}x \\y\end{bmatrix}+\begin{bmatrix}p \\q\end{bmatrix}=\begin{bmatrix}mx+p \\ny+q\end{bmatrix}" />  

因此，如果要寫成linear transformation : Ax的形式，就必須改寫成以下:  
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\begin{bmatrix}m&space;&&space;0&space;&&space;p&space;\\0&space;&&space;n&space;&&space;q&space;\\\end{bmatrix}\begin{bmatrix}x&space;\\y&space;\\&space;1\end{bmatrix}=\begin{bmatrix}mx&plus;p&space;\\ny&plus;q\end{bmatrix}" title="\begin{bmatrix}m & 0 & p \\0 & n & q \\\end{bmatrix}\begin{bmatrix}x \\y \\ 1\end{bmatrix}=\begin{bmatrix}mx+p \\ny+q\end{bmatrix}" />

注意以上的m,n即為縮放比例，而p,q即為平移距離!  
平移後的圖片如下:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv10.png)  
可以看到上圖的左方和上方因為平移而產生黑色區域!

# 圖像旋轉
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv11.png)  
- 使用cv2.getRorarionMatrix2D(center , angle , scale)來建立旋轉矩陣。  
- 前三個變數分別為: 原圖的中心點座標、旋轉角度以及縮放比例。  

根據官方API文件，我們使用圖片的(cols-1)/2 , (rows-1)/2來求取圖片中心座標。
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/cv14.png)  
求得的圖片中心和原圖shape如下:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/cv13.png)

注意二維平面上的點，使用以下旋轉矩陣進行transformation:  
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\begin{bmatrix}cos(\theta)&space;&&space;-sin(\theta)&space;\\sin(\theta)&space;&&space;cos(\theta)&space;\\\end{bmatrix}" title="\begin{bmatrix}cos(\theta) & -sin(\theta) \\sin(\theta) & cos(\theta) \\\end{bmatrix}" />  

旋轉後的圖片如下:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/cv12.png)

接著我們demo一下將原圖旋轉90和180度的圖:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv15.png)  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv16.png)  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv17.png)  

OpenCV提供一個rotate()函數可以指定旋轉方向與角度，並且**不會出現因旋轉產生的黑底**!  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/cv19.png)  
語法是加入**cv.ROTATE_(角度)_(順/逆時鐘)** 引數。  

以下是執行結果:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv18.png)

# 圖像翻轉
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv20.png)
- 使用cv2.flip(img , rotate_flag)  
- img為指定要處理的圖像、rotate_flag為水平或垂直翻轉，0為水平；1為垂直。  

以下是執行結果:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv21.png)  

# 圖像擷取
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv22.png)  
- 使用array的索引方式對圖像做擷取。  

以下是執行結果:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv23.png)  

# 圖像位元運算
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv24.png)  
- OpenCV支持4種位元運算: AND,OR,XOR,NOT  
- 運算的方式為二進位(例如: 0100 AND 1011 = 0000)  
- 運算結果若為0，則關閉該pixel；否則開啟該pixel。  
- 使用cv2.bitwise_and(img1 , img2)進行運算。(or/xor/not就替換掉and字眼)  

以下為執行結果:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv25.png) 

需注意白色像素點為255，換成二進位就是11111111，而黑色像素點為0，換成二進位就是00000000。  
因此，以XOR運算來說，兩個bit要不同值才會是1，例如: 0101 xor 0111 = 0010  

# 圖像遮罩
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv26.png) 
- 遮罩技術(masking)可以過濾我們感興趣的圖像區域。  
- 同樣使用cv2.bitwise_and(img1 , img2 , mask)，只是加入mask參數。
- mask參數使用均為0的陣列(全黑色)進行過濾，根據bitwise運算可以輕易地得到過濾的結果。(0和1運算仍為0)  

以下為執行結果:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv27.png) 
