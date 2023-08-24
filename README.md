# NVIDIA GPU Cloud setup and building NVIDIA Containers for Jetson and JetPack

If you get below error when pulling docker image or running NVIDIA docker container: 
```
ERROR: failed to solve: failed to fetch anonymous token: unexpected status: 401 Unauthorized
```
Follow next steps:

1. ### Install NGC CLI
Follow steps from [NVIDIA NGC CLI](https://ngc.nvidia.com/setup/installers/cli) for your OS (Windows, AMD64 Linux, ARM64 Linux, ARM64 MacOs, Intel MacOs).

##### AMD64 Linux Install

The NGC CLI binary for Linux is supported on Ubuntu 16.04 and later distributions.

- Click Download CLI to download the zip file that contains the binary, then transfer the zip file to a directory where you have permissions and then unzip and execute the binary. You can also download, unzip, and install from the command line by moving to a directory where you have execute permissions and then running the following command:

```python
$ wget --content-disposition https://ngc.nvidia.com/downloads/ngccli_linux.zip && unzip ngccli_linux.zip && chmod u+x ngc-cli/ngc
```
- Check the binary's md5 hash to ensure the file wasn't corrupted during download:
```python
$ find ngc-cli/ -type f -exec md5sum {} + | LC_ALL=C sort | md5sum -c ngc-cli.md5
```
- Add your current directory to path:
```python
$ echo "export PATH=\"\$PATH:$(pwd)/ngc-cli\"" >> ~/.bash_profile && source ~/.bash_profile
```

2. ### Setup Environment

- Create an [NVIDIA account](https://ngc.nvidia.com/) and get your [API Key](https://ngc.nvidia.com/setup/api-key).

- Enter the following command, including your API key when prompted:

```python
$ ngc config set

Enter API key [no-apikey]. Choices: [<VALID_APIKEY>, 'no-apikey']: 
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
Enter CLI output format type [ascii]. Choices: [ascii, csv, json]: 
Enter org [no-org]. Choices: ['xxxxxxxxxxxx']: xxxxxxxxxxxx
Enter team [no-team]. Choices: ['no-team']: no-team
Successfully saved NGC configuration to /root/.ngc/config
```

- Check your Docker configuration: 
Verify that your Docker configuration is updated to include the appropriate registry authentication information. You can do this by checking your `~/.docker/config.json` file.

- Check your NGC version:
```python
$ ngc --version

NGC CLI 3.21.0
```

- Login to registry:
For the username, enter '$oauthtoken' exactly as shown. It is a special authentication token for all users.
```python
$ docker login nvcr.io

Username: $oauthtoken
Password: <Your Key>
```
3. ### Create Dockerfile

To create your own container, choose a suitable PyTorch container version from [NVIDIA PyTorch Container Versions](https://docs.nvidia.com/deeplearning/frameworks/pytorch-release-notes/rel-22-11.html#rel-22-11).

*For example my machine specifications: `cuda:11.6.2` and `ubuntu:20.04`. I have to use `nvidia container: 22.04`.*

```python
FROM nvcr.io/nvidia/pytorch:22.04-py3
WORKDIR /working/directory/ 
COPY requirements.txt .
RUN pip install -r requirements.txt
```

- #### For NVIDIA Jetson device, you can find the Jetpack version and the relative version of L4T that it makes available using [NVIDIA® Jetson™ L4T and JetPack Support](https://www.stereolabs.com/blog/nvidia-jetson-l4t-and-jetpack-support/).

Our specifications:
- JetPack 4.6.1
- L4T 32.7.1
- Find your image name from [Containers NVIDIA L4T ML](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/l4t-ml/tags)

```python
FROM nvcr.io/nvidia/l4t-ml:r32.7.1-py3 
WORKDIR /working/directory/ 
COPY requirements.txt .
RUN pip install -r requirements.txt
```

4. ### Build Docker
```python
docker build --no-cache -t project_name .
```
5. ### Run Docker

```python
docker run -it --rm --gpus all -v working/directory/ :working/directory/  project_name
```



