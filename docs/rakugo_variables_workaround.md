# Rakugo Variables Workaround

## Problem

Now there is problem that you need to use GDScript to change those vars,
as in RakuScript you can only overwrite them, but we working on it.
```renpy
a = 10
a += 1 # and other mod operations doesn't work - error
a = 11 # works
```

## Temporary Solution

*It is temporary as we think that we fix this incontinence in near feature.*

This example show solution on character variable `eve.relationship` but it can be used on any variable.
For this to work first you crate script called *Workarounds.gd* extends from `Node` and add to singletons.
To add it to singletons go to menu *Project -> Project Settings*, then to *Autoload* tab and add *Workarounds.gd* to them.

This is *Workarounds.gd* script:
```gd
extends Node

## This example singleton make you able to use line `eve.relationship++10` in RakuScript
## using this line will add 10 to eve.relationship

func _ready():
  Rakugo.parser_add_regex_at_runtime("AdvanceEveRelationship", "^eve.relationship++<value>{NUMERIC}$")
  Rakugo.sg_custom_regex.connect(_on_custom_regex) # I just notice this line is missing in our docs

func _on_custom_regex(key:String, result:RegExMatch):
  if key == "AdvanceEveRelationship":
    var eva_relationship = Rakugo.get_variable("eva.relationship")
    Rakugo.set_variable("eva.relationship", eva_relationship + float(result.get_string(value)))
```