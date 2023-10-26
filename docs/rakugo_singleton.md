# Rakugo Singleton

Rakugo is automatically setup as a singleton when you turn on the Rakugo addon.
This means that you can call Rakugo from anywhere in your code.

## Full Exemple

Here is Full Exemple how can you make node connect to Rakugo Dialogue System that parse and execute RakuScript:

```gdscript
extends Node

const file_path = "res://Timeline.rk"

func _ready():
  Rakugo.add_custom_regex("HW", "^hello_world$")
  
  Rakugo.sg_custom_regex.connect(_on_custom_regex)
  Rakugo.sg_say.connect(_on_say)
  Rakugo.sg_step.connect(_on_step)
  Rakugo.sg_ask.connect(_on_ask)
  Rakugo.sg_menu.connect(_on_menu)
  
  Rakugo.parse_and_execute_script(file_path)

func _on_say(character:Dictionary, text:String):
  prints("say", character.get("name", "null"), text)

func _on_step():
  prints("Press 'Enter' to continue...")
  
func _on_ask(character:Dictionary, question:String, default_answer:String):
  prints("ask", character.get("name", "null"), question, default_answer)

func _on_menu(choices:Array):
  prints("menu", choices)
  
func _on_custom_regex(key:String, result:RegExMatch):
  prints("custom regex", key, result.strings)

func _process(delta):
  if Rakugo.is_waiting_step() and Input.is_action_just_pressed("ui_accept"):
    Rakugo.do_step()
    
  if Rakugo.is_waiting_ask_return() and Input.is_action_just_pressed("ui_up"):
    Rakugo.ask_return("Bob")
    
  if Rakugo.is_waiting_menu_return() and Input.is_action_just_pressed("ui_down"):
    Rakugo.menu_return(0)
```

## file_path and script_name

`file_path` is a path to a file, like : "res://Timeline.rk".

`script_name` is file name without extension. From "res://Timeline.rk" it is "Timeline".

It works in Rakugo like bellow :

```gd
var file_path = "res://Timeline.rk"

var script_name = file_path.get_file().get_basename()
```

Only methods [parse_script] or [parse_and_execute_script], use a file_path. For others and signals is a script_name.

## References

### Signals

#### sg_say

args: (character:Dictionary, text:String)

Send when a [Say] instruction is executed then Rakugo waiting call of [do_step]

```gd
func _ready():
  Rakugo.sg_say.connect(_on_say)
  
func _on_say(character:Dictionary, text:String):
  prints("say", character.get("name", "null"), text)
```

#### sg_step

Send after a [Say] instruction is executed.

```gd
func _ready():
  Rakugo.sg_step.connect(_on_step)

func _on_step():
  prints("Press 'Enter' to continue...")
```

#### sg_ask

args: (character:Dictionary, question:String, default_answer:String)

Send when a [Ask] instruction is executed then Rakugo waiting call of [ask_return].

```gd
func _ready():
  Rakugo.sg_ask.connect(_on_ask)
  
func _on_ask(character:Dictionary, question:String, default_answer:String):
  prints("ask", character.get("name", "null"), question, default_answer)
```

#### sg_menu

args: (choices:Array)

Send when a [Menu] instruction is executed then Rakugo waiting call of [menu_return].

```gd
func _ready():
  Rakugo.sg_menu.connect(_on_menu)
  
func _on_menu(choices:PackedStringArray):
  prints("menu", choices)
```

#### sg_execute_script_start

args: (script_name:String)

Send when the script with this `script_name` execution is started.

```gd
func _ready():
  Rakugo.sg_execute_script_start.connect(_on_execute_script_start)
  
func _on_execute_script_start(script_name:String):
  prints("execute_script_start", script_name)
```

#### sg_execute_script_finished

args: (script_name:String, error_str:String)

Send when the script with this `script_name` execution is finished.

If execution fail `error_str` contains error message, instead is empty.

```gd
func _ready():
  Rakugo.sg_execute_script_finished.connect(_on_execute_script_finished)
  
func _on_execute_script_finished(script_name:String, error_str:String):
  prints("execute_script_finished", script_name)
```

#### sg_custom_regex

args: (key:String, result:RegExMatch)

Send when a custom instruction is executed.

You can add one with [add_custom_regex].

```gd
func _ready():
  Rakugo.sg_custom_regex.connect(_on_custom_regex)

func _on_custom_regex(key:String, result:RegExMatch):
  match(key):
    "HW":
      prints("regex hello, world !")
```

#### sg_variable_changed

args : (var_name:String, value:Variant)

Send when a Rakugo's variable is changed.

#### sg_character_variable_changed

args : (character_tag:String, var_name:String, value:Variant)

Send when a Rakugo's character variable is changed.

### Methods

#### set_variable

params: (var_name:String, value:Variant)

```gdscript
## set or create a global variable
Rakugo.set_variable("life", 5)

## set or create a variable on character
Rakugo.set_variable("Sy.life", 5)
```

#### get_variable

