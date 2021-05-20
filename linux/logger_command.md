Trong nền tảng LINUX/UNIX, log là 1 file sẽ ghi lại toàn bộ từng hành động của hệ điều hành. Bất cứ khi nào người dùng đăng nhập vào hệ thống, nó sẽ lưu
bản ghi trong đó. Nó cũng cho phép người dùng thêm bất kỳ nội dung nào vào tệp này.

Dành cho điều này, "logger" là công cụ dòng lệnh cung cấp giao diện lệnh `shell` và cung cấp cho người dùng cách tiếp cận dễ dàng để thêm logs vào tệp
`/var/log/syslog`. Bạn có thể thêm các nội dung vào file sử dụng `logger`.

Cú pháp của tiện ích dòng lệnh này là :

```
logger [options] [log]
```
### Cách sử dụng lệnh logger với các tùy chọn:

Lệnh “logger” là một công cụ được tạo sẵn trong các hệ thống Linux. Sử dụng lệnh này, người dùng có thể thực hiện các chức năng khác nhau với các tùy chọn khác nhau:

#### Print “syslog” file:

Tệp `syslog` đóng một vai trò quan trọng trong các bản phân phối Linux vì nó lưu trữ tất cả dữ liệu nhật ký trong thư mục `/var/log`.
Để xem tệp syslog trong terminal, hãy thực hiện lệnh sau:

