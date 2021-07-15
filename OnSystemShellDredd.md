# OnSystemShellDredd
Offensive Security Labs Solutions (OSCP Preparation).

Recién acabo de hacer la maquina OnSystemShellDredd de ShellDredd. Tiene una dificultad Easy y calificado por la comunidad Easy también.

Comenzamos con la Enumeración:

![2021-07-15_18-07-1626367779](https://user-images.githubusercontent.com/82907557/125826890-78189cfd-1c8b-473f-a06f-cebb89e07f2a.jpg)

Vemos que el puerto 21, que corresponde al servicio FTP, está abierto. Como otro puerto tenemos el 61000, con un servicio ssh.

![2021-07-15_18-07-1626368035](https://user-images.githubusercontent.com/82907557/125827068-6717b7cd-ddf9-43ce-93ba-44627545404f.jpg)

En el servicio FTP, tenemos anonymous allowed. Así que entramos como Anonymous. 
Vemos que hay una carpeta llamada ".hannah". Que esto puede significar un usuario potencial para un futuro. Entramos en el directorio y vemos que hay una llave privada. Nos la cargamos y le damos los permisos necesarios.

![2021-07-15_18-07-1626368327](https://user-images.githubusercontent.com/82907557/125827629-b91b373a-4497-4fb8-8b2a-c4fb26186329.jpg)

Accedemos al servicio ssh. Pero claro, tenemos una llave privada, pero desconocemos su usuario. Bueno si pensamos un poco, el directorio de antes se llamaba "hannah" y puede significar un usuario. Entramos al servicio ssh con el usuario hannah a través del puerto 61000. 

![2021-07-15_19-07-1626368473](https://user-images.githubusercontent.com/82907557/125827909-d1c10718-da54-4d1a-ae1f-2c6dd27aae62.jpg)

Bien, ya estamos dentro. Podemos visualizar ya "local.txt". Ahora lo que falta sería la Escalada de Priviliegios.
Vemos que si hacemos un "sudo -l" para ver la lista de privilegios del usuario hannah, nos salta "-bash: sudo: command not found". 

![2021-07-15_19-07-1626368684](https://user-images.githubusercontent.com/82907557/125828840-c3248083-d70e-403e-8741-00cfa88954f0.jpg)

Pero no hay problema para eso.
Buscamos archivos/binarios SUID con el siguiente comando "find / -perm -4000 2>/dev/null".

![2021-07-15_19-07-1626368977](https://user-images.githubusercontent.com/82907557/125828956-ca689315-7f18-449c-9460-629600c7ddde.jpg)
Vemos que hay algo inusual de ver. Esta cpulimit que consiste en limitar el uso de la cpu en un proceso. Buscamos si este binario, se podra explotar. Y vemos que sí. 

![2021-07-15_19-07-1626369181](https://user-images.githubusercontent.com/82907557/125829370-800a9270-1aa9-482a-ad91-811e7b40cece.jpg)

Efectuamos el siguiente comando. Pero ojo, hay que ejecuarlo desde la ruta completa de cpulimit -> "/usr/bin/cpulimit -l 100 -f -- /bin/sh -p".

![2021-07-15_19-07-1626369290](https://user-images.githubusercontent.com/82907557/125829657-eaba3615-8c45-4f4d-966e-ff4781fc946e.jpg)

Y vaya... ya estamos como root!!
Ahora solo falta ver "proof.txt".







