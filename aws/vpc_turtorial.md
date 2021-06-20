## Amazon VPC and Subnets

Amazon Virtual Private Cloud (Amazon VPC) cho phép bạn khởi chạy các tài nguyên AWS vào một mạng ảo mà bạn đã xác định. Mạng ảo này gần giống với mạng truyền thống mà bạn sẽ vận hành trong trung tâm dữ liệu của riêng mình, với lợi ích của việc sử dụng cơ sở hạ tầng có thể mở rộng của AWS.

Mỗi VPC mà bạn tạo được tách biệt hợp lý với các mạng ảo khác trong AWS cloud và hoàn toàn có thể tùy chỉnh nó. Bạn có thể chọn dải địa chỉ IP, tạo subnets,
cấu hình `root tables`, thiết lập `network gateways`, xác định cài đặt bảo mật bằng cách sử dụng `security groups` và điều khiển danh sách có thể truy cập.`

### Default Amazon VPC
Mỗi tài khoản Amazon đều được đi kèm với một VPC mặc định được định cấu hình trước để bạn có thể bắt đầu sử dụng ngay lập tức. VPC có thể mở rộng nhiều vùng khả
dụng trong một vùng. Đây là sơ đồ của VPC mặc định:

![amazon-vpc](https://user-images.githubusercontent.com/25586998/122665511-da8d5e80-d1d1-11eb-8efc-38977cf6c4e1.jpeg)

Trong phần đầu tiên, có Amazon VPC mặc định. Khối CIDR cho VPC mặc định luôn là 16 `subnet mask`; trong ví dụ này, nó là 172.31.0.0/16. Nó có nghĩa là VPC
này có thể cung cấp tới 65.536 địa chỉ IP.

### Custom Amazon VPC
![custom-vpc](https://user-images.githubusercontent.com/25586998/122665585-466fc700-d1d2-11eb-9a3f-b4b5d5a23f26.jpeg)

VPC mặc định phù hợp để khởi chạy các phiên bản mới khi bạn đang thử nghiệm với AWS, nhưng việc ban tự tạo 1 VPC tùy chỉnh sex cho phép bạn:

- Làm cho mọi thứ an toàn hơn
- Tùy chỉnh mạng ảo của bạn, vì bạn có thể xác định dải địa chỉ IP của riêng mình
- Tạo Subnet(mạng con ảo) của bạn cả private/public
- Thắt chặt cài đặt bảo mật
### Hardware VPN Access
Theo mặc định, các instances mà bạn khởi chạy trong Amazon VPC sẽ không thể giao tiếp với mạng của bạn. Bạn có thể kết nối VPC với trung tâm dữ liệu hiện có của
mình bằng cách sử dụng `hardware VPN access`. Bằng cách đó, bạn có thể mở rộng hiệu quả trung tâm dữ liệu của mình vào cloud và tạo ra một môi trường kết hợp.
Để làm điều này, bạn sẽ cần thiết lập một `virtual private gateway`.

![aws-vpn](https://user-images.githubusercontent.com/25586998/122665766-5b008f00-d1d3-11eb-833c-a570dd1400ab.jpeg)

Có một bộ tập trung VPN ở phía Amazon của kết nối VPN. Đối với trung tâm dữ liệu của bạn, bạn cần một `customer gateway` là một thiết bị vật lý hoặc một ứng dụng
phần mềm nằm ở phía khách hàng của kết nối VPN. Khi bạn tạo kết nối VPN, một đường hầm VPN sẽ xuất hiện khi lưu lượng truy cập được tạo từ phía khách hàng của
kết nối.

### VPC Peering 

Một kết nối ngang hàng(VPC Peering) có thể được thực hiện giữa các VPC của riêng bạn hoặc với VPC trong tài khoản AWS khác, miễn là nó ở cùng khu vực.

![vpc](https://user-images.githubusercontent.com/25586998/122665844-f0038800-d1d3-11eb-852f-8288ccc9c1ae.jpeg)

Nếu bạn có các instances trong VPC A, chúng sẽ không thể giao tiếp với các instance trong VPC B hoặc C trừ khi bạn thiết lập kết nối ngang hàng. 
Peering là mối quan hệ một-một; một VPC có thể có nhiều kết nối ngang hàng với các VPC khác, nhưng tính năng kết nối gang hàng bắc cầu không được hỗ trợ.
Nói cách khác, VPC A có thể kết nối với B và C trong sơ đồ trên, nhưng C không thể giao tiếp với B trừ khi được ghép nối trực tiếp.


Ngoài ra, không thể ghép nối các VPC có CIDR chồng chéo. Trong sơ đồ, tất cả các VPC đều có các dải IP khác nhau. Nếu chúng có cùng dải IP, chúng sẽ không
thể ghép nối.

### Default VPC Deletion

Trong trường hợp VPC mặc định bị xóa, bạn nên liên hệ với bộ phận hỗ trợ AWS để khôi phục. Do đó, bạn chỉ nên xóa VPC mặc định nếu có lý do chính đáng.

## Private, Public, and Elastic IP Addresses

### Private IP addresses
Địa chỉ IP không thể truy cập qua internet được định nghĩa là private. Private ip cho phép giao tiếp giữa các instance trong cùng một mạng. 
Khi bạn khởi chạy một instance mới, một địa chỉ IP riêng sẽ được chỉ định và một tên máy chủ DNS nội bộ được cấp phát để phân giải thành địa chỉ IP riêng
của instance đó. Nếu bạn muốn kết nối với nó từ internet, nó sẽ không hoạt động. Bạn sẽ cần một địa chỉ cho điều đó

### Public IP addresses

Public IP addresses được sử dụng để liên lạc giữa các instance khác trên internet và của bạn. Mỗi phiên bản có Public IP addresses cũng được gán một tên máy chủ DNS bên ngoài. Các Public IP addresses được liên kết với các instance của bạn là từ danh sách các IP công cộng của Amazon. Khi dừng hoặc kết thúc phiên bản của bạn, Public IP addresses sẽ được giải phóng và một địa chỉ mới được liên kết với instance khi nó khởi động lại. Để lưu giữ Public IP addresses này ngay cả sau khi ngừng hoạt động hoặc chấm dứt, cần phải sử dụng một elastic IP address.

### Elastic IP Addresses
Elastic IP Addresses là các public IP tĩnh và liên tục đi kèm với tài khoản của bạn. Nếu bất kỳ phần mềm hoặc instance nào của bạn bị lỗi, chúng có thể được ánh xạ lại thành instance khác một cách nhanh chóng bằng Elastic IP Addresses. Elastic IP Addresses vẫn còn trong tài khoản của bạn cho đến khi bạn chọn phát hành. Một khoản phí được liên kết với một Elastic IP Addresses nếu nó nằm trong tài khoản của bạn, nhưng không được phân bổ cho một instance.

## Subnet

AWS định nghĩa mạng con(subnet) là một dải địa chỉ IP trong VPC của bạn. Bạn có thể khởi chạy các tài nguyên AWS vào một mạng con đã chọn. Một mạng con công cộng có thể được sử dụng cho các tài nguyên được kết nối với internet và một mạng con riêng cho các tài nguyên không được kết nối với internet.

Mặt nạ mạng(netmask) cho mạng con mặc định trong VPC của bạn luôn là 20, cung cấp tới 4.096 địa chỉ cho mỗi mạng con, với một số ít trong số đó được dành riêng cho AWS. VPC có thể mở rộng nhiều `availability zone`, nhưng mạng con luôn được ánh xạ tới một availability zone khả dụng duy nhất.

Sau đây là sơ đồ cơ bản của một mạng con:

![amazon-route-s3 (1)](https://user-images.githubusercontent.com/25586998/122668640-3b716280-d1e3-11eb-8033-8aea8e32546a.jpeg)


Đây là một VPC bao gồm các vùng khả dụng. Một mạng con được tạo bên trong mỗi `availability zone` và bạn không thể khởi chạy bất kỳ instance nào trừ khi có mạng con trong VPC của bạn.

### Public and Private Subnets
Có hai loại mạng con: public và private. Public subnet được sử dụng cho các tài nguyên phải được kết nối với internet; máy chủ web là một ví dụ. Public subnet được đặt ở chế độ công khai vì route table chính gửi lưu lượng mạng con dành cho internet đến internat gateway.

Private subnet dành cho các tài nguyên không cần kết nối internet hoặc bạn muốn bảo vệ khỏi internet; các instance cơ sở dữ liệu là một ví dụ.

## Networking

### Internet Gateway

Internet Gateway là một phần dư, horizontally scaled và là một thành phần VPC có tính khả dụng cao. Nó cho phép giao tiếp giữa các instance trong VPC của bạn và internet. Do đó, nó không có rủi ro về tính khả dụng hoặc hạn chế băng thông đối với lưu lượng mạng của bạn.


Để cung cấp cho VPC của bạn khả năng kết nối với internet, bạn cần phải gắn một internet gateway. Mỗi VPC chỉ có thể gắn một internet gateway. Gắn internet gateway là giai đoạn đầu tiên để cho phép truy cập internet vào các instance trong VPC của bạn.

![aws-vpc (1)](https://user-images.githubusercontent.com/25586998/122668793-03b6ea80-d1e4-11eb-8d84-faf128d24195.jpeg)

Trong sơ đồ này, chúng tôi đã thêm internet gateway đang cung cấp kết nối internet cho VPC của bạn. Để phiên bản EC2 được kết nối internet, bạn phải tuân thủ các quy tắc sau:

1. Attach 1 Internet gateway vào VPC của bạn
2. Đảm bảo rằng các instance của bạn có public IP hoặc elastic IP
3. Trỏ route table của subnet của bạn đến internet gateway 
4. Đảm bảo rằng security group và các quy tắc kiểm soát truy cập mạng của bạn cho phép lưu lượng truy cập có liên quan vào và ra khỏi instance của bạn

## Route Table
Amazon định nghĩa Route Table là một tập hợp các quy tắc, được gọi là các routes, được sử dụng để xác định nơi lưu lượng truy cập mạng được hướng đến. Mỗi mạng con phải được liên kết với một route table và một mạng con chỉ có thể được liên kết với một route table. Mặt khác, một route table có thể có các liên kết với nhiều mạng con. Mỗi VPC đều có một route table mặc định và bạn nên để nguyên bảng đó ở trạng thái ban đầu và tạo một route table mới để tùy chỉnh các tuyến lưu lượng mạng được liên kết với VPC của bạn. Sơ đồ bảng lộ trình như hình:

![custom-route-table](https://user-images.githubusercontent.com/25586998/122668949-fe0dd480-d1e4-11eb-8ae1-b9913482c169.jpeg)

Trong ví dụ này, chúng tôi đã thêm hai bảng route table: route table chính và route table tùy chỉnh. Route table mới hoặc route table tùy chỉnh thông báo cho internet gateway để chỉ đạo.

## NAT Devices
Network Address Translation (NAT) được sử dụng để cho phép các instances trong private subnet kết nối với internet hoặc các dịch vụ AWS khác, nhưng điều này ngăn không cho internet kết nối với các instance trong private subnet.

Như đã đề cập trước đó, public/private subnet bảo vệ tài sản của bạn khỏi được kết nối trực tiếp với internet. Ví dụ: máy chủ web của bạn sẽ nằm trong public subnet và cơ sở dữ liệu trong private subnet, không có kết nối internet. Tuy nhiên, instance cơ sở dữ liệu trong private subnet của bạn vẫn có thể cần truy cập internet hoặc khả năng kết nối với các tài nguyên AWS khác. Bạn có thể sử dụng NAT Devices để làm như vậy.

NAT Devices hướng lưu lượng truy cập từ private subnet của bạn tới internet hoặc các dịch vụ AWS khác. Sau đó, nó sẽ gửi phản hồi trở lại các instance của bạn. Khi lưu lượng truy cập được chuyển hướng đến internet, địa chỉ IP nguồn của instance của bạn sẽ được thay thế bằng địa chỉ NAT Devices và khi lưu lượng truy cập internet quay trở lại, NAT Devices sẽ dịch địa chỉ đó thành private IP của instance của bạn.

Sơ đồ NAT devices:

![NAT-device-diagram](https://user-images.githubusercontent.com/25586998/122676375-09262c00-d208-11eb-96d0-52b8555a3510.jpeg)

Trong sơ đồ, bạn có thể thấy một NAT Device được thêm vào public subnet để có được kết nối internet.


## Security Groups và Network ACLs 

Amazon định nghĩa `security group` là một tường lửa ảo kiểm soát lưu lượng truy cập cho một hoặc nhiều trường hợp. Các quy tắc được thêm vào mỗi security group, cho phép lưu lượng truy cập đến hoặc đi từ các instance liên quan của nó. Về cơ bản, một security group kiểm soát lưu lượng vào và ra cho một hoặc nhiều instance EC2. Nó có thể được tìm thấy trên cả bảng điều khiển EC2 và VPC trong bảng điều khiển quản lý web AWS.

Sơ đồ security group

![main-route](https://user-images.githubusercontent.com/25586998/122676534-ba2cc680-d208-11eb-9fe5-0bc84b9d29af.jpeg)

### Security Groups cho Web Servers
Máy chủ web cần ít nhất lưu lượng HTTP và HTTPS để truy cập nó. Sau đây là một ví dụ về bảng security group:

![security-group](https://user-images.githubusercontent.com/25586998/122676587-e1839380-d208-11eb-8af7-b2a9cd2195a2.jpeg)


Ở đây, chúng ta cho phép HTTP và HTTPS, các cổng được liên kết với chúng và các nguồn từ internet. Tất cả lưu lượng được phép đến các cổng đó, vì vậy bất kỳ lưu lượng nào khác đến trên các cổng khác nhau sẽ không thể đến được security group và các instance bên trong.

### Security Groups for Database Servers

Nếu chúng ta sử dụng là cơ sở dữ liệu máy chủ SQL, thì bạn cần mở cổng máy chủ SQL để có thể truy cập nó.

![database-servers](https://user-images.githubusercontent.com/25586998/122676644-214a7b00-d209-11eb-8873-290095dd4820.jpeg)

Ở đây chúng ta đã cho phép nguồn đến từ internet. Với là máy Windows, bạn có thể cần quyền truy cập RDP để đăng nhập và thực hiện một số quản trị. Chúng ta cũng đã thêm quyền truy cập RDP vào security. Bạn có thể để nó mở trên internet, nhưng điều đó có nghĩa là bất kỳ ai cũng có thể thử và xâm nhập vào. Trong ví dụ này, chúng ta đã thêm địa chỉ IP nguồn là 10.0.0.0, do đó, dải IP duy nhất từ địa chỉ đó có thể RDP vào cơ sở dữ liệu của bạn.

### Security Groups Rules

Có một số quy tắc được liên kết với các security group, chẳng hạn như:

- Các security group cho phép tất cả lưu lượng đi theo mặc định. Nếu bạn muốn thắt chặt an ninh của mình, điều này có thể được thực hiện theo cách tương tự như khi bạn xác định lưu lượng truy cập vào
- Các quy tắc của security group luôn được cho phép. Bạn không thể tạo các quy tắc từ chối quyền truy cập
- Các security group là stateful. Nếu một yêu cầu được gửi từ instance của bạn, lưu lượng phản hồi cho yêu cầu đó được phép chuyển vào, bất kể quy tắc nhóm bảo mật gửi đến
- Các quy tắc của một security group có thể được sửa đổi bất kỳ lúc nào và áp dụng ngay lập tức

## Network ACL
Network Access Control List (ACL) là lớp bảo mật tùy chọn cho VPC của bạn. Nó hoạt động như một bức tường lửa để kiểm soát luồng lưu lượng vào và ra một hoặc nhiều mạng con. ACL mạng có thể được thiết lập với các quy tắc tương tự như các security group của bạn.


Đây là sơ đồ network:

![network-acl](https://user-images.githubusercontent.com/25586998/122676863-09bfc200-d20a-11eb-9ea3-a041f6cfcb4b.jpeg)


Bạn có thể tìm thấy các ACL mạng nằm ở đâu đó giữa các root tables và các mạng con. Đây là một sơ đồ đơn giản:

![virtual-pvt-gtw](https://user-images.githubusercontent.com/25586998/122676884-22c87300-d20a-11eb-9f73-03e9d16e62df.jpeg)


Bạn có thể xem một ví dụ về Network ACL mặc định, được cấu hình để cho phép tất cả lưu lượng truy cập vào và ra khỏi mạng con mà nó được liên kết.

![inbound-outbound](https://user-images.githubusercontent.com/25586998/122676916-3e337e00-d20a-11eb-99d1-8f9b572bfc76.jpeg)

Mỗi Network ACL bao gồm một quy tắc * (dấu hoa thị) làm số quy tắc. Quy tắc đảm bảo rằng nếu một gói được xác định là không khớp với bất kỳ quy tắc được đánh số nào khác, lưu lượng truy cập sẽ bị từ chối. Bạn không thể sửa đổi hoặc xóa quy tắc này.


Đối với lưu lượng truy cập khi đến:
- Quy tắc 100 sẽ cho phép lưu lượng truy cập từ tất cả các nguồn
- Quy tắc * sẽ từ chối lưu lượng truy cập từ tất cả các nguồn

### Network ACL Rules 

- Mọi mạng con trong VPC của bạn phải được liên kết với một ACL, nếu không, mạng con sẽ tự động được liên kết với ACL mặc định của bạn.
- Một mạng con chỉ có thể được liên kết với một ACL. Mặt khác, một ACL có thể được liên kết với nhiều mạng con.
- ACL có một danh sách các quy tắc được đánh số được đánh giá theo thứ tự, bắt đầu từ giá trị thấp nhất. Ngay khi quy tắc khớp, lưu lượng truy cập được cung cấp bất kể quy tắc nào được đánh số cao hơn có thể mâu thuẫn với quy tắc đó. AWS khuyên bạn nên tăng các quy tắc của bạn lên hệ số 100. Điều này cho phép có nhiều khoảng trống để triển khai các quy tắc mới vào một ngày sau đó.
- Không giống như các security group, ACL là stateless(không trạng thái); phản hồi để cho phép lưu lượng truy cập vào phải tuân theo các quy tắc về lưu lượng đi.

## Tài liệu tham khảo
https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html
https://www.simplilearn.com/tutorials/aws-tutorial/aws-vpc


