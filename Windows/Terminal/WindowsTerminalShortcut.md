# How to create a `shortcut key` to open a [windows terminal](https://github.com/microsoft/terminal)

## What is Windows Terminal?

`Windows terminal is a` relatively `new and very user-friendly CLI for windows 10` (especially if compared to the default one).
This program is open source and in continuous development, you can use it to run shells like powershell, wsl2 bash, azure cloud shell etc... and it's customizable to one's taste both aesthetically and functionally.

So if you are a CLI type of guy, you should definetly try it :smile:

## What's the standard behaviour?

Normally, to open the windows terminal you have to either click on the icon (if you have it on the desktop), type it's name in the windows application search bar or run it from the run box (win + r) etc...
But I find this tedious for a program that I want to use often such as the CLI.

## What's the desired behaviour?

I wanted a shortcut key that enabled me to both open a terminal and minimize it if it's already opened.

## How did I achieve that?

I found [this very useful article](https://blog.danskingdom.com/Bring-up-the-Windows-Terminal-in-a-keystroke/) by @deadlydog that explains it well. In short I've used a scripting language for windows called [AutoHotKey](https://www.autohotkey.com/) that can be compiled to become a *.exe file as well.

The code is the following:

```bash
; Reference to this code -> https://blog.danskingdom.com/Bring-up-the-Windows-Terminal-in-a-keystroke/
SwitchToWindowsTerminal()
{
  windowHandleId := WinExist("ahk_exe WindowsTerminal.exe")
  windowExistsAlready := windowHandleId > 0

  ; If the Windows Terminal is already open, determine if we should put it in focus or minimize it.
  if (windowExistsAlready = true)
  {
    activeWindowHandleId := WinExist("A")
    windowIsAlreadyActive := activeWindowHandleId == windowHandleId

    if (windowIsAlreadyActive)
    {
      ; Minimize the window.
      WinMinimize, "ahk_id %windowHandleId%"
    }
    else
    {
      ; Put the window in focus.
      WinActivate, "ahk_id %windowHandleId%"
      WinShow, "ahk_id %windowHandleId%"
    }
  }
  ; Else it's not already open, so launch it.
  else
  {
    Run, wt
  }
}

; Hotkey to use Ctrl+Shift+C to launch/restore the Windows Terminal.
^!t::SwitchToWindowsTerminal()
```

Again, what it does is simple:
It defines a shortcut key (in my case `ctrl + alt + t`) that launches and shows the windows terminal application on the screen as if you launched it by clicking on its icon in the task bar, and minimizes the active window if the GUI is already showing.

As I mentioned, the script can be compiled to a *.exe. That is useful if you want to run the script at every startup thus having the shortcut key always available.
To attain that, you have to:
1. find your startup folder by running the `shell:startup` command in the  in the `run box`(win + r), this will open the directory where to put the *.exe file that you previously created
2. put the *.exe file in there

And there you go, the shortcut key is set :grin: