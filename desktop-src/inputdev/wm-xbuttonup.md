---
title: WM_XBUTTONUP message (Winuser.h)
description: Posted when the user releases either XBUTTON1 or XBUTTON2 while the cursor is in the client area of a window.
ms.assetid: ad726859-368a-4603-bffa-4e639bc69a6a
keywords:
- WM_XBUTTONUP message Keyboard and Mouse Input
topic_type:
- apiref
api_name:
- WM_XBUTTONUP
api_location:
- Winuser.h
api_type:
- HeaderDef
ms.topic: reference
ms.date: 08/23/2019
---

# WM\_XBUTTONUP message

Posted when the user releases either XBUTTON1 or XBUTTON2 while the cursor is in the client area of a window. If the mouse is not captured, the message is posted to the window beneath the cursor. Otherwise, the message is posted to the window that has captured the mouse.

A window receives this message through its [**WindowProc**](/windows/win32/api/winuser/nc-winuser-wndproc) function.


```C++
#define WM_XBUTTONUP                    0x020C
```



## Parameters

<dl> <dt>

*wParam* 
</dt> <dd>

The low-order word indicates whether various virtual keys are down. It can be one or more of the following values.



| Value                                                                                                                                                                                                               | Meaning                                     |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------|
| <span id="MK_CONTROL"></span><span id="mk_control"></span><dl> <dt>**MK\_CONTROL**</dt> <dt>0x0008</dt> </dl>    | The CTRL key is down.<br/>            |
| <span id="MK_LBUTTON"></span><span id="mk_lbutton"></span><dl> <dt>**MK\_LBUTTON**</dt> <dt>0x0001</dt> </dl>    | The left mouse button is down.<br/>   |
| <span id="MK_MBUTTON"></span><span id="mk_mbutton"></span><dl> <dt>**MK\_MBUTTON**</dt> <dt>0x0010</dt> </dl>    | The middle mouse button is down.<br/> |
| <span id="MK_RBUTTON"></span><span id="mk_rbutton"></span><dl> <dt>**MK\_RBUTTON**</dt> <dt>0x0002</dt> </dl>    | The right mouse button is down.<br/>  |
| <span id="MK_SHIFT"></span><span id="mk_shift"></span><dl> <dt>**MK\_SHIFT**</dt> <dt>0x0004</dt> </dl>          | The SHIFT key is down.<br/>           |
| <span id="MK_XBUTTON1"></span><span id="mk_xbutton1"></span><dl> <dt>**MK\_XBUTTON1**</dt> <dt>0x0020</dt> </dl> | The XBUTTON1 is down.<br/>      |
| <span id="MK_XBUTTON2"></span><span id="mk_xbutton2"></span><dl> <dt>**MK\_XBUTTON2**</dt> <dt>0x0040</dt> </dl> | The XBUTTON2 is down.<br/>     |



 

The high-order word indicates which button was released. It can be one of the following values:



| Value                                                                                                                                                                                                     | Meaning                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------|
| <span id="XBUTTON1"></span><span id="xbutton1"></span><dl> <dt>**XBUTTON1**</dt> <dt>0x0001</dt> </dl> | The XBUTTON1 was released.<br/>  |
| <span id="XBUTTON2"></span><span id="xbutton2"></span><dl> <dt>**XBUTTON2**</dt> <dt>0x0002</dt> </dl> | The XBUTTON2 was released.<br/> |



 

</dd> <dt>

*lParam* 
</dt> <dd>

The low-order word specifies the x-coordinate of the cursor. The coordinate is relative to the upper-left corner of the client area.

The high-order word specifies the y-coordinate of the cursor. The coordinate is relative to the upper-left corner of the client area.

</dd> </dl>

## Return value

If an application processes this message, it should return **TRUE**. For more information about processing the return value, see the Remarks section.

## Remarks

Windows supports mice with up to five buttons: left, middle, and right, plus two additional buttons called XBUTTON1 and XBUTTON2. The XBUTTON1 and XBUTTON2 buttons are often located on the sides of the mouse, near the base. These extra buttons are not present on all mice. If present, the XBUTTON1 and XBUTTON2 buttons are often mapped to an application function, such as forward and backward navigation in a Web browser.

