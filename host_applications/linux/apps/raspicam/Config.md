## Config file

This is held in `raspimjpeg.conf` and consists of keyword value lines. It is read at start up and when the camera is restarted.

|Keyword|Default Value|Description|
| :------------- | :---------- | :----------- |
|annotation|RPi Cam %Y.%M.%D_%h:%m:%s|See an command|
|anno_background|false|See ab command|
|anno3_custom_background_colour|0|See ac command|
|anno3_custom_background_Y|0|See ac command|
|anno3_custom_background_U|128|See ac command|
|anno3_custom_background_V|128|See ac command|
|anno3_custom_text_colour|0|See at command|
|anno3_custom_text_Y|255|See at command|
|anno3_custom_text_U|128|See at command|
|anno3_custom_text_V|128|See at command|
|anno_text_size|50|See as command|
|sharpness|0|See sh command|
|contrast|0|See co command|
|brightness|50|See br command|
|saturation|0|See sa command|
|iso|0|See is command|
|metering_mode|average|See mm command|
|video_stabilisation|false|See vs command|
|exposure_compensation|0|See ec command|
|exposure_mode|auto|See em command|
|white_balance|auto|See wb command|
|autowbgain_r|150|See ag command|
|autowbgain_b|150|See ag command|
|image_effect|none|See ie command|
|colour_effect_en|false|See ce command|
|colour_effect_u|128|See ce command|
|colour_effect_v|128|See ce command|
|rotation|0|See ro command|
|hflip|false|Sets horizontal flip on /off|
|vflip|false|Sets vertical flip on /off|
|sensor_region_x|0|See ri command|
|sensor_region_y|0|See ri command|
|sensor_region_w|65536|See ri command|
|sensor_region_h|65536|See ri command|
|shutter_speed|0|See ss command|
|raw_layer|false|See rl command|
|camera_num|0|See cn command|
|minimise_frag|0|MMAL MINIMISE_FRAGMENTATION 0/1 on/off|
|initial_quant|25|MMAL_PARAMETER_VIDEO_ENCODE_INITIAL_QUANT 25|
|encode_qp|31|MMAL_PARAMETER_VIDEO_ENCODE_QP_P 31|
|mmal_logfile|Set to filepath (/dev/shm/mjpeg/mmallogfile) to enable callback logging|Debug use only|
|width|512|Sets preview width|
|quality|10|Sets preview jpeg compression|
|divider|1|Sets preview fps relative to video fps|
|video_width|1920|See px command|
|video_height|1080|See px command|
|video_fps|25|See px command|
|fps_divider|1|See px command|
|video_bitrate|17000000|See bi command|
|video_buffer|0|See bu command|
|h264_buffer_size|131072|Sets h264 buffer size (0 default 65536) Higher values helps callback smoothness|
|h264_buffers|0|Sets number of buffers used by capture system|
|video_split|0|See vi command|
|MP4Box|background|See bo command|
|MP4Box_fps|25|See px command|
|MP4Box_cmd|dflt|Sets cmd used during boxing operations. See Additions and Tricks section|
|image_width|2592|See px command|
|image_height|1944|See px command|
|image_quality|10|See qu command|
|tl_interval|30|See tv command|
|motion_external|true|Select external motion detetc|
|vector_preview|false|Show vector display rather than image|
|motion_noise|20|Internal motion detect parameter|
|motion_threshold|100|Internal motion detect parameter|
|motion_image|	Internal motion detect image mask|
|motion_initframes|0|Internal motion detect parameter|
|motion_startframes|5|Internal motion detect parameter|
|motion_stopframes|50|Internal motion detect parameter|
|motion_pipe|/var/www/FIFO1|Where to send internal detect triggers|
|motion_file|0|Turn on vector capture to file|
|base_path|/var/www|Base path of web install|
|preview_path|/dev/shm/mjpeg/cam.jpg|RAM folder for preview jpgs|
|image_path|/var/www/media/im_%i_%Y%M%D_%h%m%s.jpg|Template for image capture names|
|lapse_path|/var/www/media/tl_%i_%t_%Y%M%D_%h%m%s.jpg|Template for time lapse capture names|
|video_path|/var/www/media/vi_%v_%Y%M%D_%h%m%s.mp4|Template for video capture names|
|status_file|/var/www/status_mjpeg.txt|raspimjeg status file|
|control_file|/var/www/FIFO|Listening pipe for commands|
|media_path|/var/www/media|Base storage for media files|
|macros_path|/var/www/macros|Base storage for macros|
|boxing_path|	Temp folder for capture and boxing operations|
|subdir_char|@|Character used to flatten paths in thumbnail names|
|count_format|%04d|format string for capture numbering. e.g. %05d for 5 digits|
|start_img|start_img.sh|Image capture start macro name|
|end_img|&end_img.sh|Image capture complete macro name|
|start_vid|start_vid.sh|Video capture start macro name|
|end_vid|end_vid.sh|Video capture complete macro name|
|end_box|&end_box.sh|Boxing complete macro name|
|thumb_gen|vit|Thumbnail generation enables|
|autostart|standard|Whether to start camera at start up|
|motion_detection|false|Whether to enable motion detect at start up|
|watchdog_interval|30|Check interval to see if preview still working|
|watchdog_errors|3|Trigger point for preview failure|
|user_config|/var/www/uconfig|User config file path|
|log_file|/var/www/scheduleLog.txt|Logging path|
|fullscreen|false|If true start up display fullscreen|
|log_size|5000|Maximum number of lines in log. 0 means no logging|
|enforce_lf|0|Set 1 to only process FIFO when terminated by lf|
|fifo_interval|100000|fifo polling interval in microseconds|



