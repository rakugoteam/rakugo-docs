# Rakugo Singleton

Rakugo is automatically setup as a singleton when you turn on the Rakugo addon.
This means that you can call Rakugo from anywhere in your code.

## Full Exemple

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

## References
### Signals
#### say (character:Dictionary, text:String)
send when a [Say] instruction is executed.

#### step
send after a [Say] instruction is executed.

Rakugo script reading process is stop, and expects call of [do_step].

#### ask (character:Dictionary, question:String, default_answer:String)
send when a [Ask] instruction is executed.

Rakugo script reading process is stop, and expects call of [ask_return].

#### menu (choices:Array)
send when a [Menu] instruction is executed.

Rakugo script reading process is stop, and expects call of [menu_return].
#### parser_unhandled_regex(key:String, result:RegExMatch)

### Functions

#### set_variable(var_name:String, value:Variant)

##### Example

```gdscript
# set or create a global variable
Rakugo.set_variable("life", 5)

# set or create a variable on character
Rakugo.set_variable("Sy.life", 5)
```

#### get_variable(var_name:String) -> Variant

##### Example
```gdscript
# get a global variable
var life = Rakugo.get_variable("life")

# get a character's variable
var sy_life = Rakugo.get_variable("Sy.life")
```

#### has_variable(var_name:String) -> bool

Only works with global variable.

#### define_character(char_tag:String, char_name:String)

##### Example

```gdscript
# define/create a new character
Rakugo.define_character("Sy", "Sylvie")
```

#### get_character(char_tag:string) -> Dictionary

#### get_narrator() -> Dictionary

Returns character with name defined in **Project Settings**: *addons/rakugo/narrator/name*

#### parser_add_regex_at_runtime(key:String, regex:String)

#### parse_script(file_path:String)
Parse a script and store it. You can execute it with [execute_script]

#### execute_script(file_name:String)
Execute a script previously registered with [parse_script].

file_name, is file_path without folder_path and extension.

You can use file_path.get_file().get_basename().

You do not have to use parse_script and execute_script at same time.

##### Exemple
```gdscript

const file_path = "res://Timeline.rk"

func _ready():
  Rakugo.parse_script(file_path)

func _process(_delta):
  ...
  Rakugo.execute_script(file_path.get_file().get_basename())
```

#### parse_and_execute_script(file_path:String)

Do [parse_script] and [execute_script] at same time.

#### save_game(save_name:String = "quick")

Save variables and characters in *user://save/save_name/save.json* file.

#### load_game(save_name:String = "quick")

Load variables and characters from *user://save/save_name/save.json* file if exist.

#### is_waiting_step() -> bool

Returns true when Rakugo script reading process is stop, and expects call of [do_step].

#### do_step()

Use it when [is_waiting_step] return true, to continue script reading process.

#### is_waiting_ask_return() -> bool

#### ask_return(result:Variant)

#### is_waiting_menu_return() -> bool

#### menu_return(index:int)

[Say]: rakuscript.md#say
[do_step]: rakugo_singleton.md#do_step
[Ask]: rakuscript.md#ask
[ask_return]: rakuscript.md#ask_returnresultvariant
[Menu]: rakuscript.md#menu
[menu_return]: rakuscript.md#menu_returnindexint
[parse_script]: rakugo_singleton.md#parse_script
[execute_script]: rakugo_singleton.md#execute_script
[is_waiting_step]: rakugo_singleton.md#is_waiting_step