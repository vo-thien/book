---
hidden: true
icon: honey-pot
---

# Fuzzing

<details>

<summary>Source</summary>

{% file src="../.gitbook/assets/php-fuzzer-workshop.zip" %}

{% embed url="https://app.notion.com/p/Day-9-10-Fuzzing-on-PHP-userland-85b2e949068483e5895e812b98133a83?source=copy_link" %}

{% embed url="https://app.xmind.com/share/cCOhgiPT" %}

{% embed url="https://github.com/nikic/php-fuzzer" %}

</details>



Corpus: Mẫu hợp lệ ban đầu

Mutation: Tạo ra các biến thể từ corpus

Target: Thứ muốn fuzz

Harness: Dùng để setup hàm với input trước khi tiến hành fuzz

Oracle: Đưa khái niệm "hình như đây là bug" thành công thức if/else&#x20;



Input trong php-fuzzer là byte

Thông số coverage feedback&#x20;

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

* NEW ⇒ Khi phát hiện nhánh code mới (input mới)
* REDUCE ⇒ Tìm được input nhẹ hơn mà vẫn có coverage cao
* run ⇒ Số lần thực thi&#x20;
* mem ⇒ Lượng RAM đang dùng ⇒ Nếu ổn định thì oke, nếu thấy tăng không kiểm soát thì xem lại harness
*



Sử dụng @ để skip error/warning khi gọi hàm.



{% code overflow="wrap" lineNumbers="true" expandable="true" %}
```php
<?php

require_once __DIR__ . '/step0_target_normalize_path.php';

/** @var PhpFuzzer\Config $config */

if (!isset($config) && basename(__FILE__) === basename($_SERVER['SCRIPT_FILENAME'] ?? '')) {
    fwrite(STDERR, "This is a php-fuzzer target skeleton.\n");
    fwrite(STDERR, "Run it with:\n");
    fwrite(STDERR, "  php php-fuzzer.phar fuzz level3/step1_phpfuzzer_skeleton.php\n");
    exit(1);
}

// Define the target function
$config->setTarget(function(string $input) {

    $result = normalize_path($input);
    var_dump($result);
    if (strpos($result, '..') !== false) {
        throw new Error('Directory traversal detected');
    }

});

// Define the target function
// $config->setTarget(function(string $input) {

//     $result = normalize_path($input);
//     if (@realpath($result) == "/etc/passwd") {
//         throw new Error('Directory traversal detected');
//     }

// });

$config->setMaxLen(200); 
$config->addDictionary(__DIR__ . '/dict/cheatsheet.dict');



```
{% endcode %}

* Cần phải throw new Error thì mới dừng chứ throw new Exception thì không dừng đâu





Cần đi tìm corpus và dictionary phù hợp với target
