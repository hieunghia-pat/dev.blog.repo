---
title: "Your First Qt Program"
description: Qt tutorial for C++ development
date: 2021-07-29T17:23:13+07:00
draft: false
toc: true
image: "images/qt-cpp/header.jpg"
author: NguyenNghia-PAT
tags: ["Qt", "C++"]
categories: [Qt]
---

## Your first Qt program using Qt Creator
Việc tải và cài đặt Qt Creator được thực hiện kèm với quá trình cài đặt Qt framework. Sau khi cài đặt hoàn tất, tiến hành khởi chạy Qt Creator, ta được giao diện như sau:

![](/images/qt-cpp/your-first-program/qt-1.png)

Để khởi tạo một project mới, click button New kế bên chữ Projects hoặc File->New File or Project... cửa sổ pop-up hiện lên, chọn project *Qt Widgets Application*, trong bài viết này mình để tên project là *QtExample* để các cài đặt khác ở mặc định. Khi đến phần chọn kit chọn *Desktop* hoặc *Desktop Qt 6.x.x GCC 64 bit* cho destop app, sau đó Next và Finish để hoàn tất việc tạo Project.

![](/images/qt-cpp/your-first-program/qt-2.png)

Cấu trúc của một Qt Project được tổ chức như trong hình bên:

![](/images/qt-cpp/your-first-program/qt-3.png)

