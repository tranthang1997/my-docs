# Linux pipe Command

Câu lệnh *Pipe* luôn có sẵn trên các nền tảng UNIX/Linux. Nó cho phép chuyển output của câu lệnh trước đến câu lệnh tiếp theo . Thực sự có HÀNG TẤN các tình
huống mà phương pháp này mang lại hiệu quả vô cùng. Nhưng trước khi tìm hiểu sâu hơn, có một số điều mà bạn cần biết: mỗi chương trình trong hệ thống UNIX/Linux
đều có 3 luồng dữ liệu tích hợp(built-in data streams).
* STDIN (0) – Standard input
* STDOUT (1) – Standard output
* STDERR (2) – Standard error

Khi chúng ta sẽ làm việc với "pipes", nó sẽ lấy STDOUT của một lệnh và chuyển nó đến STDIN của lệnh tiếp theo. Hãy xem một số cách phổ biến nhất mà bạn có thể kết
hợp lệnh “pipe” vào việc sử dụng hàng ngày của mình.

## Pipe command
### Cách sử dụng cơ bản
Câu lệnh `pacman`, trình quản lý gói mặc định cho *Arch Linux* và tất cả các bản phần phối *Arch-based* được dùng để in ra tất cả các gói đã cài đặt trên hệ thống của bạn:

