---
layout: post
title: excel打开UTF-8编码的csv文件乱码问题解决
comments: true
category: misc
permalink: /:year/:month/:day/:title
tag: misc, tech
---

服务器是Linux，语言PHP，生成的csv文件在Mac和Windows上用Excel打开就悲剧的乱码了。在网上查了几个教程发现问题是UTF-8保存的csv格式文件要让Excel正常打开的话，必须加入在文件最前面加入[Byte Order Mark](https://en.wikipedia.org/wiki/Byte_order_mark)，简称BOM。这样接收者收到以EF BB BF开头的字节流，就知道这是UTF-8编码了。

```
$file_name = $type.'_data_'.substr(md5($type), 0, 5).'.csv';
$f = fopen($file_name, 'w');
$title = array('订单ID', '订单状态',);
fputs($f, "\xEF\xBB\xBF"); // 就是这一行！！！
fputcsv($f, $title, ',');
fseek($f, 0);
fclose($f);
header('Content-type: file/csv');
header('Content-Disposition: attachment; filename="'.$file_name.'"');
readfile($file_name);
exit;
```