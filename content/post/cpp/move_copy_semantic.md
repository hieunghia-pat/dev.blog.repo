---
title: "Move and Copy Semantic"
date: 2021-07-29
description: Move and copy semantic - new concepts from C++11
author: hieunghia-pat
draft: false
toc: true
image: "images/cpp/header.jpg"
tags: ["C++"]
categories: [C++]
---

## `=` operator
Như đại đa số ngôn ngữ lập trình khác, toán tử `=` thường được sử dụng để "gán giá trị của biến này sang biến khác". Khái niệm *biến (variable)* chỉ đúng với những ngôn ngữ bậc thấp như Pascal hay C và do đó quan điểm như phát biểu trên cũng chỉ đúng khi sử dụng trong ngữ cảnh của các ngôn ngữ đó. Tuy nhiên với các ngôn ngữ bậc cao, khái niệm biến không còn được sử dụng nữa mà thay vào đó là khái niệm *đối tượng (object)* và các dạng tham chiếu đến các vùng nhớ chứa dữ liệu của các đối tượng.

### Until C++98
Với chuẩn C++98, toán tử `=` vẫn được xây dựng với quan điểm của ngôn ngữ C và được gọi là *assignment operator*. Khi chúng ta có một đối tượng nhất định, ví dụ như `int a = 10;`, câu lệnh `int b = a;` sẽ tạo ra một đối tượng `b` có cùng kiểu với đối tượng `a` và dữ liệu của vùng nhớ mà đối tượng `a` giữ sẽ được sao chép sang vùng nhớ của đối tượng `b`.

