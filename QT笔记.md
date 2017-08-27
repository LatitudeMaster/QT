# QT工程
# 1.工程文件配置.pro
* QT	+= core gui  //包含的模块
* greaterThan(QT_MAJOR_VERSION, 4): QT += widgets //大于Qt4版本 才包含widget模块
* TARGET = QtFirst  //应用程序名  生成的.exe程序名称
* TEMPLATE = app    //模板类型    应用程序模板
* SOURCES += main.cpp\   //源文件
*         mywidget.cpp
* HEADERS  += mywidget.h   //头文件
* CONFIG += c++11	//使用c++11的特性

# 2.按键创建
* QPushButton * btn = new QPushButton; 头文件 # include <QPushButton>
*     //设置父亲
*     btn->setParent(this);
*     //设置文字
*     btn->setText("德玛西亚");
*     //移动位置
*     btn->move(100,100);
	
# 3.对象模型
```cpp
{
    QWidget window;
	//作为父组件的 window 和作为子组件的 quit 都是QObject的子类
    QPushButton quit("Quit", &window);
}
```
# 4.信号和曹
* QPushButton * quitBtn = new QPushButton("关闭窗口",this);
* //connect()函数最常用的一般形式：connect(sender, signal, receiver, slot);
* connect(quitBtn,&QPushButton::clicked,this,&MyWidget::close);

## Qt4版本的信号槽写法
* connect(zt,SIGNAL(hungry(QString)),st,SLOT(treat(QString)));
* //这里使用了SIGNAL和SLOT这两个宏，将两个函数名转换成了字符串。

# 5.Lambda表达式
```cpp
QPushButton * myBtn = new QPushButton (this);
QPushButton * myBtn2 = new QPushButton (this);
myBtn2->move(100,100);
int m = 10;
connect(myBtn,&QPushButton::clicked,this,[m] ()mutable { m = 100 + 10; qDebug() << m; });
connect(myBtn2,&QPushButton::clicked,this,[=] ()  { qDebug() << m; });
//	[=]为值传递；[&]为引用传递
```
# 6.菜单栏
```cpp
//1.创建菜单栏,通过QMainWindow类的menubar（）函数获取主窗口菜单栏指针
	QMenuBar *	menuBar() const
//2.创建菜单，调用QMenu的成员函数addMenu来添加菜单
	QAction* addMenu(QMenu * menu)
	QMenu* addMenu(const QString & title)
	QMenu* addMenu(const QIcon & icon, const QString & title)
//3.创建菜单项，调用QMenu的成员函数addAction来添加菜单项
	QAction* activeAction() const
	QAction* addAction(const QString & text)
	QAction* addAction(const QIcon & icon, const QString & text)
	QAction* addAction(const QString & text, const QObject * receiver,const char * member, const QKeySequence & shortcut = 0)
	QAction* addAction(const QIcon & icon, const QString & text, 
	const QObject * receiver, const char * member, 
	const QKeySequence & shortcut = 0)
```
# 7.工具栏
* //使用setAllowedAreas（）函数指定停靠区域：
* setAllowedAreas（Qt::LeftToolBarArea | Qt::RightToolBarArea）
* //使用setMoveable（）函数设定工具栏的可移动性：
* setMoveable（false）//工具条不可移动, 只能停靠在初始化的位置上

# 8.状态栏
* //添加小部件
* void addWidget(QWidget * widget, int stretch = 0)
* //插入小部件
* int	insertWidget(int index, QWidget * widget, int stretch = 0)
* //删除小部件
* void removeWidget(QWidget * widget)

# 9.铆接部件(浮动窗口)
* 	QDockWidget * dock = new QDockWidget("标题",this);
*     addDockWidget(Qt::LeftDockWidgetArea,dock);
* 	//设置区域范围
* 	dock->setAllowedAreas(Qt::LeftDockWidgetArea | Qt::RightDockWidgetArea | Qt::TopDockWidgetArea);

# 10.核心部件（中心部件）
* 	QTextEdit * edit = new QTextEdit(this);
*     setCentralWidget(edit);
* ......

# 11.创建资源文件res.qrc
```xml
<RCC>
    	<qresource prefix="/images">
        	<file alias="doc-open">document-open.png</file>
    	</qresource>
    	<qresource prefix="/images/fr" lang="fr">
        	<file alias="doc-open">document-open-fr.png</file>
    	</qresource>
</RCC>
```

