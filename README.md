Add-Type @"
using System;
using System.Text;
using System.Collections.Generic;
using System.Runtime.InteropServices;

public class WindowsEnumerator {
    public delegate bool EnumWindowsProc(IntPtr hWnd, int lParam);

    [DllImport("user32.dll", CharSet = CharSet.Unicode)]
    public static extern int GetWindowText(IntPtr hWnd, StringBuilder strText, int maxCount);

    [DllImport("user32.dll", CharSet = CharSet.Unicode)]
    public static extern int GetWindowTextLength(IntPtr hWnd);

    [DllImport("user32.dll")]
    public static extern bool EnumWindows(EnumWindowsProc enumProc, IntPtr lParam);

    [DllImport("user32.dll")]
    public static extern bool IsWindowVisible(IntPtr hWnd);

    public static List<string> GetOpenWindows() {
        IntPtr lShellWindow = GetShellWindow();
        List<string> list = new List<string>();

        EnumWindows(delegate (IntPtr hWnd, int lParam) {
            if (hWnd == lShellWindow) return true;
            if (!IsWindowVisible(hWnd)) return true;

            int length = GetWindowTextLength(hWnd);
            if (length == 0) return true;

            StringBuilder builder = new StringBuilder(length);
            GetWindowText(hWnd, builder, length + 1);

            list.Add(builder.ToString());
            return true;
        }, IntPtr.Zero);

        return list;
    }

    [DllImport("user32.dll")]
    static extern IntPtr GetShellWindow();
}
"@

foreach ($window in [WindowsEnumerator]::GetOpenWindows()) {
    Write-Host $window
}
