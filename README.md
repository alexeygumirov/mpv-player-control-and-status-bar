# MPV Player scripts for Tiling Window Manager and Status Bar

Scrips for MPV player to be used with TWM and any status bar to play music, videos or Internet radio

[![Watch demo](https://github.com/alexeygumirov/mpv-player-control-and-status-bar/raw/refs/heads/main/demo/script_demo.png)](https://raw.githubusercontent.com/alexeygumirov/mpv-player-control-and-status-bar/main/demo/script_demo.mp4)

## Features

- Allows to play music, videos or Internet radio
    - Individual files or folders
- Uses Rofi and MPV player
- Status bar info
- Only few scripts, no Python
- Hot keys for play/pause, quit, next, previous, seek forward, seek backward, loop (file and playlist), shuffle.

## Prerequisites

- [MPV player](https://mpv.io/)
- [yt-dlp](https://github.com/yt-dlp/yt-dlp)
- Rofi
- [Nerd Fonts](https://www.nerdfonts.com/)
    - My scripts use `JetBrains Mono Nerd Font`. But you can change it to your preferred font.
- `notify-send` for notifications
- `jq` for JSON parsing
- `socat` for sending comamnds to MPV

## Scripts

- [`mpv-control`](./scripts/mpv-control): Main script to send commands to MPV player and to print status info.
- [`mpv-url-player`](./scripts/mpv-url-player): Script to play given URL. Primarily from the YouTube. But can be used for any URL from the source.
    - Script uses `yt-dlp` to get title of the played media stream.
- [`mpv-folder-player`](./scripts/mpv-folder-player): Script to play all music or video files from the given folder, or to play individual file (if file size is over 40MB).
- [`mpv-radio-player`](./scripts/mpv-radio-player): Script to play Internet radio stations. List of stations is stored in the [`mpv-radio-url-list`](./scripts/mpv-radio-url-list) file. File format is just comma separated values: `station name, URL`. Station line can be commented out with `#` symbol if you don't want to show them in the list.
    ```plaintext
    Chill Channel,https://cast.magicstreams.gr:2199/tunein/psychill.pls
    # Chillhop,http://stream.zeno.fm/fyn8eh3h5f8uv
    Box Lofi,http://stream.zeno.fm/f3wvbbqmdg8uv
    ```

## Key bindings example (for Xmonad)

I am using Xmoand tiling window manager. Here is part of my `xmonad.hs` configuration file with key bindings for the MPV player scripts.

```haskell
myKeys conf@(XConfig {XMonad.modMask = modm}) = M.fromList $
    -- launch a terminal
    [ ((modm, xK_Return), spawn $ XMonad.terminal conf)
    -- Bindings for MPV player
    , ((modm .|. shiftMask, xK_u), submap . M.fromList $
       [ ((0, xK_p), spawn "$HOME/.scripts/sh/mpv-control -playpause")
       , ((0, xK_g), spawn "$HOME/.scripts/sh/mpv-control -loop-file-toggle")
       , ((shiftMask, xK_g), spawn "$HOME/.scripts/sh/mpv-control -loop-playlist-toggle")
       , ((0, xK_j), spawn "$HOME/.scripts/sh/mpv-control -prev")
       , ((0, xK_h),  spawn "$HOME/.scripts/sh/mpv-control -seek-backward")
       , ((0, xK_k), spawn "$HOME/.scripts/sh/mpv-control -next")
       , ((0, xK_l),  spawn "$HOME/.scripts/sh/mpv-control -seek-forward")
       , ((0, xK_m), spawn "$HOME/.scripts/sh/mpv-control -shuffle")
       , ((0, xK_q), spawn "$HOME/.scripts/sh/mpv-control -quit")
       , ((0, xK_f), spawn "$HOME/.scripts/sh/mpv-folder-player")
       , ((0, xK_r), spawn "$HOME/.scripts/sh/mpv-radio-player")
       , ((0, xK_u), spawn "$HOME/.scripts/sh/mpv-url-player")
       ])
    ]
```

Bindings explained:
- `modm + Shift + u`, then:
    - `p`: Play/Pause
    - `g`: Enable/disable loop for file
    - `Shift + g`: Enable/disable loop for playlist
    - `j`: Previous
    - `h`: Seek backward
    - `k`: Next
    - `l`: Seek forward
    - `m`: Shuffle
    - `q`: Quit
    - `f`: Play music or video files from the folder. (call `mpv-folder-player` script)
    - `r`: Play Internet radio stations. (call `mpv-radio-player` script)
    - `u`: Play URL. (call `mpv-url-player` script)

