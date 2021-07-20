
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