# 12.对话框（QDialog）
```cpp
//1.标准对话框
	QColorDialog：		选择颜色；
	QFileDialog：			选择文件或者目录；
	QFontDialog：			选择字体；
	QInputDialog：		允许用户输入一个值，并将其值返回；
	QMessageBox：			模态对话框，用于显示信息、询问问题等；
	QPageSetupDialog：	为打印机提供纸张相关的选项；
	QPrintDialog：		打印机配置；
	QPrintPreviewDialog：打印预览；
	QProgressDialog：		显示操作过程。
//2.模态对话框
	QDialog dialog;
	dialog.setWindowTitle(tr("Hello, dialog!"));
	dialog.exec();
//3.非模态对话框
	QDialog *dialog = new QDialog;
	//setAttribute()函数设置对话框关闭时，自动销毁对话框。
    dialog->setAttribute(Qt::WA_DeleteOnClose);
    dialog->setWindowTitle(tr("Hello, dialog!"));
    dialog->show();
//4.消息对话框
QMessageBox msgBox;
msgBox.setText(tr("The document has been modified."));
msgBox.setInformativeText(tr("Do you want to save your changes?"));
msgBox.setDetailedText(tr("Differences here..."));
msgBox.setStandardButtons(QMessageBox::Save
                          | QMessageBox::Discard
                          | QMessageBox::Cancel);
msgBox.setDefaultButton(QMessageBox::Save);
int ret = msgBox.exec();
switch (ret) 
{
case QMessageBox::Save:
    qDebug() << "Save document!";
    break;
case QMessageBox::Discard:
    qDebug() << "Discard changes!";
    break;
case QMessageBox::Cancel:
    qDebug() << "Close document!";
    break;
}
//5.标准文件对话框
```

* 1.先创建一个带有文本编辑功能的窗口
```cpp
openAction = new QAction(QIcon(":/images/file-open"),tr("&Open..."), this);
openAction->setStatusTip(tr("Open an existing file"));

saveAction = new QAction(QIcon(":/images/file-save"), tr("&Save..."), this);
saveAction->setStatusTip(tr("Save a new file"));

QMenu *file = menuBar()->addMenu(tr("&File"));
file->addAction(openAction);
file->addAction(saveAction);

QToolBar *toolBar = addToolBar(tr("&File"));
toolBar->addAction(openAction);
toolBar->addAction(saveAction);

textEdit = new QTextEdit(this);
setCentralWidget(textEdit);
```
* 2.最主要的openFile()和saveFile()这两个函数的代码：
```cpp
//打开文件
void MainWindow::openFile()
{
    QString path = QFileDialog::getOpenFileName(this,
               tr("Open File"), ".", tr("Text Files(*.txt)"));
    if(!path.isEmpty()) 
{
        QFile file(path);
        if (!file.open(QIODevice::ReadOnly | QIODevice::Text)) 
{
            QMessageBox::warning(this, tr("Read File"),
                         tr("Cannot open file:\n%1").arg(path));
            return;
        }
        QTextStream in(&file);
        textEdit->setText(in.readAll());
        file.close();
    } 
else 
{
        QMessageBox::warning(this, tr("Path"),
                             tr("You did not select any file."));
     }
}
```
```cpp
//保存文件
void MainWindow::saveFile()
{
    QString path = QFileDialog::getSaveFileName(this,
               tr("Open File"), ".", tr("Text Files(*.txt)"));
    if(!path.isEmpty()) 
{
        QFile file(path);
        if (!file.open(QIODevice::WriteOnly | QIODevice::Text)) 
{
            QMessageBox::warning(this, tr("Write File"),
                         tr("Cannot open file:\n%1").arg(path));
            return;
        }
        QTextStream out(&file);
        out << textEdit->toPlainText();
        file.close();
    } 
else 
{
        QMessageBox::warning(this, tr("Path"),
                             tr("You did not select any file."));
    }
}
```
# 14.布局管理器及常用控件
# 14.1 QLabel控件使用
```cpp
//1.显示文字 （普通文本、html）
//  可以显示普通文本字符串
QLable *label = new QLable;
label->setText(“Hello, World!”);
//  可以显示HTML格式的字符串
QLabel * label = new QLabel(this);
label ->setText("Hello, World");
label ->setText("<h1><a href=\"https://www.baidu.com\">百度一下</a></h1>");
label ->setOpenExternalLinks(true);
//2.显示图片
//  首先定义QPixmap对象
QPixmap pixmap;
//  然后加载图片
pixmap.load(":/Image/boat.jpg");
//  最后将图片设置到QLabel中
QLabel *label = new QLabel;
label.setPixmap(pixmap);
//3.显示动画
//  首先定义QMovied对象，并初始化:
QMovie *movie = new QMovie(":/Mario.gif");
//  播放加载的动画：
movie->start();
//  将动画设置到QLabel中：
QLabel *label = new QLabel；
label->setMovie(movie);
```
# 14.2 QLineEdit
```cpp
//1.设置/获取内容
//?获取编辑框内容使用text（），函数声明如下：
QString	text() const
//?设置编辑框内容
void	setText(const QString &)
//2.设置显示模式
void	setEchoMode(EchoMode mode)
/*EchoMode是一个枚举类型,一共定义了四种显示模式:
?QLineEdit::Normal	 模式显示方式，按照输入的内容显示。
?QLineEdit::NoEcho	不显示任何内容，此模式下无法看到用户的输入。
?QLineEdit::Password	密码模式，输入的字符会根据平台转换为特殊字符。
?QLineEdit::PasswordEchoOnEdit	编辑时显示字符否则显示字符作为密码。
*/
```
# 15.Qt消息机制和事件
```cpp
//1.事件
//!!! Qt5
bool QObject::event(QEvent *e)
{
    switch (e->type()) {
    case QEvent::Timer:
        timerEvent((QTimerEvent*)e);
        break;

    case QEvent::ChildAdded:
    case QEvent::ChildPolished:
    case QEvent::ChildRemoved:
        childEvent((QChildEvent*)e);
        break;
    // ...
    default:
        if (e->type() >= QEvent::User) {
            customEvent(e);
            break;
        }
        return false;
    }
    return true;
}
```
```cpp
//2.事件过滤器
bool FilterObject::eventFilter(QObject *object, QEvent *event)
{
    if (object == target && event->type() == QEvent::KeyPress) 
{
        QKeyEvent *keyEvent = static_cast<QKeyEvent *>(event);
        if (keyEvent->key() == Qt::Key_Tab) {
            qDebug() << "You press tab.";
            return true;
        } else {
            return false;
        }
    }
    return false;
}
```
* 事件过滤器和被安装过滤器的组件必须在同一线程，否则，过滤器将不起作用。另外，如果在安装过滤器之后，这两个组件到了不同的线程，那么，只有等到二者重新回到同一线程的时候过滤器才会有效。

