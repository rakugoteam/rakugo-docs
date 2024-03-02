# RakuScriptDialogue


Extends **Node**


## Vars
 - [**raku_script**](#raku_script)
 - [**starting_label_name**](#starting_label_name)
 - [**auto_start**](#auto_start)

## Funcs
 - [**start_dialogue**()](#start_dialogue)
 - [**start_dialogue_from_label**(label_name : String)](#start_dialogue_from_label)

## Vars
### raku_script
*default value* : `String`

Simple node to load and run Raku Script

Raku Script (*.rk) to load and run

### starting_label_name
*default value* : `""`

Label to start raku_script when using start_dialogue()

### auto_start
*default value* : `false`

If true calls start_dialogue() when scene is ready


## Funcs
### start_dialogue()
Starts raku_script from start_dialogue_from_label 

### start_dialogue_from_label(label_name : String)
Starts raku_script from given label

