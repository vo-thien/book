---
description: https://docs.nestjs.com/
---

# nest.js

Khởi tạo dự án

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
 npm i -g @nestjs/cli
 nest new project-name
```
{% endcode %}

* Khi chỉ định project name thì sẽ tạo ra folder mới ⇒ Nếu không muốn tạo ra folder mới thì sử dụng dấu . thay cho project name

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
nest new .
```
{% endcode %}



Mặc định sử dụng Typescript



Cấu hình không tạo file test mỗi khi generate

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
"generateOptions": {
    "spec": false
  }
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

⇒ File nest-cli.json ⇒ Là file cấu hình của nestjs cli



Tạo một resource hoàn chỉnh (CRUD)

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
nest generate resource admin
```
{% endcode %}

Điều này sẽ tự động tạo ra

<table data-header-hidden><thead><tr><th align="center"></th><th align="center"></th><th data-hidden></th></tr></thead><tbody><tr><td align="center">admin.module.ts</td><td align="center">module chính</td><td></td></tr><tr><td align="center">admin.controller.ts</td><td align="center">controller</td><td></td></tr><tr><td align="center">admin.service.ts</td><td align="center">service</td><td></td></tr><tr><td align="center">DTO</td><td align="center">DTO</td><td></td></tr><tr><td align="center">entity</td><td align="center">entity</td><td></td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>





Khi build sẽ tạo ra file trong folder dist

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
nest build --webpack => Chỉ tạo 1 file duy nhất
nest run build => Ra nhiều file và folder (Được biên dịch bởi tsc, không đóng gói)
```
{% endcode %}



Xem thông tin dự án

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
nest info
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>



Provider mục đích giúp container quản lí class để inject ⇒ Sử dụng `@Injectable()`&#x20;

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>



## AOP

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>



1. **Middleware**

Middleware global

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

⇒ Đều phải đi qua middleware trước khi vào trong



Middleware router

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
nest g middleware auth --flat

--flat: Không tạo thêm folder theo tên middleware
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

* Nếu muốn chỉ định một api cụ thể thì có thể thêm tiếp consumer.apply&#x20;



2. **Guard**

Nếu true thì vào controller, False thì 403

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
nest g guard auth --flat
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>



Muốn áp dụng thì sử dụng @UseGuards()

<figure><img src="../../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>



Trường hợp muốn thêm guard ở Global

<figure><img src="../../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>



Còn cách khác để áp dụng guard ở global là thêm provider trong app.module.ts

<figure><img src="../../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>



3. **Interceptor**

Can thiệp trực tiếp vào quá trình xử lí request và response | middleware không làm được điều này

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
nest g interceptor time --flat
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>



Muốn áp dụng thì sử dụng `@UseInterceptors`&#x20;

<figure><img src="../../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>



Muốn áp dụng cho tất cả api của controller đó ⇒ Chuyển lên đầu

<figure><img src="../../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>



**Pipe**

Validate đầu vào và xử lí ngoại lệ

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
nest g pipe validate --flat
```
{% endcode %}



<figure><img src="../../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>



Ngoài ra có thể áp dụng cho toàn bộ api bằng cách sử dụng `@UsePipes` cho lên đầu

<figure><img src="../../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>



Cũng có thể áp dụng cho Global

<figure><img src="../../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>



4. **Exception Filter**

Thống nhất exception với toàn bộ hệ thống

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
nest g filter test --flat
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>



Muốn sử dụng thì dùng `@UseFilters`&#x20;

<figure><img src="../../.gitbook/assets/image (76).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>





## Hướng dẫn debug nestjs

Quá trình debug diễn ra

1. Server sẽ mở port để đợi debugger kết nối tới.
2. Debugger kết nối tới cổng mở đó
3. Khi server chạy code đến đoạn breakpoint thì sẽ dừng và gửi thông tin về cho debugger (dừng dòng nào, giá trị biến đang là gì, call stack như nào, ...)
4. Debugger (Vscode, ...) sẽ hiển thị thông tin và thao tác (step over, step into, ...)





Tại khu vực Run And Debug của IDE ⇒ Tạo file launch.json

<figure><img src="../../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>



Chọn attach

<figure><img src="../../.gitbook/assets/image (59).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>



Khởi động ứng dụng nestjs ở chế độ debug

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
nest start --debug
```
{% endcode %}

* Build và chạy ứng dụng
* Sử dụng ngầm flag --inspect

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>



Lúc này chỉ cần đặt breakpoint tại điểm muốn debug và nhấn start tại khu vực Run And Debug của IDE

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>







