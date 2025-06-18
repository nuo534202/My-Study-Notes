课程链接：[EasyX 快速入门](https://www.bilibili.com/video/BV11p4y1i74A/?spm_id_from=333.337.search-card.all.click&vd_source=714a6e39b95bf9fd6a45f805e9d4b5f9)

EasyX 是针对 C++ 的图形库，源文件只能是 `.cpp` 文件。

# EasyX 坐标和设备

- 坐标默认远点在左上角， $X$ 轴向右为正， $Y$ 轴向下为正，度量单位是像素点
- 设备：绘图表面
  - 默认的绘图窗口
  - `IMAGE` 对象

# EasyX 图形编程

## 窗口函数

```cpp
initgraph(int width, int height, int flag = NULL); // flag 是窗口样式，默认为 NULL
closegraph(); // 关闭绘图窗口
cleardevice(); // 清空绘图设备
```

## 图形绘制函数

绘图函数样式可分为无填充、有边框填充、无边框三种。

以画圆为例：

1. 无填充：`circle(int x, int y, int radius);`
2. 有边框填充：`fillcircle(int x, int y, int radius);`
3. 无边框填充：`solidcircle(int x, int y, int radius);`

画圆 `circle()`，画椭圆 `ellipse()`，画扇形 `pie()`，画多边形 `polygon()`，画矩形 `rectangle()`，画圆角矩形 `roundrect()`，画线 `line()`，画点 `putpixel()`。

- 设置填充颜色 `void setfillcolor(COLORREF color);`
- 设置边框颜色 `void setlinecolor(COLORREF color);`
- 设置线条样式 `void setlinestyle(int style, int thickness = 1, const DWORD *puserstyle = NULL, DWORD userstylecount = 0);`
- 设置背景颜色 `void setbkcolor(COLORREF color);`
- 清屏 `void cleardevice();`

## 文字绘制函数

- 在指定位置输出字符串：`void outtextxy(int x, int y, LPCTSTR str);`
  - 由于字符集错误导致找不到对应函数的解决方式
    - 在字符串前面加上大写的 `L`
    - 将字符串放入 `TEXT()`，`_T()`
    - 进 项目->属性->配置属性->高级->字符集 改为多字节字符集
- 设置当前文字颜色：`void settextcolor(COLORREF color);`
- 设置字体样式：`settextstyle(int nHeight, int nWidth, LPCTSTR IpszFace);`
  - `nHeight` 指定高度
  - `nWidth` 字符的平均宽度。如果为 $0$，则比例自适应
  - `IpszFace` 字体名称
- 获取字符串实际占用的像素高度：`textheight(LPCTSTR str);`
- 获取字符串实际占用的像素宽度：`textwidth(LPCTSTR str);`
- 设置背景混合模式：`void setbkmode(int mode);`，`setbkmode(TRANSPARENT);` 背景透明。

## 图像处理函数

- 在使用图像之前，需要定义一个变量（对象），然后把图片加载进变量才能进行使用；在使用图像的时候需要使用 EasyX 提供的数据类型 `IMAGE`，例如 `IMAGE img;`
- `void loadimage(IMAGE *pDstImg, LPCTSTR pImgFile, int nWidth = 0, int nHeight = 0, bool bResize = false);`：从文件中读取图像。
  - `pDstImg` 是保存图像的 `IMAGE` 对象指针
  - `pImgFile` 是图片文件名
  - `nWidth` 是图片的拉伸宽度
  - `nHeight` 是图片的拉伸高度
  - `bResize` 代表是否调整 `IMAGE` 大小以适应图片
- `void putimage(int dstX, int dstY, const IMAGE *pSrcImg, DWORD dwRop = SRCCOPY);`：在当前设备上绘制指定图像。
    - `dstX` 是绘制位置的 $x$ 坐标
    - `dstY` 是绘制位置的 $y$ 坐标
    - `pSrcImg` 是要绘制的 `IMAGE` 对象指针
    - `dwRop` 是三元光栅操作码

**Sample：**

```cpp
IMAGE img;
loadimage(img, "./"); // 当前文件夹的上一级目录
```

## 鼠标操作

### 鼠标消息函数（旧版）

- 鼠标消息需要使用 `MOUSEMSG` 类型，例如 `MOUSEMSG msg`;
- 然后用 `MouseHit()` 判断是否有鼠标消息（左键，右键，中间，移动），有鼠标消息返回 `true`，没有鼠标消息返回 `false`
- 如果有鼠标消息就可以进行接收鼠标消息了 `msg = GetMouseMsg();`
- 鼠标消息主要成员：
  - `uMsg` 是当前鼠标消息
  - `x` 是当前鼠标的 $x$ 坐标
  - `y` 是当前鼠标的 $y$ 坐标
- `uMsg` 可用来判断当前鼠标消息是什么消息
  - `WM_LBUTTONDOWN` 是鼠标左键消息
  - `WM_RBUTTONDOWN` 是鼠标右键消息

**Sample：**

```cpp
if (MouseHit()) {
	MOUSEMSG msg = GetMouseMsg();
	if (msg.uMsg == WM_LBUTTONDOWN) {
		// code here
	}
	if (msg.uMsg == WM_RBUTTONDOWN) {
		// code here
	}
}
```

### 鼠标消息函数（新版）

```cpp
#include <easyx.h>
#include <graphics.h>
using namespace std;

void button(const int x, const int y, const int w, const int h, const char* _text) {
	setbkmode(TRANSPARENT); // 设置文字背景透明
	setfillcolor(BROWN); // 设置填充颜色为棕色
	fillroundrect(x, y, x + w, y + h, 10, 10); // 绘制圆角矩形作为按钮
	settextstyle(30, 0, "黑体"); // 设置文字样式：30 号字，默认宽度，黑体
	char text[50];
	strcpy_s(text, _text);
	int tx = x + (w - textwidth(text)) / 2;
	int ty = y + (h - textheight(text)) / 2;
	outtextxy(tx, ty, text); // 文本居中输出
}

int main() {
	initgraph(640, 480, EX_SHOWCONSOLE); // 创建窗口后显示命令行
	button(10, 10, 200, 100, "Button");
	ExMessage msg;
	if (peekmessage(&msg, EM_MOUSE)) {
		// 获取鼠标消息，如果有鼠标消息返回 true，否则返回 false
		switch (msg.message) {
			case WM_LBUTTONDOWN:
				// 按下左键
				// code here
				break;
			case WM_RBUTTONDOWN:
				// 按下右键
				// code here
				break;
			default:
				// code here
				break;
		}
	}
	system("pause");
	return 0;
}
```

## 键盘操作与物体移动

### 键盘消息函数

作用：用于获取键盘按键消息

- `_getch();` 需要头文件 `<conio.h>`
  - 需要使用返回值来判断
  - 与非 `ASCII` 表字符的按键比较，需要使用虚拟值；例如，上 `72`，下 `80`，左 `75`，右 `77`
  - 如果是与字母比较直接写字母，比如 `A`
- `SHORT GetAsyncKeyState(int vKey);` 需要头文件 `<windows.h>`，但是由于 `EasyX` 包含了 `<windows.h>` 头文件，所以无需自己包含
  - 需要传入一个键值，如果按下返回 true
  - 上 `VK_UP`，下 `VK_DOWN`，左 `VK_LEFT`，右 `VK_RIGHT`

### 其他函数

- 在设备上不断进行绘图操作时，会发生闪频现象，此时眼睛就受不了了，针对这个现象，需要两个函数处理。
	```cpp
	BeginBatchDraw(); // 开始批量绘图
	// 绘图代码
	FlushBatchDraw(); // 刷新显示
	EndBatchDraw(); // 结束批量绘图
	```
- `GetHWnd();` 获取窗口句柄，获取之后可以用来操作窗口
  - `HWND hWnd = GetHWnd();` // 获取窗口句柄
  - 修改窗口标题：`SetWindowText(hWnd, "love")`
  - 设置模态对话框：`MessageBox(hWnd, "我是消息框", "我是标题", MB_OKCANCEL)`

**Sample：**

```cpp
#include <iostream>
#include <graphics.h>
using namespace std;

// 播放音乐
void change() {
	HWND hnd = GetHWnd(); // 获取窗口句柄
	SetWindowText(hnd, "nuo"); // 设置窗口标题
	// 弹出窗口，提示用户操作

	// 窗口置前，一定要先处理窗口内容
	MessageBox(hnd, "Congratulations", "Hint", MB_OKCANCEL);
	// 可以切换别的窗口，不用先处理当前窗口内容
	int isOk = MessageBox(nullptr, "Congratulations", "Hint", MB_OKCANCEL);
	if (isOk = IDOK)
		cout << "You click Ok" << endl;
	else if (isOk == IDCANCEL)
		cout << "You click Cancel" << endl;
}

int main() {
	initgraph(400, 400);
	system("pause");
	change();
	system("pause");
	return 0;
}
```

# 音乐播放

## windowsAPI 播放音乐

- 为了实现用 `C` 语言播放音乐，我们需要用到 `windows` 的一个 `API`
  - 首先需要包含头文件 `windows.h` 和 `mmsystem.h` (如果已经包含 `graphics.h` 则无需包含)
  - 然后还需要加载静态库 `winmm.lib`
  - 最后就可以使用 `mciSendString` 函数来播放音乐了

**Sample：**

```cpp
MCIERROR mciSendString (
	LPCSTR lpstrCommand, // 命令字符串：“命令 设备[参数]”
	LPSTR lpstrReturnString, // 接收返回信息的缓冲区，为 NULL 时不返回信息
	UINT uReturnLength, // 上述缓冲区大小
	HWND hwndCallback // 一半为 NULL
);
```

使用方法：

- 打开音乐：`mciSendString("open ./music.mp3 alias BGM", NULL, 0, NULL);`
- 播放音乐：`mciSendString("play BGM", NULL, 0, NULL);`

**Sample：**

```cpp
#include <graphics.h>
#include <mmsystem.h>
#pragma comment(lib, "winmm.lib") // 预编译指令，加载静态库
using namespace std;

// 播放音乐
void BGM() {
	// 打开音乐，alias 取别名 BGM
	mciSendString("open ./music.mp3 alias BGM", nullptr, 0, nullptr);
	// 播放音乐，repeat 是一直播放
	mciSendString("play BGM repeat", nullptr, 0, nullptr);
}

int main() {
	BGM();
	return 0;
}
```