### QtExample.pro
File này chứa các configure cho qmake khởi tạo project. [qmake](https://doc.qt.io/qt-5/qmake-manual.html) cũng tương tự như [cmake](https://cmake.org/) được sử dụng để configure các file mã nguồn (source files), header files, các dynamic files cần thiết cho việc compile project. Việc compile project được tiến hành bởi [make](https://en.wikipedia.org/wiki/Make_%28software%29) thông qua các files được tạo ra nhờ qmake. Chi tiết hơn về cách sử dụng cmake để chuẩn bị cho quá trình compile project sẽ được trình bày ở phần [tiếp theo](http://localhost:1313/post/qt-cpp/your-first-program/#your-first-qt-program-using-vs-code).

Các thành phần đáng được chú ý trong file .pro:
 - **QT**: đây là biến lưu các giá trị đại diện cho các modules sẽ được sử dụng trong project. Mặc định QT bao gồm 2 modules là core và gui. Đối với project chúng ta vừa tạo do chúng ta tạo một *Qt Widgets Application* project nên QT lúc nào bao gồm thêm module *widgets*. Sử dụng QT Qt's compiler mới có thể xác định được đường dẫn đến thư mục chứa source codes và header files của các modules được sử dụng để link các files đó lại với nhau để compile project. Chi tiết xem thêm ở [đây](https://doc.qt.io/qt-5/qmake-variable-reference.html#qt).
 - **CONFIG**: lưu các configures cho compiler như chuẩn của C++, released mode (debug or released), ... Trong giới hạn bài viết này, CONFIG được thêm vô thông tin về chuẩn C++ được sử dụng, ở đây là C++11. Bạn đọc có thể thay đổi chuẩn sử dụng bằng cách thay đổi C++11 thành C++14, C++17, C++2a, ... Phải đảm bảo các chuẩn đó được hỗ trợ bởi compiler có sẵn trên máy của bạn vì Qt sử dụng compiler được cài đặt sẵn trên máy. Chi tiết xem thêm ở [đây](https://doc.qt.io/qt-5/qmake-variable-reference.html#config).
 - **SOURCES**: chứa đường dẫn đến các files mã nguồn do lập trình viên soạn. Chi tiết xem thêm ở [đây](https://doc.qt.io/qt-5/qmake-variable-reference.html#sources).
 - **HEADERS**: chứa đường dẫn đến các header files do lập trình viên soạn. Chi tiết xem thêm ở [đây](https://doc.qt.io/qt-5/qmake-variable-reference.html#headers).
 - **FORMS**: chứa đường dẫn đến các files ui (User Interface). Chi tiết xem thêm ở [đây](https://doc.qt.io/qt-5/qmake-variable-reference.html#forms).

### mainwindow.ui
Khi sử dụng Qt Creator, việc thiết kế layout cho project có thể được thực hiện một cách đơn giản hơn thông qua Qt Designer. Đây là môi trường tương tự như [WinForm]() của Microsoft Visual Studio, thực hiện việc kéo thả để xây dựng giao diện. Qt quản lý thông tin giao diện được xây dựng bằng file .ui. Cấu trúc của file .ui tuân theo định dạng chuẩn [XML](), đây là chuẩn định dạng phổ biến cho các phương thức giao tiếp web (API) cũ, hiện nay vốn đã được thay thế thành các file định dạng [JSON](). Định dạng XML cũng được [Android Studio]() sử dụng để quản lý layout của chương trình.

Khi double click vào file *mainwindow.ui*, Qt Designer tự động được kích hoạt. Qt Creator không cho phép việc chỉnh sửa các file .ui thông qua Qt Editor. Việc chỉnh sửa này chỉ có thể thực hiện ở Qt Designer. Điều này giúp Qt đảm bảo cấu trúc của các file .ui là tối ưu cho việc interpret để tạo ra các UI components cho project.

Từ mục *Buttons*, kéo thả *Push Button* vào window form, resize window form sao cho thích hợp. Sau đó *Ctrl+S* để save. Như vậy chúng ta vừa hoàn tất một giao diện đơn giản.

![](/images/qt-cpp/your-first-program/qt-4.png)

### mainwindow.h
Những bạn nào đã vững về kỹ thuật OOP (Object Oriented Programming) của C++ thì sẽ không còn thấy xa lạ gì với các file .h hay .hpp của C++. Đây là những file chứa phần khai báo của một lớp đối tượng (Class Declaration). Trong project này *mainwindow.h* chứa các định nghĩa của lớp `MainWindow` được sử dụng để render window của chương trình.

### mainwindow.cpp
Đây là file mã nguồn của lớp `MainWindow`, tức là phần định nghĩa của lớp đối tượng này (Class Definition).

Thêm các dòng sau vào *mainwindow.cpp*:
```c++
#include "mainwindow.h"
#include "ui_mainwindow.h"

#include <QPushButton>
#include <QDebug>

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)
{
    ui->setupUi(this);

    QObject::connect(ui->pushButton, &QPushButton::clicked, []() {
        qDebug() << "Hello World";
    });
}

MainWindow::~MainWindow()
{
    delete ui;
}
```

### main.cpp
Đây là file chứa mã nguồn cho vị trí bắt đầu project. Khi compile project, *main.cpp* sẽ được thực thi đầu tiên và gọi các components khác thực thi và chương trình nhờ đó mà vận hành theo ý của người lập trình.

### Run the project
Sau khi hoàn tất các thao tác kể trên, ấn *CTrl+R* hoặc nút Run ở góc trái bên dưới của Qt Creator, quá trình build project sẽ được tiến hành. Sau khi build xong, một cửa sổ sẽ hiện lên như sau:

![](/images/qt-cpp/your-first-program/qt-5.png)

Click vào nút *Click me!*, quan sát ở phần *Application Output*, chúng ta sẽ thấy dòng chữ "Hello World" được in ra:

![](/images/qt-cpp/your-first-program/qt-6.png)

## Your first Qt program using VS Code
Với những bạn đam mê vọc vạch, tìm tòi và thích code từ A đến Z không sử dụng qua Qt Designer thì sử dụng một text editor và configure tay là một lựa chọn thú vị. Trong phạm vi series này mình giới thiệu cách sử dụng VS Code để xây dựng một Qt project.

*Note: những thao tác tiếp theo đây được tiến hành trên Ubuntu 20.04. Bạn đọc sử dụng Windows có thể tham khảo thêm các nguồn trên google nếu gặp những vấn đề về nền tảng.*

Tạo một folder cho project. Như phần trên, mình sẽ tạo một folder tên *QtExample* để làm ví dụ. Trong folder đó tạo các folder con và các file theo cấu trúc như sau:
![](/images/qt-cpp/your-first-program/qt-7.png)

### mainwindow.hpp
Trong file *mainwindow.hpp*, thêm các dòng như sau:
```c++
#ifndef MAINWINDOW_HPP
#define MAINWINDOW_HPP

#include <QMainWindow>

class MainWindow: public QMainWindow
{
    Q_OBJECT
public:
    MainWindow(QWidget* parent = nullptr);
    ~MainWindow();
};

#endif // MAINWINDOW_HPP
```

### mainwindow.cpp
Trong file *mainwindow.cpp*, thêm các dòng như sau:
```c++
#include "mainwindow.hpp"

#include <QMainWindow>

MainWindow::MainWindow(QWidget* parent)
    : QMainWindow(parent)
{

}

MainWindow::~MainWindow()
{

}
```

### main.cpp
Trong file *main.cpp*, thêm các dòng như sau:
```c++
#include <QApplication>
#include <QWidget>
#include <QPushButton>
#include <QVBoxLayout>
#include <QDebug>

#include "mainwindow.hpp"

int main(int argc, char* argv[])
{
    QApplication app(argc, argv);

    MainWindow window;

    // define a layout
    QVBoxLayout* layout = new QVBoxLayout();
    // define some buttons
    QPushButton* clickButton = new QPushButton(QString("Click me!"));
    QPushButton* cancelButton = new QPushButton(QString("Close"));
    // add buttons to the layout
    layout->addWidget(clickButton);
    layout->addWidget(cancelButton);
    // create a widget 
    QWidget* widget = new QWidget(&window);
    widget->setLayout(layout);
    // set the central widget for the windoe
    window.setCentralWidget(widget);

    QObject::connect(clickButton, &QPushButton::clicked, []() {
        qDebug() << "Hello World!";
    });

    QObject::connect(cancelButton, &QPushButton::clicked, &app, QApplication::quit);

    window.show();

    return app.exec();
}
}
```

### Run the project
Sau khi thêm các files cần thiết, mở terminal (cmd trong Windows) vào địa chỉ của project và nhập lệnh `qmake -project`. Sau khi hoàn tất, `qmake` tạo ra *QtExample.pro* trong cùng thư mục. Cấu trúc của file .pro này tương tự như mục trên. Tuy nhiên ở mục này xuất hiện thêm một vài biến mới. Bạn đọc quan tâm có thể tham khảo tại [đây](https://doc.qt.io/qt-5/qmake-manual.html). Trong bài viết này mình chỉ giới thiệu một số biến được sử dụng phổ biến:
 - TARGET: Tên của executable file mà compiler sẽ tạo ra. Với Windows file này sẽ có đuôi mở rộng là .exe.
 - INCLUDEPATH: đường dẫn tới folder chứa các header files của project. Đây thường là đường dẫn đến các folder của các header files do lập trình viên tạo ra. Trong project này, thay đổi dòng INCLUDEPATH này thành INCLUDEPATH += ./include.

Ngoài ra thêm vào file *QtExample.pro* 2 dòng sau:
```
CONFIG += c++11 debug
QT += core gui widgets
```

Sau khi hoàn tất các thao tác trên, ở terminal, vào folder build và thực thi lệnh `qmake ..` Quá trình tạo các configuration được tiến hành. Sau khi hoàn tất, các files cần thiết cho quá trình compiling được tạo ra trong folder *build*. Cuối cùng thực thi lệnh `make` để compile và export executable file. Trên Linux-based OS executable file sẽ có tên như thông số của biến TARGET trong file .pro, trong trường hợp này là *QtExample*, ở Windows file có tên là *QtExample.exe*.

Để xem kết quả, ở terminal nhập lệnh `QtExample` và enter. Một cửa sổ hiện lên như hình:
![](/images/qt-cpp/your-first-program/qt-9.png)

Trong project này, do không sử dụng Qt Designer nên việc chỉnh layout không được thuận tiện, do đó mình thêm *Close* button và tạo thêm layout cho *window* để layout dễ nhìn hơn. *Click me* button cho kết quả hệt như mục trên và nút *Close* được thêm vào với chức năng tắt chương trình:
![](/images/qt-cpp/your-first-program/qt-10.png)