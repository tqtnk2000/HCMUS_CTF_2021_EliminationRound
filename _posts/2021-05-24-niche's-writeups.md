---
layout: post
title: niche's writeups
subtitle: Hello guys, It's all niche.
tags: [pwn, misc, forensics, reverse]
comments: true
---

# PWN
### mybirthday
author: pakkunandy, 50 points


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

Boom! Ez bufferoverflow!

> Flag: **HCMUS-CTF{Just_A_Warm_Up_Pwn}**

### bank1 
author: xikhud, 50 points

Không hề có file binary và chỉ có server để connect!!!???
Vì giá trị của challenge chỉ nằm ở con số 50 nên mình cũng đoán được bài này sẽ như thế nào :D
Mạnh dạn in thẳng vài trăm ký tự vào server =>> Boom =>> Flag  
Thực ra mình nghĩ bài này mục đích chỉ là overflow được con số balance của tài khoản, vì thử phát trúng luôn nên mình cũng không suy nghĩ thêm!
```javascript
python3 -c "print('A'*100)" | nc server.quenmattiuroi portgiday
```


> Flag: **HCMUS-CTF{that_was_easy_xd}**

### bank2
author: xikhud, 100 points

Đọc assembly của bài này ta thấy chương trình nhận vào input của người dùng. Và hàm nhận vào input là hàm *gets()*. Vâng, _old buddy **gets**_, một hàm tuyệt vời của C được mô tả trên man page như sau:

> #### DESCRIPTION
       _Never use this function._

       gets() reads a line from stdin into the buffer pointed to by s
       until either a terminating newline or EOF, which it replaces with
       a null byte ('\0').  No check for buffer overrun is performed
       (see BUGS below).

Nhiệm vụ của ta là overflow được _[ebp-0xc]_ bằng với giá trị _0x66a44_:

~~~
   0x08048554 <+35>:	call   0x8048390 <printf@plt>
   0x08048559 <+40>:	add    esp,0x10
   0x0804855c <+43>:	sub    esp,0xc
   0x0804855f <+46>:	lea    eax,[ebp-0x4c]
   0x08048562 <+49>:	push   eax
   0x08048563 <+50>:	call   0x80483a0 <gets@plt>
   0x08048568 <+55>:	add    esp,0x10
   0x0804856b <+58>:	sub    esp,0x8
   0x0804856e <+61>:	push   DWORD PTR [ebp-0xc]
   0x08048571 <+64>:	lea    eax,[ebx-0x1964]
   0x08048577 <+70>:	push   eax
   0x08048578 <+71>:	call   0x8048390 <printf@plt>
   0x0804857d <+76>:	add    esp,0x10
-->0x08048580 <+79>:	cmp    DWORD PTR [ebp-0xc],0x66a44
   0x08048587 <+86>:	jne    0x804858e <Register+93>
   0x08048589 <+88>:	call   0x8048506 <getFlag>
   0x0804858e <+93>:	nop
   0x0804858f <+94>:	mov    ebx,DWORD PTR [ebp-0x4]
   0x08048592 <+97>:	leave  
   0x08048593 <+98>:	ret    
~~~

> Flag: **HCMUS-CTF{little_endian_is_fun}**

### bank3
author: xikhud, 100 points

Không hề có file binary và chỉ có server để connect!!!???
Vì giá trị của challenge chỉ nằm ở con số 50 nên mình cũng đoán được bài này sẽ như thế nào :D
Mạnh dạn in thẳng vài trăm ký tự vào server =>> Boom =>> Flag  
Thực ra mình nghĩ bài này mục đích chỉ là overflow được con số balance của tài khoản, vì thử phát trúng luôn nên mình cũng không suy nghĩ thêm!
```javascript
python3 -c "print('A'*100)" | nc server.quenmattiuroi portgiday
```


> Flag: **HCMUS-CTF{overwrite_all_the_things}**


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
