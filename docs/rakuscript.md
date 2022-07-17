# RakuScript

**RakuScript** is Rakugo Dialogue scripting language inspired by Ren'Py Scripting Language.

You can use with **RakuScriptDialogue** node or using:
```gdscript
Rakugo.parse_script("path/to/raku_script.rk")
```

## Character
```character [char_tag] [char_name]```

Create a new character with this char_tag and char_name

##### Example

```character Gd Godot```
## Variable
```[var_name] = [value] or [other_var_name] or [char_tag].[var_name]```

Create a new variable with this var_name and this value assigned.

A var_name should be 2 or more characters long ( [Issue is already opened and we work on fixing it](https://github.com/rakugoteam/Rakugo/issues/93) ).

If other_var_name or char_tag.var_name is defined, use value of other_var_name or char_tag.var_name.

If this variable already exist, value is replaced by new one.

##### Example

```life = 5```

```life = max_life```

```life = Gd.max_life```
## Character's variable
```[char_tag].[var_name] = [value] or [other_var_name] or [char_tag].[var_name]```

Create a new variable with this var_name on character with this char_tag, and this value assigned.

If other_var_name or char_tag.var_name is defined, use value of other_var_name or char_tag.var_name.

If this variable already exist on this character, value is replaced by new one.

##### Example

```Gd.friendship = 5```

```Gd.friendship = max_friendship```

```Gd.friendship = Gd.max_friendship```
## Say
```[char_tag] [String]```

Character with this char_tag say *String*

When Say is executed, a signal [say] is send with dictionary of data for the character with this char_tag and this String in parameter.

If no character with this char_tag is found, signal is send with an empty dictionary.

After Say is executed, Rakugo automatically waiting, it send a [step] signal.

##### Example

```Gd "Hello !"```
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

```"My name is <Gd.name>, and I have <life> point of life"```
## Ask
After a Ask is read, Rakugo automatically waiting a user interaction, so it send a step signal.

## Menu
After a Menu is read, Rakugo automatically waiting a user interaction, so it send a step signal.

## Jump
### Jump If
## Full Example

[#93]: https://github.com/rakugoteam/Rakugo/issues/93
[say]: rakugo_singleton.md#say-characterdictionary-textstring
[step]: rakugo_singleton.md#step
[Say]: rakuscript.md#say
[ask_return]: rakuscript.md#ask_return