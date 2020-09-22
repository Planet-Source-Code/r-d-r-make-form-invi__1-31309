<div align="center">

## Make form InVi$


</div>

### Description

This code will make your form transparent.

Please Vote
 
### More Info
 
Just add some controls(buttons,text boxes etc...)

Please Vote

Some experience with VB

Please Vote

All you will see is the controls on the form

Please Vote

You wont be able to see your form

Please Vote


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[§øûrÇÊ ÇödÊR](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/r-d-r.md)
**Level**          |Intermediate
**User Rating**    |3.3 (13 globes from 4 users)
**Compatibility**  |VB 6\.0
**Category**       |[Custom Controls/ Forms/  Menus](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/custom-controls-forms-menus__1-4.md)
**World**          |[Visual Basic](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/visual-basic.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/r-d-r-make-form-invi__1-31309/archive/master.zip)

### API Declarations

Yes look at code below Please Vote


### Source Code

```
'add this code to your module
Option Explicit
Private Declare Function CreateRectRgn Lib "gdi32" (ByVal X1 As Long, _
ByVal Y1 As Long, ByVal X2 As Long, ByVal Y2 As Long) As Long
Private Declare Function CombineRgn Lib "gdi32" (ByVal hDestRgn As _
Long, ByVal hSrcRgn1 As Long, ByVal hSrcRgn2 As Long, ByVal _
nCombineMode As Long) As Long
Private Declare Function SetWindowRgn Lib "user32" (ByVal hWnd As _
Long, ByVal hRgn As Long, ByVal bRedraw As Long) As Long
Public Sub TransparentForm(frm As Form)
  frm.ScaleMode = vbPixels
  Const RGN_DIFF = 4
  Const RGN_OR = 2
  Dim outer_rgn As Long
  Dim inner_rgn As Long
  Dim wid As Single
  Dim hgt As Single
  Dim border_width As Single
  Dim title_height As Single
  Dim ctl_left As Single
  Dim ctl_top As Single
  Dim ctl_right As Single
  Dim ctl_bottom As Single
  Dim control_rgn As Long
  Dim combined_rgn As Long
  Dim ctl As Control
  If frm.WindowState = vbMinimized Then Exit Sub
  ' Create the main form region.
  wid = frm.ScaleX(frm.Width, vbTwips, vbPixels)
  hgt = frm.ScaleY(frm.Height, vbTwips, vbPixels)
  outer_rgn = CreateRectRgn(0, 0, wid, hgt)
  border_width = (wid - frm.ScaleWidth) / 2
  title_height = hgt - border_width - frm.ScaleHeight
  inner_rgn = CreateRectRgn(border_width, title_height, wid - border_width, _
    hgt - border_width)
  ' Subtract the inner region from the outer.
  combined_rgn = CreateRectRgn(0, 0, 0, 0)
  CombineRgn combined_rgn, outer_rgn, inner_rgn, RGN_DIFF
  ' Create the control regions.
  For Each ctl In frm.Controls
    If ctl.Container Is frm Then
      ctl_left = frm.ScaleX(ctl.Left, frm.ScaleMode, vbPixels) _
        + border_width
      ctl_top = frm.ScaleX(ctl.Top, frm.ScaleMode, vbPixels) + title_height
      ctl_right = frm.ScaleX(ctl.Width, frm.ScaleMode, vbPixels) + ctl_left
      ctl_bottom = frm.ScaleX(ctl.Height, frm.ScaleMode, vbPixels) + ctl_top
      control_rgn = CreateRectRgn(ctl_left, ctl_top, ctl_right, ctl_bottom)
      CombineRgn combined_rgn, combined_rgn, control_rgn, RGN_OR
    End If
  Next ctl
  'Restrict the window to the region.
  SetWindowRgn frm.hWnd, combined_rgn, True
End Sub
'add this to your form
Private Sub Form_Resize()
  TransparentForm Me
End Sub
```

