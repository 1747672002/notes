# Windows API 笔记

## 一些不太好找的官方手册地址

窗口消息介绍的地址：https://docs.microsoft.com/zh-cn/windows/desktop/winmsg/about-messages-and-message-queues

使用键盘输入：https://docs.microsoft.com/zh-cn/windows/desktop/inputdev/using-keyboard-input



## xxxx函数 - 功能说明

### 文档地址

[Microsoft 官方手册](

[百度百科](

### 函数说明

### 语法说明

### 参数说明

### 返回说明



## OpenProcess - 打开进程

### 文档地址

[Microsoft 官方手册](https://docs.microsoft.com/zh-cn/windows/desktop/api/processthreadsapi/nf-processthreadsapi-openprocess)

[百度百科](https://baike.baidu.com/item/OpenProcess)

### 函数说明

打开现有的本地进程对象

如果指定的进程是系统进程 `(0x00000000)`，则函数将失败，最后的错误代码为 `ERROR_INVALID_PARAMETER`。

如果指定的进程是空闲进程或其中一个 `CSRSS` 进程，则此函数将失败，错误代码为 `ERROR_ACCESS_DENIED`。

因为它们

### 语法说明

```c++
HANDLE OpenProcess(
	DWORD	dwDesiredAccess,
    BOOL	bInheritHandle,
    DWORD	dwProcessId
);
```

### 参数说明

+ `dwDesiredAccess`：对进程对象的访问。根据进程的安全描述符检查次访问权限。次参数可以是一个或多个进程访问权限
  + `PROCESS_ALL_ACCESS` ：获取所有权限
  + `PROCESS_CREATE_PROCESS`：创建进程
  + `PROCESS_CREATE_THREAD`：创建线程
  + `PROCESS_DUP_HANDLE`：使用 `DuplicateHandle()` 函数复制一个新句柄
  + `PROCESS_QUERY_INFORMATION`：获取进程的令牌、退出码和优先级等信息
  + `PROCESS_QUERY_LIMITED_INFORMATION`：获取进程特定的某个信息
  + `PROCESS_SET_INFORMATION`：设置进程的某种信息
  + `PROCESS_SET_QUOTA`：使用 `SetProcessWorkingSetSize()` 函数设置内存限制
  + `PROCESS_SUSPEND_RESUME`：暂停或者恢复一个进程
  + `PROCESS_TERMINATE`：使用 `Terminate()` 函数终止进程
  + `PROCESS_VM_OPERATION`：在进程的地址空间执行操作
  + `PROCESS_VM_READ`：使用 `ReadProcessMemory()` 函数在进程中读取内存
  + `PROCESS_VM_WRITE`：使用 `WriteProcessMemory()` 函数在进程中写入内存
  + `SYNCHRONIZE`：使用 `wait()` 函数等待进程终止
+ `bInheritHandle`：如果此值为 `True`， 则此进程创建的进程将基础该句柄。否则，进程不会继承此句柄。
+ `dwProcessId`：要打开的本地进程的标识符。

### 返回说明

如果函数成功，则返回值是指定进程的打开句柄。

如果函数失败，则返回值为`NULL`。 要获取扩展错误信息，请调用 `GetLastError` 函数。



## ReadProcessMemory - 读内存

### 文档地址

[Microsoft 官方手册](https://docs.microsoft.com/zh-cn/windows/desktop/api/memoryapi/nf-memoryapi-readprocessmemory)

[百度百科](https://baike.baidu.com/item/ReadProcessMemory)

### 函数说明

从指定进程中的内存区域读取数据。

要读取的整个区域必须可访问，否则操作失败。

### 语法说明

```c++
BOOL ReadProcessMemory(
	HANDLE 	hProcess,
    LPCVOID lpBaseAddress,
    LPVOID	lpBuffer,
    SIZE_T	nSize,
    SIZE_T	*lpNumberOfBytesRead
)
```

### 参数说明

+ `hProcess`：正在读取内存的进错句柄。句柄必须具有 `PROCESS_VM_READ` 访问权限
+ `lpBaseAddress`：指向要从中读取的指定进程中的基址的指针。在发送任何数据传输之前，系统会验证基本地址和指定大小的内存中的所有数据是否都可以进行读访问，如果无法访问，则该函数将失败。
+ `lpBuffer`：指向缓存区的指针，该缓存区从指定进程的地址空间接收内容。
+ `nSize`：要从指定进程读取的字节数
+ `lpNumberOfBytesRead`：指向变量的指针，该变量接收传到指定缓存区的字节数。如果该参数设置为 `NULL`，则忽略该参数。

### 返回说明

如果函数成功，则返回值为非零值。

如果函数失败，则返回值为 `0(零)` 。要获取扩展错误信息，请调用 `GetLastError` 

如果请求的读取操作跨越到不可访问的进错区域，则该函数将失败。



## WriteProcessMemory - 写内存

### 文档地址

[Microsoft 官方手册](https://docs.microsoft.com/zh-cn/windows/desktop/api/memoryapi/nf-memoryapi-writeprocessmemory)

[百度百科](https://baike.baidu.com/item/WriteProcessMemory)

### 函数说明

将数据写入指定进程中的内存区域。要写入的整个区域必须可访问，否则操作失败。

### 语法说明

```c++
BOOL WriteProcessMemory(
	HANDLE 	hProcess,
    LPVOID	lpBaseAddress,
    LPCVOID	lpBuffer,
    SIZE_T	nSize,
    SIZE_T	*lpNumberOfBytesWritten
);
```

### 参数说明

+ `hProcess`：要修改的进程内存的句柄。句柄必须具有 `PROCESS_VM_WRITE` 和 `PROCESS_VM_OPERATION` 访问权限。
+ `lpBaeAddress`：指向要写入数据的指定进程中的基址的指针。在发生数据传输之前，系统会验证指定大小的基址和内存中的所有数据是否都可以进行写访问，如果无法访问，则该函数失败。
+ `lpBuffer`：指向缓冲区的指针，该缓冲区包含要在指定进程的地址空间中写入的数据。
+ `nSize`：要写入指定进程的字节数。
+ `lpNumberOfBytesWritten`：指向变量的指针，该变量接收传输数据到指定进程的字节数。此参数是可选的。

### 返回说明

如果函数成功，则返回值为非零值。

如果函数失败，则返回值为 `0(零)` 。要获取扩展错误信息，请调用 `GetLastError` 。如果请求的写入操作跨越到不可访问的进程区域，则该函数将失败。



## CreateProcess - 创建进程

### 文档地址

[Microsoft 官方手册](https://docs.microsoft.com/zh-cn/windows/desktop/api/processthreadsapi/nf-processthreadsapi-createprocessa)

[百度百科](https://baike.baidu.com/item/CreateProcess)

### 函数说明

创建一个新进程及其主线程。

### 语法说明

```c++
BOOL CreateProcess(
	LPCSTR					lpApplicationName,
    LPSTR					lpCommandLine,
    LPSECURITY_ATTRIBUTES	lpProcessAttributes,
    LPSECURITY_ARREIBUTES	lpThreadAttributes,
    BOOL					bInherithandles,
    DWORD					dwCreationFlags,
    LPVOID					lpEnvironment,
    LPCSTR					lpCurrentDirectory,
    LPSTARTUPINFOA			lpStartupInfo,
    LPPROCESS_INFORMATION	lpProcessInformation
);
```

### 参数说明

+ `lpApplicationName`：可执行文件的名称
+ `lpCommandLine`：指定了要传递给执行模块的参数。
+ `lpProcessAttributes`：进程安全性，值为 `NULL` 的话表示使用默认的安全属性。
+ `lpThreadAttributes`：线程安全性，值为 `NULL` 的话表示使用默认的安全属性。
+ `bInherithandles`：指定了当前进程中的可继承句柄是否可被新进程继承。
+ `dwCreationFlags`：指定了新进程的优先级及其他创建标志。
+ `lpEnvironment`：指定新进程使用的环境变量
+ `lpCurrentDirectory`：新进程使用的当前目录
+ `lpStartupInfo`：指定新进程中主窗口的位置、大小和标准句柄等。
+ `lpProcessInformation`：【out】返回新建进程的标志信息，如ID号、句柄等。

<font color='#hhgx55'>该函数命令较为复杂，建议查看上面提供的手册</font>

### 返回说明



## CreateThread - 创建线程

### 文档地址

[Microsoft 官方手册](https://docs.microsoft.com/zh-cn/windows/desktop/api/processthreadsapi/nf-processthreadsapi-createthread)	

[百度百科](https://baike.baidu.com/item/CreateThread)

### 函数说明

在主线程的基础上创建一个新线程。

线程终止运行后，线程对象仍然在系统中，必须通过 `CloseHandle` 函数来关闭该线程对象。

### 语法说明

```c++
HANDLE CreateThread(
	LPSECURITY_ATTRIBUTES	lpThreadAttributes,
    SIZE_T					dwStackSize,
    LPTHREAD_START_ROUTINE	lpStartAddress,
    __drv_aliasesMem LPVOID	lpParameter,
    DWORD					dwCreationFlags,
    LPDWORD					lpTreadId
);
```

### 参数说明

+ `lpTreadAttributes`：指向 `SECURITY_ATTRIBUTES` 结构的指针，该结构确定子进程是否可以继承返回的句柄。
+ `dwStackSize`：堆栈的初始大小，以字节为单位。
+ `lpStartAddress`：指向由线程执行的应用程序定义的函数的指针。该指针表示线程的起始地址。
+ `lpParameter`：指向要要传递给线程的变量的指针。
+ `dwCreationFlags`：控制线程创创建的标志。
  + `0`：该线程在创建后立即执行。
  + `CREATE_SUSPENDED`：线程是在挂起状态下创建的，并且在调用 `ResumeThread` 函数之前不会运行
  + `STACK_SIZE_PARAM_IS_A_RESERVATION`：所述 `dwStackSize` 数指定堆栈的初始保留大小。如果未指定此标志，则 `dwStackSize` 指定提交大小。

### 返回说明

如果函数成功，则返回值时新线程的句柄。

如果函数失败，则返回值为 `NULL` 。



## TerminateProcess - 终止指定进程

### 文档地址

[Microsoft 官方手册](https://docs.microsoft.com/zh-cn/windows/desktop/api/processthreadsapi/nf-processthreadsapi-terminateprocess)

[百度百科](https://baike.baidu.com/item/TerminateProcess)

### 函数说明

终止指定的进程及其所有线程。

### 语法说明

```c++
BOOL TerminateProcess(
	HANDLE 	hProcess,
    UINT	uExitCode,
);
```

### 参数说明

+ `hProcess`：要终止的进程的句柄。句柄必须具有 `PROCESS_TERMINATE` 访问权限，
+ `uExitCode`：进程和由此调用而终止的线程要使用的退出代码，使用 `GetExitCodeProcess` 函数检索进程的退出值。使用 `GetExitCodeThread` 函数检索线程的退出值

### 返回说明

如果函数成功，则返回值为非零值

如果函数失败，则返回值为零。要获取扩展错误信息，请调用 `GetLastError` 函数



## FindWindow - 窗口检索

### 文档地址

[Microsoft 官方手册](https://docs.microsoft.com/en-us/windows/desktop/api/winuser/nf-winuser-findwindowa)

[百度百科](https://baike.baidu.com/item/findwindow)

### 函数说明

检索顶级窗口的句柄，该窗口的类名称和窗口名称与指定的字符串匹配。

次功能不区分大小写



该函数不索引子窗口。

如果要搜索子窗口，从指定的子窗口开始，请使用 `FindWindowEx` 函数

### 语法说明

```c++
HWND FindWindow(
	LPCSTR	lpClassName,
    LPCSTR	lpWindowName,
);
```

### 参数说明

+ `lpClassName`：要索引的窗口类名
+ `lpWindowName`：要索引的窗口标题

### 返回说明

如果函数成功， 则返回值时具有指定类目和窗口名的窗口的句柄

如果函数是失败，则返回值为 `NULL`。



## IsWindow - 判断窗口句柄是否有效

### 文档地址

[Microsoft 官方手册](https://docs.microsoft.com/zh-cn/windows/win32/api/winuser/nf-winuser-iswindow)

[百度百科](https://baike.baidu.com/item/IsWindow)

### 函数说明

确定指定的窗口句柄是否有效

### 语法说明

```c++
BOOL IsWindow(
	HWND hWnd
);
```

### 参数说明

+ `hWnd`：要测试的窗口的句柄

### 返回说明

类型：`BOOL`

如果窗口句柄有效，则返回值为非零

如果窗口句柄无效，则返回值为零。



## SendMessage -像指定窗帘发送消息

### 文档地址

[Microsoft 官方手册](https://docs.microsoft.com/en-us/windows/desktop/api/winuser/nf-winuser-sendmessage)

[百度百科](https://baike.baidu.com/item/SendMessage)

### 函数说明

将指定的消息发送到一个或多个窗口。

此函数为指定的窗口调用窗口程序，直到窗口程序处理完消息再返回。

而和函数 `PostMessage` 不同，`PostMessage` 是将一个消息寄存到一个线程的消息队列后就立即返回

`SendMessage` 是等消息执行完后返回， `PostMessage` 是将消息寄送到线程队列后就立即返回。

### 语法说明

```c++
LRESULT SendMessage(
	HWND	hWnd,
    UINT	Msg,
    WPARAM	wParam,
    LPARAM	lParam
);
```

### 参数说明

+ `hWnd`：窗口过程将接收消息的窗口句柄。如果此参数为 `HWND_BROADCAST（（HWND）0xffff）` 则会将消息发送到系统中的所有顶级窗口，包括禁用或隐藏的无主窗口，重叠窗口和弹出窗口；但消息不会发送到子窗口。
+ `Msg`：要发送的消息
+ `wParam`：其他特定于消息的信息。
+ `lParam`：其他特定于消息的信息。

### 返回说明

返回值指定消息处理的结果，依赖于所发送的消息。



## PostQuitMessage - 发送终止请求

### 文档地址

[Microsoft 官方手册](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-postquitmessage)

[百度百科](https://baike.baidu.com/item/PostQuitMessage)

### 函数说明

该函数想系统指示线程已发出终止请求（退出）。

它通常用于响应 `WM_DESTROY` 消息。

### 语法说明

```c++
void PostQuitMessage(int nExitCode);
```

### 参数说明

+ `nExitCode`：应用程序的退出代码。此值用作 `WM_QUIT` 消息的 `wParam` 参数。

### 返回说明

次函数不返回值



## DestroyWindow - 关闭窗口

### 文档地址

[Microsoft 官方手册](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-destroywindow)

[百度百科](https://baike.baidu.com/item/DestroyWindow)

### 函数说明

销毁指定的窗口。

这个函数通过发送 `WM_DESTROY` 消息和 `WM_NCDESTROY` 消息使窗口无效并移除其键盘焦点。

这个函数还销毁窗口的菜单，清空线程的消息队列，销毁与窗口过程相关的定时器，检出窗口对剪贴板的拥有权，打断剪贴板器的查看链接

如果指定的窗口使父窗口或所有者窗口，则该函数会在销毁父窗口或所有者窗口时，自动销毁关联的子窗口或拥有的窗口。

该函数首先销毁子窗口或拥有窗口，然后它会破坏父窗口或所有者窗口。

### 语法说明

```c++
BOOL DestroyWindow(HWND hWnd);
```

### 参数说明

+ `hWnd`：要破坏的窗口句柄。

### 返回说明

返回类型：`BOOL`

如果函数成功，则返回值为非零值

如果函数失败，则返回值为零。如果要获取扩展错误信息，请调用 `GetLastError` 函数



## LoadMenu - 加载指定的菜单资源

### 文档地址

[Microsoft 官方手册](https://docs.microsoft.com/zh-cn/windows/win32/api/winuser/nf-winuser-loadmenua)

[百度百科](https://baike.baidu.com/item/LoadMenu)

### 函数说明

从与应用程序实例相关联的可执行文件（.exe）中加载指定的菜单资源。

### 语法说明

```c++
HMENU LoadMenu(HINSTANCE hInstance, LPCSTR lpMenuName);
```

### 参数说明

+ `hInstanc`：包含要加载的菜单资源的模块句柄。若此参数为 `NULL` ，则从当前示例中加载菜单。
+ `lpMenuName`：指向含有菜单资源名的以空结束的字符串指针。同时，次参数可由低位字上的资源标识符和高位字上的零组成。注意，菜单ID（所有资源ID类同）也可以直接由字符串构造，如ID值 `IDR_TYPER` 。

### 返回说明



## InvalidateRect - 使客户区失效

### 文档地址

[Microsoft 官方手册](https://docs.microsoft.com/zh-cn/windows/win32/api/winuser/nf-winuser-invalidaterect)

[百度百科](https://baike.baidu.com/item/InvalidateRect)

### 函数说明

该函数向指定的窗体更新区域添加一个矩形，然后窗口客户区域的这一部分将被重新绘制

### 语法说明

```c++
BOOL InvalidateRect(
	HWND	hWnd,
    const RECT *lpRect,
    BOOL	bErase
);
```

### 参数说明

+ `hWnd`：要更新的客户区所在的窗体的句柄。如果为 `NULL` ，则系统将在函数返回前重新绘制所有的窗口，然后发送 `WM_REASEBKGND` 和 `WM_PAINT` 给窗口过程处理函数。
+ `lpRect`：无效区域的矩形代表，它是一个结构体指针，存放至矩形的大小。如果为 `NULL` ，全部的窗口客户区域将被增加到更新区域中。
+ `bErase`：指定在处理更新区域时是否要擦除更新区域内的背景。如果次参数为 `TRUE`，则在调用 `BeginPaint` 函数时将删除背景。如果此参数为FALSE，则背景保持不变。

### 返回说明

如果函数成，则返回值为非零值。

如果函数失败，则返回值为零。



## BeginPaint - 准备指定窗口进行绘画填充

### 文档地址

[Microsoft 官方手册](https://docs.microsoft.com/zh-cn/windows/win32/api/winuser/nf-winuser-beginpaint)

[百度百科](https://baike.baidu.com/item/BeginPaint)

### 函数说明

该函数为指定窗口进行绘图工作的准备，并用将和绘图有关的信息填充到一个 `PAINTSTRUCT` 结构中

### 语法说明

```c++
HDC BeginPaint(
	HWND hWnd,
    LPPAINTSTRUCT lpPaint
);
```

### 参数说明

+ `hWnd`：被重绘的窗口句柄
+ `lpPaint`：指向一个用来接收绘画信息的 `PAINTSTRUCT` 结构

### 返回说明

如果函数成功，则返回值是指定窗口的显示设备上下文的句柄。

如果函数失败，则返回值为 `NULL` ，表示没有可用的显示设备上下文。



## EndPaint - 绘画结束

### 文档地址

[Microsoft 官方手册](https://docs.microsoft.com/zh-cn/windows/win32/api/winuser/nf-winuser-endpaint)

[百度百科](https://baike.baidu.com/item/EndPaint)

### 函数说明

该函数标记指定窗口的绘图过程结束；这个函数在每次调用 `BeginPaint` 函数之后被请求，但仅在绘图完成以后

### 语法说明

```c++
BOOL EndPaint(
	HWND hWnd,
    const PAINTSTRUCT *lpPaint
);
```

### 参数说明

+ `hWnd`：处理已重新绘制的窗口
+ `lpPaint`：指向 `PAINTSTRUCT` 结构的指针，该结构包含 `BeginPaint` 检索的绘制信息。

### 返回说明

返回值始终未非零值。



## TextOut - 将一个字符串写到指定位置。

### 文档地址

[Microsoft 官方手册](https://docs.microsoft.com/zh-cn/windows/win32/api/wingdi/nf-wingdi-textouta)

[百度百科](https://baike.baidu.com/item/TextOut)

### 函数说明

该函数使用当前选择的字体、背景颜色和正文颜色将一个字符串写到指定的位置上。

### 语法说明

```c++
BOOL TextOut(
	HDC hdc,
    int x,
    int y,
    LPCSTR lpString,
    int c
);
```

### 参数说明

+ `hdc`：设备上下文的句柄。
+ `x`：系统用于对齐字符串的参考点的逻辑坐标中的 `x` 坐标。
+ `y`：系统用于对齐字符串的参考点的逻辑坐标中的 `y` 坐标。
+ `lpString`：只想要绘制的字符串的指针。字符串不需要以 `\0` 结尾，因为 `c` 参数中指定了字符串的长度。
+ `c`：指定了字符串的长度。

### 返回说明

如果喊出成功，则返回值为非零值。

如果函数失败，则返回值为零。



