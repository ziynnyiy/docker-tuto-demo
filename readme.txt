// 如何執行 Image (記得，要先 build Image 後，才能執行 Image !)
// <container name> => 執行Image 後創建的容器想要取的名字 
// <image name> => 想要執行的 Image 的名字
// 記得要在執行指令中設定將 app 在容器中運行的 port 號，映射在我們電腦的 port 號上，這樣才能藉由運行我們電腦的 port 去運行容器的 port
// -d => detached tag  ==> 防止 terminal 因為運作中而無法輸入其他指令

docker run --name <container name> -p <mapped port by our PC:exposed port by container> -d <image name>:<tag name>
// Example command: docker run --name myapp_c2 -p 4000:4000 -d myapp:v1
// 如果該 Image 沒有 tag，那上面的指令的 <image name> 後面就不需要加 :<tag name>
// 重要! 每次使用 docker run 去執行 Image 就會創建一個新的 Container



// 所有常見的 docker 指令 :

// 如何暫停 container
// 執行下面指令後要等待數秒才會顯示已經停止
docker stop <container name>

// 如何執行 container
// (執行 container 時，不需要執行Image時的複雜設定，因為在執行Image時就已經設定過了)
docker start <container name>

// 如何顯示所有正在運行的 Container 
docker ps

// 如何顯示所有(運行中、暫停中的) Container
docker ps -a

// 如何顯示所有的 Images
docker images

// 如何刪除 Image (要注意，被 Container 使用的 Image 不能透過這個指令刪除)
docker image rm <Image name>

// (方法一) 如何刪除被 Container 使用的 Image
// -f => force
docker image rm <Image name> -f

// (方法二) 如何刪除被 Container 使用的 Image
// 先把使用該 Image 的 Container 都刪除
// 再刪除該 Image

// 如何刪除 Container
docker container rm <Container name>

// 如何一次刪除全部的 Images Conatiners Volumes
docker system prune -a
