# Урок 1. Механизмы пространства имен
## Задание: необходимо продемонстрировать изоляцию одного и того же приложения (как решено на семинаре - командного интерпретатора) в различных пространствах имен.

## Результатом работы будет: текст объяснения, логи выполнения, история команд и скриншоты (важно придерживаться такой последовательности).


Cоздаем каталог "hmseminar1" в домашнем каталоге:  
`mkdir ~/hmseminar1/`

Копируем исполняемый файл командного интерпретатора bash в папку bin в созданной папке:  
`cp /bin/bash ~/hmseminar1/bin/"`  
! Получаем ошибку: cp: невозможно создать обычный файл '/home/olya/hmseminar1/bin/': Это не каталог.

Создаем директорию bin и в нее копируем исполняемый файл командного интерпретатора bash:  
`mkdir ~/hmseminar1/bin/`  
`cp /bin/bash ~/hmseminar1/bin/`  

Создаем директории lib и lib64 для копирования необходимых библиотек:  
`mkdir ~/hmseminar1/lib/`  
`mkdir ~/hmseminar1/lib64/`  

Выводим список необходимыx библиотек:  
`ldd /bin/bash`  

Копируем библиотеки в папки lib и lib64:  
`cp /lib/x86_64-linux-gnu/libtinfo.so.6 ~/hmseminar1/lib/`  
`cp /lib/x86_64-linux-gnu/libc.so.6 ~/hmseminar1/lib/`  
`cp /lib64/ld-linux-x86-64.so.2 ~/hmseminar1/lib64/`  

Меняем корневую папку нашей текущей среды:  
`sudo chroot ~/hmseminar1 /bin/bash`  

Выходим из изолированной среды и копируем исполняемый файд команды ls:  
`exit`  
`cp /bin/ls ~/hmseminar1/bin/`  

Выводим список необходимыx библиотек и копируем их в свою папку:  
`ldd /bin/ls`  
`cp /lib/x86_64-linux-gnu/libselinux.so.1  ~/hmseminar1/lib/`  
`cp /lib/x86_64-linux-gnu/libpcre2-8.so.0  ~/hmseminar1/lib/`  

Входим в изолированную среду и выполняем команду ls:  
`sudo chroot ~/hmseminar1 /bin/bash`  
`ls`  

### История команд
![](https://github.com/Lokotokk/Containerization-Seminar1/blob/main/images/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202023-09-13%2009-28-58.png)  

### Выполнение
![](https://github.com/Lokotokk/Containerization-Seminar1/blob/main/images/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202023-09-13%2009-30-00.png)  

![](https://github.com/Lokotokk/Containerization-Seminar1/blob/main/images/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202023-09-13%2009-30-22.png)  

![](https://github.com/Lokotokk/Containerization-Seminar1/blob/main/images/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202023-09-13%2009-30-52.png)  

![](https://github.com/Lokotokk/Containerization-Seminar1/blob/main/images/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202023-09-13%2009-31-11.png)

![](https://github.com/Lokotokk/Containerization-Seminar1/blob/main/images/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202023-09-13%2009-31-30.png)


Создадим сетевое пространство имен с именем "hmsem1ns":  
`ip netns add hmsem1ns`  
если вы не под пользователем root, то нужно использовать sudo  

`sudo ip netns add hmsem1ns`   

Выполняем процесс в созданном пространстве имен:  
`netns exec hmsem1ns bas`h  
если вы не под пользователем root, то нужно использовать sudo  

`sudo netns exec hmsem1ns bash`  

Выполняем команду, чтобы увидеть сетевые настройки:  
`ip a`

Выполняем команду, чтобы увидеть процессы, ограниченные только пространством имен.  
`ps aux`

Углубляем уровень изоляции:  
Изоляция по Процессам и Файловой Системе:  
`unshare --net --pid --fork --mount-proc /bin/bash`  
`ps aux`  

### История команд:  
![](https://github.com/Lokotokk/Containerization-Seminar1/blob/main/images/history1.png)  

![](https://github.com/Lokotokk/Containerization-Seminar1/blob/main/images/history2.png)  

### Скриншоты выполнения:  
![](https://github.com/Lokotokk/Containerization-Seminar1/blob/main/images/1net.png)  
![](https://github.com/Lokotokk/Containerization-Seminar1/blob/main/images/2net.png)  
![](https://github.com/Lokotokk/Containerization-Seminar1/blob/main/images/3net.png)  
![](https://github.com/Lokotokk/Containerization-Seminar1/blob/main/images/4net.png)  
![](https://github.com/Lokotokk/Containerization-Seminar1/blob/main/images/5net.png)

