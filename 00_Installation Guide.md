# Linux版本與環境設置
Ubuntu 22.04 LTS (ISO映像檔下載位置:https://releases.ubuntu.com/22.04/)  
VMWare 16  
OpenCV版本 4.6.0
虛擬機配置(請參照Linux repo內的Installation markdown file)  

# 前置套件安裝
每一個套件的安裝大致可分位以下三步驟:  
1. 切換管理員 sudo -i  
2. apt list 套件名稱，並按兩下Tab查看所有可能的套件  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/opencv_install00.png)
3. apt list 套件名稱，查看指定套件細節說明(例如:相依性套件、可能衝突的版本...etc)
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/opencv_install01.png)
4. apt install 套件名稱

過程中需要安裝的套件名稱如下(安裝不分先後順序):
1. pkg-config
2. git
3. cmake
4. upzip
5. libjpeg-dev
6. libpng-dev
7. libtiff5-dev
8. libopenjp2-7-dev
9. libopenexr-dev
10. install libavcodec-dev
11. libavformat-dev
12. libswscale-dev
13. libv4l-dev
14. libxvidcore-dev
15. libx264-dev
16. liblapacke-dev
17. libopenblas-dev
18. ibeigen3-dev
19. libhdf5-dev 
20. libgtk-3-dev 
21. libgstreamer-plugins-base1.0-dev 
22. libgstreamer-plugins-good1.0-dev 
23. libatlas-base-dev 
24. gfortran
25. libtbb-dev 
26. libtesseract-dev
27. libdc1394-dev 
28. libgphoto2-dev

接著要安裝pip3:  
`apt install python3-dev`  
`wget https://bootstrap.pypa.io/get-pip.py`  
`python3 get-pip.py`  
根據檔案階層式架構，自行安裝的套件都會在 `/usr/local/bin` 內  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/opencv_install07.png)


安裝完pip3之後，順便安裝numpy:  
`pip3 install numpy`  

同時到pypi下載imutil的tar.gz檔，丟到ubuntu後使用pip3安裝  
`pip3 install ~ubuntu/imutils-0.5.4.tar.gz`

# 下載opencv主檔案
1. 到opencv github(https://github.com/opencv/opencv/releases/tag/4.6.0)下載 source code，選擇zip檔案+wget方式下載  
2. 到opencv_contrib github(https://github.com/opencv/opencv_contrib/tags)下載 source code，同樣也選擇zip檔案+wget方式下載  
記得要下載的檔名變更，因為兩個檔案名稱都叫做opencv-4.6.0.zip ，使用 `wget -O 資料夾名稱 路徑`  

# 解壓縮source code
在使用者家目錄解壓縮兩個zip/tar.gz檔，記住以下步驟:  
1. unzip -l/tar -tvf 查看壓縮檔，可以查看代表檔案沒毀損，以及檔案是否有提供資料夾，否則解壓縮後一堆檔案會塞住你的家目錄!  
2. upzip/tar -xvf 解壓縮，記得解壓縮要換名字，因為兩個檔案解壓縮後的資料夾名稱是相同的!  
這邊會將原版的opencv解壓縮資料夾叫做opencv；另一個叫做opencv_contrib  

# 編譯 opencv
1. cd opencv
2. mkdir build
3. cd build
4. 執行cmake指令:
```
cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
    -D WITH_TBB=ON \
    -D WITH_OPENJPEG=ON \
    -D BUILD_TESTS=OFF \
    -D OPENCV_ENABLE_NONFREE=ON \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D BUILD_EXAMPLES=ON .. | tee ~/cmake.txt
```

注意如果是20.04版本的Linux，需要再加上一列 -D PYTHON3_PACKAGES_PATH=/usr/local/lib/python3.x/dist-packages \ 到倒數第2列  

5. 執行指令後，查看家目錄內的cmake.txt裡面的OpenBLAS是否有FOUND(注意不要離開當前資料夾!!) 
```
less ~/cmake.txt
```
進入這個txt檔案內，按下\搜尋指定字串 OpenBLAS  

6. 如果發現是NOT FOUND，那麼執行以下指令，編輯這份cmake檔  
```
nano ~/opencv/cmake/OpenCVFindOpenBLAS.cmake
```

7. 在以下區塊加入紅色框框的字串  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/opencv_install02.png)  
編輯好之後按下Ctrl+O + Enter 以及 Ctrl+X離開  

8. 從build資料夾回到上一層，移除build資料夾並重新create一個
```
cd ..
rm -r build/
mkdir build
cd build
```
9. 接著重新執行剛剛的cmake指令
```
cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
    -D WITH_TBB=ON \
    -D WITH_OPENJPEG=ON \
    -D BUILD_TESTS=OFF \
    -D OPENCV_ENABLE_NONFREE=ON \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D BUILD_EXAMPLES=ON .. | tee ~/cmake.txt
```
10. 執行完成後，確認虛擬機核心個數，然後以核心數進行編譯(假設nproc=12，則使用make -j12)
```
nproc
time make -j8
```
以下是執行完成的畫面  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/opencv_install05.png)  

11. 編譯結束後，以管理員身分執行make install，將程式庫複製到正確的路徑
```
sudo make install
```

12. 接著切換管理員並載入opencv程式庫，可以直接reboot，不想重開也可以執行以下指令
```
sudo -i
ldconfig
```

13. 載入後確認一下是否有找到opencv相關程式庫
```
ldconfig -p | grep -i 'opencv' | wc -l
```
沒有意外的話會看到110個檔案  
![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/opencv_install03.png)  

14. 接著使用Python互動式介面確認opencv安裝是否完成
```
python3
>>> import cv2
>>> cv2.__version__
>>> exit()
```

![Image](https://github.com/EnasVen/OpenCV-4.6.0-/blob/main/pics/opencv_install04.png)  
如果匯入套件沒有出現錯誤，那麼安裝到這邊就算完成了!  
