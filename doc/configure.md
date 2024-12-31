# SMPLOlympics配置说明

**算力资源：北京超算4090**
**SMPLOlympics底层环境：glibc≥3.0左右（必须用容器），python版本≤3.8**

1. 找工程师拉取一个pytorch的apptainer容器（本质是通过apptainer拉取docker容器）
    1. 我pull的是`pytorch:21.09-py3`（也可以更新的版本，但要注意里面的python版本不高于3.8），如果自己拉取命令是`apptainer pull docker://nvcr.io/nvidia/pytorch:21.09-py3`， 但会有一定的网络问题
2. 工程师拉下来的.sif的镜像文件，需要展开成沙盒才可以写入
    1. 命令是`apptainer build --sandbox pytorch_21.09-py3 pytorch_21.09-py3.sif`，这个过程会非常久，挂着耐心等即可
    1. 成功之后会产生一个新的pytorch_21.09目录，目录下是沙盒内部的文件系统，不用管这个目录即可，因为沙盒可以读写外部的文件系统路径
3. 上传好项目文件（如SMPLOlympics），安装好isaacgym依赖
    1. 我是上官网下载Preview release手动安装，然后上传集群的（之后运行的过程中可能会遇到动态链接库的相关问题，逐个解决即可）
    1. 也可以研究SMPLOlympics给的那个isaacgym链接（指向了isaacy ros，不确定是否可以）
4. 在集群上安装miniconda3，创建自己的conda环境（如SMPLOlympics），这个过程如果遇到网络问题，最好本地下载好依赖，然后上传到集群再pip install
    1. 注意不要在沙盒里安装anaconda，anaconda太大了没法写入
5. 进入沙盒的命令
    1. `apptainer shell --nv -f -w new/pytorch_21.09-py3`，其中--nv是提供nvidia相关支持，-w是写入，-f是强制使用root
6. 在沙盒里跑SMPLOlympics可能会遇到卡住的问题，这需要清理沙盒缓存
    1. 命令是`rm -rf /root/.cache`（在沙盒里）