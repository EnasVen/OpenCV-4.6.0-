# Pixel
- 所有圖像都是由像素(Pixel)集合而成  
- 像素可表達兩種資料: 
  1. 灰階(grayscale)  
  2. 顏色(color)

# 灰階圖像
- 灰階圖像內，每一個像素值為0到255  
- 0代表黑色；255代表白色(越接近0越暗)

# 彩色圖像
- 彩色圖像一般是以RGB順序表達，R(紅)G(綠)B(藍)為光的三原色。  
- 在OpenCV內，色彩順序以BGR呈現，而非RGB  
- 以tuple(R,G,B)表達彩色像素數值，每一個維度代表該色彩強度，值在0~255

# 讀取圖檔
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/cv00.png)
使用cv2.imread()函數來讀取圖檔，其語法為:  
```
cv2.imread(filename [, flag] )
```

第二個參數可不給，預設為彩色圖像。  
根據官方API，這邊有很多種參數，常用如以下:
- cv.IMREAD_COLOR : 預設值，代表載入彩色圖像，任何透明度都會被忽略。  
- cv.IMREAD_GRAYSCALE : 以灰階模式載入圖像。  
- cv.IMREAD_UNCHANGED : 依原格式載入圖像，包含透明度、色彩通道...etc

# 顯示圖像
- 使用cv2.imshow(name , mat)呈現圖片。  
- 第一個參數為視窗標題。  
- 第二個參數為被呈現的圖片變數。  
- 可以建立多個圖像視窗，但標題要不同。  

# 等待時間
- 使用cv2.waitKey([delay])來進行等待。  
- delay參數的時間單位為毫秒，根據官方API文件，預設值為0。  
- 在等待時間內如果有按下任何按鈕或者倒數時間到，則程式繼續往下執行。  
- 若參數小於等於0，則會一直等到使用者按下按鍵為止。  

# 關閉圖像
- 使用cv2.destroyAllWindows()來關閉圖像。  
- 如果只想要關閉特定圖像，則需要使用 cv2.destroyWindow(name)來關閉，其中name引數為該視窗的標題!  
- .py程式執行結束後會自動釋放window，但如果該程式是被其他程式所呼叫的(callback)則不會自動關閉視窗。所以最好還是手動加上去關閉的指令!  

# 儲存圖像
- 使用cv2.imwrite(filename , img [, params])來儲存圖片。  
- 第一個參數為要儲存的檔名。  
- 第二個參數為圖片變數。  
- 第三個參數可傳遞key-value的對應關係(例如:cv.IMWRITE_JPEG_QUALITY)  
