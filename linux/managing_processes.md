
Quản lý các tiến trình trong Linux là một chủ đề quan trọng để tìm hiểu và hiểu rõ, vì nó là một hệ điều hành đa nhiệm và có nhiều tiến trình diễn ra cùng một lúc.
Linux cung cấp nhiều công cụ để quản lý các tiến trình, như liệt kê các tiến trình đang chạy, xử lý các tiến trình, giám sát việc sử dụng hệ thống, v.v.

Trong Linux, mọi tiếng trình được biểu diễn bằng ID (PID) của nó. Có một số thuộc tính khác đối với tiến trình như id người dùng và id nhóm nếu người dùng
hoặc nhóm chạy tiến trình. Đôi khi bạn cần phải kill hoặc tương tác với một tiến trình, vì vậy bạn nên biết cách quản lý các tiến trình này để làm cho hệ
thống của bạn hoạt động trơn tru hơn.

Trong Linux, các tiến trình có thể được quản lý bằng các lệnh như ps, pstree, pgrep, pkill, lsof, top, nice, renice and kill, v.v.

### Processes
Chạy một instance của chương trình được gọi là một tiến trình. Trong Linux, id tiến trình (PID) được sử dụng để đại diện cho một tiến trình khác biệt cho mọi tiến
trình. Có hai loại tiến trình:
- Background processes
- Foreground processes

### Background Processes
Các Background Processes bắt đầu trong terminal và tự chạy. Nếu bạn chạy một tiến trình trong thiết bị terminal, đầu ra của nó sẽ được hiển thị trong cửa sổ terminal
và bạn có thể tương tác với nó, nhưng nếu bạn không cần tương tác với tiến trình, bạn có thể chạy nó trong nền(bacnkground).

Nếu bạn muốn chạy một tiến trình trong nền, chỉ cần thêm dấu “&” vào cuối lệnh và nó sẽ bắt đầu chạy trong nền; nó sẽ giúp bạn tiết kiệm thời gian và bạn sẽ có thể
bắt đầu một quá trình khác.

Để liệt kê các tiến trình đang chạy ở chế độ nền, hãy sử dụng lệnh ‘Jobs.’ Lệnh này sẽ hiển thị tất cả các tiến trình đang chạy trong nền.

Ví dụ, upgrade là một tiến trình trình lâu dài trong Linux. Mất quá nhiều thời gian và nếu bạn muốn làm những việc khác trong khi hệ thống đang nâng cấp, hãy sử dụng
lệnh như sau:

```
ubuntu@ubuntu:~$ sudo apt-get upgrade -y &
```
Nó sẽ bắt đầu chạy trong nền. Và bạn có thể tương tác với các chương trình khác trong khi đó. Bạn có thể kiểm tra số lượng và tiến trình nào đang chạy trong nền
bằng cách gõ lệnh này.

```
ubuntu@ubuntu:~$ jobs
[1]+ Running sudo apt-get upgrade -y &
```

### Foreground processes
Tất cả các tiến trình mà chúng tôi chạy trong terminal, theo mặc định, chạy như các foreground processes trước.
Chúng ta có thể quản lý chúng bằng các lệnh foreground and background commands.

Bạn có thể đưa bất kỳ tiến trình nền nào được liệt kê trong công việc lên foreground bằng cách buộc lệnh ‘fg’ theo sau là số foreground processes.

```
ubuntu@ubuntu:~$ fg %1
sudo apt-get upgrade -y
```
Và nếu bạn muốn đưa quá trình này xuống nền, hãy gõ lệnh này:

```
ubuntu@ubuntu:~$ bg %1
```
### Liệt kê và quản lý các quy trình bằng lệnh ps

Quá trình liệt kê bằng lệnh ps là một trong những cách lâu đời nhất để xem các tiến trình đang chạy của terminal. Gõ lệnh ps để liệt kê các tiến
trình đang chạy và bao nhiêu tài nguyên hệ thống mà chúng đang sử dụng và ai đang chạy chúng.
```
ubuntu@ubuntu:~$ ps u
USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
Jim 1562 0.0 0.0 164356 6476 tty2 Ssl+ 13:07 0:00 shell
Jim 1564 5.2 0.9 881840 78704 tty2 Sl+ 3:07 13:13 dauth
Jim 2919 0.0 0.0 11328 4660 pts/0 Ss 13:08 0:00 bash
Jim 15604 0.0 0.0 11836 3412 pts/0 R+ 17:19 0:00 ps u
...snip...
```
Cột người dùng hiển thị tên người dùng trong bảng trên và PID hiển thị id tiến trình. Bạn có thể sử dụng PID để kill hoặc gửi tín hiệu kill tới một
tiến trình. % CPU hiển thị bộ xử lý phần trăm CPU và% MEM hiển thị mức sử dụng bộ nhớ truy cập ngẫu nhiên. Để kết thúc một tiến trình, hãy nhập.

