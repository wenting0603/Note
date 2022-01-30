# CNN
**做影像辨識**  

將圖片資訊縮剪的過程:
1. 特徵的大小比整張圖片小很多
2. 特徵會出現在不同區域
3. 對象縮小取樣不會影響到整個物件

![](https://i.imgur.com/Qm9F9cR.png)
 
 經過多次convolution+maxpooling後簡化圖片資訊

簡單來說:**CNN是簡化圖片後再送入DNN**  

## CNN-捲積(convolution)
1. 決定特徵值得filter
![](https://i.imgur.com/RxchHMo.png)

2. 圖片的資訊去比對
![](https://i.imgur.com/w6e2ba8.png)

![](https://i.imgur.com/ck3jIfz.png)

3. 捲積完後會從**6*6->4*4**  
如圖
數字越大越符合filter
![](https://i.imgur.com/CbsWBaT.png)

4. 有多少個filter就會進行多少次捲積 堆疊形成featuer map(立體狀)
![](https://i.imgur.com/mTTH8Kh.png)

**Convolution v.s. Fully Connected**
Convolution:
多對1，九宮格就對應到1個值  

Fully Connected:
會多對多，全部都要參一腳
![](https://i.imgur.com/TQusKQo.png)

## Max Pooling

```

```