params: (var_name:String)
return: Variant

Return `null` if not found or character with this tag does not exist. Print an error in this case.

```gdscript
## get a global variable
var life = Rakugo.get_variable("life")

## get a character's variable
var sy_life = Rakugo.get_variable("Sy.life")
```

#### has_variable

params: (var_name:String)
return: bool

Return `false` if not found or character with this tag does not exist. Print an error in this case.

```gdscript
## check if a global variable exists
var has_life = Rakugo.has_variable("life")

## check if a character's variable exists
var has_sy_life = Rakugo.has_variable("Sy.life")
```

#### define_character

params: (char_tag:String, char_name:String)

```gdscript
Rakugo.define_character("Sy", "Sylvie")
```

#### get_character

params: (char_tag:string)
return: Dictionary

If a character with this char_tag is not found return an empty Dictionary and print an error.

```gd
var sy = Rakugo.get_character("Sy")
```

#### get_narrator

return: Dictionary

Returns character with name defined in **Project Settings**: *addons/rakugo/narrator/name*

#### add_custom_regex

params: (key:String, regex:String)

Add new custom instruction to RkScript.

When this custom insctruction is read it send signal [sg_custom_regex].

Use it before [parse_script] or [parse_and_execute_script].

```gd
func _ready():
  Rakugo.parser_add_regex_at_runtime("HW", "^hello_world$")
  ...
  Rakugo.parse_and_execute_script(file_path)
```

You can use these Rakugo tokens in your regex :

```gd
NAME = "[a-zA-Z][a-zA-Z_0-9]*",
NUMERIC = "-?[1-9][0-9.]*",
STRING = "\".*\"",
```

Example : `^show_char (?<tag>{NAME})$`

GDocs about regex : https://docs.godotengine.org/en/stable/classes/class_regex.html

#### parse_script

params: (file_path:String)
return: Error

Parse a script and store it. You can execute it with [execute_script].

If all goes well return `OK`. If not print an error and return `ERR_FILE_CANT_OPEN` if file can not be opened, or `FAILED` in other cases.

```gd
const file_path = "res://Timeline.rk"

func _ready():
  Rakugo.parse_script(file_path)
```

#### execute_script

params: (script_name:String)
return: Error

Execute a script previously registered with [parse_script].

If all goes well return `OK`, print an error and return `FAILED` instead.

You do not have to use [parse_script] and [execute_script] at same time.

```gdscript
const file_path = "res://Timeline.rk"
const script_name = file_path.get_file().get_basename()

func _ready():
  Rakugo.parse_script(file_path)

func _process(_delta):
  ...
  Rakugo.execute_script(script_name)
```

#### parse_and_execute_script

params: (file_path:String)

Do [parse_script], if return `OK` then do [execute_script].

#### save_game

params: (save_name:String = "quick")

Save all variables, characters, script_name and last line readed on last executed script, in *user://save/save_name/save.json* file.

#### load_game

params: (save_name:String = "quick")

Load all variables, characters, script_name and last line readed on last executed script, from *user://save/save_name/save.json* file if existed.

Parse the script with `script_name`. To run it use [resume_loaded_script] (just bellow).

#### resume_loaded_script

Run the loaded script from last line readed.

#### is_waiting_step

return: bool

Returns `true` when Rakugo waiting call of [do_step].

#### do_step

Use it when [is_waiting_step] return `true`, to continue script reading process.

#### is_waiting_ask_return

return: bool

Returns `true` when Rakugo waiting call of [ask_return].

#### ask_return

params: (result:Variant)

Use it when [is_waiting_ask_return] return `true`, to continue script reading process.

`result` is the answer to the question ask by [sg_ask].

#### is_waiting_menu_return

return: bool

Returns `true` when Rakugo waiting call of [menu_return].

#### menu_return

params: (index:int)

Use it when [is_waiting_menu_return] return `true`, to continue script reading process.

`index` is the index of choosed choice in the choices array given by [sg_menu].

[Say]: rakuscript.md##say
[do_step]: rakugo_singleton.md##do_step
[Ask]: rakuscript.md##ask
[ask_return]: rakuscript.md##ask_return
[Menu]: rakuscript.md##menu
[menu_return]: rakuscript.md##menu_return
[parse_script]: rakugo_singleton.md##parse_script
[parse_and_execute_script]: rakugo_single##parse_and_execute_script
[execute_script]: rakugo_singleton.md##execute_script
[is_waiting_step]: rakugo_singleton.md##is_waiting_step
[sg_custom_regex]: rakugo_singleton.md##sg_custom_regex
[sg_ask]: rakugo_singleton.md##sg_ask
[is_waiting_ask_return]: rakugo_singleton.md##is_waiting_ask_return
[sg_menu]: rakugo_singleton.md##sg_menu
[is_waiting_menu_return]: rakugo_singleton.md##is_waiting_menu_return
[add_custom_regex]: rakugo_singleton.md##add_custom_regex
[resume_loaded_script]: rakugo_singleton.md##resume_loaded_script