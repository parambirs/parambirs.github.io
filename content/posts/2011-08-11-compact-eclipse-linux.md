---
title: 'A compact Eclipse IDE on Linux'
date: 2011-08-11T00:00:00-07:00
tags: ["eclipse", "linux"]
summary: "GTK configuration for a more compact look for Eclipse on Linux."
---

Eclipse on Linux seems to take up a lot more space than it does on Windows/Mac. To fix this, save the following text in file (`~/.gtkrc-2.0`) and restart Eclipse. It should have a much more compact look than before:

```
style "gtkcompact" {
font_name="Sans 8"
GtkButton::default_border={0,0,0,0}
GtkButton::default_outside_border={0,0,0,0}
GtkButtonBox::child_min_width=0
GtkButtonBox::child_min_heigth=0
GtkButtonBox::child_internal_pad_x=0
GtkButtonBox::child_internal_pad_y=0
GtkMenu::vertical-padding=1
GtkMenuBar::internal_padding=0
GtkMenuItem::horizontal_padding=4
GtkToolbar::internal-padding=0
GtkToolbar::space-size=0
GtkOptionMenu::indicator_size=0
GtkOptionMenu::indicator_spacing=0
GtkPaned::handle_size=4
GtkRange::trough_border=0
GtkRange::stepper_spacing=0
GtkScale::value_spacing=0
GtkScrolledWindow::scrollbar_spacing=0
GtkExpander::expander_size=10
GtkExpander::expander_spacing=0
GtkTreeView::vertical-separator=0
GtkTreeView::horizontal-separator=0
GtkTreeView::expander-size=8
GtkTreeView::fixed-height-mode=TRUE
GtkWidget::focus_padding=0
}
class "GtkWidget" style "gtkcompact"

style "gtkcompactextra" {
xthickness=0
ythickness=0
}
class "GtkButton" style "gtkcompactextra"
class "GtkToolbar" style "gtkcompactextra"
class "GtkPaned" style "gtkcompactextra"
```
