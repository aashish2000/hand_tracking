## Hand tracking

### 1. ファイル説明
- `palm_detection.tflite`（手のひら検出）モデルファイルと`hand_landmark.tflite`（ランマーク検出）モデルファイル：[*mediapipe*]レポジトリよりダウンロードしました。
- `anchors.csv`ファイルと`hand_tracker.py`ファイル：[*hand_tracking*]レポジトリよりダウンロードしました。
- `tensorflow-1.14.1-cp36-cp36m-linux_x86_64.whl`wheelファイル：[*tensorflow*ソースからのビルド]と[*hand_tracking*]レポジトリの説明を参考し、Python APIによる上記のモデルファイルを使えるこのカスタムwheelファイルを*tensorflow*ソースから作成しました。
- `convert.py`ファイル：この[gist]よりコピーしました。

### 2. 実施方法
- *Docker*のimageを作成します：
<pre>
$ docker build -t <i>hand_tracking</i> .
</pre>

- ホストのWebcamをアクセスできる*Docker*のcontainerを使用方法について、この[story]を参考してください。<br/>
例：ホストがmacOSの場合：
<pre>
$ open -a XQuartz
$ socat TCP-LISTEN:6000,reuseaddr,fork UNIX-CLIENT:\"$DISPLAY\"
$ export IP=$(ifconfig en0 | grep inet | awk '$1=="inet" {print $2}')
$ /usr/X11/bin/xhost + $IP
$ docker run --rm --device=/dev/video0:/dev/video0 -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$IP:0 <i>hand_tracking</i>
</pre>

### 3. 結果
![Result](/output.gif?raw=true "Result")

[*mediapipe*]: https://github.com/google/mediapipe/tree/master/mediapipe/models
[*hand_tracking*]: https://github.com/wolterlw/hand_tracking
[*tensorflow*ソースからのビルド]: https://www.tensorflow.org/install/source#docker_linux_builds
[gist]: https://gist.github.com/michaelosthege/cd3e0c3c556b70a79deba6855deb2cc8
[story]: https://medium.com/@jijupax/connect-the-webcam-to-docker-on-mac-or-windows-51d894c44468