# 16.绘图和绘图设备
# 16.1 QPainter
```cpp
class PaintedWidget : public QWidget
{
    Q_OBJECT
public:
    PaintedWidget(QWidget *parent = 0);
protected:
    void paintEvent(QPaintEvent *);
}
PaintedWidget::PaintedWidget(QWidget *parent) :
    QWidget(parent)
{
    resize(800, 600);
    setWindowTitle(tr("Paint Demo"));
}
void PaintedWidget::paintEvent(QPaintEvent *)
{
    QPainter painter(this);
    painter.drawLine(80, 100, 650, 500);
    painter.setPen(Qt::red);
    painter.drawRect(10, 10, 100, 400);
    painter.setPen(QPen(Qt::green, 5));
    painter.setBrush(Qt::blue);
    painter.drawEllipse(50, 150, 400, 200);
}
```
# 16.2 绘图设备
```cpp
//1.QPixmap
void PaintWidget::paintEvent(QPaintEvent *)
{
    QPixmap pixmap(":/Image/butterfly.png");
    QPixmap pixmap1(":/Image/butterfly1.png");
    QBitmap bitmap(":/Image/butterfly.png");
    QBitmap bitmap1(":/Image/butterfly1.png");

    QPainter painter(this);
    painter.drawPixmap(0, 0, pixmap);
    painter.drawPixmap(200, 0, pixmap1);
    painter.drawPixmap(0, 130, bitmap);
    painter.drawPixmap(200, 130, bitmap1);
}
//2.QBitmap
void PaintWidget::paintEvent(QPaintEvent *)
{
    QPainter painter(this);
    QImage image(300, 300, QImage::Format_RGB32);
    QRgb value;

    //将图片背景填充为白色
    image.fill(Qt::white);

    //改变指定区域的像素点的值
    for(int i=50; i<100; ++i)
    {
        for(int j=50; j<100; ++j)
        {
            value = qRgb(255, 0, 0); // 红色
            image.setPixel(i, j, value);
        }
    }

    //将图片绘制到窗口中
    painter.drawImage(QPoint(0, 0), image);
}
//3.QPicture
void PaintWidget::paintEvent(QPaintEvent *)
{
    QPicture pic;
    QPainter painter;
	 //将图像绘制到QPicture中,并保存到文件
    painter.begin(&pic);
    painter.drawEllipse(20, 20, 100, 50);
    painter.fillRect(20, 100, 100, 100, Qt::red);
    painter.end();
    pic.save("D:\\drawing.pic");

	 //将保存的绘图动作重新绘制到设备上
    pic.load("D:\\drawing.pic");
    painter.begin(this);
    painter.drawPicture(200, 200, pic);
    painter.end();
}
```
# 17.文件系统
| 控件            |   说明                                                    |
|:-------------   |:-----  |
| QIODevice       | 所有 I/O 设备类的父类，提供了字节块读写的通用操作以及基本接口；|
| QFileDevice     | Qt5新增加的类，提供了有关文件操作的通用实现。                 |
| QFlie           | 访问本地文件或者嵌入资源；                                  |
| QTemporaryFile  | 创建和访问本地文件系统的临时文件；                           |
| QBuffer         | 读写QbyteArray, 内存文件；                                 |
| QProcess        | 运行外部程序，处理进程间通讯；                               |
| QAbstractSocket | 所有套接字类的父类；                                        |
| QTcpSocket      |TCP协议网络数据传输；                                        |
| QUdpSocket      | 传输 UDP 报文；                                            |
| QSslSocket      | 使用 SSL/TLS 传输数据；                                     |

