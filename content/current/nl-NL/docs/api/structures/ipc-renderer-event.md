# IpcRendererEvent Object extends `Event`

* `afzender` IpcRenderer - De `IpcRenderer` instantie die de gebeurtenis oorspronkelijk heeft uitgezonden
* `senderId` Integer - De `webContents.id` die het bericht heeft verzonden, u kan `event.sender.sendTo(event.senderId, ...)` oproepen om het bericht te beantwoorden, zie [ipcRenderer.sendTo](#ipcrenderersendtowindowid-channel--arg1-arg2-) voor meer informatie. Dit geldt alleen voor berichten verzonden vanuit een verschillende renderer. Berichten rechtstreeks verzonden vanuit het hoofdproces stel `event.senderId` op `0`.
* `ports` MessagePort[] - A list of MessagePorts that were transferred with this message