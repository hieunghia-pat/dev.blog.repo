---
title: "Modern C++ - Value categories"
date: 2021-07-29T17:33:05+07:00
description: Value categories in modern C++
author: NguyenNghia-PAT
draft: false
toc: true
image: "images/cpp/header.jpg"
tags: ["C++"]
categories: [C++]
---

## Giới thiệu value categories trong C++
Có thể một số bạn đã nghe qua về khái niệm *value categories* trong C++. Đây là khái niệm ít xuất hiện ở các ngôn ngữ lập trình khác (trong phạm vi hiểu biết của tác giả thì tác giả chỉ mới biết khái niệm này được đề cập trong C++). Do ít được đề cập ở các ngôn ngữ lập trình khác nên rất ít người quan tâm đến khái niệm này cũng như ít nguồn tài liệu đề cập và bàn sâu về nó. Tuy nhiên đối với những bạn muốn học sâu và sử dụng C++ hiệu quả thì *value categories* lại là khái niệm nền tảng và rất quan trọng, đặc biệt là chúng được sử dụng thường xuyên trong các framework của C++. Trong bài viết này mình sẽ cố gắng truyền tải một cách dễ hình dung nhất có thể về *value categories* để bạn đọc có thể hiểu và sử dụng chúng linh hoạt trong các dự án C++.

### Value categories trong C++98
Trước C++11, C++ dựa trên mô hình của ngôn ngữ C định nghĩa value categories bao gồm 2 loại, để giữ cho định nghĩa không bị bóp méo mình giữ nguyên định nghĩa không dịch nó sang tiếng Việt (nguồn: [n3055](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2010/n3055.pdf)):

Every expression is either an lvalue or an rvalue:
1. An lvalue refers to an object or function
2. The result of calling a function that does not return an lvalue reference is an rvalue.

Định nghĩa này gián tiếp thừa nhận lvalue và rvalue là 2 khái niệm đối của nhau: lvalue là những đối tượng hoặc hàm số (đối tượng hàm) hoặc là những đối tượng tham chiếu đến đối tượng hoặc hàm số, nói cách khác lvalue chỉ một thực thể mà mình xác định được, gọi tên được, có vùng nhớ xác định mà mình có thể quản lý và truy cập; những đối tượng không phải lvalue là rvalue.

Ví dụ: 
```c++
    int totalGirls = 17;
    int totalBoys = 32;
    int totalStudents = totalGirls + totalBoys;
```
`totalGirls + totalBoys` là một biểu thức trả về 1 đối tượng kiểu int, tuy nhiên đối tượng này không có tên gọi cụ thể, không có vùng nhớ xác định (thực chất nó có vùng nhớ trên bộ nhớ, cụ thể là giá trị của nó được ghi trên thanh register, đây là một đối tượng tạm thời do runtime library quản lý, chúng ta không thể sử dụng và truy cập đối tượng nay bằng các kỹ thuật lập trình thông thường), do đó đối tượng mà biểu thức này trả về là 1 rvalue. Để có thể quản lý và sử dụng được đối tượng trả về này một cách hiệu quả, toán tử move ("=") đã thực hiện việc di chuyển tài nguyên của đối tượng tạm thời này vào đối tượng `totalStudents`, lúc này đối tượng `totalStudents` là một lvalue.

Note: trong phần này có đề cập đến toán tử move, bên cạnh đó "=" cũng được sử dụng để định nghĩa cho toán tử sao chép. Chi tiết hơn toán tử này, xem ở bài viết [Move and Copy Semantic](cpp/move_and_copy_semantic.md).

### Value categories trong C++11 trở về sau
Với các chuẩn từ chuẩn C++11 trở về sau, value categories được định nghĩa và phân loại sâu sắc, đầy đủ hơn nhằm khắc phục những lỗ hổng logic của ngôn ngữ. Cụ thể, chuẩn C++11 trở về sau định nghĩa value categories bao gồm 5 loại (mình cũng giữ nguyên định nghĩa để không làm ngữ nghĩa bị sai lệch khi dịch sang tiếng Việt - nguồn: [n3055](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2010/n3055.pdf)):
1. An lvalue (so-called, historically, because lvalues could appear on the left-hand side
of an assignment expression) designates a function or an object. [Example: If E is
an expression of pointer type, then *E is an lvalue expression referring to the object
or function to which E points. As another example, the result of calling a function
whose return type is an lvalue reference is an lvalue. —end example].
2. An xvalue (an “eXpiring” value) also refers to an object, usually near the end of its
lifetime (so that its resources may be moved, for example). An xvalue is the result
of certain kinds of expressions involving rvalue references. [Example: The
result of calling a function whose return type is an rvalue reference is an xvalue. —
end example].
3. A glvalue (“generalized” lvalue) is an lvalue or an xvalue.
4. An rvalue (so-called, historically, because rvalues could appear on the right-hand
side of an assignment expression) is an xvalue, a temporary object or subobject thereof, or a value that is not associated with an object.
5. A prvalue (“pure” rvalue) is an rvalue that is not an xvalue. [Example: The result
of calling a function whose return type is not a reference is a prvalue. The value of
a literal such as 12, 7.3e5, or true, is also a prvalue. —end example]

