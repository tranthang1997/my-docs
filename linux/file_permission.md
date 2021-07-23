
Quyền sở hữu file(File ownership) là một thành phần quan trọng của Linux, nó cung cấp một phương pháp bảo mật để lưu trữ file.

Mọi file trong Linux đều có các thuộc tính sau:
- Owner permissions: xác định những hành động mà chủ sở hữu file có thể thực hiện trên đó
- Group permissions: xác định những hành động nào mà người dùng - là thành viên của nhóm mà file thuộc về, có thể thực hiện trên đó
- Other (world) permissions: Các quyền dành cho những người khác, cho biết hành động mà tất cả những người dùng khác có thể thực hiện trên file đó.

### Thông tin về permission
Khi sử dụng lệnh `ls -l`, ta sẽ biết được nhiều thông tin khác nhau liên quan đến quyền đối với file như sau:

```
$ls -l /home/amrood
-rwxr-xr--  1 amrood   users 1024  Nov 2 00:10  myfile
drwxr-xr--- 1 amrood   users 1024  Nov 2 00:10  mydir
```

Như trên, có thể thấy chữ đầu tiên thể hiện đầy đủ thông tin cho biết đó là file(-) hay folder(d)
Tiếp theo, là thông tin các quyền được chia thành các nhóm(mỗi nhóm là 3 ký tự) và mỗi vị trí trong nhóm biểu thị một quyền cụ thể, theo thứ tự 
sau: read (r), write (w), execute (x).
- Ba ký tự đầu tiên (2-4) đại diện cho các quyền đối với chủ sở hữu của file. Ví dụ: -rwxr-xr-- biểu thị rằng chủ sở hữu có quyền read (r), write (w) và excute (x).
- Nhóm thứ hai gồm ba ký tự (5-7) bao gồm các quyền cho nhóm mà file thuộc về. Ví dụ: -rwxr-xr-- biểu thị rằng nhóm có quyền read (r) và excute (x),
nhưng không có quyền write.
- Nhóm ba ký tự cuối cùng (8-10) đại diện cho quyền đối với những người khác. Ví dụ, -rwxr-xr-- biểu thị rằng có quyền chỉ read (r).

### Các chế độ truy cập với file
Quyền của một file/folder là tuyến phòng thủ đầu tiên trong bảo mật của hệ thống Linux. Các khối xây dựng cơ bản của quyền Linux là quyền read, write và excute,
được mô tả bên dưới đây:
#### Read
Cấp khả năng đọc, tức là xem nội dung của file.
#### Write
Cấp khả năng sửa đổi hoặc xóa nội dung của file.
#### Excute
Người dùng có quyền excute có thể chạy file dưới dạng chương trình

### Các chế độ truy cập với thư mục
Các chế độ truy cập với thư mục được liệt kê và tổ chức theo cách giống như bất kỳ file nào khác. Nhưng có một số khác biệt như sau:

#### Read

Truy cập vào một thư mục có nghĩa là người dùng có thể đọc nội dung, có thể nhìn vào tên tệp bên trong thư mục.

#### Write

Quyền truy cập có nghĩa là người dùng có thể thêm hoặc xóa các file khỏi thư mục

#### Excute

Việc thực thi một thư mục không thực sự có ý nghĩa, vì vậy hãy coi đây là một quyền truy cập. Người dùng phải có quyền truy cập thực thi vào thư mục `bin`
để thực hiện lệnh `ls` hoặc lệnh `cd`.


### Thay đổi quyền

Để thay đổi quyền đối với file hoặc thư mục, bạn sử dụng lệnh `chmod` (change mode). Có hai cách để sử dụng `chmod` - chế độ tượng trưng(symbolic mode)
và chế độ tuyệt đối(absolute mode).

#### Sử dụng `chmod` trong Symbolic Mode

Cách dễ nhất để người mới bắt đầu sửa đổi quyền đối với file hoặc thư mục là sử dụng symbolic mode. Với các quyền symbolic, bạn có thể thêm, xóa hoặc chỉ định quyền mà bạn muốn bằng cách sử dụng các toán tử trong bảng sau.

| No  | Chmod operator & Description  |
|---|---|
| 1 | + (Thêm các quyền được chỉ định vào file hoặc thư mục)  |
| 2 | - (Xóa các quyền được chỉ định khỏi file hoặc thư mục)  |
| 3 | = (Đặt các quyền được chỉ định)  |

Đây là một ví dụ sử dụng testfile. Chạy `ls -l` trên file cho thấy rằng các quyền của file như sau:
```
$ls -l testfile
-rwxrwxr--  1 amrood   users 1024  Nov 2 00:10  testfile
```


