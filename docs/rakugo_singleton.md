# Rakugo Singleton

Rakugo is automatically setup as a singleton when you turn on the Rakugo addon.
This means that you can call Rakugo from anywhere in your code.

# Full Exemple

Here is Full Exemple how can you make node connect to Rakugo Core that parse and execute RakuScript:

```gdscript
extends Node

const file_path = "res://Timeline.rk"

func _ready():
  Rakugo.parser_add_regex_at_runtime("HW", "^hello_world$")
	
  Rakugo.connect("parser_unhandled_regex", self, "_on_parser_unhandled_regex")
  Rakugo.connect("say", self, "_on_say")
  Rakugo.connect("step", self, "_on_step")
  Rakugo.connect("ask", self, "_on_ask")
  Rakugo.connect("menu", self, "_on_menu")
	
  Rakugo.parse_and_execute_script(file_path)

func _on_say(character:Dictionary, text:String):
  prints("say", character.get("name", "null"), text)

func _on_step():
  prints("Press 'Enter' to continue...")
	
func _on_ask(character:Dictionary, question:String, default_answer:String):
  prints("ask", character.get("name", "null"), question, default_answer)

func _on_menu(choices:Array):
  prints("menu", choices)
	
func _on_parser_unhandled_regex(key:String, result:RegExMatch):
  match(key):
    "HW":
      prints("regex hello, world !")

func _process(delta):
  if Rakugo.is_waiting_step() and Input.is_action_just_pressed("ui_accept"):
    Rakugo.do_step()
		
  if Rakugo.is_waiting_ask_return() and Input.is_action_just_pressed("ui_up"):
    Rakugo.ask_return("Bob")
		
  if Rakugo.is_waiting_menu_return() and Input.is_action_just_pressed("ui_down"):
    Rakugo.menu_return(0)
```

# References
## Signals
### say

params: (character:Dictionary, text:String)

Send when a [Say] instruction is executed then Rakugo waiting call of [do_step]

```gd
func _ready():
  Rakugo.connect("say", self, "_on_say")
  
func _on_say(character:Dictionary, text:String):
  prints("say", character.get("name", "null"), text)
```

### step

Send after a [Say] instruction is executed.

### ask

params: (character:Dictionary, question:String, default_answer:String)

Send when a [Ask] instruction is executed then Rakugo waiting call of [ask_return].

```gd
func _ready():
  Rakugo.connect("ask", self, "_on_ask")
  
func _on_ask(character:Dictionary, question:String, default_answer:String):
  prints("ask", character.get("name", "null"), question, default_answer)
```

### menu

params: (choices:Array)

Send when a [Menu] instruction is executed then Rakugo waiting call of [menu_return].

```gd
func _ready():
  Rakugo.connect("menu", self, "_on_menu")
  
func _on_menu(choices:Array):
  prints("menu", choices)
```

### execute_script_finished

args: (script_name:String)

Send when a script execution is finished.

```gd
func _ready():
  Rakugo.connect("execute_script_finished", self, "_on_execute_script_finished")
  
func _on_execute_script_finished(script_name:String):
  prints("execute_script_finished", script_name)
```

### parser_unhandled_regex

params: (key:String, result:RegExMatch)

## Methods

### set_variable

params: (var_name:String, value:Variant)

```gdscript
# set or create a global variable
Rakugo.set_variable("life", 5)

# set or create a variable on character
Rakugo.set_variable("Sy.life", 5)
```

### get_variable

params: (var_name:String)
return: Variant

```gdscript
# get a global variable
var life = Rakugo.get_variable("life")

# get a character's variable
var sy_life = Rakugo.get_variable("Sy.life")
```

### has_variable

params: (var_name:String)
return: bool

Only works with global variable.

### define_character

params: (char_tag:String, char_name:String)

```gdscript
# define/create a new character
Rakugo.define_character("Sy", "Sylvie")
```

### get_character

params: (char_tag:string)
return: Dictionary

If a character with this char_tag is not found return an empty Dictionary and print an error.

```gd
var sy = Rakugo.get_character("Sy")
```

### get_narrator

return: Dictionary

Returns character with name defined in **Project Settings**: *addons/rakugo/narrator/name*

### parser_add_regex_at_runtime

params: (key:String, regex:String)

### parse_script

params: (file_path:String)
return: Error

Parse a script and store it. You can execute it with [execute_script].

If all goes well return OK. If not print an error and return ERR_FILE_CANT_OPEN if file can not be opened, or FAILED in other cases.

```gd
const file_path = "res://Timeline.rk"

func _ready():
  Rakugo.parse_script(file_path)
```

### execute_script

params: (file_name:String)
return: Error

Execute a script previously registered with [parse_script].

If all goes well return OK, print an error and return FAILED instead.

file_name, is file_path without folder_path and extension.

file_path: "res://Timeline.rk"

file_name: "Timeline"

You can use file_path.get_file().get_basename().

You do not have to use parse_script and execute_script at same time.

```gdscript
const file_path = "res://Timeline.rk"
const file_name = file_path.get_file().get_basename()

func _ready():
  Rakugo.parse_script(file_path)

func _process(_delta):
  ...
  Rakugo.execute_script(file_name)
```

### parse_and_execute_script

params: (file_path:String)

Do [parse_script] and [execute_script] at same time.

### save_game

params: (save_name:String = "quick")

Save all variables and characters in *user://save/save_name/save.json* file.

#### load_game

params: (save_name:String = "quick")

Load all variables and characters from *user://save/save_name/save.json* file if existed.

#### is_waiting_step

return: bool

Returns true when Rakugo waiting call of [do_step].

### do_step

Use it when [is_waiting_step] return true, to continue script reading process.

### is_waiting_ask_return

return: bool

Returns true when Rakugo waiting call of [ask_return].

### ask_return

params: (result:Variant)

### is_waiting_menu_return

return: bool

Returns true when Rakugo waiting call of [menu_return].

### menu_return

params: (index:int)

[Say]: rakuscript.md#say
[do_step]: rakugo_singleton.md#do_step
[Ask]: rakuscript.md#ask
[ask_return]: rakuscript.md#ask_return
[Menu]: rakuscript.md#menu
[menu_return]: rakuscript.md#menu_return
[parse_script]: rakugo_singleton.md#parse_script
[execute_script]: rakugo_singleton.md#execute_script
[is_waiting_step]: rakugo_singleton.md#is_waiting_step
