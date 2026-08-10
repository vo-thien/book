---
icon: docker
---

# Docker

Khi cài Docker cần cài thêm wsl vì Docker sử dụng các tính năng chỉ có trên Linux kernel (namspaces, cgroups, overlayfs, ...)



Wsl (Windows Subsystem for Linux) là tính năng cho phép chạy môi trường Linux trực tiếp trên máy windows

* Chạy một kernel linux bên trong một máy ảo nhẹ&#x20;
* Dễ dàng truy cập file windows ở /mnt



Sẽ có 2 loại lưu trữ để persist data: Bind mount và Volume

* Bind mount: Gắn trực tiếp thư mục trên máy host vào container
  * -v /đường/dẫn/trên/host:/đường/dẫn/trong/container
  * Phụ thuộc vào cấu trúc thư mục của host
  * Khi có thay đổi ở host thì trong docker cũng thay đổi và ngược lại
*   Volume: Docker quản lí việc lưu trữ

    * -v tên\_volume:/đường/dẫn/trong/container
    *   Có thể xem dữ liệu trực tiếp bằng docker desktop trong tab volume

        <figure><img src="../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>



    * Ngoài ra có thể xem dữ liệu thông qua terminal
      *   Liệt kê các distro đã cài trong wsl: `wsl -l`&#x20;

          <figure><img src="../.gitbook/assets/image (108).png" alt=""><figcaption></figcaption></figure>
      *   Truy cập vào distro docker-desktop: `wsl -d docker-desktop`

          <figure><img src="../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>


      *   Truy cập vào docker-desktop-disk

          <figure><img src="../.gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>


      *   Sau đó truy cập vào volume ở đường dẫn trong ảnh

          <figure><img src="../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>


      *   Xem nội dung của volume

          <figure><img src="../.gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>



## Dockerfile

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```docker
FROM node:latest

WORKDIR /app

COPY . .

RUN npm install -g http-server

EXPOSE 8080

CMD ["npx", "http-server", "-p", "8080"]
```
{% endcode %}

* **`FROM`**: Chọn image gốc làm nền
* **`WORKDIR`**: Thiết lập thư mục làm việc trong docker là /app (Các lệnh RUN, COPY, CMD, ... sẽ được thực hiện trong thư mục này)
* **`COPY`**: Sao chép thư mục hiện tại vào thư mục /app trong container
* **`RUN`**: Lệnh thực thi 1 lần khi build image
* **`EXPOSE`**: Khai báo rằng ứng dụng trong container sử dụng port 8080 (Chỉ cung cấp thông tin chứ không mở port)
* **`CMD`**: Lệnh mặc định được chạy khi container khởi động



## Tìm kiếm Image trên docker hub

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
docker search nginx
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>



## Pull Image từ docker hub

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
docker pull nginx:latest
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (103).png" alt=""><figcaption></figcaption></figure>



## Khởi tạo container từ Image

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
docker run --name nginx-demo -p 8080:80 -v C:\Users\theph\Desktop\html:/usr/share/nginx/html -e KEY1=VALUE1 -d nginx:latest

--name: Tên container
-p: 8080 là port của host, 80 là port trong docker
-v: Trong trường hợp này là bind mount
-e: Thiết lập các biến môi trường
-d: Chạy ở chế độ nền (Không chiếm terminal)
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (106).png" alt=""><figcaption></figcaption></figure>



## Xem container đang chạy + đã dừng

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
docker ps -a
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>



## Remove container

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
docker rm -f 0047849d28d3

-f: force
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>



## Thực thi lệnh trong container đang chạy

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
docker exec -it e51ca882ac88 bash

-i: interactive
-t: TTY - Cấp phát terminal ảo
bash: Lệnh muốn chạy trong container (Thường là shell: bash, sh, ...)
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>



## Xem log trong container

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
docker logs --tail 100 e51ca882ac88

--tail: Xem 100 dòng cuối
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (81).png" alt=""><figcaption></figcaption></figure>



## Build image từ source

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
docker build -t demo:1.0 -f Dockerfile.dev .

-t <name>:<tag>
-f: Dùng file Dockerfile.dev thay vì Dockerfile (Nếu không chỉ định thì mặc định là dùng Dockerfile)
.: Tìm Dockerfile và gửi toàn bộ nội dung của thư mục hiện tại để build
```
{% endcode %}



## Copy folder từ docker ra host

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
docker cp af500e757f5f:/app/node_modules ./node_modules
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (104).png" alt=""><figcaption></figcaption></figure>



