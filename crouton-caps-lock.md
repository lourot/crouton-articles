# Map the Search key to Caps Lock in crouton

> // `crouton` `ubuntu` `chromebook` `chrome os`, `caps lock`, `linux`

[Crouton](https://github.com/dnschneid/crouton) is a nice set of scripts for running Ubuntu on a
Chromebook. These notebooks have a `Search` key in place of the usual `Caps Lock` key.

This article describes how to map the `Search` key to the `Caps Lock` function in crouton.

## Determining the current *keycode* and *keysym*

In your chroot, start `xev` and press the `Search` key:

```bash
$ xev | grep keycode
```

If you haven't installed the
[keyboard target](https://github.com/dnschneid/crouton/wiki/Keyboard#chromebook-keyboard-model) you
should see

```
    state 0x0, keycode 133 (keysym 0xffeb, Super_L), same_screen YES,
    state 0x40, keycode 133 (keysym 0xffeb, Super_L), same_screen YES,
```

which means that the key has the [keycode](https://wiki.archlinux.org/index.php/Xmodmap) `133` and
is associated to the [keysym](https://wiki.archlinux.org/index.php/Xmodmap) `Super_L`, which is the
function triggered by the `Windows` key on other keyboards.

All we now need to do is to associate that same *keycode* to the `Caps_Lock` *keysym*.

## Associating Caps_Lock *keysym* to *keycode* 133.

You can do so by typing

```bash
$ xmodmap -e "keycode 133 = Caps_Lock"
```

You can now add this line to any *autostart* script you have in place. Note that this command must
be issued after any `setxkbmap` command otherwise it will be overriden.

For the record here is the usual way.

## Performing association on X server's start

Create a file `~/.Xmodmap` containing:

```
keycode 133 = Caps_Lock
```

Add the following lines to your [.xinitrc](https://wiki.archlinux.org/index.php/Xinitrc):

```
if [-s ~/.Xmodmap]; then
    xmodmap ~/.Xmodmap
fi
```

## Remapping to Escape (for vim users)

Replace `Caps_Lock` with `Escape`:

```bash
$ xmodmap -e "keycode 133 = Escape"
```
---

> // http://lourot.com/articles/crouton-caps-lock
