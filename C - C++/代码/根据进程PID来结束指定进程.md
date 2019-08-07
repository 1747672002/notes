```c++
#include <Windows.h>
#include <iostream>
#include <tchar.h>

/* ******************************************
* 传递一个进程PID，将指定进程关闭
****************************************** */
BOOL TerminateProcessFromId(DWORD dwId)
{
	BOOL bRet = FALSE;

	// 打开目标进程，取得进程句柄
	HANDLE hProcess = ::OpenProcess(PROCESS_ALL_ACCESS, FALSE, dwId);

	if (hProcess != NULL)
	{
		// 终止进程
		bRet = ::TerminateProcess(hProcess, 0);
	}

	CloseHandle(hProcess);
	return bRet;
}

/* ******************************************
* 入口 main 函数
****************************************** */
int main(int argc, char*argv[])
{
	DWORD id = 7084;
	BOOL res = TerminateProcessFromId(id);
	return res;
}

```
