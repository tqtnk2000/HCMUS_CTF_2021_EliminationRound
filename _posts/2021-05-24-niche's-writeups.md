---
layout: post
title: niche's writeups
subtitle: Hello guys, it's a niche writeup
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

>Thực ra mình nghĩ bài này mục đích chỉ là overflow được con số balance của tài khoản, vì thử phát trúng luôn nên mình cũng không suy nghĩ thêm!

```
python3 -c "print('A'*100)" | nc server.quenmattiuroi portgiday
```

> Flag: **HCMUS-CTF{that_was_easy_xd}**

### bank2
author: xikhud, 100 points

Đọc assembly của bài này ta thấy chương trình nhận vào input của người dùng. Và hàm nhận vào input là hàm *gets()*. 

Vâng, _old buddy **gets**_, một hàm tuyệt vời của C được mô tả trên man page như sau:

> #### DESCRIPTION
       > _ Never use this function. _

       > gets() reads a line from stdin into the buffer pointed to by s until either a terminating newline or EOF, which it replaces with a null byte ('\0').  No check for buffer overrun is performed.

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

Làm gì làm, đọc assembly trước đã :P

~~~
   0x08048531 <+0>:		push   ebp
   0x08048532 <+1>:		mov    ebp,esp
   0x08048534 <+3>:		push   ebx
   0x08048535 <+4>:		sub    esp,0x54
   0x08048538 <+7>:		call   0x8048440 <__x86.get_pc_thunk.bx>
   0x0804853d <+12>:	add    ebx,0x1ac3
   0x08048543 <+18>:	mov    DWORD PTR [ebp-0xc],0x0
   0x0804854a <+25>:	sub    esp,0xc
   0x0804854d <+28>:	lea    eax,[ebx-0x1993]
   0x08048553 <+34>:	push   eax
   0x08048554 <+35>:	call   0x8048390 <printf@plt>
   0x08048559 <+40>:	add    esp,0x10
   0x0804855c <+43>:	sub    esp,0xc
   0x0804855f <+46>:	lea    eax,[ebp-0x4c]
   0x08048562 <+49>:	push   eax
   0x08048563 <+50>:	call   0x80483a0 <gets@plt>
   0x08048568 <+55>:	add    esp,0x10
   0x0804856b <+58>:	sub    esp,0x8
   0x0804856e <+61>:	push   DWORD PTR [ebp-0xc]
   0x08048571 <+64>:	lea    eax,[ebx-0x1974]
   0x08048577 <+70>:	push   eax
   0x08048578 <+71>:	call   0x8048390 <printf@plt>
   0x0804857d <+76>:	add    esp,0x10
   0x08048580 <+79>:	nop
   0x08048581 <+80>:	mov    ebx,DWORD PTR [ebp-0x4]
   0x08048584 <+83>:	leave  
   0x08048585 <+84>:	ret    
~~~

Hmm, lần này ta thấy chương trình không hề kiểm tra giá trị gì để ta lấy được flag cả, nhưng để ý những hàm của chương trình này đi:

~~~
db-peda$ info functions
All defined functions:

File ./src/bank3.c:
9:	void Register();
4:	void getFlag();
18:	int main(int, char **);
~~~

Một hàm *getFlag()* ở ngay đây, và lần này ta chỉ việc overflow giá trị trả về với địa chỉ của hàm:

~~~
gdb-peda$ pdisas getFlag
Dump of assembler code for function getFlag:
-->0x08048506 <+0>:		push   ebp <-- "I'm here, guyss"
   0x08048507 <+1>:		mov    ebp,esp
   0x08048509 <+3>:		push   ebx
   0x0804850a <+4>:		sub    esp,0x4
   0x0804850d <+7>:		call   0x80485dc <__x86.get_pc_thunk.ax>
   0x08048512 <+12>:	add    eax,0x1aee
   0x08048517 <+17>:	sub    esp,0xc
   0x0804851a <+20>:	lea    edx,[eax-0x19a0]
   0x08048520 <+26>:	push   edx
   0x08048521 <+27>:	mov    ebx,eax
   0x08048523 <+29>:	call   0x80483c0 <system@plt>
   0x08048528 <+34>:	add    esp,0x10
   0x0804852b <+37>:	nop
   0x0804852c <+38>:	mov    ebx,DWORD PTR [ebp-0x4]
   0x0804852f <+41>:	leave  
   0x08048530 <+42>:	ret    
End of assembler dump.
~~~


> Flag: **HCMUS-CTF{overwrite_all_the_things}**


### secretweapon
author: pakkunandy, 100 points

Tương tự bank3, bài này cũng là bài toán overflow return address.

Tuy nhiên, ở bài này ta thấy ASLR được bật, và tác giả đã đủ nhân hậu để leak cho ta một địa chỉ. 

Nhiệm vụ của ta bây giờ chỉ là tính địa chỉ tương đối giữa địa chỉ được leak ra và địa chỉ của hàm mục tiêu (hàm *arsenal()*).

> _Xin lỗi bạn đọc, nhưng tôi đã quá lười để viết writeup_ ._.

> Flag: **HCMUS-CTF{you_know_how_to_compute_location}**


### bank4 
author: xikhud, 200 points

Ok, bài 200 điểm đầu tiên. Chạy binary ta thấy vẫn như các bài trước không có gì mới mẻ, ta nhảy vào xem luôn assembly. Lần này mình decompile code để cho write up có vẻ _đẹp và sang_ hơn một chút :D

