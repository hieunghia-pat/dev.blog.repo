---
title: "C++ - Old but gold"
author: NguyenNghia-PAT
description: C++11, C++14, C++17 - New syntax, new feature.
date: 2021-07-29T17:22:45+07:00
draft: false
toc: true
image: "images/cpp/header.jpg"
tags: ["C++"]
categories: [C++]
---
## Lời nói đầu
Bất cứ ai bắt đầu học lập trình ắt hẳn không còn xa lạ gì với cái tên C++. Được xem là một trong những ngôn ngữ mạnh mẽ với cú pháp khá rõ ràng, C++ không bao giờ không nằm trong những ngôn ngữ được lựa chọn bởi newbie. Tuy nhiên, qua nhiều diễn đàn và nhiều source code của các dự án, đa số các bạn mới học và học lâu năm về C++ vẫn còn dùng cú pháp đã cũ của C++, thậm chí có nhiều bạn vẫn còn sử dụng các lệnh của C trong các mã nguồn C++. Đây là điều không được khuyết khích bởi (1) các chuẩn mới của C++ đã tối ưu và cho phép chúng ta viết code một cách trong sáng và tinh tế hơn, giảm hẳn đi sự rườm rà cả về code lẫn logic (2) các cú pháp của C và C++ cũ khi được biên dịch các trình biên dịch sẽ sử dụng chuẩn C++ cũ hơn, điều này dẫn đến những rủi ro về logic của cú pháp lẫn ngôn ngữ do các chuẩn này chưa được tinh chỉnh và tối ưu; những cải tiến và thay đổi để cải thiện những rủi ro này được cập nhật ở các chuẩn sau, với cú pháp mới hơn và logic ngôn ngữ cũng tốt hơn. Do đó trong series các bài viết về C++ này, tôi sẽ trình bày chi tiết những điểm mới trong cú pháp và logic của ngôn ngữ, chúng hỗ trợ cho việc trừu tượng hóa task như thế nào, hiệu suất được cải thiện ra sao so với trước để giúp bạn đọc hiểu hơn và dần chuyển thói quen sang sử dụng các chuẩn mới của C++ để có thể sử dụng C++ hiệu quả hơn, khai thác hết được tiềm năng của C++ cho các dự án cá nhân cũng như của tập thể, doanh nghiệp.

***Note: mục tiêu của series này nhằm giới thiệu những tính năng và cú pháp mới của C++, không bàn kỹ về các lý thuyết cơ sở của ngôn ngữ như các khái niệm về biến, lệnh, con trỏ, lớp, kế thừa, đa hình, ... Bạn đọc có thể tham khảo các nguồn khác cho các khía cạnh này.***

## Những điểm nổi bật
Có thể thấy C++ là một trong những ngôn ngữ có sức ảnh hưởng mạnh mẽ nhất, cũng là ngôn ngữ có sức sống lâu bên nhất trong giới phát triển phần mềm. Sự ưu việt của C++ là điều không thể bàn cãi. Mang tính chất của một ngôn ngữ bán hướng đối tượng, C++ có thể được biên dịch trực tiếp thành mã máy, giúp cho quá trình xây dựng và release một project C++ diễn ra nhanh chóng. Cùng với khả năng compile và release nhanh chóng các file thực thi (executable files), C++ cũng có khả năng khai thác và sử dụng một cách hiệu quả và tối ưu bất kỳ nền tảng phần cứng nào. Tuy C++ không khai thác tối ưu phần cứng bằng C nhưng do được phát triển ở mức trừu tượng tốt hơn C nên C++ được nhiều nhà phát triển sử dụng hơn cho việc triển khai các dự án nhúng lẫn phần mềm mà không làm giảm đi đáng kể hiệu suất của sản phẩm. Đây được xem là một trong những tính năng quan trọng của C++ giúp cho ngôn ngữ này không bị lãng quên và chìm vào quá khứ, dẫu cho sự xuất hiện của hàng loạt ngôn ngữ bậc cao mới với cấu trúc trong sáng, tinh gọn và gần với ngôn ngữ con người hơn.

