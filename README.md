# John's dmenu

This is my build of suckless's [dmenu](https://tools.suckless.org/dmenu/).
I have applied the following patches:
* The [border](https://tools.suckless.org/dmenu/patches/border/) patch, to add a window border
* The [center](https://tools.suckless.org/dmenu/patches/center/) patch, which displays dmenu in the center of the screen
* The [grid](https://tools.suckless.org/dmenu/patches/grid/) patch, which adds the option of displaying items in a grid (or a vertical list)
* The [line height](https://tools.suckless.org/dmenu/patches/line-height/) patch, which adds the ability to specify a minimum line height
* The [password](https://tools.suckless.org/dmenu/patches/password/) patch, which adds a flag for censoring the input (e.g. for use if dmenu is used as a password prompt)
* The [vertfull](https://tools.suckless.org/dmenu/patches/vertfull/) patch, which tweaks item indentation in grid mode
* The [xresources](https://tools.suckless.org/dmenu/patches/xresources/) patch, which reads dmenu colors and fonts from Xresources

Additionally, I've made a couple tweaks on top of these patches:
* When the number of lines is set to be more than zero, the prompt is displayed on a separate line above the input field
* The `TEXTW` macro used to calculate input text width as it would be displayed on the screen has been tweaked to be more performant.
  For large input sequences (hundreds of lines), I noticed quite a bit of slowdown when using the center patch.
  This is due to the fact that this patch must compute the width of each line of text up-front to determine the geometry of the dmenu window.
  I have substituted `TEXTW`'s `drw_fontset_getwidth`-based implementation with one based on `strlen`, which is much faster.
  My implementation is less accurate, as it assumes all characters are the same width (and probably doesn't handle multi-byte unicode characters quite right either.
  However, I have found that a reasonable level of accuracy is maintained while the performance of this function has increased my many orders of magnitude.

To install, simply
```{bash}
cp config.def.h config.h
# Edit config.h to your liking
make
sudo make install
```
For some example dmenu-based scripts, see my [dotfiles](https://github.com/johnmastroberti/dotfiles) repo (scripts are located in `.local/bin/dmenu`).