```cpp
//1.QFile的有关操作：
int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
 
    QFile file("in.txt");
    if (!file.open(QIODevice::ReadOnly | QIODevice::Text)) {
        qDebug() << "Open file failed.";
        return -1;
    } else {
        while (!file.atEnd()) {
            qDebug() << file.readLine();
        }
    }
	QFileInfo info(file);
    qDebug() << info.isDir();
    qDebug() << info.isExecutable();
    qDebug() << info.baseName();
    qDebug() << info.completeBaseName();
    qDebug() << info.suffix();
    qDebug() << info.completeSuffix();
 
    return app.exec();
}
```
```cpp
//2.二进制文件读写
//写文件
QFile file("file.dat");
file.open(QIODevice::WriteOnly);
QDataStream out(&file);
out << QString("the answer is");
out << (qint32)42;
//读文件
QFile file("file.dat");
file.open(QIODevice::ReadOnly);
QDataStream in(&file);
QString str;
qint32 a;
in >> str >> a;
//QDataStream流
QFile file("file.dat");
file.open(QIODevice::ReadWrite);
QDataStream stream(&file);
QString str = "the answer is 42";
stream << str;
```
```cpp
//3.文本文件读写
QFile data("file.txt");
if (data.open(QFile::WriteOnly | QIODevice::Truncate)) 
{
    QTextStream out(&data);
    out << "The answer is " << 42;
}
```

 | 枚举值               |           描述                                                                                 |
 | -------------       |:-------------                                                                                  |
 |QIODevice::NotOpen   |	未打开                                                                                  |      
 |QIODevice::ReadOnly  |	以只读方式打开                                                                           |
 |QIODevice::WriteOnly |	以只写方式打开                                                                           |
 |QIODevice::ReadWrite |	以读写方式打开                                                                           |
 |QIODevice::Append    |	以追加的方式打开，新增加的内容将被追加到文件末尾                                            |
 |QIODevice::Truncate  |	以重写的方式打开，在写入新的数据时会将原有数据全部清除，游标设置在文件开头。                   |
 |QIODevice::Text      |	在读取时，将行结束符转换成 \n；在写入时，将行结束符转换成本地格式，例如 Win32 平台上是 \r\n    |
 |QIODevice::Unbuffered|	忽略缓存                                                                                 |

* 文本文件读取
| QTextStream::readLine() |	读取一行      |
| QTextStream::readAll()  |     读取所有文本  |
```cpp
//QTextStream的编码转码
stream.setCodec("UTF-8");
```
# QT教程
http://www.qter.org/portal.php?mod=view&aid=26
