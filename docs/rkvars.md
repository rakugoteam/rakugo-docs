# How use Rakugo Variables

Rakugo Variables are called _RkVar_ for short.
They are global, and accessible from both RkScript and GDScript.

## RakuScript
You can define var in [RakuScript] file, like this:
```renpy
player.life = 10
```
You can access it in any RakuScript:
```renpy
narrator "You have <player.life> life points left"
```

You can change already existing var:
```renpy
player.life += 5
```
[Read more changing already existing var.]

## GDScript
In GDScript you can define RkVars like this:
```gdscript
Rakugo.set_variable("player.life", 10)
```
You can access it in any RakuScript:
```gdscript
Rakugo.get_variable("player.life")
```
You can change it:
```gdscript
var life = Rakugo.get_variable("player.life")
life += 5
Rakugo.set_variable("player.life", life)
```
You can display text with RkVar using Label like this:
```gdscript
expand Label

func update_text():
	var life =  Rakugo.get_variable("player.life")
	text = "You have %d life points left" % life

	# or one-line version:
	text = Rakugo.replace_variables("You have <player.life> life points left")
```

To auto-update text in label using `sg_variable_changed` signal:

```gdscript
expand Label

func _ready():
	Rakugo.sg_variable_changed.connect(update_text)

func update_text(var_name:String, value):
	if var_name == "player.life":
		text = text = "You have %d life points left" % value
		
		# or one-line version:
		text = Rakugo.replace_variables("You have <player.life> life points left")
```


[Read more changing already existing var.]: rakugo_variables_workaround.md
[Rakugo Singleton]: rakugo_singleton.md
[RakuScript]: rakuscript.md