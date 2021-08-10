---
title: "Modern C++ - Auto and Range-based for loop"
date: 2021-07-29
description: Auto keyword and Range-based for loop.
author: hieunghia-pat
draft: false
toc: true
image: "images/cpp/header.jpg"
tags: ["C++"]
categories: [C++]
---

## Auto keyword - mở đầu cho xu hướng compile-time programming
`auto` lần đầu tiên được giới thiệu trong C++11 với mục đích nhằm đơn giản hóa việc sử dụng kiểu giá trị khi đặt cho một đối tượng. Như chũng ta đã biết C++ là một ngôn ngữ strong-type programming, tức là nó đòi hỏi phải khai báo trước kiểu dữ liệu của giá trị mới có thể compile về mã máy. Đây cũng là một đặc điểm chính giúp cho các chương trình được viết bằng C++ vận hành nhanh hơn và sử dụng tối ưu hơn phần cứng của thiết bị, và cũng là lý do khiến quá trình phát triển ứng dụng C++ lằng nhằng và phức tạp hơn (bản thân mình cũng thấy vậy :>). Theo sự phát triển của compiler và các chuẩn của C++, chính yêu cầu strong-type của C++ lại gây khó cho các lập trình viên khi phải xác định chính xác nhiều kiểu dữ liệu cho đối tượng mà kiểu dữ liệu đó có khi khó xác định chính xác hoặc cú pháp của nó rất dài khi viết ra. Ví dụ như đoạn code bên dưới:

```C++
    // group.hpp
    class Group
    {
    public:
        Group(std::initializer_list<std::string>&& init_names);
        ~Group();

        friend std::ostream& operator<<(std::ostream& os, Group const& group);
    private:
        std::vector<std::string> m_names;
    };
```

```C++
    // group.cpp
    Group::Group(std::initializer_list<std::string>&& init_names)
        : m_names {init_names}
    {
        
    }

    std::ostream& operator<<(std::ostream& os, Group& group)
    {
        os << "++++++" << std::endl;
        for (std::vector<std::string>::iterator it = group.m_names.begin(); it != group.m_names.end(); it++)
        {
            os << *it << std::endl;
        }
        os << "++++++" << std::endl;
        return os;
    }

    Group::~Group()
    {
        
    }   
```

```C++
    // main.cpp
    Group group({ "Charlotte", "Bolivia", "Charles", "Steven" });
    std::cout << group << std::endl;
```
The output is:
```
    ++++++
    Charlotte
    Bolivia
    Charles
    Steven
    ++++++
```
Trong câu lệnh `for (std::vector<std::string>::iterator it = group.m_names.begin(); it != group.m_names.end(); it++)`, khi xác định iterator để duyệt qua vector container, có thể thấy cú pháp để xác định kiểu dữ liệu cho iterator khá là phức tạp. Từ C++11 trở đi, chúng ta hoàn toàn có thể sử dụng từ khóa auto thay cho cú pháp xác định kiểu dữ liệu như trên một cách đơn giản:
```C++
    for (auto it = group.m_names.begin(); it != group.m_names.end(); it++)
    {
        os << *it << std::endl;
    }
```
Lúc này kết quả của chương trình vẫn không thay đổi, tuy nhiên cú pháp đã trở nên gọn đi rất nhiều.

khi một đối tượng được khai báo kiểu với từ khóa `auto`, compiler sẽ dựa trên ngữ cảnh (context) của đoạn code để xác định kiểu dữ liệu cho đối tượng và thay thế auto bằng kiểu dữ liệu đó. Điều này có nghĩa là với từ khóa `auto`, việc xác định kiểu dữ liệu cho đối tượng hoàn toàn do compiler đảm nhận và được hoàn tất trước khi đoạn code được compile về mã máy.

Tại sao chúng ta nên dùng từ khóa `auto`?