```
ubuntu@ubuntu:~$ kill [ process id (PID) ]
```
or 

```
ubuntu@ubuntu:~$ kill -9 [ process id (PID) ]
```

Sử dụng lệnh ps aux để xem tất cả các tiến trình đang chạy và thêm một đường ống để xem theo thứ tự.

```
ubuntu@ubuntu:~$ ps aux | less
```
Nếu bạn muốn sắp xếp lại các cột, bạn có thể làm điều đó bằng cách thêm cờ -e để liệt kê tất cả các tiến trình và -o để chỉ ra các cột bằng từ khóa trong lệnh ps.

```
ubuntu@ubuntu:~$ps -eo pid,user,uid,%cpu,%mem,vsz,rss,comm
PID USER UID %CPU %MEM VSZ RSS COMMAND
1 root 0 0.1 0.1 167848 11684 systemed
3032 jim 1000 16.5 4.7 21744776 386524 chrome
...snip...
```
Tùy chọn cho lệnh `ps`:

tùy chọn `u` được sử dụng để liệt kê các tiến trình của người dùng:

`ubuntu@ubuntu:~$ ps u`

tùy chọn `f` được sử dụng để hiển thị danh sách đầy đủ:
`ubuntu@ubuntu:~$ ps f`

Tùy chọn `x` được sử dụng để hiển thị thông tin về quá trình mà không có terminal.

Tùy chọn `e` được sử dụng để hiển thị thông tin mở rộng.

Một tùy chọn `a` được sử dụng để liệt kê tất cả các tiến trình với terminal.

Tùy chọn `v` được sử dụng để hiển thị định dạng bộ nhớ ảo.

`pstree` là một lệnh khác để liệt kê các tiến trình; nó hiển thị đầu ra ở định dạng cây.

