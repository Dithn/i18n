# Azioni Launcher Desktop Linux Personalizzate

## Overview

On many Linux environments, you can add custom entries to the system launcher by modifying the `.desktop` file. For Canonical's Unity documentation, see [Adding Shortcuts to a Launcher](https://help.ubuntu.com/community/UnityLaunchersAndDesktopFiles#Adding_shortcuts_to_a_launcher). For details on a more generic implementation, see the [freedesktop.org Specification](https://specifications.freedesktop.org/desktop-entry-spec/1.1/ar01s11.html).

![audacious](https://help.ubuntu.com/community/UnityLaunchersAndDesktopFiles?action=AttachFile&do=get&target=shortcuts.png)

> NOTE: The screenshot above is an example of launcher shortcuts in Audacious audio player

To create a shortcut, you need to provide `Name` and `Exec` properties for the entry you want to add to the shortcut menu. Unity will execute the command defined in the `Exec` field after the user clicked the shortcut menu item. An example of the `.desktop` file may look as follows:

```plaintext
Azioni=AvviaPausa;Prossimo;Precedente

[Azione Desktop AvviaPausa]
Nome=Avvia-Pausa
Exec=audacious -t
MostraSoloIn=Unità;

[Azione Desktop Prossimo]
Nome=Prossimo
Exec=audacious -f
MostraSoloIn=Unità;

[Azione Desktop Precedente]
Nome=Precedente
Exec=audacious -r
MostraSoloIn=Unità;
```

The preferred way for Unity to instruct your application on what to do is using parameters. You can find them in your application in the global variable `process.argv`.