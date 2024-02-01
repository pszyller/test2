# Load the required .NET assembly for screen capture
Add-Type -TypeDefinition @"
    using System;
    using System.Drawing;
    using System.Drawing.Imaging;
    using System.Runtime.InteropServices;
"@

# Function to capture the screen and save as an image
Function Capture-Screen {
    $screen = [System.Windows.Forms.Screen]::PrimaryScreen.Bounds
    $bitmap = New-Object Drawing.Bitmap $screen.Width, $screen.Height
    $graphics = [System.Drawing.Graphics]::FromImage($bitmap)

    $graphics.CopyFromScreen([System.Windows.Forms.Screen]::PrimaryScreen.Bounds.Left, [System.Windows.Forms.Screen]::PrimaryScreen.Bounds.Top, 0, 0, $screen.Size)
    
    $filePath = "C:\path\to\screenshot.png"
    $bitmap.Save($filePath, [System.Drawing.Imaging.ImageFormat]::Png)

    $graphics.Dispose()
    $bitmap.Dispose()

    Write-Host "Screenshot saved to: $filePath"
}

# Call the function to capture the screen
Capture-Screen