```
tail /var/log/syslog
```
![](https://linuxhint.com/wp-content/uploads/2021/05/image7-15.png)

#### Specify the syslog Lines
`tail` được sử dụng để ghi lại bản ghi từ các tệp syslog và in nó trong terminal. Theo mặc định, khi lệnh tail được thực thi, nó sẽ in 10 dòng logs cuối cùng
của tệp. Nhưng chúng ta cũng có thể chỉ định số dòng logs để in:

```
tail -n 30 /var/log/syslog
```
![](https://linuxhint.com/wp-content/uploads/2021/05/image9-11.png)

#### Thêm log vào tệp syslog

Ta có thể thêm bất kỳ nhận xét nào trong tệp syslog thông qua lệnh “logger” mà không cần truyền bất kỳ options nào với lệnh sau:

```
logger “For_Testing”
```
![](https://linuxhint.com/wp-content/uploads/2021/05/image8-15.png)

Chạy lệnh `tail` để in chúng lên terminal:

```
tail /var/log/syslog
```
![](https://linuxhint.com/wp-content/uploads/2021/05/image11-9.png)

#### "who" command

Lệnh "logger" cũng có thể được sử dụng để thêm đầu ra tiêu chuẩn của bất kỳ lệnh nào. Sử dụng "who" với logger thêm nó vào syslog.

```
logger `who`
```
![](https://linuxhint.com/wp-content/uploads/2021/05/image10-10.png)

#### Log Specified File:
Lệnh "logger" cho phép người dùng thêm nội dung của một tệp được chỉ định vào tệp syslog bằng cách sử dụng tùy chọn “-f”. 
Hãy tạo một tệp có tên “test_file1.txt” và thêm một số văn bản vào tệp đó:

![](https://linuxhint.com/wp-content/uploads/2021/05/image2-22.png)

Bây giờ, để in nhật ký tệp trong terminal, hãy thực hiện lệnh đã cho:

```
logger –f test_file1.txt
```
![](https://linuxhint.com/wp-content/uploads/2021/05/image1-22.png)

Chú ý: Trong lệnh tail, tail -2 có nghĩa là nó sẽ in ra hai dòng cuối cùng. Nhưng nếu bạn muốn in đầu ra chi tiết với tất cả các bản ghi, bạn không cần chỉ
định số dòng này.

#### Specify Log Size

Một số dòng log có thể là chuỗi dài và để giới hạn chúng ta sử dụng tùy chọn “–size”. Chạy tùy chọn “–size” được đề cập theo cách sau:

```
logger --size 12 12345678901122334455……
```
![](https://linuxhint.com/wp-content/uploads/2021/05/image4-17.png)


Trong lệnh trên, chúng tôi đã thêm các ký tự ngẫu nhiên vào logs và hiển thị 12 ký tự đầu tiên duy nhất bằng cách sử dụng tùy chọn size.
"tail -1" sẽ chỉ in dòng cuối cùng của kết quả hiển thị.

#### Ignore Empty Lines

Sử dụng tùy chọn “-e” nếu tệp chứa các dòng trống trong đó. Nó sẽ xóa các dòng trống khỏi tệp và in đầu ra theo cách tiêu chuẩn.


Ví dụ: thêm một số dòng trống trong tệp văn bản mà chúng ta đã tạo:

![](https://linuxhint.com/wp-content/uploads/2021/05/image3-16.png)

Chạy với tùy chọn “-e” với tên tệp “test_file1.txt” để xóa các dòng trống:
```
logger -e -f test_file1.txt
```

![](https://linuxhint.com/wp-content/uploads/2021/05/image6-16.png)

### Các file log quan trọng trên Linux
Trong linux, hầu hết các file log được chia thành 4 loại:
- Application Logs: Nhật ký ứng dụng
- Event Logs: Nhật ký sự kiện
- Service Logs: Nhật ký dịch vụ
- System Logs: Nhật ký hệ thống

Đây là 1 vài file log hay được sử dụng
- /var/log/syslog hoặc/var/log/messages : Thông điệp chung và những thứ liên quan đến hệ thống
- /var/log/kern.log : Kernel logs
- /var/log/cron.log : Crond logs (cron job)
- /var/log/maillog : Mail server logs
- /var/log/httpd/ : Apache access and error logs directory
- /var/log/lighttpd/ : Lighttpd access and error logs directory
- /var/log/boot.log : System boot log
- /var/log/mysqld.log : MySQL database server log file
- /var/log/secure hoặc /var/log/auth.log : Authentication log
- /var/log/utmp or /var/log/wtmp : Login records file
- /var/log/yum.log : Yum command log file.

### Phát hiện sự cố với Linux Logs

Phát hiện sự cố là một trong những lý do chính khiến mọi người tạo logs ở trong linux. Khi sự cố xảy ra, bạn sẽ muốn chẩn đoán nó để hiểu tại sao nó lại xảy ra và nguyên nhân là gì. Thông báo lỗi hoặc một chuỗi sự kiện có thể cung cấp cho bạn manh mối về nguyên nhân gốc rễ, chỉ ra cách tái hiện lại vấn đề và hướng dẫn bạn giải pháp. Dưới đây là 1 số vấn đề hay xảy ra:

#### Login thất bại

Vì mục đích bảo mật, bạn có thể muốn biết người dùng nào đã đăng nhập hoặc cố gắng đăng nhập vào hệ thống của bạn. Bạn có thể kiểm tra nhật ký xác thực của mình để tìm những lần đăng nhập không thành công, xảy ra khi người dùng cung cấp thông tin xác thực không chính xác hoặc không có quyền đăng nhập. Điều này thường xảy ra khi sử dụng SSH để truy cập từ xa hoặc khi sử dụng lệnh `su` để chạy lệnh với tư cách người dùng khác.

Các sự kiện không thành công thường chứa các chuỗi như “Failed password” và “user unknown”, trong khi các sự kiện xác thực thành công thường chứa các chuỗi như “Accepted password” và “session opened”. Những thông tin này sẽ được lưu lại trong file log của hệ thống `/var/log/auth.log`.

Ta có thể check lại những thông tin này bằng `grep`

```
grep "authentication failure" /var/log/auth.log
```

```
May 17 18:16:32 tranvanthang-K501LX lightdm: pam_unix(lightdm:auth): authentication failure; logname= uid=0 euid=0 tty=:0 ruser= rhost=  user=tranvanthang
May 20 00:50:34 tranvanthang-K501LX compiz: pam_unix(unity:auth): authentication failure; logname= uid=1000 euid=1000 tty= ruser= rhost=  user=tranvanthang
May 20 08:20:46 tranvanthang-K501LX compiz: pam_unix(unity:auth): authentication failure; logname= uid=1000 euid=1000 tty= ruser= rhost=  user=tranvanthang
May 20 13:47:13 tranvanthang-K501LX compiz: pam_unix(unity:auth): authentication failure; logname= uid=1000 euid=1000 tty= ruser= rhost=  user=tranvanthang
May 20 13:47:18 tranvanthang-K501LX compiz: pam_unix(unity:auth): authentication failure; logname= uid=1000 euid=1000 tty= ruser= rhost=  user=tranvanthang
May 20 13:47:25 tranvanthang-K501LX compiz: pam_unix(unity:auth): authentication failure; logname= uid=1000 euid=1000 tty= ruser= rhost=  user=tranvanthang
May 20 15:48:22 tranvanthang-K501LX compiz: pam_unix(unity:auth): authentication failure; logname= uid=1000 euid=1000 tty= ruser= rhost=  user=tranvanthang
May 20 22:46:05 tranvanthang-K501LX compiz: pam_unix(unity:auth): authentication failure; logname= uid=1000 euid=1000 tty= ruser= rhost=  user=tranvanthang
```

Khi check những thông tin này ta có thể biết được những user nào đang cố tình truy cập vào hệ thống của mình để có biện pháp xử lý.

### Nguyên nhân hệ thống reboot

Đôi khi máy chủ có thể bị stop do sự cố hệ thống hoặc khởi động lại. Làm thế nào để bạn biết nó xảy ra khi nào và ai đã làm điều đó?

#### Shutdown Command
Nếu ai đó chạy lệnh tắt theo cách thủ công, bạn có thể thấy lệnh đó trong tệp auth log. Tại đây, bạn có thể thấy ai đó đã đăng nhập từ xa với tư cách người dùng ubuntu và sau đó tắt hệ thống:

```
Jun 13 21:03:38 ip-172-31-34-37 sshd[1172]: pam_unix(sshd:session): session opened for user ubuntu by (uid=0)
Jun 13 21:03:38 ip-172-31-34-37 systemd: pam_unix(systemd-user:session): session opened for user ubuntu by (uid=0)
Jun 13 21:03:41 ip-172-31-34-37 sudo:   ubuntu : TTY=pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/sbin/shutdown -r now
```

#### Khởi động Kernel

Nếu bạn muốn xem khi nào máy chủ khởi động lại bất kể lý do (bao gồm cả sự cố), bạn có thể tìm kiếm ở trong file (/var/log/kern.log). Syslog cũng áp dụng cho “kern” cho các kernel logs.


Các bản ghi sau được tạo ngay sau khi khởi động. Lưu ý dấu thời gian giữa các dấu ngoặc là 0: dấu này theo dõi lượng thời gian kể từ khi Kernel khởi động. Bạn cũng có thể tìm boot logs bằng cách tìm kiếm “BOOT_IMAGE”.

```
tranvanthang@tranvanthang-K501LX:~$ grep "BOOT_IMAGE" /var/log/kern.log
Jun 13 21:04:22 ip-172-31-34-37 kernel: [    0.000000] Linux version 4.15.0-1039-aws (buildd@lgw01-amd64-034) (gcc version 7.3.0 (Ubuntu 7.3.0-16ubuntu3)) #41-Ubuntu SMP Wed May 8 10:43:54 UTC 2019 (Ubuntu 4.15.0-1039.41-aws 4.15.18)
Jun 13 21:04:22 ip-172-31-34-37 kernel: [    0.000000] Command line: BOOT_IMAGE=/boot/vmlinuz-4.15.0-1039-aws root=LABEL=cloudimg-rootfs ro console=tty1 console=ttyS0 nvme_core.io_timeout=4294967295
```

### Phát hiện vấn đề với bộ nhớ
Có một số lý do khiến máy chủ có thể gặp sự cố, nhưng một nguyên nhân phổ biến là hết bộ nhớ. 


Khi RAM và swap hoàn toàn cạn kiệt, kernel sẽ bắt đầu tắt các tiến trình — thường là những tiến trình sử dụng nhiều bộ nhớ nhất và thời gian tồn tại ngắn nhất. Lỗi xảy ra khi hệ thống của bạn đang sử dụng tất cả bộ nhớ của nó và một tiến trình mới hoặc hiện có cố gắng truy cập bộ nhớ bổ sung.

Lúc này hãy tìm trong tệp logs của bạn để tìm các chuỗi như “Out of Memory” hoặc các cảnh báo kernel. Các chuỗi này chỉ ra rằng hệ thống của bạn đã cố tình dừng tiến trình hoặc ứng dụng thay vì cho phép tiến trình bị crash.

Bạn có thể tìm trong `/var/log/kern.log` hoặc `/var/log/syslog`

```
grep "Out of memory" /var/log/syslog
```

```
Jun 13 21:30:26 ip-172-31-34-37 kernel: [ 1575.404070] Out of memory: Kill process 16471 (memkiller) score 838 or sacrifice child
Jun 13 21:30:26 ip-172-31-34-37 kernel: [ 1575.408946] Killed process 16471 (memkiller) total-vm:144200240kB, anon-rss:562316kB, file-rss:0kB, shmem-rss:0kB
Jun 13 21:30:27 ip-172-31-34-37 kernel: [ 1575.518686] oom_reaper: reaped process 16471 (memkiller), now anon-rss:0kB, file-rss:0kB, shmem-rss:0kB
```

### Theo dõi Cron Job Errors
`cron daemon` là một bộ lập lịch chạy các lệnh vào các ngày và giờ được chỉ định. Nếu quá trình không chạy hoặc không kết thúc, thì lỗi cron sẽ xuất hiện trong tệp nhật ký của bạn.

Bạn có thể tìm thấy các tệp này trong `/var/log /cron`, `/var/log/messages` hay `/var/log/syslog` tùy thuộc vào cách phân phối của bạn. Có nhiều lý do khiến một cron job có thể thất bại. Thông thường, các vấn đề nằm ở tiến trình hơn là bản thân daemon cron.

Theo mặc định, cron job sẽ xuất ra nhật ký hệ thống và xuất hiện trong tệp `/var/log/syslog`. Bạn cũng có thể chuyển hướng đầu ra của các lệnh cron của mình đến một đích khác, chẳng hạn như đầu ra tiêu chuẩn hoặc một tệp khác. Trong ví dụ này, ta đặt "Hello world" vào logger command. Điều này tạo ra hai `log events`: một từ cron job và một từ logger command. Tham số -t đặt tên cron job thành “helloCron” :

```
$ crontab -e
*/5 * * * * echo 'Hello World' 2>&1 | /usr/bin/logger -t helloCron
```

Và khi con job chạy ta sẽ có được nội dung logs như sau:

```
Apr 28 22:20:01 ip-172-31-11-231 CRON[15296]: (ubuntu) CMD (echo 'Hello World!' 2>&1 | /usr/bin/logger -t helloCron)
Apr 28 22:20:01 ip-172-31-11-231 helloCron: Hello World!
```

Mỗi cronjob sẽ ghi nhật ký khác nhau dựa trên loại công việc cụ thể và cách nó xuất dữ liệu. Lúc này ta đã có manh mối cho nguyên nhân gốc rễ của các vấn đề trong logs hoặc bạn có thể thêm logs bổ sung nếu cần. Trong hầu hết các trường hợp, bạn chỉ nên để cron ghi lại kết quả đầu ra của các lệnh của bạn.
#### Tài liệu tham khảo

https://linuxhint.com/use-logger-command-linux/
