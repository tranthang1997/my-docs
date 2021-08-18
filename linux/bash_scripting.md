### Bash scripting là gì

Một tập lệnh Bash trong thuật ngữ máy tính tương tự như một tập lệnh trong thuật ngữ sân khấu. Nó là một tài liệu nêu rõ những gì cần nói và làm.
Ở đây, thay vì một người đọc và thực hiện kịch bản, nó được đọc và thực thi (hoặc thực thi) bởi máy tính.


Một tập lệnh Bash cho phép chúng ta xác định một loạt các hành động mà sau đó máy tính sẽ thực hiện mà chúng ta không cần phải tự nhập lệnh.
Nếu một nhiệm vụ cụ thể được thực hiện thường xuyên hoặc nó có tính phản kháng, thì một tập lệnh có thể là một công cụ hữu ích.


Một tập lệnh Bash được thông dịch (đọc và hành động) bởi một thứ gọi là trình thông dịch. Có nhiều trình thông dịch khác nhau trên một hệ thống linux
điển hình nhưng chúng tôi đã học Bash shell vì vậy chúng tôi sẽ giới thiệu các tập lệnh bash ở đây.

Bất cứ thứ gì bạn có thể chạy trên dòng lệnh mà bạn có thể đặt vào một tập lệnh(script) và chúng sẽ hoạt động giống hệt nhau. Ngược lại, bất cứ thứ gì bạn có
thể đưa vào một tập lệnh(script), bạn có thể chạy trên dòng lệnh và một lần nữa nó sẽ hoạt động giống hệt nhau.


1 script chỉ là một tệp văn bản thuần túy và nó có thể có bất kỳ tên nào bạn thích. Bạn tạo chúng giống như cách bạn làm với bất kỳ tệp văn bản nào khác,
chỉ với một trình soạn thảo văn bản cũ thuần túy (chẳng hạn như VI).

### Ví dụ
Dưới đây là một kịch bản đơn giản.Hãy tạo một tệp tương tự và chạy nó để cảm nhận cách chúng hoạt động. Tập lệnh này sẽ in một thông báo
ra màn hình (sử dụng một chương trình gọi là echo) sau đó cung cấp cho chúng tôi danh sách những gì có trong thư mục hiện tại của chúng tôi.

`echo <message>`

