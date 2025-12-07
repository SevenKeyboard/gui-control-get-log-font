#Requires AutoHotkey v1.1.0+
;==============================================================
; guiControlGetLogFont — Retrieve LOGFONT structure for a GUI or control
;
; GitHub: https://github.com/SevenKeyboard/gui-control-get-log-font
; Author: SevenKeyboard Ltd. (2025)
; License: The Unlicense
;
; Documentation / References:
;   Re: How to get the current Gui font w/o creating a control?:
;     https://www.autohotkey.com/boards/viewtopic.php?t=161#p309740
;   Re: Get Gui Color:
;     https://www.autohotkey.com/boards/viewtopic.php?t=13683#p257565
;==============================================================
class VersionManager_guiControlGetLogFont
{
    static _ := VersionManager_guiControlGetLogFont._init()
    _init()    {
        global
        GUICONTROLGETLOGFONT_VERSION := "1.0.0"
    }
}
guiControlGetLogFont(hWnd_or_guiName:="", addTextTemporarily:=true)    {
    static WM_GETFONT:=0x31, SMTO_NORMAL:=0x0000
    if (hWnd_or_guiName=="")
        hWnd_or_guiName:=A_DefaultGui
    logFont:={Height:""
        ,Width:""
        ,Escapement:""
        ,Orientation:""
        ,Weight:""
        ,Italic:""
        ,Underline:""
        ,StrikeOut:""
        ,CharSet:""
        ,OutPrecision:""
        ,ClipPrecision:""
        ,Quality:""
        ,PitchAndFamily:""
        ,FaceName:""}
    hWnd:=0, paramType:=""
    if (hWnd_or_guiName~="D)^-?(?:[[:digit:]]+|0[Xx][[:xdigit:]]+)$"
    && dllCall("User32.dll\IsWindow", "Ptr",hWnd_or_guiName))    {
        hWnd:=hWnd_or_guiName, paramType:="hWnd"
    }  else if !(hWnd_or_guiName~="\W")    {
        gui %hWnd_or_guiName%:+LastFoundExist
        if (hWnd:=winExist())    {
            paramType:="gui"
        }  else  {
            guiControlGet hWnd, Hwnd, % hWnd_or_guiName
            if (!ErrorLevel)
                paramType:="guiControl"
            else
                hWnd:=0
        }
        hWnd:=format("{:d}",hWnd)
    }
    if (!hWnd)
        return logFont
    if (!dllCall("User32.dll\SendMessageTimeout", "Ptr",hWnd, "UInt",WM_GETFONT, "Ptr",0, "Ptr",0, "UInt",SMTO_NORMAL, "UInt",50, "Ptr*",hFont:=0, "Ptr"))
        return logFont
    if (!hFont)    {
        if (!addTextTemporarily)
            return logFont
        if (paramType!=="gui")
            return logFont
        gui %hWnd_or_guiName%:Add, Text, xp yp wp hp Hidden hwndhText
        if (!dllCall("User32.dll\SendMessageTimeout", "Ptr",hText, "UInt",WM_GETFONT, "Ptr",0, "Ptr",0, "UInt",SMTO_NORMAL, "UInt",50, "Ptr*",hFont:=0, "Ptr"))
            return logFont
        dllCall("User32.dll\DestroyWindow", "Ptr",hText)
    }
    sizeLF:=dllCall("Gdi32.dll\GetObject", "Ptr",hFont, "Int",0, "Ptr",0)
    varSetCapacity(buf, sizeLF, 0)
    dllCall("Gdi32.dll\GetObject", "Ptr",hFont, "Int",sizeLF, "Ptr",&buf)
     logFont.Height             := numGet(buf, 0,"Int")
    ,logFont.Width              := numGet(buf, 4,"Int")
    ,logFont.Escapement         := numGet(buf, 8,"Int")
    ,logFont.Orientation        := numGet(buf, 12,"Int")
    ,logFont.Weight             := numGet(buf, 16,"Int")
    ,logFont.Italic             := numGet(buf, 20,"Char")
    ,logFont.Underline          := numGet(buf, 21,"Char")
    ,logFont.StrikeOut          := numGet(buf, 22,"Char")
    ,logFont.CharSet            := numGet(buf, 23,"Char")
    ,logFont.OutPrecision       := numGet(buf, 24,"Char")
    ,logFont.ClipPrecision      := numGet(buf, 25,"Char")
    ,logFont.Quality            := numGet(buf, 26,"Char")
    ,logFont.PitchAndFamily     := numGet(buf, 27,"Char")
    ,logFont.FaceName           := strGet(&buf+28)
    return logFont
}