# RakuScript

**RakuScript** is Rakugo Dialogue scripting language inspired by Ren'Py Scripting Language.

You can use with **RakuScriptDialogue** node or using:
```gdscript
Rakugo.parse_script("path/to/raku_script.rk")
```

## Full Example

## Character

```character [char_tag] [char_name]```

Equivalent in GDScript:
```gdscript
Rakugo.define_character(char_tag, char_name)
```
Create/Define a new character with this char_tag and char_name

##### Example

```character Gd Godot```

## Variable

```python
# set variable
[var_name] = [value]

# get variable
[other_var_name]

# get variable from character
[char_tag].[var_name]
```

Equivalent in GDScript:
```gdscript
# set variable
Rakugo.set_variable(var_name, value)

# get variable
Rakugo.get_variable(other_var_name)

 # get variable from character
Rakugo.get_variable(char_tag.var_name)
```

Create a new variable with this var_name and this value assigned.

A var_name should be 2 or more characters long ( [Issue is already opened and we work on fixing it](https://github.com/rakugoteam/Rakugo/issues/93) ).

If other_var_name or char_tag.var_name is defined, use value of other_var_name or char_tag.var_name.

If this variable already exist, value is replaced by new one.

##### Example

```python

# create / set variable
life = 5

# assign value form other variable
life = max_life

# assign value from character
life = Gd.max_life

```
## Character's variable

```python
# set variable
[char_tag].[var_name] = [value]

# get variable from character
[char_tag].[var_name]
```

Create a new variable with this var_name on character with this char_tag, and this value assigned.

If other_var_name or char_tag.var_name is defined, use value of other_var_name or char_tag.var_name.

If this variable already exist on this character, value is replaced by new one.

##### Example

```python
# create / set character variable
Gd.friendship = 5

# assign value from other variable
Gd.friendship = max_friendship

# assign value from character
Gd.friendship = Gd.max_friendship
```

## Say

```[char_tag] [String]```

Character with this char_tag say *String*

When Say is executed, a signal [say] is send with dictionary of data for the character with this char_tag and this String in parameter.

If no character with this char_tag is found, signal is send with an empty dictionary.

After Say is executed, Rakugo automatically waiting, it send a [step] signal.

##### Example

```python
Gd "Hello !"
```

### No character

```[String]```

Say *String*

When Say is executed, a signal [say] is send with empty dictionary and this String in parameter.

##### Example

```"Hello, world !"```

### Use variables

```<[var_name]> or <[char_tag].[var_name]>```

You can use variables in Say, Rakugo replace them by their values in String before send signal [say].

##### Example

```
"My name is <Gd.name>, and I have <life> point of life"
```

## Ask

```
[var_name] = ? [question]
```

```
[var_name] = ? [question] [placeholder]
```

```
[var_name] = ? [character_tag] [question] [placeholder]

```

It can be used get user input as string,
for example you can use it to ask user for a his/her/their name.

After a Ask is executed, Rakugo waiting call of [ask_return] signal:

```gdscript
Rakugo.ask_return(answer)
```

### Example

```
player.name = ? "What is your name ?"
player.age = ? "How old are you ?"
```

## Menu

It can be used to create a ui that switches between different branches
of dialogue tree.

**WIP**

After a Menu is executed, Rakugo waiting call of [ask_menu] signal:

```gdscript
Rakugo.ask_menu(branch_index)
```

## Jump

```
jump [label_name] or jump [menu_label_name]
```

It will jump dialogue branch with given label_name or menu_label_name.

### Example

```
jump start

jump shop_menu
```

### Jump If

```
jump [label_name] if [condition]
```

It will jump dialogue branch with given
label_name or menu_label_name if condition is true.

### Example

```
jump emily_date if emily.relationship >= 20
```


[#93]: https://github.com/rakugoteam/Rakugo/issues/93
[say]: rakugo_singleton.md#say-characterdictionary-textstring
[step]: rakugo_singleton.md#step
[Say]: rakuscript.md#say
[ask_return]: rakuscript.md#ask_return
[ask_menu]: rakuscript.md#ask_menu