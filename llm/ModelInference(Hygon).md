# HyGon:zzz:

> :question: How to run the LLM to accomplish QA inference in HyGon server like `K100-AI`

## Install HyGon Tutorial

:pushpin: Install the corresponding dcu driver according to the server model. u can check the hardware by the following command

:white_check_mark: check the dcu version​

```bash
rocm-smi
```

:white_check_mark: ​check the cpu version

```bash
sudo lscpu
```

:white_check_mark: check the system version

```bash
sudo dmidecode -t system
```

Usually, server has been configured with the necessary tool dependencies. If u want to install yourself, u can access the [developers community](https://cancon.hpccube.com:65024/6/main) to download the pointed driver version.

## Run the LLM

:pushpin: ​install the docker container which can use the pytorch and transformers

u can access the container from the HyGon docker hub: https://sourcefind.cn/#/image/dcu/pytorch?activeName=overview. The version of python, torch, transformers are 3.10, 2.3, 4.45, respectively.

:white_check_mark: pull the docker container

```bash
docker pull image.sourcefind.cn:5000/dcu/admin/base/pytorch:2.3.0-ubuntu22.04-dtk24.04.3-py3.10
```

:white_check_mark: Create an docker container install by the pulled image

```bash
docker run -it \
--name {name}
--network=host \
--ipc=host \
--shm-size=16G \
--device=/dev/kfd \
--device=/dev/mkfd \
--device=/dev/dri \
-v /opt/hyhal:/opt/hyhal \
--group-add video \
--cap-add=SYS_PTRACE \
--security-opt seccomp=unconfined \
image.sourcefind.cn:5000/dcu/admin/base/pytorch:2.1.0-centos7.6-dtk23.10-py38 \
/bin/bash
```

:white_check_mark: ​After starting the docker containers, you can enter the container using the following command

```bash
docker exec -it {container_name} /bin/bash
```

:pushpin: After enter the container, you can run the LLM by vllm

you can refer to the document [vllm]() to get the detailed instructions. The following is sample running command.