Đọc qua định nghĩa để có nắm được cái ý ban đầu như vậy, giờ chúng ta đi vào từng loại với các ví dụ để dễ hình dung hơn định nghĩa của 5 loại values trên:

```c++
int getTotal(int totalGirls, int totalBoys)
{
    return totalGirls + totalBoys;
}

int totalGirls = 17;
int totalBoys = 32;
int totalStudents = getTotal(totalGirls, totalBoys);
```

#### lvalue 

*An lvalue (so-called, historically, because lvalues could appear on the left-hand side
of an assignment expression) designates a function or an object.*

Trong đoạn code trên, có thể dễ dàng nhận ra `totalStudents` chính là lvalue (như đã phân tích ở đoạn code trước đó). lvalue là khái niệm tương đối dễ hình dung, chúng ta không gặp khó khăn nào khi nhận diện ra chúng nên mình đề cập nhanh qua mục này.

#### xvalue 
*An xvalue (an “eXpiring” value) also refers to an object, usually near the end of its
lifetime (so that its resources may be moved, for example). An xvalue is the result
of certain kinds of expressions involving rvalue references*

Trong đoạn code trên, chúng ta lưu ý ở câu lệnh khởi tạo giá trị cho `totalStudents` nhận giá trị trả về của lời gọi hàm `getTotal(totalGirls, totalBoys)`. Bản thân hàm `getTotal` chỉ đơn giản là hàm tính tổng 2 giá trị của 2 đối tượng tham số đầu vào, giá trị trả về của phép cộng 2 đối tượng đó được xem là một dạng rvalue (tương tự như cách phân tích ở đoạn code trước đó). rvalue này sau đó trở thành giá trị trả về của hàm số. Hàm số trả về một rvalue, tuy nhiên rvalue này có thời gian tồn tại rất ngắn, nó sẽ mất ngay sau khi thực hiện xong câu lệnh `int totalStudents = getTotal(totalGirls, totalBoys);` dẫu cho câu lệnh này có nằm ở bất kỳ vị trí nào trong scope. Lúc này rvalue mà biểu thức gọi hàm này đang giữ để copy sang cho `totalStudents` chính là xvalue. Hay nói cách khác, trong các quá trình copy hay move kết quả của một biểu thức từ đối tượng A sang đối tượng B, A đang là rvalue trở thành xvalue và B là lvalue. Lưu ý A chỉ là xvalue trong ngữ cảnh của câu lệnh đang thực hiện quá trình copy hay move kết quả thôi, ra khỏi ngữ cảnh của câu lệnh này A lại trở thành rvalue như bình thường.

#### glvalue 
*A glvalue (“generalized” lvalue) is an lvalue or an xvalue.*

Định nghĩa này khá rõ ràng rồi, mình chỉ nhắc lại ở đây khái niệm của nó thôi.

#### rvalue
*An rvalue (so-called, historically, because rvalues could appear on the right-hand
side of an assignment expression) is an xvalue, a temporary object or subobject thereof, or a value that is not associated with an object.*

Trong đoạn code trên, trong thân hàm `getTotal` có một biểu thức tính tổng của 2 đối tượng trước khi thực hiện đến câu lệnh return, lúc này đối tượng mà biểu thức `totalGirls + totalBoys` là một rvalue. Ngoài ra, trong câu lệnh `int totalStudents = getTotal(totalGirls, totalBoys);` đối tượng mà `getTotal(totalGirls, totalBoys);` trả về vừa là rvalue, cũng vừa là xvalue trong ngữ cảnh của copy semantic.

#### prvalue 
*A prvalue (“pure” rvalue) is an rvalue that is not an xvalue.*

Như đã phân tích ở *rvalue* có những câu lệnh mà trong ngữ cảnh của đối tượng có thể đồng thời là *rvalue* và *xvalue*, trong những ngữ cảnh mà *rvalue* không kiêm thêm vai trò của *xvalue* thì lúc đó *rvalue* được gọi thành *prvalue*.

Sự xuất hiện thêm nhiều định nghĩa mới cho value categories trong chuẩn C++11 trở về sau chúng ta có thể dễ nhận ra là do sự xuất hiện của một khái niệm mới: Move Semantic và Copy Semantic. Lý do và lợi ích của 2 khái niệm này mang lại, các bạn cho thể tìm hiểu thêm tại [Move and Copy Semantic](cpp/move_and_copy_semantic.md)

## Reference types trong C++
Reference types hay các kiểu tham chiếu trong C++ là các kiểu dữ liệu của các đối tượng mà nó đóng vai trò là một alias - một cái tên khác của một đối tượng khác.

Reference types có 2 loại:
1. lvalue reference type: kiểu dữ liệu của các đối tượng tham chiếu đến các lvalue.
2. rvalue reference type: kiểu dữ liệu của các đối tượng tham chiếu đến các rvalue.

