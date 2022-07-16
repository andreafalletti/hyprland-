# Master layout
The master layout makes one window be the "master", taking the left part of the screen, and tiles the rest on the right.

# Quirks

The right, "slave" windows will always be split uniformly. You cannot change their size.

![master1](https://user-images.githubusercontent.com/43317083/179357849-321f042c-f536-44b3-9e6f-371df5321836.gif)

You can, however, resize the master window.

![master2](https://user-images.githubusercontent.com/43317083/179357863-928b0b5a-ff10-4edc-aa76-3ff88c59c980.gif)

# Config
*category name `master`*

`special_scale_factor=float` - (0.0 - 1.0) the scale of the special workspace windows

`new_is_master=bool` - whether a newly open window should replace the master or join the slaves.

`new_on_top=bool` - whether a newly open window should be on the top of the stack