Sau đó, mỗi lệnh `chmod` kết hợp vói toán tử  từ bảng trước được chạy trên tệp thử nghiệm, theo sau là `ls –l`, ta có thể thay đổi quyền đối với file này:

```
$chmod o+wx testfile
$ls -l testfile
-rwxrwxrwx  1 amrood   users 1024  Nov 2 00:10  testfile
$chmod u-x testfile
$ls -l testfile
-rw-rwxrwx  1 amrood   users 1024  Nov 2 00:10  testfile
$chmod g = rx testfile
$ls -l testfile
-rw-r-xrwx  1 amrood   users 1024  Nov 2 00:10  testfile
```


Đây là cách bạn có thể kết hợp các lệnh này trên một dòng:

```
$chmod o+wx,u-x,g = rx testfile
$ls -l testfile
-rw-r-xrwx  1 amrood   users 1024  Nov 2 00:10  testfile
```

#### Sử dụng `chmod` với Absolute Permissions
Cách thứ hai để sửa đổi quyền với lệnh chmod là sử dụng một số để chỉ định từng bộ quyền cho file.

Mỗi quyền được gán một giá trị, như bảng sau hiển thị và tổng số của mỗi nhóm quyền cung cấp một số cho nhóm đó:

| No  | Mô tả  | Ref  |
|---|---|---|
| 1  | No permission  | ---  |
| 2  | Excute permission  | --x  |
| 3  | Write permission  | -w-  |
| 4  | Execute and write permission: 1 (execute) + 2 (write) = 3  | -wx |
| 5  | Read permission  | r-- |
| 6  | Read and execute permission: 4 (read) + 1 (execute) = 5 | r-x |
| 7  | Read and write permission: 4 (read) + 2 (write) = 6  | rw- |
| 8  | All permissions: 4 (read) + 2 (write) + 1 (execute) = 7  | rwx |


Đây là một ví dụ sử dụng file thử nghiệm. Chạy `ls -l` trên file thử nghiệm cho thấy rằng các quyền của tệp như sau:

```
$ls -l testfile
-rwxrwxr--  1 amrood   users 1024  Nov 2 00:10  testfile
```

Sau đó, ta có thể thay đổi quyền với file trên như sau:

```
$ chmod 755 testfile
$ls -l testfile
-rwxr-xr-x  1 amrood   users 1024  Nov 2 00:10  testfile
$chmod 743 testfile
$ls -l testfile
-rwxr---wx  1 amrood   users 1024  Nov 2 00:10  testfile
$chmod 043 testfile
$ls -l testfile
----r---wx  1 amrood   users 1024  Nov 2 00:10  testfile
```
### Thay đổi owners và group
Trong khi tạo tài khoản trên Linux, nó chỉ định owner ID và group ID cho mỗi người dùng. Tất cả các quyền được đề cập ở trên cũng được chỉ định dựa trên Owner và Group.
Hai lệnh có sẵn để thay đổi Owner và Group.

- chown − Lệnh chown là viết tắt của "change owner" và được sử dụng để thay đổi Owner và Group.

- chgrp − Lệnh chgrp là viết tắt của "change group" và được sử dụng để thay đổi group của một tệp.

### Thay đổi Ownership
Lệnh `chown` thay đổi quyền sở hữu của một file. Cú pháp cơ bản như sau:

```
$ chown user filelist
```

Giá trị của `user` có thể là tên hoặc id người dùng (uid) của người dùng trên hệ thống.

Ví dụ:

```
$ chown amrood testfile
```
Sẽ thay đổi quyền sở hữu của testfile sang user amrood.

Chú ý: Người dùng cấp cao(root) có khả năng không hạn chế để thay đổi quyền sở hữu của bất kỳ tệp nào nhưng người dùng bình thường chỉ có thể thay đổi quyền sở hữu của những tệp mà họ sở hữu.

### Thay đổi Group Ownership
Lệnh `chgrp` thay đổi quyền sở hữu nhóm của một tệp. Cú pháp cơ bản như sau:

```
chgrp group filelist
```
Giá trị của `group` có thể là group name hay group id trong hệ thống.

Ví dụ: `chgrp special testfile`

### SUID and SGID File Permission
Thường thì khi một lệnh được thực thi, nó sẽ phải được thực thi với các đặc quyền đặc biệt để hoàn thành nhiệm vụ của nó.

Ví dụ: khi bạn thay đổi mật khẩu của mình bằng lệnh `passwd`, mật khẩu mới của bạn sẽ được lưu trữ trong tệp `/etc/shadow`.

