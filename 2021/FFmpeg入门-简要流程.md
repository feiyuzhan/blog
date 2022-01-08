

FFmpeg的音频流处理的简要流程如下：

Step1. 采集音频

1. 打开audio device
	- Avdevice_register_all()  注册设备
	- Av_find_input_format() 获取格式
	- Avformat_open_input()  打开设备，返回指针，供后续API读取数据
	
2. 读取数据

	- Av_init_packet() 初始化存放的packet

	- Av_read_frame()
	- Av_packet_unref() 释放资源

3. 释放上下文 av_format_close()



Step2. 重采样（if needed）

使用重采样上下文，定义输出的通道数、采样大小和采样率等



Step3. 音频编码

- 创建编码器 avcodec_find_encoder
- 创建上下文 avcodec_alloc_context3
- 打开编码器 avcodec_open2



