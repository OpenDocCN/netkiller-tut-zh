# 部分 XV. Multimedia

## 第 186 章 Audio

### *Digital Audio Workstation*

http://revo-create.com/viewthread.php?tid=11731

混音系列

1.  ardour

2.  Pulseaudio

取样器

1.  Qsynth

## 1. ardour

http://www.ardour.org/

## 2. LMMS

http://lmms.sourceforge.net/

## 3. Qsynth

http://qsynth.sourceforge.net/

## 4. Rosegarden

http://www.rosegardenmusic.com/

## 5. TerminatorX

http://terminatorx.org/

## 6. Pulseaudio

http://pulseaudio.org/

## 7. Synthesizer

合成器

1.  ZynAddSubFX

### 7.1. ZynAddSubFX

http://zynaddsubfx.sourceforge.net/

## 8. Drums

### 8.1. Hydrogen

#### Hydrogen is an advanced drum machine for GNU/Linux. It's main goal is to bring professional yet simple and intuitive pattern-based drum programming.

http://www.hydrogen-music.org/

## 第 187 章 Video

[List of Open Source Video Software](http://openvideoalliance.org/wiki/index.php?title=List_of_Open_Source_Video_Software)

## 1. OpenShot

[`www.openshot.org`](http://www.openshot.org)

## 2. cinelerra-cv

[`cinelerra.org/`](http://cinelerra.org/)

## 3. FFmpeg

[`ffmpeg.org/`](http://ffmpeg.org/)

Converting video and audio has never been so easy.

```
$ ffmpeg -i input.mp4 output.avi	

```

## 第 188 章 Graphics

## 1. ImageMagick

homepage: http://www.imagemagick.org/

### 1.1. install

```

$ sudo apt-get install imagemagick

```

### 1.2. convert

#### 1.2.1. 批量转换

```
convert *.jpg gkp-*.png

```

#### 1.2.2. resize

批量修改图片尺寸

```

find ./ -name '*.jpg' -exec convert -resize 600x480 {} {} \;

```

以长边为准

```
for img in $(find ./album/ -type f -name *.jpg)
do
        width=$(identify -format "%w" $img)
        height=$(identify -format "%h" $img)
        if [ $width -gt $height ]; then
                convert -resize 900x600 $img $img
        else
                convert -resize 600x900 $img $img
        fi
done

```

#### 1.2.3. PDF to PNG

将 PDF 文档每页生成一个 PNG 图片

```
convert -quality 05 NetkillerVersion.pdf output.png 

```

查看结果

```
$ ls output-*
output-0.png    output-14.png  output-20.png  output-27.png  output-33.png  output-3.png   output-46.png  output-52.png  output-59.png  output-65.png  output-71.png  output-78.png  output-84.png  output-90.png  output-97.png
output-100.png  output-15.png  output-21.png  output-28.png  output-34.png  output-40.png  output-47.png  output-53.png  output-5.png   output-66.png  output-72.png  output-79.png  output-85.png  output-91.png  output-98.png
output-101.png  output-16.png  output-22.png  output-29.png  output-35.png  output-41.png  output-48.png  output-54.png  output-60.png  output-67.png  output-73.png  output-7.png   output-86.png  output-92.png  output-99.png
output-10.png   output-17.png  output-23.png  output-2.png   output-36.png  output-42.png  output-49.png  output-55.png  output-61.png  output-68.png  output-74.png  output-80.png  output-87.png  output-93.png  output-9.png
output-11.png   output-18.png  output-24.png  output-30.png  output-37.png  output-43.png  output-4.png   output-56.png  output-62.png  output-69.png  output-75.png  output-81.png  output-88.png  output-94.png
output-12.png   output-19.png  output-25.png  output-31.png  output-38.png  output-44.png  output-50.png  output-57.png  output-63.png  output-6.png   output-76.png  output-82.png  output-89.png  output-95.png
output-13.png   output-1.png   output-26.png  output-32.png  output-39.png  output-45.png  output-51.png  output-58.png  output-64.png  output-70.png  output-77.png  output-83.png  output-8.png   output-96.png			

```

## 2. GraphicsMagick

http://www.graphicsmagick.org/

### 2.1. 安装

#### 2.1.1. CentOS 安装

```
yum install GraphicsMagick		

```

#### 2.1.2. 编译安装

```

tar zxf GraphicsMagick-1.3.12.tar.gz
cd GraphicsMagick-1.3.12
./configure --prefix=/srv/GraphicsMagick-1.3.12
make && make install
ln -s /srv/GraphicsMagick-1.3.12/ /srv/GraphicsMagick

```

### 2.2. mogrify

格式转换

```
/usr/bin/gm mogrify -format png *.jpg

```

## 3. Photivo

## 4. How to add metadata to digital pictures from the command line

exiftool

## 第 189 章 Music score

## 1. LilyPond

http://lilypond.org/

```
sudo apt-get install lilypond

```

### 1.1. Example

#### 1.1.1. PNG/PDF/PS

```

\version "2.14.2"
\relative c'' {
  <<
    \new Staff { \clef "treble" c4 }
    \new Staff { \clef "bass" c,,4 }
  >>
}

```

```
lilypond --png -o abc.png example.ly
lilypond --pdf -o abc example.ly

```

#### 1.1.2. Latex

```

\documentclass{article}
\begin{document}
    An easy song to learn on the piano is Mary Had a Little Lamb:\\
    \begin{lilypond}
        \score { % start of musical score
          <<
            % beginning of musical staff. the \relative c' means that the
            % notes are an octave higher than the default (ie: notes are
            % notes are relative to middle c)
            \new Staff \relative c' {
                e4 d c d e e e2 d4 d d2 e4 g g2
                e4 d c d e e e e d d e d c1
            } % end of staff
          >>
        } % end of musical score
    \end{lilypond}
\end{document}

```

```
lilypond-book --format=latex --lily-output-dir=lilyfiles mary.lytex
latex mary.tex

```

```
pdflatex --shell-escape your.tex

```

## 2. MuseScore

http://musescore.org/

乐谱制作

## 第 190 章 Stream

## 1. broadcast streaming

### 1.1. gnump3d - A streaming server for MP3 and OGG files

过程 190.1. 

1.  installation

    ```
    $ sudo apt-get install gnump3d				

    ```

2.  configure

    ```
    $ sudo vim /etc/gnump3d/gnump3d.conf

     root = /var/music

    ```

3.  copy some mp3 file to directory /var/music

4.  testing

    http://127.0.0.1:8888/

### 1.2. icecast2 - Ogg Vorbis and MP3 streaming media server

http://www.icecast.org/

#### 1.2.1. 

过程 190.2. 

1.  installation

    ```
    $ sudo apt-get install icecast2

    ```

2.  configure

    /etc/default/icecast2

    ```
    $ sudo vim /etc/default/icecast2
     #ENABLE=false
     ENABLE=true					

    ```

    /etc/icecast2/icecast.xml

    ```

          <authentication>
              <!-- Sources log in with username 'source' -->
              <source-password>your-password</source-password>
              <!-- Relays log in username 'relay' -->
              <relay-password>your-password</relay-password>

              <!-- Admin logs in with the username given below -->
              <admin-user>admin</admin-user>
              <admin-password>your-password</admin-password>
          </authentication>

    ```

3.  starting

    ```
    $ sudo /etc/init.d/icecast2 start

    ```

4.  testing

    http://localhost:8000/

#### 1.2.2. installation from source

过程 190.3. 配置步骤

1.  安装 lib 库

    ```
    netkiller@Linux-server:~/icecast-2.3.1$ sudo apt-get install libxslt1.1
    netkiller@Linux-server:~/icecast-2.3.1$ sudo apt-get install libxslt1-dev
    netkiller@Linux-server:~/icecast-2.3.1$ sudo apt-get install libshout3
    netkiller@Linux-server:~/icecast-2.3.1$ sudo apt-get install libshout3-dev

    ```

2.  $ sudo ./configure --prefix=/usr/local/icecast

    make;make install

    ```
    netkiller@Linux-server:~/icecast-2.3.1$ ./configure --prefix=/usr/local/icecast
    netkiller@Linux-server:~/icecast-2.3.1$ make
    netkiller@Linux-server:~/icecast-2.3.1$ sudo make install
    netkiller@Linux-server:~/icecast-2.3.1$ cd /usr/local/icecast/
    netkiller@Linux-server:/usr/local/icecast$ ls
    bin  etc  share

    ```

    创建 icecast2 用户

    修改所有者

    ```
    netkiller@Linux-server:/usr/local/icecast$ cd ..
    netkiller@Linux-server:/usr/local$ adduser icecast2
    netkiller@Linux-server:/usr/local$ sudo chown icecast2.icecast2 -R icecast/

    ```

3.  运行 icecast

    ```
    netkiller@Linux-server:/usr/local$ su icecast2
    netkiller@Linux-server:/usr/local$ /usr/local/icecast/bin/icecast -b -c /usr/local/icecast/etc/icecast.xml

    ```

4.  配置 icecast

    管理员/密码

    admin-user: 管理员用户名

    admin-password: 管理员密码

    ```
    icecast2@Linux-server:/usr/local/icecast$ vi etc/icecast.xml

        <authentication>
            <!-- Sources log in with username 'source' -->
            <source-password>hackme</source-password>
            <!-- Relays log in username 'relay' -->
            <relay-password>hackme</relay-password>

            <!-- Admin logs in with the username given below -->
            <admin-user>admin</admin-user>
            <admin-password>chen</admin-password>
        </authentication>

    ```

5.  测试 http://netkiller.8800.org:8000/

### 1.3. shoutcast

shoutcast...

### 1.4. PeerCast

homepage: http://www.peercast.org/

## 2. WebRTC

https://webrtc.org/

WebRTC is a free, open project that provides browsers and mobile applications with Real-Time Communications (RTC) capabilities via simple APIs. The WebRTC components have been optimized to best serve this purpose.

## 第 191 章 RTSP Server

https://www.linux-projects.org/uv4l/tutorials/rtsp-server/

## 第 192 章 其他命令