```
pacman -Qqe
```
![image](https://user-images.githubusercontent.com/25586998/115266849-c2ae4580-a162-11eb-8dec-855d5841cabb.png)

Có thể thấy đây là 1 danh sách rất dài các package. Vậy làm thế nào về việc chỉ chọn một vài thành phần trong danh sách trên. Chúng ta có thể sử dụng "grep".
Nhưng bằng cách nào? Có một cách sẽ là kết xuất đầu ra vào một tệp tạm thời, "grep" đầu ra mong muốn và xóa tệp đó đi. Bản thân chuỗi nhiệm vụ này có thể được
chuyển thành 1 script duy nhất nhanh và gọn. Bây giờ hãy xem sức mạnh của "pipe":
```
pacman -Qqe | grep <target>
```
![image](https://user-images.githubusercontent.com/25586998/115266900-cd68da80-a162-11eb-9661-e4689e05f85e.png)

Thật tuyệt vời phải không nào. Ta dùng ký tự "|" để thực hiện gọi lệnh "pipe". Nó sẽ lấy STDOUT từ phần bên trái và đưa nó vào STDIN của phần bên phải.
Trong ví dụ nói trên, lệnh "pipe" thực sự đã chuyển đầu ra ở sau khi thực hiện "grep". Và đây là cách nó diễn ra:

```
pacman -Qqe > ~/Desktop/pacman_package.txt
grep python ~/Desktop/pacman_package.txt
```

![image](https://user-images.githubusercontent.com/25586998/115266931-d8236f80-a162-11eb-90ea-74388d32521b.png)

### Multiple piping
Về cơ bản, không có gì đặc biệt với cách sử dụng nâng cao của lệnh "pipe".

Ví dụ dưới đây về cách sử dụng nhiều "pipe" kết hợp

```
pacman -Qqe | grep p | grep t | grep py
```
![image](https://user-images.githubusercontent.com/25586998/115266967-deb1e700-a162-11eb-9a9b-dcfebabad34a.png)

Đầu ra của câu lệnh được lọc ngày càng nhiều hơn bằng “grep” thông qua một loạt các piping. Đôi khi, khi chúng ta đang làm việc với nội dung của một tệp,
nó có thể rất, rất lớn. Tìm ra đúng vị trí của mục nhập mong muốn của chúng tôi có thể khó khăn. Hãy tìm kiếm tất cả các mục nhập bao gồm các chữ số 1 và 2.

```
cat demo.txt | grep -n 1 | grep -n 2
```

![image](https://user-images.githubusercontent.com/25586998/115266997-e5405e80-a162-11eb-91b6-fab96001e3bc.png)

### Thao tác với danh sách tệp và thư mục

Phải làm gì khi bạn đang xử lý một thư mục có HÀNG TẤN tệp trong đó? Thật là khó chịu khi ta phải cuộn qua toàn bộ danh sách. Vậy ại sao không làm cho nó dễ 
chịu hơn với pipe. 

Trong ví dụ này, hãy xem danh sách tất cả các tệp trong thư mục `/usr/bin`.

```
ls -l <target_dir> | more
```
![image](https://user-images.githubusercontent.com/25586998/115267024-ebced600-a162-11eb-808c-c85f006f511e.png)

Ở đây, `ls` in tất cả các tệp và thông tin của chúng. Sau đó, "pipe" chuyển nó cho `more` để làm việc với. Nếu bạn chưa biết, thì `more` là một công cụ có thể 
biến các văn bản thành một chế độ xem màn hình cùng một lúc. Tuy nhiên, đó là một công cụ cũ và theo tài liệu chính thức, `less` được khuyến nghị nhiều hơn.

```
ls -l /usr/bin | less
```
![image](https://user-images.githubusercontent.com/25586998/115267040-f0938a00-a162-11eb-92e2-5a45e1dc565a.png)

### Sorting output

Có một công cụ tích hợp sẵn `sort` sẽ nhập văn bản và sắp xếp chúng. Công cụ này là một viên ngọc quý thực sự nếu bạn đang làm việc với một cái gì đó thực sự lộn
xộn. Ví dụ: tôi nhận được tệp này chứa đầy các chuỗi ngẫu nhiên:

```
cat demo.txt
```

![image](https://user-images.githubusercontent.com/25586998/115267060-f5f0d480-a162-11eb-8bc7-86de0b527190.png)

Bây giờ hãy pipe nó đến `sort`:

```
cat demo.txt | sort
```

![image](https://user-images.githubusercontent.com/25586998/115267076-fab58880-a162-11eb-94af-d327cd56590d.png)
Thật tuyệt vời phải không nào, bây giờ nội dung file đã được sắp xếp lại 1 cách ngăn nắp.

## In kết hợp với một pattern cụ thể

```
ls -l | find ./ -type f -name "*.txt" -exec grep 00110011 {} \;
```

![image](https://user-images.githubusercontent.com/25586998/115267100-fee1a600-a162-11eb-9b35-6b6a123efdcf.png)

Đây là một lệnh khá xoắn não, đúng không. Đầu tiên, `ls` sẽ xuất ra danh sách tất cả các tệp trong thư mục. Công cụ `find` lấy kết quả, tìm kiếm các tệp `.txt`
và thực thi `grep` để tìm kiếm “00110011”. Lệnh này sẽ kiểm tra mọi tệp văn bản trong thư mục có phần mở rộng là TXT và tìm kiếm các kết quả phù hợp.

### In nội dung tệp với một phạm vi cụ thể
.
Khi bạn đang làm việc với một tệp lớn, bạn thường có nhu cầu kiểm tra nội dung của một phạm vi nhất định. Chúng ta có thể làm được điều đó với sự kết hợp khéo
léo của `cat`, `head`, `tail` và `pipe`. Lệnh `head` xuất ra phần đầu của nội dung và `tail` xuất ra phần cuối cùng.

```
cat <file> | head -6
```

![image](https://user-images.githubusercontent.com/25586998/115267130-04d78700-a163-11eb-8151-c1ea85f9c6b0.png)

```
cat <file> | tail -6
```
![image](https://user-images.githubusercontent.com/25586998/115267142-0a34d180-a163-11eb-8a0d-7cb56024fb81.png)

### Unique values

Khi làm việc với các đầu ra trùng lặp, nó có thể khá khó chịu. Đôi khi, đầu vào trùng lặp có thể gây ra các vấn đề nghiêm trọng. Trong ví dụ này, hãy truyền
`uniq` trên một luồng văn bản và lưu nó vào một tệp riêng biệt.

Ví dụ: đây là một tệp văn bản chứa một danh sách lớn các số dài 2 chữ số. Chắc chắn có nội dung trùng lặp ở đây:

```
cat duplicate.txt | sort
```
![image](https://user-images.githubusercontent.com/25586998/115267165-0f921c00-a163-11eb-9bed-6d7c8261f408.png)

Bây giờ hãy thực hiện quá trình lặp

```
cat duplicate.txt | sort | uniq > unique.txt
```

![image](https://user-images.githubusercontent.com/25586998/115266147-26843e80-a162-11eb-9600-b2a4eb0fd4e3.png)

Ta hãy xem kết quả:

```
bat unique.txt
```
![image](https://user-images.githubusercontent.com/25586998/115266263-4451a380-a162-11eb-82a8-9fc0b3fccbe9.png)

### Error pipes

Đây là một method thú vị. Phương pháp này được sử dụng để chuyển hướng STDERR đến STDOUT và tiếp tục với pipe. Điều này được biểu thị bằng ký hiệu `|&`.

Ví dụ: hãy tạo một lỗi và gửi kết quả đến một số công cụ khác. Trong ví dụ này, tôi chỉ nhập một số lệnh ngẫu nhiên và chuyển lỗi đến `grep`.

```
adsfds |& grep n
```
![image](https://user-images.githubusercontent.com/25586998/115266740-a90cfe00-a162-11eb-9e85-ab9e81670530.png)


### Tham khảo
http://www.linfo.org/pipes.html
https://linuxhint.com/linux_pipe_command/
