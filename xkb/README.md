RATIONALE
=========

zxcvorak is a layout intended to provide a combination of the best elements of
qwerty along with the best elements of dvorak (in my opinion).

In particular, the dvorak home row `aoeuidhtns-` is well-balanced and the
grouping of English vowels has an appealing aesthetic. The cluster of punctuation
characters `',.` or `"<>` coincidentally makes for a great set of bindings in
window managers to control focus switching and cycling, as it can be operated
with one hand -- the same one that would otherwise only hit M-<TAB>.

Meanwhile, the qwerty layout works well with Emacs and GNU bindings, as it has
the `zxcvb` group in the place that the binding scheme presumes. Combinations
such as M-x and C-c are much more convenient to type in the qwerty group than
with the scattered dvorak positions. C-g -- a commonly used binding (particularly
when the user is transitioning to a new layout :-) is in arguably a better
position than in either of the original layouts.

Thus the "basic" zxcvorak variant: essentially beginning with a dvorak layout,
then applying a roughly minimal set of transpositions that preserves the home
row and the top-left punctuation cluster. 

The "hack" variant also adjusts the right-hand side punctuation positions, such
as parentheses and slashes to be somewhat more useful for hacking (with an
emphasis on Lisp). Parens, brackets and braces are reachable without the shift
modifier, and the transplanted characters are vaguely matched to their original
positions. Both slashes (forward and back) are on the same key. 


INSTALL
=======

*  Copy the `zxcvorak` file to /usr/share/X11/xkb/symbols/

*  Insert the `<layout>` snippet as shown below into /usr/share/X11/xkb/rules/evdev.xml and /usr/share/X11/xkb/rules/base.xml

   The segment should be added inside the `<layoutList>` element which is inside the `<xkbConfigRegistry>` root element.

    <xkbConfigRegistry>
    
      ...
    
      <layoutList>
    
        ...

        ====================== start below ======================
    
        <layout>
          <configItem>
            <name>zxcvorak</name>
            <shortDescription>zxcvorak</shortDescription>
            <description>zxcvorak = dvorak + qwerty</description>
            <languageList><iso639Id>eng</iso639Id></languageList>
          </configItem>
          <variantList>
            <variant>
              <configItem>
                <name>basic</name>
                <description>Basic</description>
              </configItem>
            </variant>
            <variant>
              <configItem>
                <name>hack</name>
                <description>Hacking</description>
              </configItem>
            </variant>
          </variantList>
        </layout>
    
        ====================== end above ========================

        ...
    
      </layoutList>
    
      ...
    
    </xkbConfigRegistry>

Note: You may only need to add the XML snippet to one of the two files, depending
on how your keyboard is installed in X11 and what driver it uses.

* Now you can enable a zxcvorak layout in your desktop environment via its settings utility.

* To enable the zxcvorak layout glabally in the login manager, edit its Xsetup script to invoke `setxkbmap zxcvorak`. For example, for KDM, edit `/etc/kde4/kdm/Xsetup` and append:
    [ -f /usr/share/X11/xkb/symbols/zxcvorak ] && setxkbmap zxcvorak



WARNING: XKB
============

Any changes to these XML files may get overwritten by package upgrades of
xkb-data or equivalent because XKB is ridiculously not modular. Currently the policy
upstream appears to be that custom keyboard layouts must be submitted to the
central project for approval and inclusion as an extra layout; and personal inventions
are generally not accepted. There is no sensible, modular way to add a custom layout
that can also be detected automatically by desktop environments such as KDE or Gnome.

We could use ~/.Xmodmap to blindly load a layout, but that may not play well with the
layout switcher and config applications.

We could also use dpkg-divert or similar to avoid having the XML file overwritten on
updates, but that is probably just as bad as allowing it to be overwritten as upgrades
will be lost silently. Probably the most reasonable working method is to manually
update the file as the package overwrites the file infrequently (and this could be
automated somewhat, perhaps to notify if not also to merge our changes). 