```c++
void Register(void)

{
  char name [64];
  int balance;
  
  printf("[+] Please enter your name: ");
  gets(name);
  printf("[+] Thanks for the registration, your balance is %d.\n",0);
  return;
}

void getFlag(void)

{
  if ((o1 == 0) || (o2 == 0)) {
    system("echo \"hcmasd-cft{nah_nah_nah_not_today}\"");
  }
  else {
    system("cat flag.txt"); //notice me
  }
  return;
}
```

Sau khi xem xét được 2 hàm thì ta thấy mục đích vẫn là đưa chương trình trở về hàm *getFlag()*. Tuy nhiên, để ý thì hàm này kiểm tra xem 2 biến _o1_ và _o2_ có bằng 0 không, và 2 biến này được lưu trên vùng nhớ của chương trình.

Hmm?!?

Nhưng ta đâu nhất thiết phải trở về đầu hàm  *getFlag()* đâu nhỉ? Nếu chúng ta ăn gian bớt thì sao?? 

Tuy nhiên, đoạn code ở trên không hề phản ánh rõ cái gì đang ở bên dưới máy tính, và ta dĩ nhiên không thể nhảy tùy tiện về dòng code *system("cat flag.txt");* được

~~~
   0x08048906 <+0>:		push   ebp
   0x08048907 <+1>:		mov    ebp,esp
   0x08048909 <+3>:		push   ebx
   0x0804890a <+4>:		sub    esp,0x4
 $ 0x0804890d <+7>:		call   0x8048a06 <__x86.get_pc_thunk.ax>
 $ 0x08048912 <+12>:	add    eax,0x916ee
   0x08048917 <+17>:	mov    edx,DWORD PTR [eax+0x133c]
   0x0804891d <+23>:	test   edx,edx
   0x0804891f <+25>:	je     0x8048941 <getFlag+59>
   0x08048921 <+27>:	mov    edx,DWORD PTR [eax+0x1340]
   0x08048927 <+33>:	test   edx,edx
   0x08048929 <+35>:	je     0x8048941 <getFlag+59>
 * 0x0804892b <+37>:	sub    esp,0xc
 * 0x0804892e <+40>:	lea    edx,[eax-0x2d838] 		-->'cat flag.txt'
 * 0x08048934 <+46>:	push   edx
 * 0x08048935 <+47>:	mov    ebx,eax
 * 0x08048937 <+49>:	call   0x804fc20 <system>
   0x0804893c <+54>:	add    esp,0x10
   0x0804893f <+57>:	jmp    0x8048955 <getFlag+79>
   0x08048941 <+59>:	sub    esp,0xc
   0x08048944 <+62>:	lea    edx,[eax-0x2d828]
   0x0804894a <+68>:	push   edx
   0x0804894b <+69>:	mov    ebx,eax
   0x0804894d <+71>:	call   0x804fc20 <system>
   0x08048952 <+76>:	add    esp,0x10
   0x08048955 <+79>:	nop
   0x08048956 <+80>:	mov    ebx,DWORD PTR [ebp-0x4]
   0x08048959 <+83>:	leave  
   0x0804895a <+84>:	ret    
~~~

Xem xét kĩ thì ta thấy hàm system nhận vào chỉ 1 tham số, chính là chuỗi '_cat flag.txt_', tuy nhiên chuỗi này được lấy bằng cách tính vị trí tương đối từ thanh ghi $eax.

Nếu lần lên trên ta sẽ thấy *$eax* đã được gán bằng giá trị 0x0848912 sau lời gọi hàm <__x86.get_pc_thunk.ax> (hàm này trả về giá trị của thanh ghi pc, chính là địa chỉ lệnh cần thực hiện tiếp theo), và sau đó được tăng thêm 0x916ee đơn vị. Vậy thanh ghi *$eax* bây giờ có giá trị là 0x080da000. 

Vậy làm sao để gán giá trị đó cho eax trước khi trở về?

ROPgadget hân hạnh tài trợ chương trình này! Tìm một gadget "pop eax; ret" hay tương tự và push thêm giá trị *$eax* mong muốn, sau đó trở về địa chỉ ngay trước khi lời gọi system được thực hiện và ta được flag!!

> Flag: **HCMUS-CTF{trungdeptrai}**

### bank5
author: xikhud, 200 points

Tiếp tục là một bài ROP, và lần này không có bất kỳ hàm *getFlag()* nào T.T

Tuy nhiênnnnn, ta chỉ cần ROP là đủ để chiếm shell của server, và lần này ta sẽ gọi thẳng syscall để boom luôn cái server.

Đơn giản chỉ cần tìm đủ những gadget cần thiết để chỉnh các thanh ghi và gọi hàm theo đúng như trong sách giáo khoa là được. 

Lần này mình xin phép nhường bạn đọc và không giải thích gì thêm ._.

> Hint: ở đây ta có thể gọi syscall execve('/bin/sh') và tùy biến các giá trị thanh ghi cho đúng với yêu cầu 

> Flag: **HHCMUS-CTF{rop_and_shellcode}**

### bank6
author: xikhud, 200 points

Lần này là một bài giới hạn input nhận vào (_bye bye gets, welcome to scanf_). Việc giới hạn này làm ta không thể overflow được giá trị trả về, nhưng 


> Flag: **HCMUS-CTF{0ff_by_on3}**



