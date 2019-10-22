# 挖掘机小组：中期作业实验报告
## 小组成员：林蒙 曾欣科 张云霞 徐铭美 冯李逍 郑文寅 程南江 陈宇 雷小唐 蒲文博（排名不分先后） 
## 一、目的
对绵阳出租车起点、终点坐标进行起点聚类、终点聚类和OD线聚类。
## 一、数据说明
一个csv文件，包含了起点经度、维度，还有终点经度、维度。  
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/csv%E6%96%87%E4%BB%B6.png)
## 二、聚类
### 1、起点聚类
目标：对起点位置进行聚类   
算法：
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

# 1、把起点位置显示在地图
start_point <- select(point_data, 'Lng', 'Lat') # 取指定列
global_map <- leaflet() # 生成一张地图
global_map <- addTiles(global_map) # 添加标题,必须有,否则不显示地图
addMarkers(global_map, lng = ~Lng, lat = ~Lat, data = start_point)

# 2、对起点进行聚类
start_point.pam <- pam(start_point, k = 3)

# 3、聚类后画图，分颜色
icon.blue <- makeAwesomeIcon(icon= '', markerColor = 'blue')
icon.green <- makeAwesomeIcon(icon= '', markerColor = 'green')
icon.red <- makeAwesomeIcon(icon= '', markerColor = 'red')
global_map <- addAwesomeMarkers(global_map, lng = ~Lng, lat = ~Lat, icon = icon.blue, data = start_point[start_point.pam$clustering==1,])
global_map <- addAwesomeMarkers(global_map, lng = ~Lng, lat = ~Lat, icon = icon.green, data = start_point[start_point.pam$clustering==2,])
addAwesomeMarkers(global_map, lng = ~Lng, lat = ~Lat, icon = icon.red, data = start_point[start_point.pam$clustering==3,])
```
起点分布图：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E8%B5%B7%E7%82%B9.png)   
用k-means进行聚类，k=3，聚类结果如下：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E8%B5%B7%E7%82%B9%E8%81%9A%E7%B1%BB.png)
### 2、终点聚类
目标：对终点坐标进行聚类
算法：
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

# 1、把起点位置显示在地图
start_point <- select(point_data, 'Lng', 'Lat') # 取指定列
global_map <- leaflet() # 生成一张地图
global_map <- addTiles(global_map) # 添加标题,必须有,否则不显示地图

addMarkers(global_map, lng = ~Lng, lat = ~Lat, data = start_point)

# 2、对起点进行聚类
start_point.pam <- pam(start_point, k = 3)

# 3、聚类后画图，分颜色
icon.blue <- makeAwesomeIcon(icon= '', markerColor = 'blue')
icon.green <- makeAwesomeIcon(icon= '', markerColor = 'green')
icon.red <- makeAwesomeIcon(icon= '', markerColor = 'red')
global_map <- addAwesomeMarkers(global_map, lng = ~Lng, lat = ~Lat, icon = icon.blue, data = start_point[start_point.pam$clustering==1,])
global_map <- addAwesomeMarkers(global_map, lng = ~Lng, lat = ~Lat, icon = icon.green, data = start_point[start_point.pam$clustering==2,])
addAwesomeMarkers(global_map, lng = ~Lng, lat = ~Lat, icon = icon.red, data = start_point[start_point.pam$clustering==3,])
```
终点分布图：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E7%BB%88%E7%82%B9.png)   
用k-means进行聚类，k=3，聚类结果如下：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E7%BB%88%E7%82%B9%E8%81%9A%E7%B1%BB.png)
### 3、OD线聚类
目标：对OD线进行聚类
算法：
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
line_data.pam <- pam(line_data, k = 3)

# 3、聚类后画图，分颜色
global_map <- leaflet() # 生成一张地图
global_map <- addTiles(global_map) # 添加标题,必须有,否则不显示地图,必须有返回 <-
color = c("blue", "green", "red", "black")
n <- as.numeric(rownames(line_data))
for (i in n) {
  global_map <- addPolylines(global_map, lng = c(line_data[i,1], line_data[i,3]), lat = c(line_data[i,2], line_data[i,4]), weight = 1, color = color[line_data.pam$clustering[i]])
}
global_map
```
OD线分布图：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E7%BA%BF%20%E6%9C%AA%E5%88%86%E7%B1%BB%20%E5%85%88%E7%94%BB%E8%B5%B7%E7%82%B9%E5%90%8E%E7%94%BB%E7%BB%88%E7%82%B9%20%E6%9C%89%E8%A6%86%E7%9B%96%E7%8E%B0%E8%B1%A1.png)   
用k-means进行聚类，k=3，聚类结果如下：   
![](https://github.com/jelly-lemon/midterm_homework/blob/master/image/%E7%BA%BF%20%E8%81%9A%E7%B1%BB.png)


