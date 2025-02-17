# MindIE:zzz:

> :question:How to run the LLM to accomplish QA inference in HW server like `A800 T1` or `A800 T2` 

## Install Ascend Tutorial

:pushpin: Install the corresponding Npu driver according to the server model. u can check the hardware by the following command

:white_check_mark: ​check the npu version

```bash
npu-smi info
```

:white_check_mark: ​check the cpu version

```bash
sudo lscpu
```

:white_check_mark: check the system version

```bash
sudo dmidecode -t system
```

:pushpin: Next download and install the corresponding Ascend toolkit。

Usually, you need to install the [Ascend CANN Toolkit and Kernels](https://www.hiascend.com/developer/download/community/result?module=cann). Please follow the installation [tutorial](https://www.hiascend.com/en/document). After completing the installation, you need to configure the environment variables, which are usually located in the `/usr/local/Ascend/ascend-toolkit/set_env.sh`

```bash
source /usr/local/Ascend/ascend-toolkit/set_env.sh
```

## install MindIE

:pushpin: install MindIE by docker :stars:

Download the corresponding image file from the HW dockerhub, and then you can refer to the following command to start a docker container.

:white_check_mark: Import the docker image compressed file into Docker images like following.

```bash
docker load < image.tar
```

:white_check_mark: Create an docker container instance by the imported image. u can refer to the following command.​

```bash
docker run -it \
--name {name} \
--ipc=host \
--network=host \
--device=/dev/davinci0 \
--device=/dev/davinci1 \
--device=/dev/davinci2 \
--device=/dev/davinci3 \
--device=/dev/davinci4 \
--device=/dev/davinci5 \
--device=/dev/davinci6 \
--device=/dev/davinci7 \
--device=/dev/davinci_manager \
--device=/dev/devmm_svm \
--device=/dev/hisi_hdc \
-v /usr/local/dcmi:/usr/local/dcmi \
-v /usr/local/bin/npu-smi:/usr/local/bin/npu-smi \
-v /usr/local/sbin/npu-smi:/usr/local/sbin/npu-smi \
-v /usr/local/Ascend/driver/lib64/common:/usr/local/Ascend/driver/lib64/common \
-v /usr/local/Ascend/driver/lib64/driver:/usr/local/Ascend/driver/lib64/driver \
-v /etc/ascend_install.info:/etc/ascend_install.info \
-v /etc/vnpu.cfg:/etc/vnpu.cfg \
-v /usr/local/Ascend/driver/version.info:/usr/local/Ascend/driver/version.info \
-v {dir_path}:{dir_path} \
{image}:{tag} /bin/bash
```

:white_check_mark: ​After starting the docker containers, you can enter the container using the following command

```bash
docker exec -it {container_name} /bin/bash
```

## Run MindIE

:pushpin: ​MindIE is typically located at the path `/usr/local/Ascend/mindie/`

To start the LLM inference service, follow these steps:

1. First, set up the env variables and navigate to the path of the `mindie-service` program. You can find the `set_env.sh` in the directory at the path `/usr/local/Ascend` .
2. Next, modify the `config.json` file located in the `conf` directory. Detailed parameter instructions can be found in the [MindIE Server Config](https://www.hiascend.com/document/detail/zh/mindie/100/mindieservice/servicedev/mindie_service0285.html). :exclamation: ​By the way, we need to change output token parameter `maxIterTimes` , because if default value is to short only 512.
3. Finally, run the startup script `mindieservice-demaone`, which is located in the `bin` directory.

:white_check_mark: ​The corresponding bash command as following:

```bash
# enter the program directory
cd /usr/local/Ascend/mindie/latest/mindie-service
# modify the config
vim conf/config.json
# run the mindieservice backend
nohup ./bin/mindieservice_demo.. > output.log &
```

:white_check_mark: After start the LLM model, you can test by the following command:

```bash
curl -X POST http://{ip:port}/v1/chat/completions \
-H "Content-Type: application/json" \
-d '{
    "model": {model_name},
    "messages": [
        {
            "role": "system",
            "content": "You are a helpful assistant."
        },
        {
            "role": "user", 
            "content": "隧道施工建设"
        }
    ],
    "stream": true
}'
```

## Log

:pushpin: There are three log file when using the MindIE.​

You can get the `mindie-service.log` and `mindie_audit.log` at the path `/usr/local/Ascend/mindie/latest/mindie-service/logs` and get the `plog` logs at the path `/root/ascend/log/`. **You can change the log level in `config.json` when you want to modify the log information**

:exclamation: If the mindie-service fails to start, it usually silently fails, and there is no useful output on the screen. The log file set in `config.json` also contains no content. This situation is quite frustrating as there is no feedback. Additionally, there is a log directory located at `$HOME/mindie`.
