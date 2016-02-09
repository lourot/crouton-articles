# Simulate middle mouse click in emacs on crouton

> // `crouton` `emacs` `ubuntu` `chromebook` `chrome os`

[Crouton](https://github.com/dnschneid/crouton) is a nice set of scripts for running Ubuntu on a
Chromebook. These notebooks have no middle mouse button. If you also run emacs in crouton, this is
for you.

This article describes how to simulate a middle mouse click in emacs in order to paste the
[primary clipboard](https://wiki.archlinux.org/index.php/Clipboard).

## Map `alt` + `click` to middle button in crouton

[This official wiki page](https://github.com/dnschneid/crouton/wiki/Keyboard#map-altclick-to-middle-button)
describes how `alt` + `click` can be mapped to the middle button in crouton:

1) Install `xdotool` in your chroot:

```bash
# apt-get install xdotool
```

2) Create in your chroot `~/.xbindkeysrc.scm` containing:

```
; Map Alt+click to middle button
; Map on Release so that it does not appear both buttons are pressed
(xbindkey '(release alt "b:1") "xdotool click --clearmodifiers 2")
```

3) Restart your chroot.

### Explanation

In each chroot, crouton starts for you [Xbindkeys](https://wiki.archlinux.org/index.php/Xbindkeys)

```bash
$ ps aux | grep xbind
/usr/bin/xbindkeys -f /home/lourot/.xbindkeysrc.scm
```

and you've just told it to use [xdotool](https://github.com/jordansissel/xdotool) to simulate
another key when releasing `alt` + `click`.

## Configure emacs

Unfortunately with the previous trick, emacs hears `alt` + `middle-click` instead of just
`middle-click`. Therefore we need to make `alt` + `middle-click` paste the primary clipboard in
emacs. To do so, add the following line to your emacs configuration and restart it:

```
(global-set-key (kbd "<M-mouse-2>") 'mouse-yank-primary)
```

For other emacs configuration tips for crouton, see
[crouton-emacs-conf](https://github.com/AurelienLourot/crouton-emacs-conf).

---

> // http://lourot.com/articles/crouton-emacs-middle-click
