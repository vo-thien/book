# NukeViet

## Hướng dẫn install

**Bước 1**: Tải source từ [https://github.com/nukeviet/nukeviet/releases](https://github.com/nukeviet/nukeviet/releases)

**Bước 2**: Chuyển vào htdocs trong xampp

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

**Bước 3**: Start xampp và truy cập [http://localhost/phpmyadmin](http://localhost/phpmyadmin) để tạo database

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

**Bước 4**: Truy cập [http://localhost/nukeviet-4.6.02/install/index.php](http://localhost/nukeviet-4.6.02/install/index.php) để tiến hành cài đặt

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Cho phép extension zip và gd

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Điền thông tin Database và Submit

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

Tạo tài khoản admin ban đầu

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>



## Hướng dẫn setup debug

**Bước 1**: Kiểm tra phiên bản php

Tạo file tại C:\xampp\htdocs\phpinfo.php với nội dung

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
<?php phpinfo(); ?>
```
{% endcode %}

Sau đó truy cập http://localhost/phpinfo.php và copy toàn bộ nội dung của trang web đó (Ctrl + A => Ctrl + C)

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>



**Bước 2**: Truy cập Xdebug Wizard

Truy cập [https://xdebug.org/wizard](https://xdebug.org/wizard) và paste nội dung vừa copy ở Bước 2 vào ô trống sau đó nhấn Analyse my phpinfo() output

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

Sau khi Analyse hãy làm theo hướng dẫn số 1 và số 2

<figure><img src="../../../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>

Tại số 3 khi chỉnh sửa php.ini thì thêm đoạn này vào cuối file sau đó restart apache (giống hướng dẫn số 4)

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
[xdebug]
zend_extension = "C:\xampp\php\ext\php_xdebug.dll"
xdebug.mode = debug
xdebug.start_with_request = yes
xdebug.client_port = 9003
xdebug.client_host = localhost
xdebug.idekey = VSCODE
xdebug.log = "C:\xampp\php\logs\xdebug.log"
```
{% endcode %}

&#x20;

**Bước 3**: Cấu hình Vscode

Cài đặt extension PHP Debug

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

Tạo file launch.json trong thư mục .vscode với nội dung

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Listen for Xdebug",
            "type": "php",
            "request": "launch",
            "port": 9003
        }
    ]
}

```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>



**Bước 4**: Đặt breakpoint và start debug

<figure><img src="../../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>