![image](https://user-images.githubusercontent.com/25586998/129937797-70b431c0-f90c-457b-9cbd-06bcca36d6a5.png)

Hãy giải thích nó:
- Dòng 1: Linux là một hệ thống không có phần mở rộng nên không yêu cầu các tập lệnh phải có phần mở rộng .sh. Tuy nhiên, người ta thường đeo chúng
  vào để dễ nhận biết.
- Dòng 2: Dòng đầu tiên của tập lệnh phải luôn là dòng này. Dòng này xác định trình thông dịch nào nên được sử dụng. Hai ký tự đầu tiên được gọi là `shebang`.
  Sau đó (quan trọng, không có khoảng trắng) là đường dẫn đến trình thông dịch
- Dòng 3 và 4: Bất cứ điều gì theo sau dấu # đều là một `comment`. Trình thông dịch sẽ không chạy điều này, nó chỉ ở đây vì lợi ích của chúng tôi.
  Comment tốt là bao gồm tên của bạn và ngày bạn viết kịch bản cũng như mô tả nhanh một dòng về những gì nó hoạt động ở đầu kịch bản.
- Dòng 6: Chúng ta sẽ sử dụng một lệnh có tên là `echo`. Nó sẽ chỉ in bất cứ thứ gì bạn đặt sau nó, dưới dạng các đối số dòng lệnh, ra màn hình.
  Hữu ích cho việc in tin nhắn.
- Dòng 7: Bước tiếp theo của tập lệnh của chúng ta là in nội dung của thư mục hiện tại của chúng ta.
- Dòng 9: Một tập lệnh phải có quyền thực thi trước khi nó có thể được chạy. Ở đây tôi chỉ chứng minh rằng tệp có quyền phù hợp
- Dòng 12: Bây giờ chúng ta chạy script. Tôi sẽ giải thích lý do tại sao chúng ta cần ./ sâu hơn một chút ở phần sau.
- Dòng 13 và 14: Kết quả đầu ra từ việc chạy (hoặc thực thi) tập lệnh.

### 1 số điểm quan trọng
#### Shebang
Dòng đầu tiên của tập lệnh sẽ cho hệ thống biết trình thông dịch nào nên được sử dụng trên tệp này. Điều quan trọng là đây là dòng đầu tiên của kịch bản.
Điều quan trọng nữa là không có khoảng trắng. Hai ký tự đầu tiên #! (shebang) cho hệ thống biết rằng ngay sau nó sẽ là một đường dẫn đến trình thông dịch
sẽ được sử dụng. Nếu chúng tôi không biết thông dịch viên của mình ở đâu thì chúng tôi có thể sử dụng một chương trình có tên `which` để tìm.

`which <program>`

![image](https://user-images.githubusercontent.com/25586998/129939221-e91b62b3-69f4-4ff1-8a64-d668969ec00c.png)

Nếu chúng ta bỏ qua dòng này thì tập lệnh Bash của chúng ta có thể vẫn hoạt động. Hầu hết các shell (bao gồm bash) sẽ cho rằng chúng là trình thông dịch
nếu một trình không được chỉ định. Tuy nhiên, khi thực hành tốt nhất là luôn bao gồm thông dịch viên. 
Sau đó, bạn hoặc ai đó có thể chạy tập lệnh của bạn trong các điều kiện mà bash không phải là trình bao hiện đang được sử dụng và điều này có thể dẫn đến kết quả không thể đạt được.


#### Tên của script
Linux là 1 extensionless system. Điều đó có nghĩa là chúng ta có thể đặt tên tập lệnh của mình bằng bất cứ thứ gì chúng ta thích và nó sẽ không ảnh hưởng đến việc nó
đang chạy theo bất kỳ cách nào. Mặc dù việc đặt một phần mở rộng .sh vào các tập lệnh của chúng tôi là thông thường, nhưng điều này hoàn toàn là để thuận tiện và không bắt buộc.

Chúng ta có thể đặt tên tập lệnh ở trên chỉ đơn giản là `myscript` hoặc thậm chí `myscript.jpg` và nó vẫn chạy khá ổn.

#### Comments

Một comment chỉ là một ghi chú trong tập lệnh không được chạy, nó chỉ ở đó vì lợi ích của bạn. Nhận xét rất dễ đưa vào, tất cả những gì bạn cần làm là đặt một dấu
thăng (#) thì bất kỳ thứ gì sau đó đều được coi là comment. Một comment có thể là cả một dòng hoặc ở cuối dòng.

![image](https://user-images.githubusercontent.com/25586998/129940483-89210258-1765-46e3-92c5-ac6c629dbdb7.png)


Thông thường, bạn nên đưa một chú thích vào đầu tập lệnh với mô tả ngắn gọn về những gì tập lệnh đó thực hiện và cũng như ai đã viết nó và khi nào.
Đây chỉ là những điều cơ bản mà mọi người thường muốn biết về một script.

#### Tại sao lại là ./

Linux được thiết lập theo cách của nó, phần lớn là vì những lý do hợp lý. Điều đặc biệt này thực sự làm cho hệ thống an toàn hơn một chút cho chúng tôi.
Đầu tiên là một chút kiến thức nền tảng. 

Khi chúng ta gõ lệnh trên dòng lệnh, hệ thống sẽ chạy qua một loạt thư mục đặt trước, tìm kiếm chương trình mà chúng ta đã chỉ định.
Chúng ta có thể tìm ra các thư mục này bằng cách xem một biến PATH cụ thể (sẽ tìm hiểu thêm về các thư mục này trong phần tiếp theo).

![image](https://user-images.githubusercontent.com/25586998/129941666-e8e5ab37-9686-4f6e-a374-3ddf8161911a.png)

Hệ thống sẽ tìm trong thư mục đầu tiên và nếu nó tìm thấy chương trình, nó sẽ chạy, nếu không nó sẽ kiểm tra thư mục thứ hai, v.v. Các thư mục được
phân tách bằng dấu hai chấm (:).


Hệ thống sẽ không tìm kiếm trong bất kỳ thư mục nào ngoài những thư mục này, thậm chí nó sẽ không tìm thấy trong thư mục hiện tại của bạn. Tuy nhiên,
chúng ta có thể ghi đè hành vi này bằng cách cung cấp một đường dẫn. Khi chúng ta làm như vậy, hệ thống sẽ thông báo một cách hiệu quả "À, bạn đã cho
tôi biết phải tìm ở đâu để tìm tập lệnh, vì vậy tôi sẽ bỏ qua PATH và đi thẳng đến vị trí bạn đã chỉ định." (.) sẽ đại diện cho thư mục hiện tại của chúng ta,
vì vậy khi chúng ta viết ./myscript.sh, chúng ta thực sự đang gọi hệ thống xem trong thư mục hiện tại để tìm tập lệnh.


Chúng ta cũng có thể sử dụng một đường dẫn tuyệt đối (`/home/ryan/linuxtutorialwork/myscript.sh`) và nó sẽ hoạt động hoàn toàn giống nhau, hoặc một đường dẫn
tương đối nếu chúng ta hiện không ở trong cùng một thư mục với tập lệnh (`../linuxtutorialwork/myscript.sh`).

Nếu có thể chạy các tập lệnh trong thư mục hiện tại của bạn mà không có cơ chế này thì chẳng hạn, ai đó sẽ dễ dàng tạo một tập lệnh độc hại trong một thư mục
cụ thể và đặt tên là ls hoặc một cái gì đó tương tự. Mọi người sẽ không thường xuyên chạy nó nếu họ muốn xem những gì có trong thư mục đó.

#### Permissions

Tập lệnh phải có quyền thực thi trước khi chúng ta có thể chạy nó (ngay cả khi chúng ta là chủ sở hữu của tệp). Vì lý do an toàn, bạn không có quyền thực
thi theo mặc định nên bạn phải thêm quyền đó. Một lệnh tốt để chạy để đảm bảo tập lệnh của bạn được thiết lập đúng là `chmod 755`.

#### Variables


Một biến là một vùng chứa cho một phần dữ liệu đơn giản. Chúng hữu ích nếu chúng ta cần tìm ra một thứ cụ thể và sau đó sử dụng nó sau này.
Các biến rất dễ đặt và dễ tham chiếu nhưng chúng có một cú pháp cụ thể phải tuân theo chính xác để chúng hoạt động.
- Khi chúng ta đặt một biến, chúng ta chỉ định tên của biến đó, theo sau là dấu bằng (=), sau đó trực tiếp là giá trị.
  (Vì vậy, không có khoảng trắng nào ở hai bên của dấu =.)
- Khi chúng ta tham chiếu đến một biến, chúng ta phải đặt một dấu đô la ($) trước tên biến.

![image](https://user-images.githubusercontent.com/25586998/129943483-e51b70db-c2ec-40cd-925f-ac3d2a629f97.png)

#### Command line arguments

Khi chúng ta chạy một tập lệnh, có một số biến được đặt tự động. Dưới đây là một số trong số họ:
- $0: Tên của script.
- $1 - $9: Bất kỳ đối số dòng lệnh nào được cung cấp cho tập lệnh. $1 là đối số đầu tiên, $2 là đối số thứ hai, v.v.
- $#: Có bao nhiêu đối số dòng lệnh đã được cung cấp cho tập lệnh.
- $*: Tất cả các đối số dòng lệnh.

![image](https://user-images.githubusercontent.com/25586998/129944027-46a8b773-31e6-4b77-8565-dc6d8f96a9fe.png)

#### Back ticks

Cũng có thể lưu kết quả đầu ra của một lệnh vào một biến và cơ chế được sử dụng cho đó là backtick(`). Đây là một ví dụ.

![image](https://user-images.githubusercontent.com/25586998/129944716-47e7ab2f-4c8e-4d93-8c99-74c012965620.png)


#### Câu lệnh If
Dưới đây là 1 ví dụ câu lệnh if:

![image](https://user-images.githubusercontent.com/25586998/129945292-8e28a209-ba18-4197-887c-a55914261a36.png)

- Dòng 6: Câu lệnh if đầu tiên của chúng ta. Trong câu lệnh này, chúng ta đang hỏi nếu số lượng đối số ($#) không bằng (!=) 1.
- Dòng 8: Nếu không thì script đã không được gọi đúng cách. In một tin nhắn giải thích cách sử dụng nó.
- Dòng 9: Vì tập lệnh chưa được gọi đúng cách, chúng ta muốn thoát tập lệnh trước khi tiếp tục.
- Dòng 10: Để biểu thị sự kết thúc của câu lệnh if, chúng ta có một dòng duy nhất có fi (if ngược) trên đó.
- Dòng 11: Câu lệnh If có thể kiểm tra rất nhiều thứ khác nhau. Ở đây dấu chấm than (!) Có nghĩa là không, -d có nghĩa là 'đường dẫn tồn tại
  và là một thư mục'. Vì vậy, dòng đọc là 'Nếu thư mục không tồn tại'
- Dòng 22: Có thể yêu cầu người dùng nhập. Lệnh chúng ta sử dụng cho điều đó được đọc. `read` nhận một đối số duy nhất là biến để lưu trữ câu trả lời.
- Dòng 23: Hãy xem cách người dùng phản hồi và hành động theo đó.

### Tài liệu tham khảo

https://ryanstutorials.net/linuxtutorial/scripting.php



