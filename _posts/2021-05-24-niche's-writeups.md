---
layout: post
title: niche's writeups
subtitle: Hello guys, It's all niche.
tags: [pwn, misc, forensics, reverse]
comments: true
---

## PWN
### mybirthday
author: pakkunandy, 50 points

####Investigate: 
Đề cho ta một file elf 32bit, đầu tiên ta chạy thử:
Chương trình yêu cầu chúng ta nhập vào và xuất ra thời gian theo hệ thống.

Ta dissassembly hàm main của chương trình:  
~~~
   0x080492bb <+60>:	add    esp,0x10
   0x080492be <+63>:	sub    esp,0x4
   0x080492c1 <+66>:	push   0x1e
   0x080492c3 <+68>:	lea    eax,[ebp-0x24]
   0x080492c6 <+71>:	push   eax
   0x080492c7 <+72>:	push   0x0
   0x080492c9 <+74>:	call   0x80490a0 <read@plt>
   0x080492ce <+79>:	add    esp,0x10
-->0x080492d1 <+82>:	cmp    DWORD PTR [ebp-0xc],0xcabbfeff
   0x080492d8 <+89>:	jne    0x80492ee <main+111>
   0x080492da <+91>:	sub    esp,0xc
   0x080492dd <+94>:	lea    eax,[ebx-0x1fe1]
   0x080492e3 <+100>:	push   eax
   0x080492e4 <+101>:	call   0x8049254 <run_cmd>
   0x080492e9 <+106>:	add    esp,0x10
   0x080492ec <+109>:	jmp    0x8049300 <main+129>
   0x080492ee <+111>:	sub    esp,0xc
   0x080492f1 <+114>:	lea    eax,[ebx-0x1fd7]
   0x080492f7 <+120>:	push   eax
   0x080492f8 <+121>:	call   0x8049254 <run_cmd>
~~~
Chương trình sẽ đọc vào một chuỗi có độ dài 0x1e và đọc vào [ebp-0x24], sau đó kiểm tra [ebp-0xc] có bằng 0xcabbfeff hay không. 
Boom! Ez overflow!

> Flag: **HCMUS-CTF{Just_A_Warm_Up_Pwn}**



Here's a useless table:

| Number | Next number | Previous number |
| :------ |:--- | :--- |
| Five | Six | Four |
| Ten | Eleven | Nine |
| Seven | Eight | Six |
| Two | Three | One |


How about a yummy crepe?

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg)

It can also be centered!

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg){: .mx-auto.d-block :}

Here's a code chunk:

~~~
var foo = function(x) {
  return(x + 5);
}
foo(3)
~~~

And here is the same code with syntax highlighting:

```javascript
var foo = function(x) {
  return(x + 5);
}
foo(3)
```

And here is the same code yet again but with line numbers:

{% highlight javascript linenos %}
var foo = function(x) {
  return(x + 5);
}
foo(3)
{% endhighlight %}

## Boxes
You can add notification, warning and error boxes like this:

### Notification

{: .box-note}
**Note:** This is a notification box.

### Warning

{: .box-warning}
**Warning:** This is a warning box.

### Error

{: .box-error}
**Error:** This is an error box.
