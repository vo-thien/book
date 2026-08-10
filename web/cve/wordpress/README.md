# WordPress

## Hướng dẫn setup debug

Yêu cầu:

* Xampp
* Phiên bản mã nguồn Wordpress
* Vscode



**Bước 1**

Giải nén mã nguồn wordpress vào thư mục C:\xampp\htdocs

<figure><img src="../../../.gitbook/assets/image (116).png" alt=""><figcaption></figcaption></figure>

**Bước 2**: Tạo database

Truy cập [http://localhost/phpmyadmin](http://localhost/phpmyadmin) để tạo

**Bước 3**:&#x20;

Truy cập [http://localhost/wp701](http://localhost/wp701) và điền thông tin để Install WordPress

<figure><img src="../../../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>

**Bước 4**

Chỉnh sửa file C:\xampp\htdocs\wp701\wp-config.php, thêm vào ngay bên trên dòng /\* That's all, stop editing! Happy publishing. \*/&#x20;

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
define( 'WP_DEBUG', true );
define( 'WP_DEBUG_LOG', true );
define( 'WP_DEBUG_DISPLAY', false );
@ini_set( 'display_errors', 0 );
define( 'SCRIPT_DEBUG', true );
define( 'WP_ENVIRONMENT_TYPE', 'local' );
define( 'WP_AUTO_UPDATE_CORE', false );
```
{% endcode %}

Bước 5

Tạo file launch.json trong thư mục .vscode

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Listen for Xdebug",
            "type": "php",
            "request": "launch",
            "port": 9003,
            "pathMappings": {
                "C:/xampp/htdocs/wp701": "${workspaceFolder}"
            }
        }
    ]
}
```
{% endcode %}