## Lịch sử phát triển
[Bjarne Stroustrup](https://en.wikipedia.org/wiki/Bjarne_Stroustrup) - Nhà khoa học máy tính người Đan Mạch, đã xây dựng ngôn ngữ *C with classes* cho luận văn tiến sĩ của ông vào khoảng năm 1979. Năm 1985, Stroustrup cho xuất bản tài liệu [The C++ Programming language](https://www.amazon.com/The-Programming-Language-hardcover-Edition/dp/0321958322) được xem như là tài liệu tham khảo, nghiên cứu đầu tiên về ngôn ngữ C++.

Năm 1985, Hội đồng chuẩn hóa C++ *(International Organization for Standardization (ISO) on C++)* công bố tiêu chuẩn quốc tế đầu tiên cho ngôn ngữ này: [ISO/IEC 14882:1998](www.iso.org/iso/catalogue_detail.htm?csnumber=25845), được biết đến với cái tên gần gũi hơn là C++98. Năm 2003, hội dồng chuẩn hóa xem xét và giải quyết các thắc mắc, bất cập của C++98, cho ra đời tiêu chuẩn mới cho C++ mang tên [C++03](www.iso.org/iso/catalogue_detail.htm?csnumber=25845).

Giữa năm 2011, hội đồng chuẩn hóa C++ thông qua dự án [boost](https://www.boost.org/) đã thêm vào C++03 nhiều tính năng mới mẻ, giúp cho C++ linh hoạt hơn về mặt logic và cú pháp. Tiêu chuẩn mới này được hội đồng thông qua với tê gọi [C++11](https://www.iso.org/standard/50372.html)

Cùng với quá trình phát triển và được sử dụng rộng rãi bởi các nhà phát triển phần mềm, các lập trình viên trên khắp thế giới. Hội đồng chuẩn hóa C++ lần lượt nhận được các phản hồi của các lỗ hổng logic về cú pháp và trình biên dịch của C++, thảo luận, cải thiện và thay đổi, chuẩn hóa lại ngôn ngữ, C++ theo thời gian lần lượt được trang bị các chuẩn mới [C++14](https://www.iso.org/standard/64029.html), [C++17](https://www.iso.org/standard/68564.html), chuẩn ổn định mới nhất tính đến thời điểm hiện tại là [C++20](https://www.iso.org/standard/79358.html) và chuẩn tiếp theo vẫn đang được hội đồng chuẩn hóa xem xét, thông qua là C++23 chuẩn bị ra mắt trong tương lai gần.

## C++ thông qua sách vở
Các cuốn sách, khóa học về lập trình C++ đa số được thiết kế và xây cách đây 4, 5 năm. Thậm chí có những cuốn sách về C, C++ được viết và xuất bản cách đây gần 7, 8 năm. Do sự hạn chế về mặt ngôn ngữ mà đa số các học sinh, sinh viên khi bắt đầu học C++ đếu tiếp xúc với các khóa học hay giáo trình tiếng Việt. Hệ quả là các cấu trúc, cú pháp của C++ mà họ tiếp thu là của chuẩn C++ cũ ([C++98](www.iso.org/iso/catalogue_detail.htm?csnumber=25845) hay [C++03](www.iso.org/iso/catalogue_detail.htm?csnumber=25845)). Do đó không thể tránh khỏi việc nhiều sinh viên, học sinh vẫn còn sử dụng các cú pháp cũ này trong các bài tập cũng như dự án của mình. Một lý do khác khách quan hơn đó là các framework của C++ đã được phát triển từ rất lâu đời (có thể kể đến như Qt), bản thân mã nguồn của các framework này cũng sử dụng các cú pháp của chuẩn C++ cũ do các nhà phát triển vẫn chưa kịp cập nhật, nâng cấp. Nhưng hơn hết, ngay cả khi sử dụng đan xen, ở bất cứ đoạn mã nguồn nào, nếu có thể thì việc sử dụng các cú pháp, tính năng mới của C++ vẫn rất hữu ích và hiệu quả, cả về mặt cú pháp vẫn logic mà tôi sẽ chỉ ra trong suốt series này. Do đó tôi vẫn khuyến khích bạn đọc nghiên cứu và sử dụng các chuẩn mới hơn của C++, đặc biệt khuyến khích các bạn tham khảo và nghiên cứu thông qua các tài liệu nước ngoài.

## Vài quy ước
Mặc dù C++ là một ngôn ngữ [bán hướng đối tượng](https://www.geeksforgeeks.org/c-partially-object-oriented-language/#:~:text=Here%20are%20the%20reasons%20C%2B%2B,using%20an%20object%20even%20once.) tuy nhiên sự phát triển của các chuẩn C++ và thư viện chuẩn Standard Library (std) của nó đã khiến ngôn ngữ này không còn mang tính chất của một ngôn ngữ bán hướng đối tượng nữa. Bản chất bán hướng đối tượng của ngôn ngữ tất nhiên không bị mất đi, những tất cả các kiểu dữ liệu (thậm chí là các kiểu dữ liệu cơ sở - primary value types như int, float, char, ...), các thao tác với I/O devices hay quản lý luồng, tiến trình, ... đều đã được trừu tượng hóa thành các class, dẫn đến C++ hiện tại mang tính chất của một ngôn ngữ hướng đối tượng hơn là bán hướng đối tượng. Do đó trong series bài viết này, mình sử dụng thuật ngữ "đối tượng" thay cho thuật ngữ "biến" khi đề cập đến các khái niệm về "biến" mà bạn đọc đã biết khi học Pascal ở thời cấp 2.

Khi khai báo một đối tượng với kiểu dữ liệu của nó, do đặc thù của cú pháp mà sẽ có nhiều cách khai báo cho cùng 1 ý nghĩa. Ví dụ:
```c++
    int a = 10;
    int const* pA_1 = &a;
    const int* pA_2 = &a;
```
ở cả 2 cách khai báo trên, cả `pA_1` và `pA_2` đều là các con trỏ hằng (cần phân biệt con trỏ hằng và con trỏ trỏ đến hằng, tức vùng nhớ chỉ cho phép đọc (read-only)). Do vậy để tránh sự không đồng nhất về cách khai báo như trên, trong series bài viết này mình sẽ sử dụng cách khai báo đối tượng với quy ước:

## Lời kết
Cuối cùng, do quá trình viết blog và kiến thức của tôi về C++ chắc chắn vẫn còn một vài điểm cần phải được cải thiện, nên tôi luôn đón nhận mọi đóng góp và phản hồi về nội dung cũng như kiến thức của bản thân. Mọi liên lạc và phản hồi bạn đọc có thể liên hệ qua [Facebook](https://www.facebook.com/hieunghia.nhn) hoặc [Email](19520178@gm.uit.edu.vn) cá nhân của tôi. Cảm ơn bạn đọc đã hỗ trợ và đóng góp ý kiến.