# wine-lightroom-patches

Wine patches for Lightroom Classic.

## 0001-win32u-no-window-size-clamp.patch

Fixes the Library filmstrip stopping after ~239 photos in big folders.

Wine limits window sizes to 32767 and positions to -32768..32767
(`dlls/win32u/window.c`). Lightroom's filmstrip is one wide child window that
scrolls by moving left, so it gets cut off and then stops moving. Windows 7+
doesn't have the size limit.

The patch removes both limits.

Tested with Wine 11.21 staging and Lightroom Classic 16.0.1 on a folder with
4661 photos. It's a workaround: a proper Wine fix probably only needs to drop
the position limit for child windows.

## Usage

```sh
patch -p1 < patches/0001-win32u-no-window-size-clamp.patch
```

Nix:

```nix
wine.overrideAttrs (old: {
  patches = (old.patches or [ ]) ++ [ ./patches/0001-win32u-no-window-size-clamp.patch ];
})
```

Found with help from an AI assistant, tested by hand.

License: LGPL-2.1-or-later (same as Wine).