Ưu điểm đầu tiên có thể thấy đó là tối ưu về mặt cú pháp cũng như logic khi chúng ta xây dựng code. Việc xác định kiểu dữ liệu cho một đối tượng đôi khi nó là quá trình khá là nhàm chán, chúng ta khi xây dựng chương trình nhiều lúc không muốn dành quá nhiều tập trung vào việc chọn kiểu dữ liệu sao cho thích hợp mà chúng ta cần dành sự tập trung đó vào việc xây dựng chức năng cho đối tượng như thế nào để phù hợp. Lúc này `auto` xuất hiện như một giải pháp giúp tối ưu quá trình logic của chúng ta khi xây dựng code, hỗ trợ tốt hơn quá trình trừu tượng hóa vấn đề khi lập trình.

Một ưu điểm quan trọng hơn đó là việc xác định kiểu dữ liệu cho đối tượng của chúng ta đôi khi không tối ưu bằng việc để cho compiler đảm nhiệm vấn đề đó. Do compiler khi xác định kiểu dữ liệu cho đối tượng sẽ sử dụng đến ngữ cảnh của code nên chắc chắn rằng nó sẽ giúp chúng ta xác định tốt hơn và tối ưu về mặt bộ nhớ hơn kiểu dữ liệu của đối tượng trong code của chúng ta.

Note: Khi định nghĩa các lớp hoặc hàm, không thể sử dụng từ khóa `auto` như trong ví dụ sau:
```C++
    // group.hpp
    class Group
    {
    public:
        Group(std::initializer_list<std::string>&& init_names);
        ~Group();

        friend std::ostream& operator<<(std::ostream& os, Group const& group);
    private:
        std::vector<std::string> m_names;
    };
```
không thể thay `Group(std::initializer_list<std::string>&& init_names);` thành `Group(auto&& init_names);` vì lúc này compiler sẽ báo lỗi `error: no matching function for call to ‘Group::Group(<brace-enclosed initializer list>)`. Lỗi này do trong main.cpp, chúng ta khởi tạo đối tượng `group` bởi một `std::initializer_list`, nhưng compiler lại không thể tìm được constructor nào phù hợp cho việc khởi tạo với đối số kiểu dữ liệu này do không thể xác định được `auto&&` trong ngữ cảnh có thể được thay bằng kiểu dữ liệu nào là phù hợp.

## Range-based for loop - Tiếp cận mới cho tư duy sử dụng for-loop
Cú pháp range-based for loop đã xuất hiện trong nhiều ngôn ngữ bậc cao khác nhau như Javascript, Java, C#, Python, ... (trong các ngôn ngữ này, range-based for loop được gọi là các for-in loop hay for-each loop). Tuy nhiên trước C++11 for loop trong C++ vẫn còn là index-based for loop. Từ C++11 trở đi, for-in loop được giới thiệu và nhanh chóng trở thành for loop được sử dụng phổ biến hơn for với index-based for loop trước đó. Với ưu điểm cú pháp gọn hơn, gần hơn với tư duy của con người, for-in loop thường được sử dụng với các container-based class trong C++, bao gồm các containers trong STL (Standard Template Library) và các container-based class. Do vậy chúng ta chỉ sử dụng từ khóa `auto` chi ngữ cảnh trong code rõ ràng cho compiler xác định được kiểu dữ liệu tương ứng cho đối tượng.

Cú pháp chung của range-based for loop như sau:
```C++
    for ( range_declaration : range_expression ) 
        loop_statement;
```

Trong đó:
 + range_declaration: đối tượng được sử dụng để duyệt qua từng đối tượng trong container.
 + range_expression: container chứa các đối tượng cần được duyệt qua.

range_declaration có ý nghĩa như một iterator nhưng nó không phải là một iterator. Chi tiết hơn về Iterator trong C++ bạn đọc có thể xem qua [Iterator trong C++](/post/cpp/iterator).

range_expression thường là các container-based class. Chi tiết hơn về container-base class bạn đọc có thể xem qua [Container trong C++](/post/cpp/container).

Ví dụ cho việc sử dụng range-based for loop, chúng ta có thể viết lại câu lệnh for-loop ở ví dụ trên như sau:

```C++
    for (std::string const& name: group.m_names)
    {
        os << name << std::endl;
    }
```
hoặc kết hợp với từ khóa `auto`:

```C++
    for (auto const& name: group.m_names)
    {
        os << name << std::endl;
    }
```