# Web Fuzzing

## So sánh Fuzzing vs Brute Force

* Fuzzing: Bao gồm các kí tự lỗi định dạng, không hợp lệ, vô nghĩa
* Brute Force: Thử một cách có hệ thống nhiều khả năng khác nhau

⇒ Ví dụ: Khi mở cánh cửa bị khóa

* Fuzzing: Ném mọi thứ tìm thấy vào cửa (chìa khóa, tua vít, ...)
* Brute force: Thử mọi tổ hợp trên một chùm chìa khóa

## Cài đặt Go, Python và Pipx

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
```
{% endcode %}

## Wordlist

* [https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/common.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/common.txt)
* [https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/big.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/big.txt)
* [https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/raft-large-directories.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/raft-large-directories.txt)
* [https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/DirBuster-2007\_directory-list-2.3-medium.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt)

Kết hợp extension

* .php,.html,.txt,.bak,.js

## ffuf

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
go install github.com/ffuf/ffuf/v2@latest
```
{% endcode %}

### Directory and File Enumeration

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
ffuf -w common.txt -u http://154.57.164.81:32680/webfuzzing_hidden_path/FUZZ -e .php,.html,.txt,.bak,.js -v -ic -recursion -recursion-depth 3 -rate 500

-w: wordlist
-u: url
-e: extension
-v: verbose
-ic: ignore comment
-recursion: đệ quy
-recursion-depth 3: tối đa 3 cấp
-rate 500: Gửi tối đa 500 req/s
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

### Parameter Discovery

GET:

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
ffuf -w common.txt -u http://154.57.164.75:31265/get.php?x=FUZZ -rate 500
```
{% endcode %}

POST:

<pre class="language-bash" data-overflow="wrap" data-line-numbers data-expandable="true"><code class="lang-bash"><strong>ffuf -w common.txt -u http://154.57.164.75:31265/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -rate 500 -v
</strong><strong>
</strong><strong>-X: http method
</strong><strong>-H: Header
</strong><strong>-d: Dữ liệu phần body
</strong></code></pre>

<figure><img src="../../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
curl -d "y=SUNWmc" http://154.57.164.75:31265/post.php
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

## gobuster

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
go install github.com/OJ/gobuster/v3@latest
```
{% endcode %}

### VHost Fuzzing

Trong lab này chỉ có địa chỉ ip

```
154.57.164.66:30495
```

⇒ Cần cấu hình trỏ domain `inlanefreight.htb` tới địa chỉ ip bên trên

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
echo "154.57.164.66 inlanefreight.htb" | sudo tee -a /etc/hosts

- /etc/hosts không hỗ trợ port
- Muốn sửa /etc/hosts cần có quyền sudo, nếu sử dụng >> thì không sử dụng được sudo => Phải dùng lệnh tee

-a: append
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>



{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
gobuster vhost -u http://inlanefreight.htb:31032/ -w common.txt --append-domain --xl 157

vhost: Chế độ tìm virtual host
-w: wordlist
--append-domain: Nối domain vào sau mỗi từ trong wordlist
--xl 157: Loại bỏ response có size là 157
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>



### Subdomain Fuzzing

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
gobuster dns --domain inlanefreight.com -w subdomain.txt --no-error

dns: Check có tồn tại hay không bằng cách query dns
--domain: domain
-w: wordlist
--no-error: Không hiển thị lỗi khi domain không tồn tại
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>





## FeroxBuster

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
curl -sL https://raw.githubusercontent.com/epi052/feroxbuster/main/install-nix.sh | sudo bash -s $HOME/.local/bin
```
{% endcode %}

## wfuzz/wenum

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```
pipx install git+https://github.com/WebFuzzForge/wenum
pipx runpip wenum install setuptools
```
{% endcode %}



## Final Test

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
ffuf -w common.txt -u http://154.57.164.81:31500/FUZZ -e .php,.html,.txt -v -ic -recursion -fs 281
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>



Sau khi truy cập [http://154.57.164.81:31500/admin/panel.php](http://154.57.164.81:31500/admin/panel.php) thì nhận được thông báo invalid parameter

<figure><img src="../../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>



Fuzz parameter

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
ffuf -w common.txt -u http://154.57.164.81:31500/admin/panel.php?accessID=FUZZ -v -ic -fs 58
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>



{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
echo "154.57.164.81:31500 fuzzing_fun.htb" | sudo tee -a /etc/hosts
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

Fuzz vhost

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
gobuster vhost -u http://fuzzing_fun.htb:31500/ -w common.txt --append-domain --xs 403,400

--xs: Loại bỏ status
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>



Fuzz directory ở vhost vừa tìm được

{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```bash
ffuf -w common.txt -u http://fuzzing_fun.htb:31500/godeep/stoneedge/bbclone/FUZZ -H "Host: hidden.fuzzing_fun.htb:31500" -e .php,.html -v -ic -fs 290
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

