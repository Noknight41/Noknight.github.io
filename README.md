# Try to think Differrent (by thinhnp6)

## Assignment 1: Homepage

### Problem statement 1

Ai cũng có nơi để  trở về , đó gọi là nhà. Trên internet, đó gọi là homepage. Vậy nhiệm vụ của bài tập này là tạo 1 trang homepage của mình. 

### Problem Analysis

Một homepage của chúng ta sẽ có gì ? 

- Tự giới thiệu, các liên kết mạng xã hội,...
- Các bài blog của mình: chủ đề  có thể  nghiêm túc, có thể vui nhộn, mục đích xây dựng 1 bức tranh hoàn chỉnh cho người đọc về  bản thân.
- Có thể  có các project, kinh nghiệm làm việc của bản thân để  xây dựng tín dụng đối với người đọc về  mình.
- Có thể  có những thẻ (tag) để  giúp người đọc tìm kiếm bài đọc cùng chủ đề .

### Solution

Sử  dụng web tĩnh (Static Web), tại sao ?

Static Web so với Dynamic Web, cả hai đều có ưu và nhược điểm:

- Dynamic Web: 
    + Pro: Dễ  cập nhật nội dung, đa dạng về  tương tác giữa web và user
    + Con: Tốn thời gian trong xây dựng, các vấn đề  về  hiệu năng làm việc

- Static Web:
    + Pro: Xây dựng nhanh chóng, không đòi hỏi quá nhiều tài nguyên, ưu thế  trong hiệu năng
    + Con: Vấn đề  trong nâng cấp lên quy mô lớn

Quay lại vấn đề  của chúng ta, homepage cá nhân của chúng ta chỉ cần ở mức độ đơn giản, công sức cho nên dùng web tĩnh phù hợp hơn, như một MVP nhằm tìm kiếm xem có tập khách hàng hứng thú với mình. Ở đây, mình sử  dụng Hugo vì sự đa dạng trong số  lượng template Hugo cung cấp, khiến quy trình xây dựng trở nên nhanh hơn nữa

Link homepage: [https://noknight41.github.io/](https://noknight41.github.io/)

## Assignment 2: Design System

### Problem statement 2

Giả sử  chút xíu thôi, một cái giả sử  hơi nhỏ xíu, trang blog của bạn được truy cập bởi một số lượng user rất lớn, lên đến 10000 user trong thời gian cao điểm. Đề xuất thiết kế  cơ sở hạ tầng của hệ thống cho trang blog giải quyết được vấn đề  này.

### Problem Analysis

Vấn đề  ở đây là gì ? Nếu như trang web với 10000 user truy cập cùng lúc, chuyện gì sẽ xảy ra?

- Vấn đề  đầu tiên sẽ gặp phải sẽ là trang web sẽ load chậm, tương tác có độ trễ  lớn hay tệ hơn là crash hoàn toàn trang web. 
- Nguyên nhân là khi có nhiều người như vậy truy cập như vậy, website sẽ yêu cầu server nhiều tác vụ, bao gồm tác vụ truy cập database để  lấy nội dung bài blog, yêu cầu đến truy cập trang web con, yêu cầu business logic khác, ...
- Nếu không đủ số  lượng server, server không đủ lớn, hiện tượng trên là một vấn đề  thường xuyên
- Bên cạnh đó, một số vần đề khác như khoảng cách địa lý: Mình đặt server ở Việt Nam, vậy user của mình ở Mỹ sẽ gặp khó khăn trong việc load trang web và tài nguyên của mình.

### Solution

Link blog: [https://noknight41.github.io/post/sysdes/](https://noknight41.github.io/post/sysdes/)

## Tham khảo

- [System Design Primer](https://github.com/donnemartin/system-design-primer#master-slave-replication)
- [Vertical Scaling vs Horizontal Scaling](https://github.com/donnemartin/system-design-primer#load-balancer)
- [Database Partioning](https://en.wikipedia.org/wiki/Partition_(database))
- [Database Replication](https://github.com/donnemartin/system-design-primer#replication)
- [Application Layer](https://github.com/donnemartin/system-design-primer#application-layer)
- [Caching](https://github.com/donnemartin/system-design-primer#cache)
- [CDN](https://www.creative-artworks.eu/why-use-a-content-delivery-network-cdn/)