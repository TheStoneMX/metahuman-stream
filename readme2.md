Avatar
lipku
Updated 1 day ago
lipku/metahuman-stream/base
Running time 10680h No.42
GitHub Star3.5k
35 1.0k
README Discussion Area 8 Version 1
Image Build
Basic Environment
Framework and Version PyTorch / 1.11.0 / 3.8(ubuntu20.04) / 11.3
CUDA Version 11.3

Code Directory
/root/metahuman-stream/

Run
python app.py --listenport 6006 --transport webrtc
Access http://<autodl external network address>/webrtcapi.html on the local browser, select stun, and click start
You can see the video on the home Unicom, Mobile, and Telecom networks, but some networks may not be connected. If the network is not connected, you can achieve it through the following transfer method.
If you can see the video but not talk, it is usually because of https problems. Access the URL through ssh tunneling. Refer to the following configuration
https://www.autodl.com/docs/ssh_proxy/

rtcpush to other cloud services
On Alibaba Cloud or Tencent Cloud Service (no GPU required)
Start srs

export CANDIDATE='<server external network ip>'
docker run --rm --env CANDIDATE=$CANDIDATE \
-p 1935:1935 -p 8080:8080 -p 1985:1985 -p 8000:8000/udp \
registry.cn-hangzhou.aliyuncs.com/ossrs/srs:5 \
objs/srs -c conf/rtc.conf
Need to open ports tcp 1985, udp 8000

On autodl
Modify web/rtcpushapi.html
Replace

var url = "http://"+host+":1985/rtc/v1/whep/?app=live&stream=livestream"
with

var url = "http://Alibaba Cloud Public Network IP:1985/rtc/v1/whep/?app=live&stream=livestream"

Author: lipku
Link: https://www.codewithgpu.com/i/lipku/metahuman-stream/base
Source: CodeWithGpu
Copyright belongs to the author. For commercial reprint, please contact the author for authorization. For non-commercial reprint, please indicate the source.
