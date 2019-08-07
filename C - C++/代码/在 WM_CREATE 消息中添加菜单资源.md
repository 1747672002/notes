首先需要创建好菜单资源

```c++
LRESULT CALLBACK MainWndProc(HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam)
{
    // 加载菜单资源
	HMENU hMenu = ::LoadMenu(::GetModuleHandle(NULL), (LPCTSTR)IDR_TYPER);
    
    switch(msg)
    {
        case WM_CREATE:
            {
                // 在指定窗口设置菜单资源
                ::SetMenu(hwnd, hMenu);
            }
            break;
        case WM_DESTROY:
            ...
    }
}
```
