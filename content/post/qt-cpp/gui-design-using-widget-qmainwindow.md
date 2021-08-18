---
title: "GUI Design using Qt Widgets - QMainWindow"
description: Qt tutorial for C++ development
date: 2021-08-12
draft: false
toc: true
image: "images/qt-cpp/header.jpg"
author: hieunghia-pat
tags: ["Qt", "C++"]
categories: [Qt]
---

## Introduction
Bất cứ chương trình GUI nào cũng đều có một cửa sổ hiện lên khi được click chạy. Một chương trình GUI có thể có một hoặc nhiều cửa sổ. Tuy nhiên nhìn chung tất cả các chương trình đều chỉ duy trì cho mình một cửa sổ làm việc chính *(main window)*. Ở cửa sổ này người dùng có thể tìm thấy tất cả các công cụ và chức năng để tương tác, giao tiếp và sử dụng các tính năng mà chương trình cung cấp. Qt framework cũng xây dựng nên một lớp đối tượng chuyên biệt cho việc thiết kế và xây dựng nên main window cho các chương trình GUI, đó chính là [QMainWindow](https://doc-snapshots.qt.io/qt6-dev/qmainwindow.html). Trong bài viết này, mình sẽ trình bày chi tiết những phương thức quan trọng của `QMainWindow` để chúng ta có thể nhanh chóng và thuận tiện xây dựng layout cho chương trình GUI.

Trong bài viết này, mình sẽ sử dụng lại project QtExample đã được sử dụng ở các bài viết trước, tuy nhiên để bạn đọc hình dung được rõ hơn chức năng và cách sử dụng `QMainWindow` và các lớp đối tượng liên quan, mình sẽ không đề cấp đến việc sử dụng *Qt Designer* để thiết kế main window. Ví dụ về việc sử dụng *Qt Designer* sẽ được đề cập trong phần [Appendix](/post/qt-cpp/gui-design-using-widget-qmainwindow/#appendix-design-qmainwindow-using-qt-designer) của bài viết.

## QMainWindow
Qt xây dựng `QMainWindow` và các lớp đối tượng khác [liên quan](https://doc-snapshots.qt.io/qt6-dev/widget-classes.html#main-window-and-related-classes) phục vụ cho việc thiết kế và xây dựng giao diện GUI cho chương trình. Với Qt, cấu trúc của một *main window* được sắp xếp như hình sau: 

![](/images/qt-cpp/gui-design-using-widget-qmainwindow/qt-1.png)

### Menu bar
*Menu bar* chính là vị trí đặt các menu tùy chọn như *File*, *Edit*, *View*, ... Ứng với từng tùy chọn này là các chức năng được gom chung thành 1 nhóm và được bố trí, sắp xếp sao cho logic về bố cục. Ví dụ như với mục *File* chúng ta sẽ gom nhóm các chức năng liên quan đến việc xử lý, thao tác file (open, save, save as, close, quit, ...), hay mục *View* sẽ bao gồm các chức năng cho phép chúng ta điều chỉnh layout và cách hiển thị của nội dung trong cửa sổ. 

Qt giới thiệu lớp đối tượng `QMenuBar` và `QMenu` để xây dựng nên thanh menu cho window. `QMainWindow` cho phép chúng ta tạo ra một thanh menu bằng cách gọi phương thức `QMainWindow::setMenuBar(QMenuBar* menuBar)`.

Trước tiên chúng ta thử tạo ra một thanh menu với mục *File* và *Edit*:
```c++
// mainwindow.h
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>
#include <QAction>

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    MainWindow(QWidget *parent = nullptr);
    ~MainWindow();

private:
    // Actions for File menu
    QAction* newAction;
    QAction* openAction;
    QAction* saveAction;
    QAction* saveAsAction;
    QAction* quitAction;

    // Actions for Edit menu
    QAction* cutAction;
    QAction* copyAction;
    QAction* pasteAction;
    QAction* undoAction;
    QAction* redoAction;

    void setupFileMenu();
    void setupEditMenu();

};
#endif // MAINWINDOW_H

```

```c++
// mainwindow.cpp
#include "mainwindow.h"

#include <QMenu>
#include <QAction>

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
{
    setWindowTitle("MainWindow");
    resize(QSize(500, 700));
    setupFileMenu();
    setupEditMenu();
}

void MainWindow::setupFileMenu()
{
    // create the File menu
    QMenu* fileMenu = menuBar()->addMenu("&File");

    // establish actions for the File menu
    newAction = new QAction("New", this);
    fileMenu->addAction(newAction);
    openAction = new QAction("Open", this);
    fileMenu->addAction(openAction);
    saveAction = new QAction("Save", this);
    fileMenu->addAction(saveAction);
    saveAsAction = new QAction("Save As...", this);
    fileMenu->addAction(saveAsAction);
    quitAction = new QAction("Quit", this);
    fileMenu->addAction(quitAction);
}

void MainWindow::setupEditMenu()
{
    // create the Edit menu
    QMenu* editMenu = menuBar()->addMenu("&Edit");

    // establish actions for the Edit menu
    cutAction = new QAction("Cut", this);
    editMenu->addAction(cutAction);
    copyAction = new QAction("Copy", this);
    editMenu->addAction(copyAction);
    pasteAction = new QAction("Paste", this);
    editMenu->addAction(pasteAction);
    undoAction = new QAction("Undo", this);
    editMenu->addAction(undoAction);
    redoAction = new QAction("Redo", this);
    editMenu->addAction(redoAction);
}

MainWindow::~MainWindow()
{

}

```

```c++
// main.cpp
#include "mainwindow.h"

#include <QApplication>

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    MainWindow window;

    window.show();

    return a.exec();
}
```
Kết quả:
![](/images/qt-cpp/gui-design-using-widget-qmainwindow/qt-2.png)

`QMainWindow` đã được thiết lập sẵn một thuộc tính `QMenuBar` làm thanh menu mặc định. Chúng ta có thể truy cập đến thanh menu này thông qua phương thức `QMainWindow::menuBar()`. Với từng mục của *menu*, các chức năng như *New*, *Open*, *Save*, ... được Qt gọi là các *Action* và định nghĩa chúng bởi lớp đối tượng *QAction*. Các *action* này được thêm vào menu thông qua phương thức `QMenu::addAction(QAction* action)`.


*Note: luôn nhớ rằng mọi object trong Qt đều phải nhận một `QObject` khác làm parent. Do vậy khi khởi tạo các *action*, `this` được sử dụng làm parent cho các *action* này và các *action* này sẽ được giải phóng bộ nhớ khi đối tượng `MainWindow` cũng giải phóng bộ nhớ của chính nó.*

Trong quá trình định nghĩa tên của các menu, mình đã sử dụng ký tự `&`. Mục đích của việc sử dụng ký tự này là để Qt tạo ra phím tắt nhanh để mở menu. Ví dụ như với menu *File*, chúng ta có thể mở chúng thông qua phím tắt `Alt+f` (f tương ứng với ký tự được gạch dưới trong <u>F</u>ile).

Với các *action*, để tạo phím tắt chúng ta sử dụng phương thức `QAction::setShortcut(QKeySequence const& shortcut)`. `QKeySequence` là lớp đối tượng được định nghĩa để tạo ra các tổ hợp phím. Chúng ta có thể khởi tạo `QKeySequence` bằng nhiều cách, hai cách phổ biến được sử dụng nhiều để tạo có cú pháp:

```c++
    QKeySequence(QString("Ctrl+N"));// or QKeySequence(QString("Ctrl+n"));
    QKeySequence(Qt::Ctrl | Qt::Key_N);
```
lần lượt thêm các shortcut cho các *action*:
```c++
void MainWindow::setupFileMenu()
{
    // create the File menu
    QMenu* fileMenu = menuBar()->addMenu("&File");

    // establish actions for the File menu
    newAction = new QAction("&New", this);
    newAction->setShortcut(QKeySequence(Qt::CTRL | Qt::Key_N));
    fileMenu->addAction(newAction);
    openAction = new QAction("&Open", this);
    openAction->setShortcut(QKeySequence(Qt::CTRL | Qt::Key_O));
    fileMenu->addAction(openAction);
    saveAction = new QAction("Save", this);
    saveAction->setShortcut(QKeySequence(Qt::CTRL | Qt::Key_S));
    fileMenu->addAction(saveAction);
    saveAsAction = new QAction("Save As...", this);
    saveAsAction->setShortcut(QKeySequence(Qt::CTRL | Qt::SHIFT | Qt::Key_S));
    fileMenu->addAction(saveAsAction);
    quitAction = new QAction("&Quit", this);
    quitAction->setShortcut(QKeySequence(Qt::CTRL | Qt::Key_Q));
    fileMenu->addAction(quitAction);
}
```

```c++
void MainWindow::setupEditMenu()
{
    // create the Edit menu
    QMenu* editMenu = menuBar()->addMenu("&Edit");

    // establish actions for the Edit menu
    cutAction = new QAction("&Cut", this);
    cutAction->setShortcut(QKeySequence(Qt::CTRL | Qt::Key_X));
    editMenu->addAction(cutAction);
    copyAction = new QAction("&Copy", this);
    copyAction->setShortcut(QKeySequence(Qt::CTRL | Qt::Key_C));
    editMenu->addAction(copyAction);
    pasteAction = new QAction("&Paste", this);
    pasteAction->setShortcut(QKeySequence(Qt::CTRL | Qt::Key_V));
    editMenu->addAction(pasteAction);
    undoAction = new QAction("&Undo", this);
    undoAction->setShortcut(QKeySequence(Qt::CTRL | Qt::Key_Z));
    editMenu->addAction(undoAction);
    redoAction = new QAction("&Redo", this);
    redoAction->setShortcut(QKeySequence(Qt::CTRL | Qt::Key_Y));
    editMenu->addAction(redoAction);
}
```

sau đó connect các *action* với các [slots]() để kiểm tra hoạt động của các shortcut:
```c++
// mainwindow.h
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>
#include <QAction>

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    MainWindow(QWidget *parent = nullptr);
    ~MainWindow();

public slots:
    // slots for File menu
    void newMenu();
    void openMenu();
    void saveMenu();
    void saveAsMenu();
    void quitMenu();

    // slots for Edit menu
    void cutMenu();
    void copyMenu();
    void pasteMenu();
    void undoMenu();
    void redoMenu();

private:
    // Actions for File menu
    QAction* newAction;
    QAction* openAction;
    QAction* saveAction;
    QAction* saveAsAction;
    QAction* quitAction;

    // Actions for Edit menu
    QAction* cutAction;
    QAction* copyAction;
    QAction* pasteAction;
    QAction* undoAction;
    QAction* redoAction;

    void setupFileMenu();
    void setupEditMenu();
    void createConnections();
};
#endif // MAINWINDOW_H

```

```c++
void MainWindow::newMenu()
{
    qDebug() << "File->New";
}

void MainWindow::openMenu()
{
    qDebug() << "File->Open";
}

void MainWindow::saveMenu()
{
    qDebug() << "File->Save";
}

void MainWindow::saveAsMenu()
{
    qDebug() << "File->Save As...";
}

void MainWindow::quitMenu()
{
    qDebug() << "File->Quit";
}

void MainWindow::cutMenu()
{
    qDebug() << "Edit->Cut";
}

void MainWindow::copyMenu()
{
    qDebug() << "Edit->Copy";
}

void MainWindow::pasteMenu()
{
    qDebug() << "Edit->Paste";
}

void MainWindow::undoMenu()
{
    qDebug() << "Edit->Undo";
}

void MainWindow::redoMenu()
{
    qDebug() << "Edit->Redo";
}
```

```c++
void MainWindow::createConnections()
{
    // connections for File menu
    QObject::connect(newAction, &QAction::triggered, this, &MainWindow::newMenu);
    QObject::connect(openAction, &QAction::triggered, this, &MainWindow::openMenu);
    QObject::connect(saveAction, &QAction::triggered, this, &MainWindow::saveMenu);
    QObject::connect(saveAsAction, &QAction::triggered, this, &MainWindow::saveAsMenu);
    QObject::connect(quitAction, &QAction::triggered, this, &MainWindow::quitMenu);

    // connections for Edit menu
    QObject::connect(cutAction, &QAction::triggered, this, &MainWindow::cutMenu);
    QObject::connect(copyAction, &QAction::triggered, this, &MainWindow::copyMenu);
    QObject::connect(pasteAction, &QAction::triggered, this, &MainWindow::pasteMenu);
    QObject::connect(undoAction, &QAction::triggered, this, &MainWindow::undoMenu);
    QObject::connect(redoAction, &QAction::triggered, this, &MainWindow::redoMenu);
}
```


*Note: Khái niệm [signals và slots](/post/qt-cpp/signals-and-slots) sẽ được bàn kỹ hơn ở bài viết sau. Trong phạm vi bài viết này bạn đọc tạm hiểu `signal` là tín hiệu được tạo ra bởi một `QObject` khi nhận một sự kiện (event) và `slot` là phương thức (functor) bắt và respond lại `signal` đó.*

Sau khi thực thi, mỗi lần chúng ta ấn tổ hợp phím tắt cho bất cứ chức năng nào (new, save, open, ...), ở mục *Application Output* (hay terminal nếu bạn đọc sử dụng VS Code) sẽ hiện ra đường dẫn tương ứng (File->New, File->Save, ...).

### Toolbars
*Toolbars* mà chũng ta hay gặp nhất chính là thanh các icons nằm ngay bên dưới *menu bar* trong các phần mềm MS Office. `QMainWindow` cũng thiết lập mặc định các phương thức để hỗ trợ chúng ta thiết kế *toolbars* cho giao diện.

```c++
// mainwindow.h
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>
#include <QAction>

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    MainWindow(QWidget *parent = nullptr);
    ~MainWindow();

public slots:
    // slots for File menu
    void newMenu();
    void openMenu();
    void saveMenu();
    void saveAsMenu();
    void quitMenu();

    // slots for Edit menu
    void cutMenu();
    void copyMenu();
    void pasteMenu();
    void undoMenu();
    void redoMenu();

private:
    // Actions for File menu
    QAction* newAction;
    QAction* openAction;
    QAction* saveAction;
    QAction* saveAsAction;
    QAction* quitAction;

    // Actions for Edit menu
    QAction* cutAction;
    QAction* copyAction;
    QAction* pasteAction;
    QAction* undoAction;
    QAction* redoAction;

    // Actions for Toolbars
    QAction* newToolbar;
    QAction* openToolbar;
    QAction* saveToolbar;
    QAction* quitToolbar;

    void setupFileMenu();
    void setupEditMenu();
    void createMenuConnections();
    void createToolbars();
    void createToolbarConnection();
};
#endif // MAINWINDOW_H
```

```c++
void MainWindow::createToolbars()
{
    QToolBar* toolbar = addToolBar(QString("Main Toolbars"));
    newToolbar = toolbar->addAction(QPixmap(":/icons/actions/document-new.png"), "New");
    openToolbar = toolbar->addAction(QPixmap(":/icons/actions/document-open.png"), "Open");
    saveToolbar = toolbar->addAction(QPixmap(":/icons/actions/document-save.png"), "Save");
    toolbar->addSeparator();
    quitToolbar = toolbar->addAction(QPixmap(":/icons/actions/process-stop.png"), "Quit");
    toolbar->setMovable(false);
}
```

```c++
void MainWindow::createToolbarConnection()
{
    QObject::connect(newToolbar, &QAction::triggered, this, &MainWindow::newMenu);
    QObject::connect(openToolbar, &QAction::triggered, this, &MainWindow::openMenu);
    QObject::connect(saveToolbar, &QAction::triggered, this, &MainWindow::saveMenu);
    QObject::connect(quitToolbar, &QAction::triggered, this, &MainWindow::quitMenu);
}
```
Luôn nhớ gọi các phương thức gởi tạo mới thêm ở constructor:
```c++
MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
{
    setWindowTitle("MainWindow");
    resize(QSize(500, 700));
    setupFileMenu();
    setupEditMenu();
    createMenuConnections();
    createToolbars();
    createToolbarConnection();
}
```
Kết quả:
![](/images/qt-cpp/gui-design-using-widget-qmainwindow/qt-3.png)

Trong Qt, các *icons* trên *toolbars* cũng được gọi là các *action*. Tuy nhiên cần lưu ý rằng mặc dù *New action* trên thanh *toolbar* và *New action* trong menu *File* có tác dụng như nhau, nhưng hai *action* này là hai đối tượng `QAction` khác nhau. Chúng chỉ cùng sử dụng cùng một `slot` để thực hiện cùng một chức năng chứ chúng không reference lẫn nhau.

*Note: phương thức `QToolBar::addSeperator()` cho phép thêm vào toolbars một đường line nhằm phân tách rõ ràng hơn nhóm các button các chức năng tương tự nhau.*

Các *action* trên *toolbar* sử dụng icons đễ giúp người sử dụng dễ dàng hơn khi tiếp xúc và sử dụng. Phương thức `QToolBar::addAction(QPixmap const& icon, QString const& name)` hỗ trợ việc tạo ra *action* kèm theo icon tương ứng. `QPixmap` là lớp đối tượng được sử dụng để định nghĩa các đối tượng hình ảnh trong Qt. Sử dụng `QPixmap::QPixmap(QString const& filePath)` chúng ta có thể tạo ra đối tượng lưu hình ảnh và sử dụng đối tượng này làm hình ảnh cho icon của *action* trên thanh toolbar.

*Note: chúng ta cũng có thể thêm icon cho action ở menu bar bằng phương thức `QAction::setIcon(QIcon const& icon)` hoặc sử dụng constructor `QAction::QAction(QIcon const& icon, QString const& name, QObject* parent = nullptr)`.*

Ở câu lệnh `toolbar->setMovable(false);` nếu chúng ta truyền vào đối số giá trị `true`, chúng ta có thể di chuyển được thanh *toolbar* sang các vị trí khác:

![](/images/qt-cpp/gui-design-using-widget-qmainwindow/qt-4.png)

### Central widget
Như tên gọi của nó, đây là nơi chúng ta xây dựng nên layout cho nội dung chính của chương trình. Trong bài viết này, mình sẽ sử dụng `QLabel` để thiết kế một ứng dụng cho phép chúng ta hiển thị và xem hình ảnh.

```c++
// mainwindow.hpp

...
#include <QScrollArea>
#include <QLabel>

private:
    ...
    QScrollArea* scrollArea;
    QLabel* imageLabel;
    QImage image;

    bool loadFile(QString const& fileName);
    static void initializeImageDialog(QFileDialog& dialog, QFileDialog::AcceptMode acceptMode);
    ...
```

```c++
// mainwindow.cpp
MainWindow::MainWindow(QWidget* parent)
    : QMainWindow(parent)
{
    ...
    imageLabel = new QLabel(this);
    imageLabel->setBackgroundRole(QPalette::Base);
    imageLabel->setSizePolicy(QSizePolicy::Ignored, QSizePolicy::Ignored);
    imageLabel->setScaledContents(true);

    scrollArea = new QScrollArea(this);
    scrollArea->setAlignment(Qt::AlignCenter);
    scrollArea->setWidget(imageLabel);
    scrollArea->setVisible(false);
    
    setCentralWidget(scrollArea);
}
```

`QScrollArea` là một *widget* cho phép hiển thị nội dung (ảnh, văn bản, ...) mà chúng ta có thể "scroll" để hiển thị nếu phần nội dung vượt quá diện tích hiện thị cho phép của window. Trong ví dụ này đối tượng kiểu `QScrollArea` được sử dụng để hiện thị nội dung của đối tượng kiểu `QLabel`. Trong Qt, `QLabel` không những được sử dụng để hiện thị nội dung dạng chữ mà còn được sử dụng để hiển thị nội dung dạng hình ảnh (`QImage` hay `QPixmap`).

Viết lại `slot openMenu()` để thực hiện chức năng mở một file hình ảnh:
```c++
void MainWindow::openMenu()
{
    QFileDialog openDialog(this, QString("Open File"));
    initializeImageDialog(openDialog, QFileDialog::AcceptOpen); // create an open-file dialog

    while (openDialog.exec() == QDialog::Accepted && !loadFile(openDialog.selectedFiles().constFirst()))
    {
        // loop until user selected a valid file
    }
}

bool MainWindow::loadFile(const QString &fileName)
{
    QImageReader reader(fileName);
    reader.setAutoTransform(true);
    image = reader.read();
    if (image.isNull())
    {
        QMessageBox::information(this, QApplication::applicationDisplayName(), QString("Cannot load %1: %2").arg(fileName, reader.errorString()));
        return false;
    }

    // if loaded image successfully
    imageLabel->setPixmap(QPixmap::fromImage(image));
    imageLabel->adjustSize(); // resize the label to fit its content
    scrollArea->setVisible(true);

    return true; // loaded image successfully
}

void MainWindow::initializeImageDialog(QFileDialog& dialog, QFileDialog::AcceptMode acceptMode)
{
    static bool isFirstTime = true;

    if (isFirstTime)
    {
        isFirstTime = false;
        // get the images' locations
        QStringList imageLocations = QStandardPaths::standardLocations(QStandardPaths::PicturesLocation);
        // let the dialog open the last one
        dialog.setDirectory(imageLocations.isEmpty() ? QDir::currentPath() : imageLocations.last());
    }

    QStringList mimeTypeFilter;
    QByteArrayList const mimeTypes = acceptMode == QFileDialog::AcceptOpen ? QImageReader::supportedMimeTypes() : QImageWriter::supportedMimeTypes();
    for (auto const& mimeType : mimeTypes)
    {
        mimeTypeFilter.append(mimeType);
    }
    mimeTypeFilter.sort();
    dialog.setMimeTypeFilters(mimeTypeFilter);
    dialog.selectMimeTypeFilter("image/jpeg");
    dialog.setAcceptMode(acceptMode); // whether open or save dialog
    if (acceptMode == QFileDialog::AcceptSave)
    {
        dialog.setDefaultSuffix("jpg"); // default image type to save is .jpg
    }
}
```

Đoạn code trên đã sử dụng `QFileDialog` và các *container* trong Qt để thực hiện chức năng hiển thị và xử lý đường dẫn đến file ảnh cũng như `QMessagseBox` để thông báo cho user trong trường hợp file được chọn không hợp lệ hay các sự cố khác phát sinh. Chi tiết hơn về các lớp đối tượng trên bạn đọc xem thêm tại [GUI Design using Qt Widget - QFileDialog](/post/qt-cpp/gui-design-using-widget-qfiledialog.md), [GUI Design using Qt Widget - QMessageBox](/post/qt-cpp/gui-design-using-widget-qmessagebox.md) và [Containers In Qt](/post/qt-cpp/container.md).

Sau khi thực thi chương trình, nếu chúng ta ấn tổ hợp phím `Ctrl + O` hoặc click *open icon* trên task bar, một hộp thoại sẽ hiện ra cho phép chúng ta chọn hiển thị file ảnh bất kỳ.

![](/images/qt-cpp/gui-design-using-widget-qmainwindow/qt-5.gif)


### Dock widget

### Status bar
Đây là nơi hiện thị các trạng thái mà người lập trình viên muốn thông tin đến cho người sử dụng. Trong ví dụ này mình sẽ minh họa việc sử dụng *status bar* bằng cách hiển thị *tooltips* của các *action* trên *toolbar*.

Thay đổi phương thức `void MainWindow::createToolbars()` như sau:
```c++
void MainWindow::createToolbars()
{
    QToolBar* toolbar = addToolBar(QString("Main Toolbars"));
    newToolbar = toolbar->addAction(QPixmap(":/icons/actions/document-new.png"), "New");
    newToolbar->setStatusTip(QString("Create a new image"));
    openToolbar = toolbar->addAction(QPixmap(":/icons/actions/document-open.png"), "Open");
    openToolbar->setStatusTip(QString("Open a new image"));
    saveToolbar = toolbar->addAction(QPixmap(":/icons/actions/document-save.png"), "Save");
    saveToolbar->setStatusTip(QString("Save an image"));
    toolbar->addSeparator();
    quitToolbar = toolbar->addAction(QPixmap(":/icons/actions/process-stop.png"), "Quit");
    quitToolbar->setStatusTip(QString("Quit the application"));
    toolbar->setMovable(false);
}
```

Lưu ý chúng ta phải thiết lập cho *status bar* của `QMainWindow` có thể hiển thị trong constructor:

```c++
MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
{
    ...

    statusBar()->setVisible(true);
}
```

kết quả:
![](/images/qt-cpp/gui-design-using-widget-qmainwindow/qt-6.gif)

## Appendix

### Adding resources into Qt project
