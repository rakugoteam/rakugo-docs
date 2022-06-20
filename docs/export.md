# How to Setup Export for Godot project that use Rakugo

## HTML 5 Export

There is problem with HTML 5 export of Rakugo for Firefox: https://itch.io/post/5713938

We need to setup ***Variant**/Export Type* as *Threads* in HTML5 export settings.

![](https://user-images.githubusercontent.com/7337158/162470941-c30307a3-30a9-4ba0-8908-d7d9c0bde6b8.png)

## All Exports

For any type of export, we need add RakuScript  *`*.rk`* files to export settings as by default, only files not recognized by Godot are ignored.


![](https://user-images.githubusercontent.com/7337158/162471020-a9ae8269-6a49-4ab7-a960-79333f35f7c6.png)