Ví dụ:
```C++
    void swapValue(int const& x, int const& y)
    {
        int tmp = x;
        x = y;
        y = tmp;
    }

    int x = 10;
    int y = 100;
    swapValue(x, y);
    std::cout << x << " " << y << std::endl;
    // output value: 100 10
```

Trong đoạn code trên, trong đối tượng hàm `swapValue(int const& x, int const& y);`, `x` và `y` là các đối tượng reference, cụ thể là lvalue reference. Bản chất x và y ở global scope đã được định nghĩa và cấp phát một bố nhớ trên stack, tức là có vùng nhớ riêng và tên định danh cho vùng nhớ riêng (tên định danh trên vùng nhớ của đối tượng do compiler quản lý và được lưu ở instruction của chương trình). Khi trở thành tham số đầu vào của hàm `swapValue` x và y trong `swapValue`'s scope được tạo ra là một tên gọi khác, tức là một định danh khác, của cùng một vùng nhớ của ::x và ::y trên stack. Do đó khi thực hiện thao tác thay đổi giá trị của 2 biến, kết quả là cả `swapValue::x`, `swapValue::y` và `::x`, `::y` đều được thay đổi giá trị cho nhau.

```c++
    // ClassManagement.cpp
    void ClassManagement::setClassName(std::string className)
    {
        this->className = className;
    }
    std::string& ClassManagement::getClassName() {
        return this->className;
    }
```

```C++
    // main.cpp
    ClassManagement* classManager = new ClassManagement();
    // set the class name
    classManager->setClassName("12a1");
    classManager->getClassName() = "12a1";  // same behavior as the previous line
    // do some tasks
    // ...

    delete classManager;
```

Trong ví dụ trên ở set functor không có gì để bàn cãi: hàm này trực tiếp gán giá trị mới cho thuộc tính của class.

Tuy nhiên ở get functor, có thể thấy giá trị mà get functor này trả về là một lvalue reference, và nó trỏ đến đối tượng className của đối tượng kiểu ClassManagement. Do đó chúng ta hoàn toàn có thể thông qua lời gọi get functor này để thay đổi giá trị của thuộc tính của đối tượng kiểu ClassManagement.

```C++
    // ClassManagement.cpp
    void ClassManagement::setClassName(std::string& newName)
    {
        this->lvalue_name = newName;
    }

    void ClassManagement::setClassName(std::string&& newName)
    {
        this->rvalue_name = newName;
    }
```

```C++
    // main.cpp
    ClassManagement* classManager = new ClassManagement();
    std::string className = "12a1";
    std::string& lvalue_className = className;
    std::string&& rvalue_className = "Class: " + className;
    // set class name
    classManager->setClassName(lvalue_className); // classManager->lvalue_name == "12a1"
    classManager->setClassName(rvalue_className); // classManager->rvalue_name == "Class: 12a1"
    // do some tasks
    // ...

    delete classManager;
```

Ở dòng `std::string&& rvalue_className = "Class: " + className;`, `"Class: " + className` trả về một rvalue, rvalue này được chuyển vào đối tượng kiểu rvalue do chương trình của chúng ta quản lý, tức là đặt cho vùng nhớ của đối tượng rvalue `"Class: " + className` một định danh mà chúng ta sử dụng được, truy cập được.

Note:
 - Thông thường khi viết định nghĩa cho các lớp, người ta hay kèm theo const qualifier cho các đối số đầu vào. Ví dụ như `void ClassManagement::setClassName(std::string const& newName)`. Điều này có 2 lợi ích (1) quá trình truyền tham số vào method của class được tối ưu hơn không phải là truyền nguyên một đối tượng vào như cách định nghĩa truyền thống (``void ClassManagement::setClassName(std::string newName)``) (2) *const qualifier* giúp đảm bảo cho giá trị của đối số đầu vào không bị thay đổi bên trong method của class. Đa số các class của std sử dụng cách định nghĩa này khi xây dựng các class nhằm tối ưu tốc hiệu suât của chương trình.
 - `std::string&&` và `std::string const&&` có ý nghĩa như nhau vì bản chất chúng ta không thể thay đổi được giá trị của các rvalue. Việc sử dụng cú pháp `std::string const&&` này có ý nghĩa về mặt logic cú pháp, giúp code clean hơn, dễ đọc và hiểu ý nghĩa của đoạn code hơn.
 - Trong hai lưu ý trên, `std::string` được sử dụng với mục đích minh họa, những lưu ý này vẫn đúng với các kiểu dữ liệu khác (bao gồm standard defined types và user defined types).

## Lời kết
Nắm được các khái niệm value categories cũng như reference type là một bước nền tảng rất quan trọng cho việc hiểu và sử dụng các phương thức có sẵn của std cũng như của các framework của C++. Hi vọng qua bài viết này bạn đọc có thêm nhiều hình dung hơn về value categories cũng như reference type trong C++. Ngoài ra bạn đọc cũng có thể tham khảo hơn các bài viết và giải thích khác của các lập trình viên dày dạn kinh nghiệm trên github hoặc stackoverflow, ...