![image](https://user-images.githubusercontent.com/25586998/133928274-3c82fa2e-45c3-4937-add4-907d20c1d72e.png)

Một số tùy chọn cho lệnh `pstree`

-n được sử dụng để phân loại các tiến trình theo PID

-H được sử dụng cho các tiến trình làm nổi bật.

```
ubuntu@ubuntu:~$ pstree -H [PID]
ubuntu@ubuntu:~$ pstree -H 6457
```

-a được sử dụng để hiển thị đầu ra, bao gồm các đối số dòng lệnh.

`ubuntu@ubuntu:~$ pstree -a`

-s được sử dụng để gieo cây hoặc tiến trình cụ thể

```
ubuntu@ubuntu:~$ pstree -s [PID]
ubuntu@ubuntu:~$ pstree -s 6457
```

[userName] được sử dụng để hiển thị các tiến trình do người dùng sở hữu.

```
ubuntu@ubuntu:~$ pstree [userName]
ubuntu@ubuntu:~$ pstree jim
pgrep
```

Với lệnh pgrep, bạn có thể tìm thấy một tiến trình đang chạy dựa trên các tiêu chí nhất định. Bạn có thể sử dụng tên đầy đủ hoặc chữ viết tắt của tiến trình để
tìm hoặc theo tên người dùng hoặc các thuộc tính khác. lệnh pgrep tuân theo mẫu sau:

```
ubuntu@ubuntu:~$ Pgrep [option] [pattern]
ubuntu@ubuntu:~$ pgrep -u jim chrome
Options for pgrep command
```

-i được sử dụng để tìm kiếm không phân biệt chữ hoa chữ thường

```
ubuntu@ubuntu:~$ Pgrep -i firefox
```
Với lệnh pkill, bạn có thể gửi tín hiệu đến một tiến trình đang chạy dựa trên các tiêu chí nhất định. Bạn có thể sử dụng tên đầy đủ hoặc chữ viết tắt của
tiến trình để tìm hoặc theo tên người dùng hoặc các thuộc tính khác. lệnh pgrep tuân theo mẫu sau.

```
ubuntu@ubuntu:~$ Pkill [Options] [Patterns]
ubuntu@ubuntu:~$ Pkill -9 chrome
Options for pkill command
```
–Signal được sử dụng để gửi tín hiệu, ví dụ: SIGKILL, SIGTERM, v.v.

```
ubuntu@ubuntu:~$ Pkill --signal SIGTERM vscode
```

-15 được sử dụng để gửi tín hiệu kết thúc.
```
ubuntu@ubuntu:~$ Pkill -15 vlc
lsof (List of Open Files)
```

Tiện ích dòng lệnh này được sử dụng để liệt kê các tệp được mở bởi một số quá trình. Và như chúng ta đã biết, tất cả các hệ thống UNIX / Linux đều nhận ra
mọi thứ dưới dạng tệp, vì vậy sẽ rất tiện lợi khi sử dụng lệnh `lsof` để liệt kê tất cả các tệp đã mở.

```
ubuntu@ubuntu:~$ lsof
```
![image](https://user-images.githubusercontent.com/25586998/133928536-c9f68caa-ce20-4dfd-a537-08dec99c39f1.png)

Trong bảng lệnh lsof ở trên, FD đại diện cho mô tả tệp, cwd đại diện cho thư mục làm việc hiện tại, txt có nghĩa là tệp văn bản, mem có nghĩa là tệp được ánh
xạ bộ nhớ, mmap có nghĩa là thiết bị được ánh xạ bộ nhớ, REG đại diện cho tệp thông thường, DIR đại diện cho thư mục, rtd nghĩa là thư mục gốc.
Có các tùy chọn khác mà bạn có thể sử dụng với lệnh lsof.

-c được sử dụng để liệt kê các tệp đang mở theo tên tiến trình của chúng.

```
ubuntu@ubuntu:~$ lsof -c chrome
```

-u được sử dụng để liệt kê các tệp đang mở bởi người dùng.

```
ubuntu@ubuntu:~$ lsof -u jim
```

### Quy trình lập danh sách và quản lý bằng lệnh `top`

Với lệnh trên cùng, bạn có thể hiển thị chế độ xem thời gian thực của các tiến trình hệ thống đang chạy. Nó hiển thị các tiến trình tùy thuộc vào việc sử dụng CPU.
Bạn có thể sắp xếp cột theo ý mình.

Lệnh trên cùng cũng cung cấp một số thông tin về hệ thống của bạn, chẳng hạn như hệ thống đã chạy được bao lâu hoặc có bao nhiêu người dùng gắn bó với hệ thống
và bao nhiêu tiến trình đang chạy, bao nhiêu CPU và RAM đang được sử dụng và danh sách của từng tiến trình.

```
ubuntu@ubuntu:~$ top

Tasks: 291 total, 1 running, 290 sleeping, 0 stopped, 0 zombie

%Cpu(s) : 2.3us, 0.3sy, 0.0ni, 97.0id, 0.3wa, 0.0hi, 0.0si, 0.0st

MiB Mem: 7880.6 total, 1259.9 free, 3176 used, 3444.4 buff/cache

MiB Swap: 2048.0 total, 2048.0 free, 0.0 used. 4091.8 avail Mem

PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND

3241 jim 20 0 20.7g 33512 10082 S 1.7 4.2 0:54.24 chrome

3327 jim 20 0 4698084 249156 86456 S 1.3 3.1 1:42.64 chrome

2920 jim 20 0 955400 410868 14372 S 1.0 5.1 7:51.04 chrome

3423 jim 20 0 4721584 198500 10106 S 1.0 2.5 0:49.00 chrome

3030 jim 20 0 458740 114044 66248 S 0.7 1.4 3:00.47 chrome

3937 jim 20 0 4610540 104908 72292 S 0.7 1.3 0:05.91 chrome

1603 jim 20 0 825608 67532 40416 S 0.3 0.8 3:13.52 Xorg

1756 jim 20 0 4154828 257056 10060 S 0.3 3.2 5:53.31 gnome-s+

1898 jim 20 0 289096 29284 5668 S 0.3 0.4 1:06.28 fusuma

3027 jim 20 0 587580 14304 75960 S 0.3 1.8 9:43.59 chrome

3388 jim 20 0 4674192 156208 85032 S 0.3 1.9 0:13.91 chrome

3409 jim 20 0 4642180 140020 87304 S 0.3 1.7 0:15.36 chrome

3441 jim 20 0 16.5g 156396 89700 S 0.3 1.9 0:25.70 chrome

….snip….
```

Bạn cũng có thể thực hiện một số hành động với lệnh trên cùng để thực hiện các thay đổi trong các tiến trình đang chạy; đây là danh sách dưới đây.

- u bằng cách nhấn “u”, bạn có thể hiển thị một quá trình đang chạy bởi một người dùng nhất định.
- M bằng cách nhấn “M”, bạn có thể sắp xếp theo mức sử dụng RAM hơn là mức sử dụng CPU.
- P bằng cách nhấn “P”, bạn có thể sắp xếp theo mức sử dụng CPU.
- 1 bằng cách nhấn “1” chuyển đổi giữa việc sử dụng CPU nếu có nhiều hơn một.
- R bằng cách nhấn “R”, bạn có thể thực hiện việc sắp xếp đầu ra của mình đảo ngược.
- h bằng cách nhấn “h”, bạn có thể đến trợ giúp và nhấn bất kỳ phím nào để quay lại

Lưu ý quy trình nào đang tiêu tốn nhiều bộ nhớ hoặc CPU hơn. Những tiến trình đang tiêu tốn nhiều bộ nhớ hơn có thể bị kill và những tiến trình đang tiêu tốn
nhiều CPU hơn có thể được cải tiến để giảm bớt tầm quan trọng của chúng đối với bộ xử lý.

Kill quy trình ở trên cùng: Nhấn k và viết ID tiến trình bạn muốn hủy. Sau đó gõ 15 hoặc 9 để giết bình thường hoặc ngay lập tức; bạn cũng có thể kết thúc
một tiến trình bằng lệnh kill hoặc killall.

### Lập danh sách & quản lý quy trình với System Monitor

Linux có một gnome giám sát hệ thống để hiển thị các tiến trình đang chạy một cách linh hoạt hơn. Để khởi động màn hình hệ thống, hãy nhấn phím windows và
nhập màn hình hệ thống, nhấp vào biểu tượng của nó và bạn có thể xem các quy trình trong các cột. Bằng cách nhấp chuột phải vào chúng, bạn có thể kill,
dừng hoặc chỉnh sửa lại tiến trình:



Các quy trình đang chạy được hiển thị với tài khoản người dùng theo thứ tự bảng chữ cái. Bạn có thể sắp xếp các quy trình theo bất kỳ tiêu đề trường nào như CPU,
Bộ nhớ, v.v., chỉ cần nhấp vào chúng, và chúng sẽ được sắp xếp; ví dụ: nhấp vào CPU để xem tiến trình nào đang tiêu thụ nhiều năng lượng CPU nhất. Để quản lý các
quy trình, hãy nhấp chuột phải vào chúng và chọn tùy chọn bạn muốn thực hiện với quy trình. Để quản lý tiến trình, hãy chọn các tùy chọn sau:

- Properties - hiển thị các cài đặt khác liên quan đến một quy trình.
- Memory maps - hiển thị bản đồ bộ nhớ hệ thống để hiển thị thư viện và các thành phần khác đang được sử dụng trong bộ nhớ cho tiến trình
- Open file - hiển thị tệp nào được mở bởi quá trình.
- Change Priority- hiển thị một thanh bên mà từ đó bạn có thể chỉnh sửa lại quy trình với các tùy chọn từ rất cao đến rất thấp và tùy chỉnh.
- Stop - tạm dừng tiến trình cho đến khi bạn chọn tiếp tục.
- Continue - khởi động lại quá trình bị tạm dừng.
- Kill - Giết một tiến trình ngay lập tức.

### Giết một quá trình với kill and killall
Kill, và lệnh killall được sử dụng để giết/kết thúc một tiến trình đang chạy. Các lệnh này cũng có thể được sử dụng để gửi tín hiệu hợp lệ đến một tiến trình
đang chạy, như yêu cầu một tiên trình tiếp tục, kết thúc hoặc đọc lại các tệp cấu hình, v.v. Các tín hiệu có thể được viết theo cả hai cách bằng số hoặc bằng tên.
Sau đây là một số tín hiệu thường được sử dụng.

```
SIGHUP 1 Detects hang-up signal on controlling terminal.

SIGINT 2 Interpreted from keyboard.

SIGQUIT 3 Quit from the keyboard.

SIGILL 4 Illegal instructions.

SIGTRAP 5 Is used for tracing a trape.

SIGABRT 6 is used for aborting signal from abort(3).

SIGKILL 9 Is used for sending a kill signal.

SIGTERM 15 Is used for sending a termination signal.

SIGCONT 19,18,25 Is used to continue a process if stopped.

SIGSTOP 17,19,23 Is used for stopping processes.
```
Các giá trị khác nhau của SIGCONT và SIGSTOP được sử dụng trong các hệ điều hành Unix/Linux khác nhau.

Sử dụng lệnh kill để gửi tín hiệu đến xử lý bởi PID.

Lưu ý quy trình mà bạn muốn gửi tín hiệu tiêu diệt. Bạn có thể tìm thấy id quy trình (PID) bằng `ps` hoặc lệnh `top`.

```
PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND

7780 jim 20 0 12596 4364 3460 R 33.3 3.2 13:54:12 top
```

```
ubuntu@ubuntu:~$ kill 7780

ubuntu@ubuntu:~$ kill -15 7780 or $ kill -SIGTERM 7780

ubuntu@ubuntu:~$ kill -9 7780 or $ kill -SIGKILL 7780
```

Sử dụng lệnh `killall` để gửi tín hiệu đến một tiến trình bằng tên.

```
ubuntu@ubuntu:~$ killall -9 firefox
```

```
ubuntu@ubuntu:~$ killall SIGKILL chrome
```


### Tài liệu tham khảo

https://linuxhint.com/manage-processes-ubuntu-linux/

