# Tiny Menu #

This is meant to be used simiarly to the much more feature-rich "Hydra" package
where keys are "chained" to create groups of commands.  This package always
presents an interactive menu in the minibuffer, and can also generate a dynamic
menu of all available menus.

To use the tiny menu, simply define the `tiny-menu-items` variable and then map
keys to the items using the `tiny-menu-run-item` macro.

`tiny-menu-items` is an alist of "items" where each "item" is a menu of
available options.  A menu is a list whose car is the name of the menu and cdr
is a list of menu items.  Each menu item is a list with three elements: the raw
character code a user must press to select the item, the display name for the
item, and the function to be called when the item is selected.

For example, it might look like this:

```
'(("buffer-menu" ("Buffer operations"
                  ((?k "Kill" kill-this-buffer)
                   (?b "Bury" bury-buffer))))
  ("help-menu"   ("Help operations"
                  ((?f "Describe function" describe-function)
                   (?k "Describe key"      describe-key)))))
```

This value defines two menus, one named "buffer-menu" and another named
"help-menu".  You can now bind a key to display a menu of the menus by calling
`tiny-menu` with no arguments, but the prompt letters will simply be
alphabetical.  It is much more ergonomic to map specific keys to each of the
menus, which is simplified by using the `tiny-menu-run-item` macro, like this:

(define-key some-map (kbd "<key>") (tiny-menu-run-item "buffer-menu"))

The macro returns an anonymous interactive function suitable for key binding.

It may also be useful (though not required) to use a custom prefix key if all of
the menus are related.  This is covered in other Emacs documentation, but for
the sake of convenience, this is how you could do that:

```
(define-prefix-command tiny-menu-map)
(define-key tiny-menu-map
  (kbd "<menu-key-1>") (tiny-menu-run-item "menu-1"))
(define-key tiny-menu-map
  (kbd "<menu-key-2>") (tiny-menu-run-item "menu-2"))
(define-key global-map (kbd "<prefix>") tiny-menu-map)
```
