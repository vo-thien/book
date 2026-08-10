# npm vs npx

npm: Cài đặt, gỡ, cập nhật package

npx: Chạy package ngay lập tức



npx khi chạy cũng sẽ tải package về nhưng lưu ở thư mục cache chứ không cài trên máy

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
C:\Users\theph\AppData\Local\npm-cache
```
{% endcode %}

Khi chạy lại lần nữa nó sẽ kiểm tra cache. Nếu tồn tại thì sử dụng luôn, nếu không tồn tại thì tải lại để chạy



