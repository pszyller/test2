# Load the required .NET assembly
Add-Type -TypeDefinition @"
    using System;
    using System.Drawing;
    using System.Windows.Forms;
"@

# Capture the screen
$screen = [System.Windows.Forms.SystemInformation]::VirtualScreen
$bitmap = New-Object Drawing.Bitmap $screen.Width, $screen.Height
$graphics = [System.Drawing.Graphics]::FromImage($bitmap)
$graphics.CopyFromScreen($screen.Left, $screen.Top, 0, 0, $screen.Size)

# Specify the path and filename for the screenshot (change as needed)
$filePath = "C:\path\to\screenshot.png"

# Save the screenshot as an image file (PNG format)
$bitmap.Save($filePath, [System.Drawing.Imaging.ImageFormat]::Png)

# Clean up resources
$graphics.Dispose()
$bitmap.Dispose()

Write-Host "Screenshot saved to: $filePath"
