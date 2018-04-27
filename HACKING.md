Building
========
For build instructions see the README.md

Pull requests
=============
Before filing a pull request run the tests:

    ninja -C _build test

Use descriptive commit messages, see

   https://wiki.gnome.org/Git/CommitMessages

and check

   https://wiki.openstack.org/wiki/GitCommitMessages

for good examples.

Coding Style
============
We mostly use kernel style but

* Use spaces, never tabs
* Use 2 spaces for inentation

GTK+ style function argument indentation
----------------------------------------
Use GTK+ style function argument indentation. It's harder for renames but it's
what GNOME upstream projects do.

*Good*:

    static gboolean
    button_clicked_cb (HdyDialerCycleButton *self,
                       GdkEventButton       *event)

*Bad*:

    static gboolean
    button_clicked_cb (HdyDialerCycleButton *self, GdkEventButton *event)


Braces
------
Everything besides functions and structs have the opening curly brace on the same line.

*Good*:

    if (i < 0) {
        ...
    }

*Bad*:

    if (i < 0)
    {
        ...
    }


Signals
-------
Prefix signal enum names with *SIGNAL_*.

*Good*:

    enum {
      SIGNAL_CYCLE_START = 0,
      SIGNAL_CYCLE_END,
      SIGNAL_LAST_SIGNAL,
    };

Also note that the last element ends with a comma to reduce diff noise when
adding further signals.


Properties
----------
Prefix property enum names with *PROP_*.

*Good*:

    enum {
      PROP_0 = 0,
      PROP_CYCLE_TIMEOUT,
      PROP_LAST_PROP,
    };

Also note that the last element ends with a comma to reduce diff noise when
adding further properties.

Comment style
-------------
In comments use full sentences and proper capitalization, punctation.

*Good*:

    /* Make sure we don't overflow. */

*Bad:*

    /* overflow check */


Callbacks
---------
Signal callbacks have a *_cb* suffix.

*Good*:

    g_signal_connect(self, "clicked", G_CALLBACK (button_clicked_cb), NULL);

*Bad*:

    g_signal_connect(self, "clicked", G_CALLBACK (handle_button_clicked), NULL);


Static functions
----------------
Static functions don't need the class prefix.  E.g. with a type foo_bar:

*Good*:

    static gboolean
    button_clicked_cb (HdyDialerCycleButton *self,
                       GdkEventButton       *event)

*Bad*:

    static gboolean
    foo_bar_button_clicked_cb (HdyDialerCycleButton *self,
                               GdkEventButton       *event)

Self argument
-------------
The first argument is usually the object itself so call it *self*. E.g. for a
non public function:

*Good*:

    static gboolean
    expire_cb (FooButton *self)
    {
      g_return_val_if_fail (BAR_IS_FOO_BUTTON (self), FALSE);
      ...
      return FALSE;
    }

And for a public function:

*Good*:

    gint
    foo_button_get_state (FooButton *self)
    {
      FooButtonPrivate *priv = bar_foo_get_instance_private(self);

      g_return_val_if_fail (BAR_IS_FOO_BUTTON (self), -1);
      return priv->state;
    }

User interface files
--------------------
User interface files should end in *.ui*. If there are multiple ui
files put them in a ui/ subdirectory below the sources
(e.g. *src/ui/main-window.ui*).

### Properties
Use minus signs instead of underscores in property names:

*Good*:

	<property name="margin-left">12</property>

*Bad":

	<property name="margin_left">12</property>