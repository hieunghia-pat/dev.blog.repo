---
title: "GUI Design using Qt Widgets - Introduction"
description: Qt tutorial for C++ development
date: 2021-08-09
draft: false
toc: true
image: "images/qt-cpp/header.jpg"
author: hieunghia-pat
tags: ["Qt", "C++"]
categories: [Qt]
---

## Qt Widgets
Qt Widgets là một module trong Qt framework. Module này là tập hợp các GUI (Graphic User Interface) components được viết sẵn. Những components này thường là những components được sử dụng nhiều khi xây dựng một chương trình GUI như *Button, Text Line, Tab, Dialog, Main Window, ...* Bên cạnh việc cung cấp những components có sẵn, Qt còn cho phép chúng ta kế thừa những components này để tự tạo ra những customized components phục vụ cho ý tưởng viết chương trình của bản thân. Về mặt màu sắc và form hình thì các components này mặc định sử dụng theme của hệ điều hành mà chương trình đang được thực thi (lưu ý là kể cả khi chương trình được viết trên Linux-base OS hay MacOS, khi chạy trên Windows nó vẫn sử dụng theme và form của Windows cho các components trong chương trình), điều này giúp chương trình được viết mang tính chất của một cross-platform application: *look more native-platform.*

![Theme của các GUI application lần lượt trên Windows, MacOS và Ubuntu](/images/qt-cpp/gui-design-using-widget-introduction/qt-2.png)

Các kỹ thuật về theming cho các components trong Qt Widgets sẽ được trình bày kỹ hơn ở các bài viết [sau](/post/gui-design-using-widget-theming). Trong bài viết này mình sẽ giới thiệu qua về cấu trúc cũng như một số khái niệm về Qt Widgets cũng như một số ví dụ minh họa đơn giản.

## Structure
![](/images/qt-cpp/gui-design-using-widget-introduction/qt-1.png)
Như hình trên chúng ta có thể thấy QWidgets được xây dựng dựa trên hai object nền tảng của Qt là `QObject` và `QPaintDevice`.
`QObject` định nghĩa nên một lớp đối tượng đặc biệt của Qt với đầy đủ những tính năng thiết yếu cho việc xây dựng nên các components của mọi chương trình trên mọi nền tảng. `QPaintDevice` định nghĩa nên một lớp đối tượng với những tính năng chuyên cho việc xây dựng và hiển thị yếu tố đồ họa của một components. Chi tiết về cấu trúc đồ họa được sử dụng trong Qt, mình sẽ trình bày chi tiết hơn ở bài viết [sau](/post/gui-design-using-widget-graphic).

Dựa trên QWidgets, Qt tiếp tục xây dựng nên những lớp đối tượng khác để phục vụ chuyên biệt cho những tính năng nhất định như `QLineEdit` để tạo ra các components cho phép nhập dữ liệu dạng text, `QAbstractButton` để tạo ra các *button*, `QMainWindow` để tạo ra các cửa sổ cho chương trình, ... Nhờ vào sự đa dạng của Qt Widgets mà quá trình xây dựng nên một GUI app không còn nặng về vấn đề code layout và đồ họa mà chỉ tập trung vào phần design theme và logic của chương trình.

## QWidget examples
Trong bài viết này, mình sẽ sử dụng lại project QtExample để ví dụ về cách sử dụng `QWidget`.

`QWidget` là lớp đối tượng có render engine riêng, có tất cả các tính chất của một `QObject`, nên các đối tượng `QWidget` có thể được render và hiển thị độc lập mà không cần sự bao bọc của đối tượng `QMainWindow`.

Để thêm một đối tượng QWidget vào project, tại thanh hiển thị cấu trúc của project, click phải lên *QtExample* -> Add New... ->C++ Class. Để tên class là MyButton, base class là QWidget sau đó Next -> Finish. QT Creator lúc này sẽ tự động tạo ra 2 file *mybutton.h* và *mybutton.cpp* trong project. 

Vào file *mybutton.cpp*, tại constructor thêm các dòng sau:
```C++
MyButton::MyButton(QWidget *parent) : QWidget(parent)
{
    setFixedSize(QSize(100, 70));
    setToolTip(QString("This is a tooltip"));
}

```

