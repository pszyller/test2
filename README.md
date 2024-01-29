Add-Type @"
using System;
using System.Runtime.InteropServices;
public class Window {
    [DllImport("user32.dll")]
    [return: MarshalAs(UnmanagedType.Bool)]
    public static extern bool SetWindowPos(IntPtr hWnd, IntPtr hWndInsertAfter, int X, int Y, int cx, int cy, uint uFlags);
    
    public static readonly IntPtr HWND_TOPMOST = new IntPtr(-1);
    public static readonly uint SWP_NOSIZE = 0x0001;
    public static readonly uint SWP_NOMOVE = 0x0002;
}
"@

$process = Start-Process "notepad.exe" -PassThru
$process.WaitForInputIdle()

[Window]::SetWindowPos($process.MainWindowHandle, [Window]::HWND_TOPMOST, 0, 0, 0, 0, [Window]::SWP_NOMOVE -bor [Window]::SWP_NOSIZE)
