# 這一整個檔案等同於一個 Image
# 每一行 code 都代表幫 image 新增加的一層 layer 

# 在 (Image) runtime 時會將生成的容器設定為 node 16 (基於 alpine-linux)，如果沒有，會自動從 docker hub 中下載下來)
FROM node:16-alpine 

# 安裝 nodemon 才能透過 Volume 的特性去觀察檔案中實時的變化
RUN npm install -g nodemon

# 將容器的工作目錄設為 /app (在這行底下的"所有"指令 都會被執行於 "app" 資料夾)
# (在 Image 創建 Container 時，就會自動新增一個命名為 "app" 的資料夾)
WORKDIR /app

# 在 (Image) runtime 時會將當前 dockerfile 所在的資料夾內的 package.josn 檔案複製，並自動丟到容器的 /app 目錄下
# 因為要使用 "layer caching" 的特性，又要避免次序問題，所以把這行獨立出來當作一層 layer
# (如果還是有點混淆的話，可以將這個檔案和 lesson 8 的檔案做比較，會比較清楚 ~)
COPY package.json . 

# 在 (Image) runtime 時會執行 RUN 後面的指令
# (因為在上一層時已經把 package.json 複製下來了，所以執行 npm install 會把所有設定的 dependencies 都一併下載下來)
RUN npm install

# ---- 如果 app.js 裡面的 codes 改變的話，分隔線以上的 layer 還是會以 caching 的方式取出，而非重新 process 一次 ----

# 在 (Image) runtime 時會將當前 dockerfile 所在的資料夾內的所有檔案複製，並自動丟到容器的 /app 目錄下
# 因為上一層的關係，當前的工作目錄為 /app ，所以加上 " . ." 以複製到整個工作資料夾
COPY . .

# 告訴容器這個 app 會運作在容器的哪個 port 上 
# (要注意，這個 app 是運作在容器的 port上，不是我們電腦的 port上)
EXPOSE 4000

# 當容器已經被完全建立，且開始運作時會執行以下的指令
CMD ["npm", "run", "dev"]



# 最後，要 build 這個 Image，
# 要先打開 terminal 並 cd 至這個 Dockerfile 所在的這個資料夾

# 再執行  "docker build -t <Image name>:<Tag name> ."  這個指令
# Example: docker build -t myapp:v1 . 
# 1. 記得要把前後的 "" 去掉
# 2. 冒號後面接著的是你想要設定的 Tag 名稱，如果沒有要設定 Tage 可以不加冒號
# 3. 最後的 "空白格 + ." 不能省略， 點代表的是 Dockerfile 可以在目前的工作目錄的資料夾內找到 

# 執行每一層指令時等於是幫(我們正在創建的)這個 docker image 增加了新的一層 (image)
# 最後的最後，指令完成時可以去 docker desktop 的 Images 頁面檢查，這個 Image 是否有成功創建 !


# (~ 以下為學習筆記 ~)
# docker 在 build Image 時有一個特性，叫做 layer caching
# 如果之前有 build 過一樣的 Image ，就會直接從 cache 中取出一樣的 Image layer，而不是重新 process 一遍
# 但是當某一層的 layer 跟之前存在 cache 中的 layer 不一樣時
# 這層 layer 與之後的所有 layer 都不能使用 caching 的方式，而是都要重新 process 一遍
# 有一個小技巧是可以把預計不會改變且 "沒有次序問題" 的 layer 往上拉，藉此減少欲改變的 layer 後的 layer 數量