首先import套件並創造一個黑底畫布(其實就是一個全為0的ndarray)  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv01.png)


# 繪製直線
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv02.png)
- 使用cv2.line(img , pt1 , pt2 , color [, thickness [,linetype]])  
- 前4個為必要參數，分別代表圖像變數、起始座標、終點座標、線條顏色  
- 粗細與線條樣式可不給，根據官網API文件，預設為1和LINE_8  

# 繪製矩形
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv03.png)
- 使用cv2.rectangle(img , pt1 , pt2 , color [,thickness [,lineType]])  
- 前4個為必要參數，分別代表圖像變數、左上座標、右下座標、線條顏色  

# 繪製圓形
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv04.png)
- 使用cv2.circle(img , center , radius , color [,thickness [,lineType]])  
- 前4個為必要參數，分別代表圖像變數、圓中心座標、圓半徑、線條顏色  
- 其中thickness輸入-1代表填滿色彩!  

# 繪製橢圓
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv05.png)
- 使用cv2.ellipse(img , center , axes , angle , startAngle , endAngle , color [, thickness [, lineType]])  
- 前7個為必要參數，分別代表圖像變數、橢圓中心、長短軸(以tuple表示)、傾斜角度(長軸與x軸夾角)、橢圓區域初始角度(以x軸出發順時鐘)、橢圓區域結束角度(以x軸出發順時鐘)以及線條顏色  
  
# 繪製多邊形
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv06.png)
- 使用np.array建立二維陣列，維度為(n,2)，代表座標平面上的n個點座標位置。  
- 將二維陣列reshape成三維陣列，其中第一個維度使用-1來自動指定，後面2個維度設定為1,2  
- 接著使用cv2.polylines(img , [pts] , bool  , (B,G,R))來繪製多邊形。  
- 前4個變數分別為: 圖像變數、頂點陣列、是否closed(也就是說是否要把多邊形邊長圍起來)以及多邊形邊長顏色。  

# 顯示文字
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv07.png)  
- 字型根據官方API文件設定  
- 使用cv2.putText(img , str , pt1 , font ,font_size , font_color , font_thickness , lineType )  
- 須注意文字起算座標位置和圖像不同，putText起算位置在左下。  
- 前8個變數分別為: 圖像變數、呈現文字(使用字串)、左下座標(使用tuple)、字型、字體大小、字體顏色、字體粗細、線條格式  

上述執行後的結果如下圖:  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/cv08.png) 
