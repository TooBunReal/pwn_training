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

![image](https://github.com/TooBunReal/pwn_training/assets/89735990/4b231de1-6ddd-4280-b94d-c2f52f58bc3a)

- Tại hàm vuln ta thấy giá trị của rsi sẽ chứa địa chỉ của buffer.

![image](https://github.com/TooBunReal/pwn_training/assets/89735990/2f565a62-40fa-4b3a-910c-35cd22ba40a8)

![image](https://github.com/TooBunReal/pwn_training/assets/89735990/4f4fdf1b-e6b2-47bb-bcaa-da5a0f580bd5)


![image](https://github.com/TooBunReal/pwn_training/assets/89735990/4bdd6d65-714d-4e4b-8a36-0fe3d8d218cf)

Tiến hành debug ta có được giá trị của buffer là 0x28 (40)
Bây giờ mình chỉ cần lắp đầy nó và từ rip trở đi, ta sẽ thực hiện được đoạn code mà mình muốn.