Là một người dùng thông thường, bạn không có quyền đọc hoặc ghi vào tệp này vì lý do bảo mật, nhưng khi bạn thay đổi mật khẩu của mình, bạn cần có quyền ghi vào tệp này. Điều này có nghĩa là chương trình `passwd` phải cung cấp cho bạn các quyền bổ sung để bạn có thể ghi vào tệp `/etc/shadow`.

Các quyền bổ sung được cấp cho các chương trình thông qua cơ chế được gọi là Set User ID (SUID) and Set Group ID (SGID) bits.

Khi bạn thực thi một chương trình đã bật bit SUID, bạn kế thừa các quyền của chủ sở hữu chương trình đó. Các chương trình không có bộ bit SUID được chạy với quyền của người dùng đã khởi động chương trình.

Đây cũng là trường hợp của SGID. Thông thường, các chương trình thực thi với quyền nhóm của bạn, nhưng thay vào đó, nhóm của bạn sẽ được thay đổi chỉ đối với chương trình này thành chủ sở hữu nhóm của chương trình.

Các bit SUID và SGID sẽ xuất hiện dưới dạng ký tự "s" nếu có quyền. Bit "s" SUID sẽ nằm trong các bit quyền nơi thường có quyền thực thi của chủ sở hữu.

Ví dụ, có lệnh sau:

```
$ ls -l /usr/bin/passwd
-r-sr-xr-x  1   root   bin  19031 Feb 7 13:47  /usr/bin/passwd*
$
```

Cho thấy rằng bit SUID được đặt và lệnh thuộc sở hữu của người dùng root. Một chữ cái viết hoa S ở vị trí thực thi thay vì chữ thường s chỉ ra rằng bit thực thi không được đặt.


Nếu bit sticky được bật trên thư mục, chỉ có thể xóa tệp nếu bạn là một trong những người dùng sau:

- Chủ sở hữu của thư mục sticky
- Chủ sở hữu của tệp đang bị xóa
- Người dùng siêu cấp, root

Để đặt các bit SUID và SGID cho bất kỳ thư mục nào, hãy thử lệnh sau:

```
$ chmod ug+s dirname
$ ls -l
drwsr-sr-x 2 root root  4096 Jun 19 06:45 dirname
$
```
### Khi nào quyền là quan trọng

Đối với một số người dùng máy tính chạy hệ điều hành Mac hoặc Windows, bạn có thể không nghĩ đến quyền, nhưng những môi trường đó không tập trung nhiều vào quyền dựa trên người dùng đối với tệp trừ khi bạn ở trong môi trường công ty.Nhưng n bạn đang chạy một hệ thống dựa trên Linux và bảo mật dựa trên quyền được đơn giản hóa và có thể dễ dàng sử dụng để hạn chế quyền truy cập tùy ý.

Đây là một số tài liệu và thư mục mà bạn nên tập trung vào và thiết lập các quyền tối ưu:

- `home directories`: Thư mục chính của người dùng rất quan trọng vì bạn không muốn người dùng khác có thể xem và sửa đổi các tệp trong tài liệu của mình. 
Để khắc phục điều này, bạn sẽ muốn thư mục có quyền drwx______ (700), vì vậy giả sử người khác muốn thực thi các quyền chính xác trên thư mục chính của họ - người dùng user1 có thể được thực hiện bằng cách ra lệnh chmod `700 /home/user1`.

- `bootloader configuration files`: Nếu bạn quyết định cài đặt mật khẩu để khởi động các hệ điều hành cụ thể thì bạn sẽ muốn xóa quyền đọc và ghi khỏi tệp cấu hình khỏi tất cả người dùng trừ root. Để thực hiện, bạn có thể thay đổi quyền của tệp thành 700.

- `Tệp cấu hình hệ thống và daemon`: Việc hạn chế quyền đối với các tệp cấu hình hệ thống và daemon là rất quan trọng để hạn chế người dùng chỉnh sửa nội dung, có thể không nên hạn chế quyền đọc, nhưng hạn chế quyền ghi là điều bắt buộc. Trong những trường hợp này, tốt nhất có thể sửa đổi các quyền thành 644.

- `firewall scripts`: Có thể không phải lúc nào cũng cần thiết phải chặn tất cả người dùng đọc `firewall file`, nhưng nên hạn chế người dùng ghi vào tệp. Trong trường hợp này, `firewall file` được người dùng root tự động chạy khi khởi động, vì vậy tất cả những người dùng khác không cần có quyền, vì vậy bạn có thể chỉ định quyền là 700.

### Tài liệu tham khảo
https://www.tutorialspoint.com/unix/unix-file-permission.htm
