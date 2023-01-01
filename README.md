# pytorch-mnist-dist

## 使用方式
1. build 运行时的镜像
    ```shell
    # docker build -f Dockerfile -t ${YOUR_IMAGE_REPO}/pytorch-with-tensorboard:1.5.1-cuda10.1-cudnn7-runtime ./
    docker build -f Dockerfile -t registry.cn-huhehaote.aliyuncs.com/lumo/pytorch-with-tensorboard:1.5.1-cuda10.1-cudnn7-runtime ./
    ```
2. 在当前目录本地启动一个镜像
    ```shell
    # docker run -itd --name pytorch-1.5 -v $PWD:/workspace ${YOUR_IMAGE_REPO}/pytorch-with-tensorboard:1.5.1-cuda10.1-cudnn7-runtime bash
    docker run -itd --name pytorch-1.5 -v $PWD:/workspace registry.cn-huhehaote.aliyuncs.com/lumo/pytorch-with-tensorboard:1.5.1-cuda10.1-cudnn7-runtime bash
    ```
3. 进入容器
    ```shell
    docker exec -it pytorch-1.5 bash
    ```
4. 执行以下命令
    ```shell
    #gloo
    python mnist.py --backend gloo
    #nccl
    python mnist.py --backend nccl
    #mpi（pytorch 在编译的时候需要指定 mpi）
    python mnist.py --backend mpi
    ```

## 说明
1. MNIST/ 存放的是 mnist 数据集
2. mpi 的分布式需要 pytorch 编译的时候支持 mpi, 并且在有 mpi 的环境中才能执行



arena --loglevel debug submit pytorch --name=pytorch-local-git --gpus=1  --workers=1 --selector gpu_node=true --image=registry.cn-beijing.aliyuncs.com/ai-samples/pytorch-with-tensorboard:1.5.1-cuda10.1-cudnn7-runtime  --sync-mode=git --sync-source=https://code.aliyun.com/370272561/mnist-pytorch.git "python /root/code/mnist-pytorch/mnist.py"



arena --loglevel info submit pytorch \    --name=pytorch-local-git \    --gpus=1 \    --image=registry.cn-beijing.aliyuncs.com/ai-samples/pytorch-with-tensorboard:1.5.1-cuda10.1-cudnn7-runtime \    --sync-mode=git \    --sync-source=https://github.com/WangAooa/gts-test.git \    "python /root/code/mnist-pytorch/mnist.py --backend gloo"