## Pipe

The following commands are supported by raspimjpeg when received from the pipe. Many are equivalents of values in the config file. Others are associated with several parameters. They are sent in as a serial stream as a 2 character command, space, and space separated parameters.

The original scheme did not use LF terminators. LF terminators are now supported and recommended to improve reliability in command recognition. The old scheme is still supported. To send from a command line use something like `echo 'im' >/var/www/FIFO` (where this may need to be adjusted according to the install folder.)

The old scheme without lf terminators could sometimes cause problems if there was an interruption during sending a command into the pipe. You can force lf terminators to be used by setting enforce_lf to 1 in the `raspimjpeg.conf` config file. You can also set the poll rate using `fifo_interval` (minimum 100000 microseconds).

The main Pipe is FIFO which is used by the python script. Commands may also be sent to extra Pipes named FIFO11 up to FIFO19. Additionnal pipes are not used in the PlanktoScope setting. All commands are treated the same no matter which pipe is used. This can be useful to make sure that commands from other sources don't get mixed up. The installer makes FIFO11 as an input; if others are required then they should be made manually and will then be recognised automatically next time the camera system is started.

|Cmd|Parameters|Description|
| :-------------: | :----------: | :----------- |
|an|Text|set annotation|
|ab|0/1|annotation background off/on|
|px|AAAA BBBB CC DD EEEE FFFF GG|set resolution video = AxB px , C fps, box with D fps, image = ExF px, G fps divider)|
|bu|Number|set pre-trigger video buffer in mSec (approx)|
|as|Number|Set text size 0-99|
|at|E YYY UUU VVV|Set Text colour E (0/1 enable ) Colour as Y:U:V|
|ac|E YYY UUU VVV|Set background colour E (0/1 enable ) Colour as Y:U:V|
|sh|Number|set sharpness (range: [-100;100]; default: 0)|
|co|Number|set contrast (range: [-100;100]; default: 0)|
|br|Number|set brightness (range: [0;100]; default: 50)|
|sa|Number|set saturation (range: [-100;100]; default: 0)|
|is|Number|set ISO (range: [100;800]; default: 0=auto)|
|vs|Number|0/1 turn off/on video stabilisation|
|ec|Number|set exposure compensation (range: [-10;10]; default: 0)|
|em|Keyword|set exposure mode (range: [off/auto/night/nightpreview/backlight/spotlight/sports/snow/beach/verylong/fixedfps/antishake/fireworks]; default: auto)|
|wb|Keyword|set white balance (range: [off/auto/sun/cloudy/shade/tungsten/fluorescent/incandescent/flash/horizon]; default: auto)|
|ag|RRR BBB|set white balance off red_gain blue gain (100 = 1.0; default: 150)|
|mm|Keyword|set metering mode (range: [average/spot/backlit/matrix]; default: average)|
|ie|Keyword|set image effect (range: [none/negative/solarise/posterize/whiteboard/blackboard/sketch/denoise/emboss/oilpaint/hatch/gpen/pastel/watercolour/film/blur/saturation/colourswap/washedout/posterise /colourpoint/colourbalance/cartoon]; default: none)|
|ce|A BB CC|set colour effect (A=enable/disable, effect = B:C)|
|ro|Number|set rotation (range: [0/90/180/270]; default: 0)|
|fl|Number|Set horisontal flip(hflip) and vertical flip(vflip). 0={hflip=0,vflip=0}, 1={hflip=1,vflip=0}, 2={hflip=0,vflip=1}, 3={hflip=1,vflip=1}, default: 0|
|ri|AAAAA BBBBB CCCCC DDDDD|set sensor region (AAAAA BBBBB CCCCC DDDDD, x=A, y=B, w=C, h=D)|
|ss|Number|Set shutter speed|
|qu|Number|set output image quality (range: [0;100]; default: 85)|
|pv|QQ WWW DD|set preview quality (0-100) default 25, Width (128-1024) default 512, Divider (1-16) default 1|
|bi|Number|set output video bitrate (range: [0;25000000]; default: 17000000)|
|bo|Number|Set mp4 boxing mode 0=off,1=background|
|rl|0/1|0/1 disable / enable raw layer|
|ru|0/1|0/1 halt/restart RaspiMJPEG and release camera|
|rs|1|Reset user config to default|
|md|0/1|0/1 stop/start motion detection|
|ca|0/1 [t]|0/1 stop/start video capture, t if present specifies capture duration in seconds|
|im|1|capture image|
|tl|0/1|Stop/start timelapse|
|tv|Number|Set timelapse between images number * 1/10 seconds.|
|vi|Number|Sets video split interval in seconds. 0 is no splitting.|
|sc|1|Scan and set video and image indexes|
|sy|macro|Execute macro|
|um|number macro|Update macro|
|vp|0/1|Turn motion vector_preview off/on|
|mn|Number|Set motion_noise parameter|
|mt|Number|Set motion_threshold parameter|
|mi|Filename|Set motion_image parameter|
|ms|Number|Set motion_initframes parameter|
|mb|Number|Set motion_startframes parameter|
|me|Number|Set motion_stopframes parameter|
|mz|1/0|Disable or enable motion mask|
|cn|1/2|Set camera number (Compute module only)|
|qp|A BB CC|Set h264 encoding pars A=minimise_frag BB=initial_quant CC=encode_qp|
|ls|Number|Set log_size|
|ig|ANA DIG|set image gain, analog and digital (100 = 1.0; default: 150)|