### Since C++11
Chuẩn C++11 ra đời với sự xuất hiện của các [value categories](http://localhost:1313/post/cpp/value_categories/#gi%E1%BB%9Bi-thi%E1%BB%87u-value-categories-trong-c) và các khái niệm [value reference](http://localhost:1313/post/cpp/value_categories/#reference-types-trong-c). Lúc này, quan điểm về toán tử `=` là một phép "gán" không còn phù hợp nữa, vì thuật ngữ "gán" này nghiêng về ý nghĩa *copy* dữ liệu hơn so với các cách tham chiếu đến dữ liệu mà C++11 đã định nghĩa. Xem qua ví dụ sau:
```c++
// point.hpp
class Point
{
public:
    Point(float x = 0, float y = 0, float z = 0); // user defined constructor, default is the 2D point
    Point(Point const& p); // copy constructor
    Point(Point&& p); // move constructor

    // other methods go here

    ~Point();
private:
    float m_x, m_y, m_z; // m_ stands for member variable
}
```

```c++
// main.cpp
    Point A(1, 0); // user defined constructor is called
    Point& rA = A; // rA is lvalue reference
    Point B = A; // copy constructor is called. This statement equals Point B { A }; or Point B(A);
    Point C = Point(3, 1); // move constructor is called.
                            // This statement equals Point C(Point(3, 1)); or Point C { Point(3, 1) };
```
Ở dòng lệnh `Point& rA = A;` rõ ràng đối tượng `rA` không được tạo ra bất kỳ vùng nhớ mới nào độc lập, mà nó là một đối tượng *lvalue*, tức là về mặt ý nghĩa, `rA` được xem như là một alias của đối tượng `A`. Do vậy trong ngữ cảnh này khái niệm *assignment operator* không còn phù hợp nữa và từ chuẩn C++11 trở về sau, khái niệm này dần được thay thế bằng các khái niệm mới bao quát hơn và phù hợp hơn: *copy operator* và *move operator*.

Tuy vào cách khởi tạo mà constructor tương ứng sẽ được gọi. Với những dạng khởi tạo sử dụng dữ liệu từ các đối tượng đã được định nghĩa khác (các đối tượng lvalue), *copy constructor* tương ứng sẽ được gọi. Tương tự với các đối tượng được khởi tạo bằng các đối tượng rvalue. Bằng cách định nghĩa lại toán tử  `=` trong ngữ cảnh khởi tạo một đối tượng (object initialization), C++ cho phép chúng ta tự định nghĩa nên nhiều phương thức khởi tạo khác nhau nằm linh hoạt với từng trường hợp và ngữ cảnh. Hơn hết nếu có thể tận dụng được các kỹ thuật *value reference* cho các constructor chúng ta có thể tối ưu tốc độ thực thi của chương trình lên đáng kể. Như ở ví dụ sau:
```c++
// point.hpp
class Point
{
public:
    Point();
    Point(float x, float y, float z = 0);
    Point(Point p); // instead of Point(Point const& p);
    Point(Point&& p);

    // other methods go here

    ~Point();
private:
    float m_x, m_y, m_z; // m_ stands for member variable
}
```

```c++
// main.cpp
    Point A(1, 0); // user defined constructor is called
    Point B = A; // copy constructor is called. This statement equals Point B { A }; or Point B(A);
```
Ở copy constructor, nếu chúng ta bỏ đi ký tự `&` (reference operator), khi thực thi dòng lệnh `Point B = A;` chương trình sẽ phải tạo ra một *local object* cụ thể trong trường hợp này chính là `Point const p`. Lúc này một vùng nhớ mới sẽ được cấp phát và cấp cho đối tượng `p`, sau đó dữ liệu từ vùng nhớ của đối tượng `A` được sao chép sang vùng nhớ của đối tượng `p`. Rõ ràng quá trình này tốn kém và phức tạp hơn nhiều so với việc sử dụng tham số kiểu *lvalue reference*.

<i><strong>Note:</strong>
 - Trong ví dụ trên, khi thay đổi copy constructor, mình thay không chỉ bỏ đi ký tự `&` mà còn bỏ luôn cả từ khóa `const`. Đây là một kỹ thuật có chủ đích. Về thực chất khi sử dụng các đối số kiểu lvalue reference, do các đối số này sử dụng chung vùng nhớ với đối tượng mà nó tham chiếu đến, nên khi chúng ta thay đổi giá trị của chúng, đối tượng mà chúng tham chiếu đến cũng sẽ thay đổi theo. Điều này thường dẫn đến những lỗi logic trong quá trình thực thi code. Do đó khi sử dụng đối số kiểu lvalue reference, mình vẫn hay thêm từ khóa `const` với mục đích ko cho phép các đối số này thay đổi tác động đến các đối tượng bên ngoài.
 - Khi chuyển sang sử dụng *local object* hay vì *reference object* cho các đối số, do các đối tượng này có vùng nhớ độc lập, mang nội dung tương tự như các đối tượng được truyền vào nên chúng ta có thể tự do thay đổi các đối tượng đối số này mà không lo những tác động ảnh hưởng khác, do đó không cần phải sử dụng `const` trong trường hợp này.
</i>

## Move and Copy Semantic
### Move and Copy Semantic in assignment
Với một lớp đối tượng bất kỳ, nếu không được định nghĩa thì C++ mặc định cho chúng toán tử *copy* (`operator=(<ClassName> [const]& <ObjectName>)`) và toán tử *move* (`operator=(<ClassName>&& <ObjectName>`). Tùy vào vế bên phải của biểu thức mà toán tử tương ứng (*copy* hoặc *move*) được gọi.

Ví dụ như lớp đối tượng `Point` được sử dụng ở trên (giả sử các constructors chỉ khởi tạo các attributes và không làm gì khác):
```c++
// point.hpp
class Point
{
public:
    Point(float x = 0, float y = 0, float z = 0); // user defined constructor, default is the 2D point
    Point(Point const& p); // copy constructor
    Point(Point&& p); // move constructor

    Point operator=(Point const& p)
    {
        std::cout << "In copy assignment" << std::endl;
        return p;
    }

    Point operator=(Point&& p)
    {
        std::cout << "In move assignment" << std::endl;
        return p;
    }

    ~Point();
private:
    float m_x, m_y, m_z; // m_ stands for member variable
}
```

```c++
// main.cpp
    Point A(2, 5);
    Point B;
    B = A;
    B = Point(10, 5);
```
Kết quả sau khi thực hiện đoạn code trên:
```
In copy assignment
In move assignment
```

### Move and Copy Semantic in construction
Nội dung của phần này đã được trình bày kỹ ở mục [`=` operator since C++11](http://localhost:1313/post/cpp/move_copy_semantic/#since-c11), ở đây mình chủ yếu cung cấp implementation của lớp đối tượng `Point` và các kết quả khi sử dụng compiler chuẩn C++11.

```c++
// point.cpp
Point::Point(float x, float y, float z)
    : m_x(x), m_y(y), m_z(z)
{
    std::cout << "In user-defined constructor" << std::endl;
}

Point::Point(Point const& p)
    : m_x(p.m_x), m_y(p.m_y), m_z(p.m_z)
{
    std::cout << "In copy constructor" << std::endl;
}

Point::Point(Point&& p)
    : m_x(p.m_x), m_y(p.m_y), m_z(p.m_z)
{
    std::cout << "In move constructor" << std::endl;
}
```

```c++
// main.cpp
    Point A(2, 5);
    Point B (A);
    Point C { A };
    Point D { Point(10, 5) };
    Point E( Point(10, 7) );
```
Kết quả sau khi thực thi đoạn chương trình trên:
```
In user-defined constructor
In copy constructor
In copy constructor
In move constructor
In move constructor
```

## Appendix
Sẽ như thế nào nếu chúng ta chạy chương trình sau:
```c++
// point.hpp
#ifndef POINT_HPP
#define POINT_HPP

class Point
{
public:
    Point(float x = 0, float y = 0, float z = 0);
    Point(Point const& p);
    Point(Point&& p);
    ~Point();

    Point operator=(Point const& p);
    Point operator=(Point&& p);

private:
    float m_x, m_y, m_z;

};

#endif // POINT_HPP
```

```c++
// point.cpp
#include <iostream>
#include "point.hpp"

Point::Point(float x, float y, float z)
    : m_x(x), m_y(x), m_z(z)
{
    std::cout << "In user definded constructor" << std::endl;
}

Point::Point(Point const& p)
    : m_x(p.m_x), m_y(p.m_y), m_z(p.m_z)
{
    std::cout << "In copy constructor" << std::endl;
}

Point::Point(Point&& p)
    : m_x(p.m_x), m_y(p.m_y), m_z(p.m_z)
{
    std::cout << "In move constructor" << std::endl;
}

Point Point::operator=(Point const& p)
{
    std::cout << "In copy assignment" << std::endl;
    return Point(p);
}

Point Point::operator=(Point&& p)
{
    std::cout << "In move assignment" << std::endl;
    return Point(p);
}

Point::~Point()
{
    
}
```

```c++
// main.cpp
#include "point.hpp"

int main()
{
    Point A(2, 5);
    Point B;
    B = A;
    B = Point(10, 5);

    return 0;
}
```
Kết quả:
```
In user definded constructor
In user definded constructor
In copy assignment
In copy constructor
In user definded constructor
In move assignment
In copy constructor
```
Bạn đọc hãy thử giải thích kết quả trên.