Trong file *main.cpp*, thay đổi các dòng sau:
```c++
#include "mybutton.h"

#include <QApplication>

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);

    MyButton button;
    button.show();

    return a.exec();
}
```

Sau khi thực thi chương trình, một cửa sổ nhỏ hiện lên. Nếu chúng ta để con trỏ chuột lên cửa sổ, một lúc sau sẽ thấy một dòng text hiện ra, đó là một *Tooltip*.

![](/images/qt-cpp/gui-design-using-widget-introduction/qt-3.png)

Ở constructor của `MyButton` có lệnh `setFixedSize(QSize(100, 70));`, lệnh này dùng để thiết lập cố định kích thước của cửa sổ với chiều rộng là 100 và chiều cao là 70. `QSize` là một lớp đối tượng được sử dụng để mô tả kích cỡ của một object. Qt cung cấp rất nhiều dạng của đối tượng QSize như `QSize`(two-dimensional size determined by integer numbers) và `QSizeF`(two-dimensional size determined by floating-point numbers). Chi tiết hơn về các cách khởi tạo cũng như các attributes và methods của `QSize` và `QSizeF` bạn đọc có thể tham khảo thêm tại [document](https://doc.qt.io/qt-6/qsize.html) của Qt.

Chỉnh sửa lại một chút trong file *mybutton.h*, chúng ta sẽ được một customized class kế thừa từ `QAbstractButton` class như sau:
```c++
#ifndef MYBUTTON_H
#define MYBUTTON_H

#include <QPushButton>
#include <QWidget>

class MyButton : public QPushButton
{
    Q_OBJECT
public:
    explicit MyButton(QWidget *parent = nullptr);

signals:

};

#endif // MYBUTTON_H

```

Tương tự chỉnh sửa lại trong *mybutton.cpp*:
```c++
#include "mybutton.h"

#include <QPushButton>

MyButton::MyButton(QWidget *parent) : QPushButton(parent)
{
    setFixedSize(QSize(70, 30));
    setText("Click Me!");
}

```

và cả trong *main.cpp*:
```c++
#include "mybutton.h"

#include <QApplication>

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);

    MyButton button;
    button.show();

    return a.exec();
}

```

Khi chạy chương trình chúng ta được một cửa sổ là một *button* với label "Click Me!" như sau:
![](/images/qt-cpp/gui-design-using-widget-introduction/qt-4.png)

`QPushButton` có thêm thuộc tính `text`, chúng ta có thể truy cập thuộc tính này thông qua hàm truy cập (get functor) `QPushButton::text()` và thiết lập giá trị cho nó thông qua hàm set (set functor) `QPushButton::setText(QString& text)`. 

<i>Note:
 - Trong ví dụ này mình không thừa kết lớp `QAbstractButton` bởi `QAbstractButton` như tên gọi của nó được Qt xây dựng nên mới chỉ là một *Abstract Class*, khi thừa kế chúng ta phải re-implement các virtual method của nó như `QAbstractButton::paintEngine()` mà điều này nằm ngoài phạm vi thảo luận của bài viết hiện tại. Do đó mình sử dụng `QPushButton`, thừa kế từ `QAbstractButton`, đã được Qt xây dựng hoàn chỉnh.
 - `QPushButton` thừa kế từ `QAbstractButton`, `QAbstractButton` lại thừa kế từ `QWidget`, do đó `QPushButton` gián tiếp thừa kế từ `QWidget` và nó cũng là một Qt Widget. `QWidget` thừa kế từ `QObject` do đó nó cũng là `QObject`, nên nó cũng có chung phương pháp quản lý bộ nhớ như `QObject`. Constructor của `MyButton` nhận một đối tượng kiểu `QWidget` làm parent bởi một cách tổng quát `MyButton` có thể nhận parent là bất cứ đối tượng nào, miễn đối tượng đó có kiểu được thừa kế từ `QWidget`. Khi đó việc khởi tạo bằng `QPushButton(parent)` sẽ thêm đối tượng kiểu `MyButton` vào danh sách cách children của đối tượng `parent`. Khi `parent` được giải phóng bộ nhớ, đối tượng kiểu `MyButton` này cũng đồng thời được giải phóng theo. Đây chính là kỹ thuật quản lý bộ nhớ của `QObject`.
</i>