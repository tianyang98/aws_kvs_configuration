## aws_kvs_configuration
how to stream video through the camera on your local device or RTSP address to kinesis video stream on AWS.

refer to the doc:https://docs.aws.amazon.com/zh_cn/kinesisvideostreams/latest/dg/gs-send-data.html#gs-send-data-2

Firstly, unzip and get the file `amazon-kinesis-video-streams-producer-sdk-cpp`;

Enter in kinesis-video-native-build inside;

download `Homebrew` to make the process of installing packages easier.

Run sentence below in terminal:
>brew install pkg-config openssl cmake gstreamer gst-plugins-base gst-plugins-good gst-plugins-bad gst-plugins-ugly log4cplus

>sudo ./min-install-script

>sudo ./gstreamer-plugin-install-script

Then we need to configure the environment variables in case of the error showing "no elements kvssink":

>export LD_LIBRARY_PATH=/Users/yangtian/Documents/2nd_semester/cloud_computing/hw2/amazon-kinesis-video-streams-producer-sdk-cpp/kinesis-video-native-build/downloads/local/lib:$LD_LIBRARY_PATH

>export PATH=/Users/yangtian/Documents/2nd_semester/cloud_computing/hw2/amazon-kinesis-video-streams-producer-sdk-cpp/kinesis-video-native-build/downloads/local/bin:$PATH

>export GST_PLUGIN_PATH=/Users/yangtian/Documents/2nd_semester/cloud_computing/hw2/amazon-kinesis-video-streams-producer-sdk-cpp/kinesis-video-native-build/downloads/local/lib:$GST_PLUGIN_PATH

Run the Gstreamer plugin:

>gst-launch-1.0 autovideosrc ! videoconvert ! video/x-raw,format=I420,width=1280,height=720 ! vtenc_h264_hw allow-frame-reordering=FALSE realtime=TRUE max-keyframe-interval=45 bitrate=512 ! h264parse ! video/x-h264,stream-format=avc,alignment=au,profile=baseline ! kvssink stream-name=MyKinesisVideoStream storage-size=512 access-key=* secret-key=* aws-region=*

After this, you should see codes running continuously on your terminal, the camera is opened and on AWS KVS it shows your face in `MyKinesisVideoStream`.

To stream video through RTSP:

In this way I download the App called `IP camera` in Appstore, bc it gives you the RTSP addresses directly and you can turn on/off the RTSP service easily. Note that you should add "admin:admin@" in the RTSP address to give it permission.

The only different with above is the gst command:

>AWS_ACCESS_KEY_ID=* AWS_SECRET_ACCESS_KEY=* AWS_DEFAULT_REGION=* gst-launch-1.0 rtspsrc location=*(your RTSP address) ! rtph264depay ! h264parse ! kvssink stream-name=$ storage-size=512



