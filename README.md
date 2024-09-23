Real time interactive streaming digital human， realize audio video synchronous dialogue. It can basically achieve commercial effects.  
实时交互流式数字人，实现音视频同步对话。基本可以达到商用效果

[ernerf效果](https://www.bilibili.com/video/BV1PM4m1y7Q2/)  [musetalk效果](https://www.bilibili.com/video/BV1gm421N7vQ/)  [wav2lip效果](https://www.bilibili.com/video/BV1Bw4m1e74P/)

## Features
1. Support multiple digital human models: ernerf, musetalk, wav2lip
2. Support voice cloning
3. Support digital human speech interruption
4. Support full body video stitching
5. Support rtmp and webrtc
6. Support video editing: play custom video when not speaking

## 1. Installation

Tested on Ubuntu 20.04, Python3.10, Pytorch 1.12 and CUDA 11.3

### 1.1 Install dependency

```bash
conda create -n nerfstream python=3.10
conda activate nerfstream
conda install pytorch==1.12.1 torchvision==0.13.1 cudatoolkit=11.3 -c pytorch
pip install -r requirements.txt
#If you do not train the ernerf model, you do not need to install the following libraries
pip install "git+https://github.com/facebookresearch/pytorch3d.git"
pip install tensorflow-gpu==2.8.0
pip install --upgrade "protobuf<=3.20.1"
```
If you use pytorch2.1, use torchvision 0.16 (you can go to the torchvision official website to find the matching version according to the pytorch version), cudatoolkit does not need to be installed
FAQ for installation [FAQ](/assets/faq.md)
For setting up the linux cuda environment, please refer to this article https://zhuanlan.zhihu.com/p/674972886


## 2. Quick Start
By default, the Ernerf model is used, and webrtc is used to push the stream to srs
### 2.1 Run srs
```
export CANDIDATE='<server external network ip>'
docker run --rm --env CANDIDATE=$CANDIDATE \
-p 1935:1935 -p 8080:8080 -p 1985:1985 -p 8000:8000/udp \
registry.cn-hangzhou.aliyuncs.com/ossrs/srs:5 \
objs/srs -c conf/rtc.conf
```

### 2.2 Start Digital Human:

```python
python app.py
```

If you cannot access huggingface, before running
```
export HF_ENDPOINT=https://hf-mirror.com
```

Open http://serverip:8010/rtcpushapi.html with a browser, enter any text in the text box, and submit. Digital Human broadcasts the text
Note: The server needs to open ports tcp:8000,8010,1985; udp:8000

## 3. More Usage
Usage instructions: <https://livetalking-doc.readthedocs.io/>
  
## 4. Docker Run
No need for previous installation, just run directly.
```
docker run --gpus all -it --network=host --rm registry.cn-beijing.aliyuncs.com/codewithgpu2/lipku-metahuman-stream:vjo1Y6NJ3N
```
The code is in /root/metahuman-stream, first git pull the latest code, then execute the command with steps 2 and 3

The following images are provided
- autodl image: <https://www.codewithgpu.com/i/lipku/metahuman-stream/base>
[autodl tutorial](autodl/README.md)


## 5. Performance analysis
1. Frame rate
The overall fps tested on the Tesla T4 graphics card is about 18. If the audio and video encoding and streaming are removed, the frame rate is about 20. The 4090 graphics card can reach more than 40 frames per second.
2. Latency
The overall latency is about 3s
(1) TTS latency is about 1.7s. Edgetts is currently used. Each sentence needs to be converted and input at one time. TTS can be optimized to stream input
(2) Wav2vec latency is 0.4s. 18 frames of audio need to be cached for calculation
(3) SRS forwarding latency. Set the SRS server to reduce buffering latency. For specific configuration, see https://ossrs.net/lts/zh-cn/docs/v5/doc/low-latency

## 6. TODO
- [x] Add chatgpt to realize digital human dialogue
- [x] Sound cloning
- [x] Replace the digital human with a video when it is muted
- [x] MuseTalk
- [x] Wav2Lip
- [ ] TalkingGaussian

---
If this project is helpful to you, please help click a star. Interested friends are also welcome to improve the project together.
* Knowledge Planet: https://t.zsxq.com/7NMyO Precipitate high-quality common problems, best practices, and problem solving
* WeChat public account: Digital Human Technology
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/l3ZibgueFiaeyfaiaLZGuMGQXnhLWxibpJUS2gfs8Dje6JuMY8zu2tVyU9n8Zx1yaNncvKHBMibX0ocehoITy5qQEZg/640?wxfrom=12&tp=wxpic&usePicPrefetch=1&wx_fmt=jpeg&amp;from=appmsg)  

