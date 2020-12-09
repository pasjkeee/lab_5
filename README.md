# lab_5

[org 0x7c00]


mov ah, 0x0e ;pechat
mov al, 0x0a ; Enter
int 0x10	;vzaim s ekranom
mov al, 0x0d ; возврат каретки
start_:
mov bx, str1
call Print

mov ah , 0x00
int 0x16	;vz s klaviaturoj
mov ah, 0x0e 
int 0x10	;vzaim s ekranom
mov dx, ax

mov bx, str2
call Print

mov ah , 0x00
int 0x16 ;vz s klaviaturoj
mov ah, 0x0e
int 0x10 ;vzaim s ekranom
mov cx, ax

mov dh, 0x00     
mov ch, 0x00
cmp dx, cx 
je Equal_ 
jmp Not_Equal_




End_: 
jmp areyou_

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
 
Equal_:
	mov bx, str3
	call Print
	jmp End_

Not_Equal_:
pusha
	mov bx, str4
	call Print
popa
jmp End_


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

good_:
	mov bx, str6
	call Print
	jmp myloop_
	
notgood_:
	mov bx, str7
	call Print
	jmp $	

	
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

vniz_:
mov ah, 0x0e ;pechat
mov al, 0x0a ; Enter
int 0x10
jmp myloop_

pravo_:
mov ah, 0x03
mov bh, 0x00
int 0x10
inc dl
mov ah, 0x02
mov bh, 0x00
int 0x10
jmp myloop_

verx_:
mov ah, 0x03
mov bh, 0x00
int 0x10
dec dh
mov ah, 0x02
mov bh, 0x00
int 0x10
jmp myloop_

levo_:
mov ah, 0x0e ;pechat
mov al, 0x08 ; Enter
int 0x10
jmp myloop_

 
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
