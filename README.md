Add-Type -AssemblyName System.Windows.Forms
Add-Type -AssemblyName System.Drawing

$form = New-Object System.Windows.Forms.Form
$form.Text = "Text Overlay"
$form.TopMost = $true
$form.WindowState = [System.Windows.Forms.FormWindowState]::Maximized
$form.FormBorderStyle = [System.Windows.Forms.FormBorderStyle]::None
$form.BackColor = [System.Drawing.Color]::Black
$form.Opacity = 0.5
$form.TransparencyKey = $form.BackColor

$label = New-Object System.Windows.Forms.Label
$label.Text = "Hello, World!"
$label.Font = New-Object System.Drawing.Font("Arial", 48, [System.Drawing.FontStyle]::Bold)
$label.AutoSize = $true
$label.ForeColor = [System.Drawing.Color]::White
$label.BackColor = [System.Drawing.Color]::Transparent

$form.Controls.Add($label)

$form.Add_Shown({$form.Activate()})
[System.Windows.Forms.Application]::Run($form)
