# 挖掘机小组：中期作业实验报告
## 小组成员：林蒙 曾欣科 张云霞 徐铭美 冯李逍 郑文银 程南江 陈宇 雷小唐 蒲文博（排名不分先后） 
## 一、目的
对绵阳出租车起点、终点坐标进行起点聚类、终点聚类和OD线聚类。
## 一、数据说明
一个csv文件，包含了起点经度、维度，还有终点经度、维度。  
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/csv%E6%96%87%E4%BB%B6.png)
## 二、聚类
### 1、起点聚类
#### 目标：对起点位置进行聚类   
#### 算法：使用PAM算法对坐标进行聚类。   
输入：簇的数目k和包含n个对象的数据库   
输出：k个簇，使得所有对象与其距离最近中心点的相异度总和最小   
（1） 任意选择k个对象作为初始的簇中心点    
（2） Repeat   
（3） 指派每个剩余对象给离他最近的中心点所表示的簇   
（4） Repeat   
（5） 选择一个未被选择的中心点Oi   
（6） Repeat   
（7） 选择一个未被选择过的非中心点对象Oh   
（8） 计算用Oh代替Oi的总代价并记录在S中   
（9） Until 所有非中心点都被选择过   
（10） Until 所有的中心点都被选择过   
（11） If 在S中的所有非中心点代替所有中心点后的计算出总代价有小于0的存在，then找出S中的用非中心点替代中心点后代价最小的一个，并用该非中心点替代对应的中心点，形成一个新的k个中心点的集合   
（12） Until 没有再发生簇的重新分配，即所有的S都大于0.
#### 代码：   
```{r}
install.packages("leaflet") # 安装地图
install.packages("cluster")

library(leaflet) # 加载地图
library(dplyr) # select函数
library(cluster)

# 读取经纬度,起点,终点
file_path = "C:/Users/lemon laptop/OneDrive/研究生/数据挖掘/中期作业及说明/df3.csv"
csv_data <- read.csv(file=file_path, header=TRUE)
start_point <- select(csv_data, 'Lng', 'Lat') # 取指定列
color = c("blue", "green", "red", "black", "orange", "pink", "purple", "white")

# 1、把起点位置显示在地图
global_map <- leaflet() # 生成一张地图
global_map <- addTiles(global_map) # 添加标题,必须有,否则不显示地图
global_map <- addMarkers(global_map, lng = ~Lng, lat = ~Lat, data = start_point)
global_map

# 2、对起点进行聚类
k = 2
start_point.pam <- pam(start_point, k = k)

# 3、聚类后画图，分颜色
global_map <- leaflet() # 生成一张地图
global_map <- addTiles(global_map) # 添加标题,必须有,否则不显示地图
for (i in 1:k) { # 遍历k个聚类
  icon = makeAwesomeIcon(icon= '', markerColor = color[i])
  global_map <- addAwesomeMarkers(global_map, lng = ~Lng, lat = ~Lat, icon = icon,
                                  data = start_point[start_point.pam$clustering==i,])
}
global_map
```
#### 结果：
起点分布图：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E8%B5%B7%E7%82%B9.png)
PAM聚类，k=2：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E8%B5%B7%E7%82%B9%E8%81%9A%E7%B1%BB%20k%3D2.png)      
PAM聚类，k=3：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E8%B5%B7%E7%82%B9%E8%81%9A%E7%B1%BB%20k%3D3.png)   
PAM聚类，k=4：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E8%B5%B7%E7%82%B9%E8%81%9A%E7%B1%BB%20k%3D4.png)   
PAM聚类，k=5：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E8%B5%B7%E7%82%B9%E8%81%9A%E7%B1%BB%20k%3D5.png)   
PAM聚类，k=6：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E8%B5%B7%E7%82%B9%E8%81%9A%E7%B1%BB%20k%3D6.png)   
PAM聚类，k=7：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E8%B5%B7%E7%82%B9%E8%81%9A%E7%B1%BB%20k%3D7.png)   
#### 分析：   
当k=3时聚类比较合理，坐标点分布相对集中。   
红色点最多，主要分布在涪城区和游仙区，这是绵阳市的中心区域，发展比较好。人们主要在这里居住，人口密度大，出行要求自然就多些。
客流量大，并且多为市内出行。   
蓝色点次之，散落在绵阳的西边，西边主要是其他城市到绵阳的线路，所以坐标点主要分布在公路周围。乘客可能从这些位置上车到绵阳。   
绿色点最少，位于绵阳南边。人流量较少。

