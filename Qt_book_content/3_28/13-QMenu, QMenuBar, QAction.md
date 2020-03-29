## 13 - QMenu, QMenuBar, QAction

widget.h

```c++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QMenu>
#include <QMenuBar>
#include <QAction>
#include <QLabel>

class Widget : public QWidget
{
    Q_OBJECT

public:
    Widget(QWidget *parent = nullptr);
    ~Widget();

private:
    QMenu *menu[3];
    QMenuBar *menuBar;
    QLabel *lbl;
    QAction *act[2];

private slots:
    void trigerMenu(QAction *act);

};
#endif // WIDGET_H

```

widget.cpp

```c++
#include "widget.h"

Widget::Widget(QWidget *parent)
    : QWidget(parent)
{
    menuBar = new QMenuBar(this); // 메뉴바 생성

    menu[0] = new QMenu("File"); // 메뉴[0] = "File" 생성
    menu[0]->addAction("Edit");  // 메뉴[0]안에 액션 추가
    menu[0]->addAction("View");
    menu[0]->addAction("Tools");

    act[0] = new QAction("New", this);
    act[0]->setShortcut(Qt::CTRL | Qt::Key_A); // 옆에 키 CTRL+A 추가 
    act[0]->setStatusTip("This is a New menu.");

    act[1] = new QAction("Open", this);
    act[1]->setCheckable(true); // 현재 액션 체크표시

    menu[1] = new QMenu("Save");
    menu[1]->addAction(act[0]); // New Action 추가
    menu[1]->addAction(act[1]); // Open ACtion 추가

    menu[2] = new QMenu("Print");
    menu[2]->addAction("Page Setup");
    menu[2]->addMenu(menu[1]); // Print 메뉴에 Save 메뉴 통째로 추가 

    menuBar->addMenu(menu[0]); // 메뉴바에 메뉴 목록 표시
    menuBar->addMenu(menu[1]);
    menuBar->addMenu(menu[2]);

    lbl = new QLabel("", this);

    connect(menuBar, SIGNAL(triggered(QAction *)), this, SLOT(trigerMenu(QAction *)));

    menuBar->setGeometry(0,0,600,40);
    lbl->setGeometry(10, 200, 200, 40);
}

void Widget::trigerMenu(QAction *act){
    QString str = QString("Selected Menu : %1").arg(act->text()); // 액션 객체 문자열변환 
    lbl->setText(str); // 라벨 문자 표시 
}

Widget::~Widget()
{
}


```

***

<img src="./13-gui.PNG">

