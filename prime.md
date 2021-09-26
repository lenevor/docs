# Prime Console

- [Introduction](#introduction)
- [Writing Commands](#writing-commands)

<a name="introduction"></a>
## Introduction

Prime is the command line interface included in Lenevor. This file exists at the root of your application as the `prime` script and provides a number of helpful commands that can help you while build a specific action in your application. To view a list of all available `prime` commands, you may use the `list` command, as follows: 

    php prime list

<img src="https://drive.google.com/file/d/1D2cGwqXNziTEydV5F8_CjHg2_6-Rk1sc/view?usp=sharing" />

<a name="writing-commands"></a>
## Writing Commands

In addition to the commands provided with `Prime`, you may build your own custom commands. These commands are stored by default in the `App/Console/Commands` directory; however, you may choose the location you consider to be appropriate to store your commands, which can be loaded through the System `Autoloader` by the framework. 