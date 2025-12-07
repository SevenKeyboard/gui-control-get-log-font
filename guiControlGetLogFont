#Requires AutoHotkey v2.0.0+
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
    static _ := this._init()
    static _init()    {
        global
        GUICONTROLGETLOGFONT_VERSION := "1.0.0"
    }
}
guiControlGetLogFont(hWnd_or_guiObj, addTextTemporarily:=true)    {
    static WM_GETFONT:=0x31, SMTO_NORMAL:=0x0000
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
    hWnd:=0
    switch
    {
        case (hWnd_or_guiObj is gui):               hWnd:=hWnd_or_guiObj.Hwnd
        case (hWnd_or_guiObj is gui.control):       hWnd:=hWnd_or_guiObj.Hwnd
        default:
            if (hWnd_or_guiObj~="D)^-?(?:[[:digit:]]+|0[Xx][[:xdigit:]]+)$"
            && dllCall("User32.dll\IsWindow", "Ptr",hWnd_or_guiObj))
                hWnd:=hWnd_or_guiObj
    }
    if (!hWnd)
        return logFont
    if (!dllCall("User32.dll\SendMessageTimeout", "Ptr",hWnd, "UInt",WM_GETFONT, "Ptr",0, "Ptr",0, "UInt",SMTO_NORMAL, "UInt",50, "Ptr*",&hFont:=0, "Ptr"))
        return logFont
    if (!hFont)    {
        if (!addTextTemporarily)
            return logFont
        if !(hWnd_or_guiObj is gui)
            return logFont
        guiCntl:=hWnd_or_guiObj.add("Text","xp yp wp hp Hidden")
        if (!dllCall("User32.dll\SendMessageTimeout", "Ptr",hText:=guiCntl.hWnd, "UInt",WM_GETFONT, "Ptr",0, "Ptr",0, "UInt",SMTO_NORMAL, "UInt",50, "Ptr*",&hFont:=0, "Ptr"))
            return logFont
        dllCall("User32.dll\DestroyWindow", "Ptr",hText)
    }
    sizeLF:=dllCall("Gdi32.dll\GetObject", "Ptr",hFont, "Int",0, "Ptr",0)
    buf:=buffer(sizeLF, 0)
    dllCall("Gdi32.dll\GetObject", "Ptr",hFont, "Int",sizeLF, "Ptr",buf.Ptr)
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
    ,logFont.FaceName           := strGet(buf.Ptr+28)
    return logFont
}