### 2、终点聚类
#### 目标：对终点坐标进行聚类   
#### 算法：PAM，同上
#### 代码：
```{r}
install.packages("leaflet") # 安装地图
install.packages("cluster")

library(leaflet) # 加载地图
library(dplyr) # select函数
library(cluster)

# 读取经纬度,起点,终点
file_path = "C:/Users/lemon laptop/OneDrive/研究生/数据挖掘/中期作业及说明/df3.csv"
csv_data <- read.csv(file=file_path, header=TRUE)
end_point <- select(csv_data, 'Lng_e', 'Lat_e') # 取指定列
color = c("blue", "green", "red", "black", "orange", "pink", "purple", "white")

# 1、把位置显示在地图
global_map <- leaflet() # 生成一张地图
global_map <- addTiles(global_map) # 添加标题,必须有,否则不显示地图,必须有返回 <-
global_map <- addMarkers(global_map, lng = ~Lng_e, lat = ~Lat_e, data = end_point)
global_map

# 2、进行聚类
k = 7
end_point.pam <- pam(end_point, k = k)

# 3、聚类后画图，分颜色
global_map <- leaflet() # 生成一张地图
global_map <- addTiles(global_map) # 添加标题,必须有,否则不显示地图
for (i in 1:k) { # 遍历k个聚类
  icon = makeAwesomeIcon(icon= '', markerColor = color[i])
  global_map <- addAwesomeMarkers(global_map, lng = ~Lng_e, lat = ~Lat_e, icon = icon,
                                  data = end_point[end_point.pam$clustering==i,])
}
global_map
```
#### 结果：
终点分布图：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E7%BB%88%E7%82%B9.png)   
PAM聚类，k=2，聚类结果如下：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E7%BB%88%E7%82%B9%E8%81%9A%E7%B1%BB%20k%3D2.png)
PAM聚类，k=3，聚类结果如下：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E7%BB%88%E7%82%B9%E8%81%9A%E7%B1%BB%20k%3D3.png)
PAM聚类，k=4，聚类结果如下：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E7%BB%88%E7%82%B9%E8%81%9A%E7%B1%BB%20k%3D4.png)
PAM聚类，k=5，聚类结果如下：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E7%BB%88%E7%82%B9%E8%81%9A%E7%B1%BB%20k%3D5.png)
PAM聚类，k=6，聚类结果如下：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E7%BB%88%E7%82%B9%E8%81%9A%E7%B1%BB%20k%3D6.png)
PAM聚类，k=7，聚类结果如下：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E7%BB%88%E7%82%B9%E8%81%9A%E7%B1%BB%20k%3D7.png)
#### 分析：

### 3、OD线聚类
#### 目标：对OD线进行聚类   
#### 算法：PAM算法，同上   
#### 代码：
```{r}
install.packages("leaflet") # 安装地图
install.packages("cluster")

library(leaflet) # 加载地图
library(dplyr) # select函数
library(cluster)

# 读取经纬度,起点,终点
file_path = "C:/Users/lemon laptop/OneDrive/研究生/数据挖掘/中期作业及说明/df3.csv"
csv_data <- read.csv(file=file_path, header=TRUE)
line_data <- select(csv_data, 'Lng', 'Lat', 'Lng_e', 'Lat_e') # 取指定列
color = c("blue", "green", "red", "black", "orange", "pink", "purple", "white")

# 1、把位置显示在地图
global_map <- leaflet() # 生成一张地图
global_map <- addTiles(global_map) # 添加标题,必须有,否则不显示地图,必须有返回 <-
icon.blue <- makeAwesomeIcon(icon= '', markerColor = 'blue')
icon.red <- makeAwesomeIcon(icon= '', markerColor = 'red')
global_map <- addAwesomeMarkers(global_map, lng = ~Lng_e, lat = ~Lat_e, icon = icon.red, data = line_data)
global_map <- addAwesomeMarkers(global_map, lng = ~Lng, lat = ~Lat, icon = icon.blue, data = line_data)
global_map
n <- as.numeric(rownames(line_data))
for (i in n) {
  global_map <- addPolylines(global_map, lng = c(line_data[i,1], line_data[i,3]), lat = c(line_data[i,2], line_data[i,4]), weight = 1)
}
global_map

# 2、进行聚类
k = 4
line_data.pam <- pam(line_data, k = k)

# 3、聚类后画图，分颜色
global_map <- leaflet() # 生成一张地图
global_map <- addTiles(global_map) # 添加标题,必须有,否则不显示地图,必须有返回 <-
n <- as.numeric(rownames(line_data))
for (i in n) {
  global_map <- addPolylines(global_map, lng = c(line_data[i,1], line_data[i,3]), lat = c(line_data[i,2], line_data[i,4]), weight = 2, color = color[line_data.pam$clustering[i]])
}
global_map
```
#### 结果：   
OD线分布图：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E7%BA%BF%20%E6%9C%AA%E5%88%86%E7%B1%BB%20%E5%85%88%E7%94%BB%E8%B5%B7%E7%82%B9%E5%90%8E%E7%94%BB%E7%BB%88%E7%82%B9%20%E6%9C%89%E8%A6%86%E7%9B%96%E7%8E%B0%E8%B1%A1.png)
PAM聚类，k=2，聚类结果如下：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E7%BA%BF%E8%81%9A%E7%B1%BB%20k%3D2.png)
PAM聚类，k=3，聚类结果如下：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E7%BA%BF%E8%81%9A%E7%B1%BB%20k%3D3.png)
PAM聚类，k=4，聚类结果如下：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E7%BA%BF%E8%81%9A%E7%B1%BB%20k%3D4.png)
#### 分析：   

