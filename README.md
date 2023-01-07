# HW4

# Steps

## A. Open RTMP Server

- **Reference:** https://github.com/twtrubiks/nginx-rtmp-tutorial

### 1. Local to Server

```yaml
version: '3.5'
services:

  nginx-rtmp:
    image: tiangolo/nginx-rtmp
    restart: always
    container_name: nginx-rtmp
    ports:
      - "9099:1935"
      - "9098:80"
    volumes:
      - ./nginx_simple.conf:/etc/nginx/nginx.conf
      - ./stat.xsl:/usr/local/nginx/html/stat.xsl
```

### 2. Capture the frame and to Server

```yaml
version: '3.5'
services:

  nginx-rtmp:
    image: tiangolo/nginx-rtmp
    restart: always
    container_name: nginx-rtmp
    ports:
      - "9097:1935"
      - "9096:80"
    volumes:
      - ./nginx_simple.conf:/etc/nginx/nginx.conf
      - ./stat.xsl:/usr/local/nginx/html/stat.xsl
```

### 3. How to run the service?

```bash
$ docker-compose up -d
```

## B. Push the local stream to server

```bash
$ ffmpeg -f avfoundation -framerate 30 -i "{webcam id} or {video_path}" -s 1920x1080 -f flv rtmp://{server_ip}:9099/live/test
```

## C. Capture the stream and Model Inference

```bash
$ cd ~/video_stream/
$ python3 inference_2_server.py
```

## D. Make stream files of .m3u8 and .ts

```bash
$ cd ~/video_stream/video
$ python3 FFMPEG_Playlist.py —input_file rtmp://{server_ip}:9097/live/test
```

## E. Open FLASK Server

```bash
$ cd ~/video_stream/
$ python3 app.py
```

- The live stream can be showed in http://{server_ip}:9090