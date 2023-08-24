# pwn_training
## bof
## fmt
### chall1

- src:
```c 
__int64 __fastcall main(int a1, char **a2, char **a3)
{
  char s[136]; // [rsp+0h] [rbp-90h] BYREF
  unsigned __int64 v5; // [rsp+88h] [rbp-8h]

  v5 = __readfsqword(0x28u);
  setbuf(stdin, 0LL);
  setbuf(stdout, 0LL);
  setbuf(stderr, 0LL);
  printf("Enter your name: ");
  fgets(s, 127, stdin);
  printf("Hello ");
  printf(s);
  putchar(10);
  return 0LL;
}```

- 

