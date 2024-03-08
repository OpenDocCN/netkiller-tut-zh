# 第 185 章 Graphics

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