# ClickCatcher

Example plugin for x64dbg to handle mouse click events.

```c++
static DWORD WINAPI SelectionChanged(LPVOID)
{
    SELECTIONDATA sel;
    GuiSelectionGet(GUI_DUMP, &sel);
    _plugin_logprintf("[" PLUGIN_NAME "] WM_LBUTTONDOWN, Dump: %p\n", sel.start);
    return 0;
}

PLUG_EXPORT void CBWINEVENT(CBTYPE cbType, PLUG_CB_WINEVENT* info)
{
    auto msg = info->message;
    if (msg->message == WM_LBUTTONDOWN && info->result)
    {
        //Left mouse button down event, create new thread to not clog up the message loop
        CloseHandle(CreateThread(nullptr, 0, SelectionChanged, nullptr, 0, nullptr));
    }
}
```
