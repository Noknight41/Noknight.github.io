---
title: Starter to System Design
subtitle: How to build a blog website for a not-so little customer base ?
date: 2022-05-11T16:34:49+07:00
tags: ["VNG-Fresher", "VN"]
---

Imagine you have to design a system for serving blogs to massive readers (10k tps). How would you design the system?

<!--more-->

Giả sử  chút xíu thôi, một cái giả sử  hơi nhỏ xíu, trang blog của bạn được truy cập bởi một số lượng user rất lớn, lên đến 10000 user trong thời gian cao điểm. Đề xuất thiết kế cơ sở hạ tầng của hệ thống cho trang blog giải quyết được vấn đề  này.

### Problem Analysis

Vấn đề  ở đây là gì ? Nếu như trang web với 10000 user truy cập cùng lúc, chuyện gì sẽ xảy ra?

- Vấn đề  đầu tiên sẽ gặp phải sẽ là trang web sẽ load chậm, tương tác có độ trễ  lớn hay tệ hơn là crash hoàn toàn trang web. 
- Nguyên nhân là khi có nhiều người như vậy truy cập như vậy, website sẽ yêu cầu server nhiều tác vụ, bao gồm tác vụ truy cập database để  lấy nội dung bài blog, yêu cầu đến truy cập trang web con, yêu cầu business logic khác, ...
- Nếu không đủ số  lượng server, server không đủ lớn, hiện tượng trên là một vấn đề  thường xuyên
- Bên cạnh đó, một số vần đề khác như khoảng cách địa lý: Mình đặt server ở Việt Nam, vậy user của mình ở Mỹ sẽ gặp khó khăn trong việc load trang web và tài nguyên của mình.



### Vertical Scaling 

Ta đã xác định nguyên nhân của vấn đề  là sự thiếu hụt về lượng. Vậy ta cứ thêm lượng vào vấn đề là mọi chuyện sẽ ok phải không ? Lượng thiếu hụt chỗ nào, thêm vào chỗ  đó. Ít server ? Tăng lượng server ? Server yếu ? Gắn bộ nhớ, thêm nhân cho bộ xử lý, thay ổ đĩa từ HDD sang SSD. Một cách đơn giản, ta cho Server scale theo số lượng user.


Tất nhiên, đây là một giải pháp hoàn toàn hợp lý, được gọi là Vertical Scaling (Scale theo chiều dọc - liên tưởng đến xếp nhiều server thành từng chồng). 

- Không cần thay đổi quá nhiêu về kiến trúc trang web
- Đơn giản mà hiệu quả


Tuy nhiên, khi ta đưa giải pháp này ra, ta sẽ gặp **1 vài** điểm bất hợp lý:

- Tiền - Nguồn cơ của mọi tội ác =)
- Tồn tại giới hạn mà ta có thể nâng cấp server lên phiên bản state-of-the-art hiện tại
- Không thể hiện được kiến thức của ta về thiết kế hệ thống

Vì vậy, trong bài tập thiết kế hệ thống, ta sẽ tập trung bàn về  Horizontal Scaling (Scale theo chiều ngang - liên tưởng đến dàn trải, chia nhỏ workload cho nhiều server)

Tuy nhiên, ta cũng phải nhấn mạnh rằng, Horizontal Scaling cần đi chung Vertical Scaling và cũng cần liên tục cân nhắc khi đối mặt với vần đề này.

### Horizontal Scaling

#### Giai đoạn 1 (Humble Beginning)

Tiếp cận vấn đề này Horizontal Scaling, chúng ta nên đi từ cấu trúc đơn giản đi lên. Dưới đây là 1 kiến trúc đơn giản của trang web của mình.

![Image_1](/img/sysdes/1.png)

#### Giai đoạn 2 (Database Partition)

Bắt đầu từ đây, trước khi ta nâng cấp số lượng server lên nhiều server, ta nhận thấy sử dụng 1 database cho toàn bộ có thể tạo ra bottleneck cho toàn bộ hệ thống, tương tự như 1 server của chúng ta. 

Chính vì vậy, ta chia database thành 2 thành phần như hình dưới: MySQL database lưu trữ thông tin update liên tục như user, comment, tag, ..., trong khi Object Store lưu trữ các thông tin không update thường xuyên như, file CSS, JS, Video, hình ảnh, ... Điều này còn giúp tăng khả năng Vertical Scaling của database của chúng ta. 

![Image_2](/img/sysdes/2.png)

#### Giai đoạn 3 (Load Balancing)

Trang web ta phổ biến hơn, lượng user truy cập nhiều hơn và hệ thống ta phải tiếp tục thực hiện Vertical Scaling thêm. Thay vì 1 server, ta thêm nhiều server để đảm bảo luôn đủ server cho thời gian đông đúc. Nhưng 1 vấn đề khác lại xảy ra, DNS thay vì chọn server đang không có load quá nhiều của chúng ta, lại chọn đúng cái server đang nhận khác nhiều công việc, dẫn đến không sử dụng hết công suất của server hiện có. Đây là lúc ta cần đến Load Balancing.

Load Balacer là một thành phần quan trọng của cơ sở hạ tầng mạng, thường được sử dụng để cải thiện hiệu suất cũng như độ tin cậy của các trang web, giúp máy chủ ảo hoạt động đồng bộ và hiệu quả hơn thông qua việc phân phối đồng đều tài nguyên.

Bên cạnh đó, bằng cách sử dụng Load Balancer, những yêu cầu từ người dùng sẽ được tiếp nhận và xử lý trước khi được phân chia đến các máy chủ. Đồng thời, trong quá trình phản hồi , những thông tin đó cũng được xét duyệt thông qua Load Balancer, giúp ngăn cản việc người dùng giao tiếp trực tiếp với máy chủ, tránh được các cuộc tấn công như DDoS.

![Image_3](/img/sysdes/3.2.png)

#### Giai đoạn 4 (Database Replication)

Trang web của chúng ta đã trên con đường bền vững giữa Horizontal Scaling và Vertical Scaling. Tuy nhiên, bottleneck của chúng ta lại quay về  cái database, đặc biệt là MySQL database. Với trang web chúng ta phát triển, chúng ta cần ưu tiên về redundacy và avaiability.Chính vì thể ta cần nhiều database hơn và một phương pháp được áp dụng để giải quyết vấn đề trên là Database Replication, cụ thể là Master-Slave Replication.

Ý tưởng Master-Slave Replication là, có một database server làm database chính (Master), tất cả request đọc/ghi đều thực hiện trên server này. Ngoài ra, các datbase server phụ (Slave) chỉ đảm nhiệm đọc dữ liệu mà thôi.

Trên thực tế, trang web blogs của chúng ta có nhiều yêu cầu READ nhiều hơn WRITE nên áp dụng pattern này là hợp lý. Một số ưu điểm mà ta có được là:

- Truy vấn đọc được xử lý tốt hơn
- Tránh được thiếu thống nhất về data so với áp dụng pattern Multi-Master
- Có khả năng phục hồi dữ liệu nếu database Master bị sự cố

Bên cạnh đó, ta có thể thêm các SQL Read Replica để giảm load truy cập vào Master.

![Image_4](/img/sysdes/3.3.png)

#### Giai đoạn 5 (Application Layer + Cache + CDN)

Ở đây ta có thể thấy sự phân chia giữa Web server và các API gọi đến Database. Thực hiện phân chia Web Layer khỏi Application Layer (bao gồm các API) cho phép ta thực hiện Scale và cấu hình cả hai lớp một cách độc lập. 

Chúng ta đã đi khá xa rồi mà chưa nhắc đến khái niệm caching. Nếu 1 process để load 1 trang web mà user đã gọi trước của chúng ta bao gồm:
- User gọi đến Load Balancer xin trả về trang web đó
- Load balancer kiểm tra yêu cầu, routing đến 1 server đang trống
- Server gọi API đến database chứa dữ liệu đó
- Server truy xuất từ nhiều database khác để lấy về đầy đủ trang web yêu cầu
- Server trả về cho user trang web đó

Thay vào đó, sao ta không lưu trang web đó một bộ nhớ đệm, truy xuất bộ nhớ đệm nếu chứa trang web đó và trả về. Thật đơn giản mà hiệu quả

Về vấn đề khoảng cách địa lý nêu đầu bài, bên cạnh dàn trải lượng lớn server hiện có ra khắp nơi, tăng tính availbility lên, ta có thể áp dụng công nghệ CDN. CDN hoạt động giống như cache, nhưng CDN được đặt ở nhiều nơi, tăng tính availablity về mặt địa lý. 

CDN lấy nội dung mới từ máy chủ của bạn khi người dùng đầu tiên yêu cầu nội dung. Bạn để nội dung trên máy chủ của mình và viết lại các URL để trỏ đến CDN. Điều này dẫn đến yêu cầu chậm hơn cho đến khi nội dung được lưu vào bộ nhớ cache trên CDN. Các trang web có lưu lượng truy cập lớn hoạt động tốt với các CDN, vì lưu lượng truy cập được trải đều hơn và chỉ còn lại nội dung được yêu cầu gần đây trên CDN.

![Image_5](/img/sysdes/4.3.png)


### Kết luận

Như vậy, vừa rồi là phân trình bày của mình về ý tưởng thiết kế 1 trang web blogs và scale lên cho số lượng user khổng lồ dựa trên kiến thức mình thu thập từ 

Tất nhiên, scaling và optimizing là một quá trình lặp đi lặp lại không ngừng và việc tìm tòi về lĩnh vực này cũng vậy

## Tham khảo

- [System Design Primer](https://github.com/donnemartin/system-design-primer#master-slave-replication)
- [Vertical Scaling vs Horizontal Scaling](https://github.com/donnemartin/system-design-primer#load-balancer)
- [Database Partioning](https://en.wikipedia.org/wiki/Partition_(database))
- [Database Replication](https://github.com/donnemartin/system-design-primer#replication)
- [Application Layer](https://github.com/donnemartin/system-design-primer#application-layer)
- [Caching](https://github.com/donnemartin/system-design-primer#cache)
- [CDN](https://www.creative-artworks.eu/why-use-a-content-delivery-network-cdn/)

