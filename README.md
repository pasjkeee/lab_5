# lab_5

# Oперационная система

## Функции:

+ Сравнить два символа

+ При достижении возраста 18 лет
	
	+ Способность двигать курсор
	
### Get start
```assembly
[org 0x7c00]    

mov ah, 0x0e 
mov al, 0x0a ; Enter
int 0x10	
mov al, 0x0d 

```

### Запрос на ввод двух симовлов
+ Ввод производится посимвольно, вводятся оба символа подряд

```assembly


start_:
mov bx, str1
call Print

mov ah , 0x00
int 0x16	
mov ah, 0x0e 
int 0x10	
mov dx, ax

mov bx, str2
call Print

mov ah , 0x00
int 0x16 ;vz s klaviaturoj
mov ah, 0x0e
int 0x10 ;vzaim s ekranom
mov cx, ax

```

### Сравнение

```assembly

mov dh, 0x00     
mov ch, 0x00
cmp dx, cx 
je Equal_ 
jmp Not_Equal_

```

### Операция вывода

```assembly

Print:
pusha ;sohr reg v stek
        mov ah, 0x0e
        mov al, 0x0a ; Enter
        int 0x10
        mov al, 0x0d ; возврат каретки
        int 0x10
        
        loop_:
                mov ax, [bx]
                cmp al, 0
                je break_
                mov ah, 0x0e
                int 0x10
                inc bx
                jmp loop_
break_:
popa
ret

```

### Вывод при условии равнества двух символов

```assembly

Equal_:
	mov bx, str3
	call Print
	jmp End_

```

### Вывод при условии неравнества двух символов

```assembly

Not_Equal_:
pusha
	mov bx, str4
	call Print
popa
jmp End_

```

### Промежуточная метка

```assembly

End_: 
jmp areyou_

```

### Функция, которая запрашивает есть ли 18 лет

```assembly

areyou_:
mov bx, str5
call Print
mov ah , 0x00
int 0x16 ;
cmp al, 0x79
je good_
cmp al, 0x6E
je notgood_
jmp areyou_ 

```

### Если пользователь вводит y то переходит на метку good а далее переход к движению кусором


```assembly

good_:
	mov bx, str6
	call Print
	jmp myloop_
	
notgood_:
	mov bx, str7
	call Print
	jmp $	

```

### Проверка введенного символа

```assembly

myloop_:
mov ah , 0x00
int 0x16 ;
cmp al, 0x73
je vniz_
cmp al, 0x61
je levo_
cmp al, 0x64
je pravo_
cmp al, 0x77
je verx_
cmp al, 0x71
je start_
jmp myloop_

```

### Курсор вниз

```assembly

vniz_:
mov ah, 0x0e ;pechat
mov al, 0x0a ; Enter
int 0x10
jmp myloop_

```

### Курсор вправо

```assembly

pravo_:
mov ah, 0x03
mov bh, 0x00
int 0x10
inc dl
mov ah, 0x02
mov bh, 0x00
int 0x10
jmp myloop_

```

### Курсор вверх


```assembly

verx_:
mov ah, 0x03
mov bh, 0x00
int 0x10
dec dh
mov ah, 0x02
mov bh, 0x00
int 0x10
jmp myloop_

```

### Курсор влево


```assembly

levo_:
mov ah, 0x0e ;pechat
mov al, 0x08 ; Enter
int 0x10
jmp myloop_

```


### Шалоны строк


```assembly

str1:
db 'Input A: ', 0
str2:
db 'Input B: ', 0
str3:
db 'Symbol is equal.', 0
str4:
db 'Symbol is not equal.', 0
str5:
db 'Are you 18? (y/n) ', 0
str6:
db 'Good', 0
str7:
db 'Not good', 0

times 510-($-$$) db 0 

dw 0xaa55
```

+ times 510-($-$$) db 0	;добавление в машинный код нулей до адреса 510
+ dw 0xaa55	;запись magic number

