### 分布式训练DPP

##### 模型并行

模型并行主要应用于模型相比显存来说更大，一块 GPU 无法加载的场景，通过把模型切割为几个部分，分别加载到不同的 GPU 上，来进行训练

##### 数据并行

这个是日常会应用的比较多的情况。即每个 GPU 复制一份模型，将一批样本分为多份分发到各个GPU模型并行计算。因为求导以及加和都是线性的，数据并行在数学上也有效。采用数据并行相当于加大了 batch_size，得到更准确的梯度或者加速训练。

> 注意：多卡训练要考虑通信开销的，是个trade off的过程，不见得四块卡一定比两块卡快多少，可能是训练到四块卡的时候通信开销已经占了大头



##### torch.dist.init_process_group()

是 PyTorch 中用于初始化分布式训练进程组的函数。通过调用这个函数，可以设置分布式训练的环境，例如通信后端（如 `gloo`、`mpi` 、`nccl` 等）、节点的排名、世界大小等信息，从而为后续的分布式数据并行、模型并行等操作做好准备。

- `backend`：指定分布式后端的名称，例如 `nccl`、`gloo` 或 `mpi`

    >一般来说使用 `nccl` 对于GPU分布式训练，使用 `gloo` 对CPU进行分布式训练，`mpi` 需要从源码重新编译。

- `init_method`：进程直接通信的管道

    >`tcp://<ip_address>:<port>`：使用TCP协议进行通信，需要指定IP地址和端口号，多机训练
    >
    >`file://<path_to_file>`：使用本地文件进行通信，需要指定一个共享文件的路径，多卡训练
    >
    >`env://`：使用环境变量进行通信，不需要额外的参数

- `world_size`：分布式训练进程总数，多卡训练时就是 GPU 数量

- `rank`：当前进程的编号，int

    > 通常需要使用 RANK 来决定当前进程的角色和任务，例如是否是主进程、是否需要保存模型等

注意：对于多机训练，要注意关闭防火墙；对于多卡训练，一般在服务器中 NAS 盘用于存数据集，带显卡的机器和 NAS 盘不属于同一个机器。所以要注意管道文件要放在显卡机上面，NAS 可能会有防火墙，不然可能会报错：

```python
terminate called after throwing an instance of 'std::system_error'
	what():  flock: No locks available
```



参考：

[分布式介绍](https://blog.csdn.net/ytusdc/article/details/122091284)

[init函数](https://blog.csdn.net/weixin_38252409/article/details/134965424)
