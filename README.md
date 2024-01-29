Add-Type @"
using System;
using System.Runtime.InteropServices;
public class Window {
    [DllImport("user32.dll")]
    [return: MarshalAs(UnmanagedType.Bool)]
    public static extern bool SetWindowPos(IntPtr hWnd, IntPtr hWndInsertAfter, int X, int Y, int cx, int cy, uint uFlags);

    public static readonly IntPtr HWND_NOTOPMOST = new IntPtr(-2);
    public static readonly uint SWP_NOSIZE = 0x0001;
    public static readonly uint SWP_NOMOVE = 0x0002;
}
"@

$process = Get-Process | Where-Object { $_.MainWindowTitle -like "*YourWindowTitle*" } | Select-Object -First 1
if ($process -ne $null) {
    [Window]::SetWindowPos($process.MainWindowHandle, [Window]::HWND_NOTOPMOST, 0, 0, 0, 0, [Window]::SWP_NOMOVE -bor [Window]::SWP_NOSIZE)
} else {
    Write-Host "Process with specified window title not found."
}
