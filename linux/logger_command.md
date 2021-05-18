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

#### Display Help

Gõ tùy chọn “–help” để hiển thị thông báo trợ giúp về lệnh “logger” và các tùy chọn của nó:

![](https://linuxhint.com/wp-content/uploads/2021/05/image5-16.png)

#### Tài liệu tham khảo
https://linuxhint.com/use-logger-command-linux/
