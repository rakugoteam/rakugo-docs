# RakuScript

**RakuScript** is Rakugo Dialogue scripting language inspired by Ren'Py Scripting Language.

You can use with **RakuScriptDialogue** node or using:

```gdscript
Rakugo.parse_script("path/to/raku_script.rk")
```

## Character

=== "RakuScript"

    ```renpy
    character [char_tag] [char_name]
    ```

=== "GDScript"

    ```gdscript
    Rakugo.define_character(char_tag, char_name)
    ```

Create/Define a new character with this `char_tag` and `char_name`

### Character Example


```character Gd Godot```

## Variable

!!! note
    - Setting variable create a new variable with this var_name and this value assigned.

    - If this variable already exist, value is replaced by new one.

    - [**Read more about RakugoVars**](rkvars.md)

=== "RakuScript"

    ```renpy
    # set variable
    [var_name] = [value]

    # get variable
    [other_var_name]

    # get variable from character
    [char_tag].[var_name]
    ```

=== "GDScript"

    ```gdscript
    # set variable
    Rakugo.set_variable(var_name, value)

    # get variable
    Rakugo.get_variable(other_var_name)

    # get variable from character
    Rakugo.get_variable(char_tag.var_name)
    ```

### Variable Example


```renpy

# create / set variable
life = 5

# assign value form other variable
life = max_life

# assign value from character
life = Gd.max_life

```

## Character's variable

!!! note
    - Setting variable create a new variable with this var_name on character with this `char_tag`, and this value assigned.

    - If this variable already exist on this character, value is replaced by new one.

    - [**Read more about RakugoVars**](rkvars.md)

```renpy
# set variable
[char_tag].[var_name] = [value]

# get variable from character
[char_tag].[var_name]
```

### Character's variable example

```renpy
# create / set character variable
Gd.friendship = 5

# assign value from other variable
Gd.friendship = max_friendship

# assign value from character
Gd.friendship = Gd.max_friendship
```

## Say

!!! info Using double quotes

    Form version 2.2 you can use double quotes "it is a "really" good one" in any string in RakuScript.

```
[char_tag] [String]
```

Character with this char_tag say *String*

When Say is executed, a signal [sg_say] is send with dictionary of data for the character with this char_tag and this String in parameter.

If no character with this char_tag is found, signal is send with an empty dictionary.

After Say is executed, Rakugo automatically waiting, it send a [sg_step] signal.

```gdscript
if Rakugo.is_waiting_step():
    Rakugo.do_step()
```

### Say Example

```renpy
# Character with tag `Gd` says "Hello!"
Gd "Hello !"

# No character / narrator says "Hello!"
"Hello!"
```

Say *String*

!!! note
    When Say is executed, a signal [say] is send with empty dictionary and this String in parameter.

### Use variables

You can use variables in Say, Rakugo replace them by their values in String before send signal [say].

```renpy
# <[var_name]> or <[char_tag].[var_name]>
"My name is <Gd.name>, and I have <life> point of life"
```

## Ask

It can be used get user input as string,
for example you can use it to ask user for a his/her/their name.

```renpy
[var_name] = ? [question]
[var_name] = ? [question] [placeholder]
[var_name] = ? [character_tag] [question] [placeholder]
```
### Ask Example

```renpy
player.name = ? "What is your name ?"
player.age = ? "How old are you ?"
```
### Use variables

You can use variables in Ask, Rakugo replace them by their values in String before send signal [sg_ask].

```renpy
# <[var_name]> or <[char_tag].[var_name]>
player.name = ? "What is your name ?" "<default_player_name>"
```
After a Ask is executed, Rakugo waiting call of [ask_return]:

```gdscript
if Rakugo.is_waiting_ask_return():
    Rakugo.ask_return("Bob")
```

## Menu

It can be used to create a ui that switches between different branches of dialogue tree.

```renpy
menu menu_emily:
    "Talk with emily" > emily_talk
    "Wait"
    "Blop" > test_dialog
```

It must start it from menu menu_name:
`"Choice"` - is choice and `>` is used to jump to other dialog/label or menu.
Menu block ends with empty line.

After a Menu is executed, Rakugo waiting call of [menu_return]:

```gdscript
if Rakugo.is_waiting_menu_return():
    Rakugo.menu_return(branch_index)
```

### Use variables

You can use variables in Menu, Rakugo replace them by their values in String before send signal [sg_menu].

```renpy
# <[var_name]> or <[char_tag].[var_name]>

# player can change name of npc called by default emily
emily.name = ? "What is her name ?" "<emily.name>"

menu menu_emily:
    "Talk with <emily.name>" > emily_talk
    "Wait"
    "Blop" > test_dialog
```

## Jump

```renpy
jump [label_name] or jump [menu_label_name]
```

It will jump dialogue branch with given label_name or menu_label_name.

### Jump Example

```renpy
jump start

jump shop_menu
```

### Jump If

```renpy
jump [label_name] if [condition]
```

It will jump dialogue branch with given
label_name or menu_label_name if condition is true.

### Jump If Example

```renpy
jump emily_date if emily.relationship >= 20
```

[sg_say]: rakugo_singleton.md#sg_say
[sg_ask]: rakugo_singleton.md#sg_ask
[sg_menu]: rakugo_singleton.md#sg_menu
[sg_step]: rakugo_singleton.md#sg_step
[ask_return]: rakuscript.md#ask_return
[menu_return]: rakuscript.md#menu_return