記錄圖像的一些基本操作，如:縮放、平移、旋轉、翻轉、擷取、位元運算與遮罩。  

# 圖像縮放 + 圖像平移
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/cv09.png)
- 使用cv2.warpAffine(src , M ,dsize [, dst [, flags [, borderMode[, borderValue]]]] )  
- 前三個必要參數分別為: 圖檔路徑、平移矩陣與圖像大小  
- 注意圖像大小的順序是(cols , rows)，而這兩個參數分別為原圖的第一維度(opencv內圖片的y方向)、第二維度(opencv內圖片x方向)長度  
- 平移矩陣是2 by 3 的理由:  
我們在2為平面上要做點座標的變換，以矩陣型態表示就會試以下的形式:
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\begin{bmatrix}m&space;&&space;0&space;&space;\\0&space;&&space;n&space;&space;\\\end{bmatrix}\begin{bmatrix}x&space;\\y\end{bmatrix}&plus;\begin{bmatrix}p&space;\\q\end{bmatrix}=\begin{bmatrix}mx&plus;p&space;\\ny&plus;q\end{bmatrix}" title="\begin{bmatrix}m & 0 \\0 & n \\\end{bmatrix}\begin{bmatrix}x \\y\end{bmatrix}+\begin{bmatrix}p \\q\end{bmatrix}=\begin{bmatrix}mx+p \\ny+q\end{bmatrix}" />  
因此，如果要寫成linear transformation : Ax的形式，就必須改寫成以下:  
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\begin{bmatrix}m&space;&&space;0&space;&&space;p&space;\\0&space;&&space;n&space;&&space;q&space;\\\end{bmatrix}\begin{bmatrix}x&space;\\y&space;\\&space;1\end{bmatrix}=\begin{bmatrix}mx&plus;p&space;\\ny&plus;q\end{bmatrix}" title="\begin{bmatrix}m & 0 & p \\0 & n & q \\\end{bmatrix}\begin{bmatrix}x \\y \\ 1\end{bmatrix}=\begin{bmatrix}mx+p \\ny+q\end{bmatrix}" />

注意以上的m,n即為縮放比例，而p,q即為平移距離!  

# 圖像旋轉



# 圖像翻轉



# 圖像擷取



# 圖像位元運算



# 圖像遮罩
