---
title: "Qt - Cross-platform Framework for C++ development"
description: Qt tutorial for C++ development
date: 2021-08-07
draft: false
toc: true
image: "images/qt-cpp/header.jpg"
author: hieunghia-pat
tags: ["Qt", "C++"]
categories: [Qt]
---

## Introduction 
C++ là một trong những ngôn ngữ mạnh mẽ và phổ biến, có mặt rộng khắp các mặt của ngành công nghiệp phần mềm lẫn tự động hóa. Do sự phổ biến của C++ với hiệu năng và những tính ưu việt của nó đã làm nảy sinh nhu cầu về một framework mạnh mẽ, hỗ trợ lập trình viên C++ có thể dễ dàng và nhanh chóng thiết kế một hệ thống phần mềm lớn và phức tạp. Trong ngành công nghiệp phần mềm hiện nay có không ít framework cho C++ chuyên biệt cho nhiều lĩnh vực cụ thể, đa dạng như [Caffe](https://caffe.berkeleyvision.org/) - [Caffe2](https://caffe2.ai/) - [Flashlight](https://ai.facebook.com/blog/flashlight-fast-and-flexible-machine-learning-in-c-plus-plus/) cho Deep learning, [Array Fire](https://arrayfire.org/docs/index.htm) cho việc tính toán mảng đa chiều, [OpenCV](https://opencv.org/) cho xử lý ảnh, [OpenGL](https://www.opengl.org//) cho xử lý đồ họa, ... Trong lĩnh vực phát triển phần mềm, [Qt](https://www.qt.io/) là một trong những framework được biết đến bởi sự mạnh mẽ và phổ biến. Nó mạnh mẽ bởi Qt hỗ trợ việc thiết kế và xây dựng nên phần mềm đa nền tảng (Cross-platfotm programming), bao gồm bốn nền tảng chính là Linux, MacOS, Windows và Android và thậm chí là cả nền tảng Web. Rất nhiều phần mềm sử dụng trên các distros Linux đều được viết dựa trên Qt (Qt và [Gtk](https://www.gtk.org/) là 2 cross-platform phổ biến của C++ và C, rất nhiều phần mềm trên các distros Linux được phát triển nhờ vào 2 framework này).

## Download and Installation
Qt hỗ trợ việc phát triển phần mềm đa nền tảng, nên việc tải và cài đặt nó cũng khá dễ dàng và thuận tiện. Bạn đọc có thể tải và sử dụng Qt trên cả Windows, Linux distros (ubuntu, Arch Linux, Linux Mint, RedHat, ...) và MacOS. Hướng dẫn tải và cài đặt bạn đọc có thể tham khảo tại trang chủ của [Qt](https://www.qt.io/download) hoặc các nguồn khác trên google và youtube. Hiện tại Qt phiên bản 5.15.2 là phiên bản ổn định nhất, nhưng mình vẫn khuyến khích bạn đọc sử dụng phiên bản 6.0 trở lên để có thể sử dụng Qt với các tính năng mới của các chuẩn C++ sau này.

## Qt for other languages
Qt được viết hoàn toàn bằng C++, tuy nhiên [The Qt Company](https://www.qt.io/company) cũng đã đồng thời phát triển để  Qt có thể được sử dụng được với một số ngôn ngữ khác như Python và Java. Với Python bạn đọc có thể tìm hiểu cách cài và sử dụng PyQt hoặc PySide cho việc phát triển chương trình dựa trên Qt. Hiện tại đã có PyQt6 và PySide6 tương thích với Qt 6.0. 

*Note: [PyQt](https://riverbankcomputing.com/software/pyqt/intro) được phát triển bởi Phil Thompson - một kỹ sư phần mềm của công ty [The Riverbank Computing](https://riverbankcomputing.com/). Do các vấn đề về bản quyền, năm 2009, Nokia, công ty đang sở hữu Qt toolkit vào thời điểm đó, đã phát triển và cho ra đời [PySide](https://doc.qt.io/qtforpython/) và sau này được duy trì và phát triển bởi The Qt Company. Trên thực tế hai framework này khá là tương đồng với nhau và theo nhiều ý kiến đánh giá, PyQt và cả PySide tương tự nhau gần 99%. Do đó việc lựa chọn PyQt hay PySide không phải là vấn đề lớn. Trong blog này mình chỉ giới thiệu [PySide](/post/pyside-python/introduction) đến bạn đọc bởi nó chính chuyên hơn và được hỗ trợ bởi QtCreator - IDE chuyên cho việc phát triển phần mềm sử dụng Qt cũng do The Qt Company phát triển.*

## IDE
The Qt Company khuyến khích sử dụng Qt Creator cho việc phát triển app bằng Qt do Qt Creator được tích hợp rất nhiều công cụ hỗ trợ cho việc viết code, bao gồm cả QT Designer để thiết kế layout. Tuy nhiên những bạn nào thích try hard với code có thể sử dụng các text editor phổ biến như VS Code, Sublime Text hay Vim để code, sau đó sử dụng [QMake]() để build project và [make]() để export executable file. Lựa chọn IDE là tùy vào gu của bạn đọc. Mình cũng sẽ có 1 [bài viết](/post/qt-cpp/your-first-program) về cách sử dụng text editor (cụ thể là VS Code) để xây dựng project bằng Qt.