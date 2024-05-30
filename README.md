# Q-SYS-AVer-PTC310UV2-Camera
Q-Sys User Component (AVer PTC310UV2, PTC310HWV2, PTC320UNV2, PTC330UV2)

# Q-SYS-AVer-PTC310UV2-PTC310HWV2-PTC320UNV2-PTC330UV2-Camera
- Based on AVer PTC310 user component.

## NOTES:
- Audio Encoding Type control has been included (combo box) but only one option is available (AAC) for visibility. The camera does not have any set/get request to modify this. Only AAC is available for encoding audio.
- Have NOT added any of the 'Sleep Timer' functions on the System page as these are only available when video streaming is USB Only.

### 30th May 2024 - Glen Gorton
Tested running Firmware v0.1.0000.64 [D:H] (PTC310UV2) and v0.1.0000.64 [C:H] (PTC320UNV2)
- No modification to code.

### 31st May 2023 - Glen Gorton
Tested running Firmware v0.1.0000.48 [D:H] (PTC310UV2)

- No modification to code.

### 23rd March 2023 - Glen Gorton
Tested running Firmware v0.1.0000.43 [D:H] (PTC310UV2)

Additions:
- 'NTP' (net_ntp_enable) (Button Toggle) from the Network web interface tab added to Poll() function. Poll response example: net_ntp_enable=0
- 'NTP Server' (net_ntp_server) (Text Box) from the Network web interface tab added to GetUnitInfoNetwork() function. Poll response example: net_ntp_server="pool.ntp.org"
- 'SSHD' (sshd_enable) (Button Toggle) from the Network web interface tab added to Poll() function. Poll response example: sshd_enable=1
- 'HTTPS' (GetHttpsEnable) (Text Combo Box) from the Network web interface tab added to Poll() function. Poll response example: https_enable=0
- 'Visca Port Mode' (vsc_port_mode) (Text Combo Box) from the Network web interface tab added to Poll() function. Poll response example: vsc_port_mode=0
- 'Visca Port Number' (vsc_port_num) (Text Box) from the Network web interface tab added to GetUnitInfoNetwork() function. Poll response example: vsc_port_num="52381"

Modifications:
- Found the 'USB Audio' CGI command has changed and no longer requires a camera reboot. HttpDownload("GetUacEnable") disabled. CGI command is now Get=usb_audio_enable and Set=usb_audio_enable,3,1.
- Split the Poll() function into 9x smaller groups of CGI commands groups. A 1sec Timer.CallAfter seperates them.
- The pollTimer:Start(0) that was within the Capture() function has been commented out and moved into the Connect() function as Timer.CallAfter(Poll, 3). pollTimer:Start is restarted within the Poll() function.
- The pollTimer.EventHandler = Poll that was within the Capture() function has been moved to the end of the script.
- HLS Stream Status poll response changed so that it's not calling HLSStatusUpdate() after every response, but only if the response if different from the last. This prevents the GetUnitInfoNetwork() function being called on every response.
- RTMP Stream Status poll response changed so that it's not calling RTMPStatusUpdate() after every response, but only if the response if different from the last. This prevents the GetUnitInfoNetwork() function being called on every response.

### 18th November 2022 - Glen Gorton
Tested running Firmware v0.1.0000.35 [C:H]

Additions:
- 'RK Version' (rk_ver) (Indicator Text) from the System web interface tab added to GetUnitInfo() function.
- 'Status LiveView' (live_view_status) (Button Toggle) toggle from the System web interface tab added.
- 'USB Video Auto Tracking' (usb_stm_sync_trk_en) (Button Toggle) toggle from the System web interface tab added.
- 'Help Improve AVer Camera' (ga_user_event_trace_on) (Text Combo Box) Disable/Enable from the System web interface tab added.
- 'Tracking Gesture Control' (trk_gesture_mode) (Text Combo Box) drop-down from the Tracking Settings->Gesture (Beta) web interface tab added.
- 'RTSP Audio Enable' (sys_rtsp_audio_en) (Button Toggle) toggle from the Network web interface tab added.
- 'HLS Stream Status' (vdo_hls_status) (Indicator Text) text indicator from the Network web interface tab added.
- 'HLS Stream URL' (hls_url) (Text Box) text field from the Network web interface tab added.
- 'HLS Start-Stop Stream' (vdo_hls_enable) (Button Toggle) toggle from the Network web interface tab added.
- 'Video Color Mode' (sys_color_mode) (Text Combo Box) selection from the Video & Audio tab web interface tab added.
- 'Allow Resolution Under 720p' (res_under_720p_enable) (Button Toggle) toggle from the Video & Audio tab web interface tab added.
- 'Lens Distortion Correction' (img_ldc_mode) (Button Toggle) toggle from the Camera Settings web interface tab added.
- 'Multi-Presenter Detection' (trk_multi_detect) (Text Combo Box) (trk_multi_detect_preset) selection from the Tracking Settings web interface.

Modifications:
- 'Video Mode' has been updated to also include NDI.
- 'Preset Accuracy' control deteled. This toggle does not appear on the Live View->Preset web interface.
- 'Stream Video Output' resolution "960x540" removed from selection as this is not on the Video&Audio web interface.

Functional Changes:
- Commented out the SetStatus functions under the PreviewTimer EventHandler in the Capture() function. These statuses are ultimately set in the Result() function that is called for every HttpDownload and HttpUpload.