Use the following code to get the information in the *wParam* parameter:


```
fwKeys = GET_KEYSTATE_WPARAM (wParam); 
fwButton = GET_XBUTTON_WPARAM (wParam); 
```



Use the following code to obtain the horizontal and vertical position:


```
xPos = GET_X_LPARAM(lParam); 
yPos = GET_Y_LPARAM(lParam); 
```



As noted above, the x-coordinate is in the low-order **short** of the return value; the y-coordinate is in the high-order **short** (both represent *signed* values because they can take negative values on systems with multiple monitors). If the return value is assigned to a variable, you can use the [**MAKEPOINTS**](/windows/desktop/api/wingdi/nf-wingdi-makepoints) macro to obtain a [**POINTS**](/windows/win32/api/windef/ns-windef-points) structure from the return value. You can also use the [**GET\_X\_LPARAM**](/windows/desktop/api/windowsx/nf-windowsx-get_x_lparam) or [**GET\_Y\_LPARAM**](/windows/desktop/api/windowsx/nf-windowsx-get_y_lparam) macro to extract the x- or y-coordinate.

> [!IMPORTANT]
> Do not use the [**LOWORD**](../winmsg/loword.md) or [**HIWORD**](../winmsg/hiword.md) macros to extract the x- and y- coordinates of the cursor position because these macros return incorrect results on systems with multiple monitors. Systems with multiple monitors can have negative x- and y- coordinates, and **LOWORD** and **HIWORD** treat the coordinates as unsigned quantities.

 

Unlike the [**WM\_LBUTTONUP**](wm-lbuttonup.md), [**WM\_MBUTTONUP**](wm-mbuttonup.md), and [**WM\_RBUTTONUP**](wm-rbuttonup.md) messages, an application should return **TRUE** from this message if it processes it. Doing so will allow software that simulates this message on Windows systems earlier than Windows 2000 to determine whether the window procedure processed the message or called [**DefWindowProc**](/windows/desktop/api/winuser/nf-winuser-defwindowproca) to process it.

## Requirements



| Requirement | Value |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------|
| Minimum supported client<br/> | Windows 2000 Professional \[desktop apps only\]<br/>                                                |
| Minimum supported server<br/> | Windows 2000 Server \[desktop apps only\]<br/>                                                      |
| Header<br/>                   | <dl> <dt>Winuser.h (include Windowsx.h)</dt> </dl> |



## See also

<dl> <dt>

**Reference**
</dt> <dt>

[**GET\_KEYSTATE\_WPARAM**](/windows/win32/api/winuser/nf-winuser-get_keystate_wparam)
</dt> <dt>

[**GET\_X\_LPARAM**](/windows/desktop/api/windowsx/nf-windowsx-get_x_lparam)
</dt> <dt>

[**GET\_XBUTTON\_WPARAM**](/windows/win32/api/winuser/nf-winuser-get_xbutton_wparam)
</dt> <dt>

[**GET\_Y\_LPARAM**](/windows/desktop/api/windowsx/nf-windowsx-get_y_lparam)
</dt> <dt>

[**GetCapture**](/windows/win32/api/winuser/nf-winuser-getcapture)
</dt> <dt>

[**SetCapture**](/windows/win32/api/winuser/nf-winuser-setcapture)
</dt> <dt>

[**WM\_XBUTTONDBLCLK**](wm-xbuttondblclk.md)
</dt> <dt>

[**WM\_XBUTTONDOWN**](wm-xbuttondown.md)
</dt> <dt>

**Conceptual**
</dt> <dt>

[Mouse Input](mouse-input.md)
</dt> <dt>

**Other Resources**
</dt> <dt>

[**MAKEPOINTS**](/windows/desktop/api/wingdi/nf-wingdi-makepoints)
</dt> <dt>

[**POINTS**](/windows/win32/api/windef/ns-windef-points)
</dt> </dl>

 

