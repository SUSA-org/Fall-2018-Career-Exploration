# Using bash

When runing commands on another instance or computer, use these commands to make sure commands do not get executed after ending the terminal.

### Using nohup

This creates a file called FILE that stores the output of running the command.

```sh
$ nohup COMMAND > FILE
```
This saves output of the commadn into FILE, and immediately shuts the instance down. (Saves $$$ on AWS EC2 instances.)

```sh
$ nohup COMMAND > FILE && sudo halt/shutdown
```


### Using tmux 

Alternatively, use tmux. However, this does require asserting that tmux is already installed on your instance. Or installing using 

```sh
brew install tmux
```

This will run a given COMMAND in a tmux session called SESSION NAME (no spaces in the actual name). ^B-D will cleanly detach the tmux session, so that quitting the shell will not kill the command.

```sh
$ tmux new -s SESSION NAME
$ COMMAND
$ ^B-D
Session detached
```

To reattach to the session, and see the output, run

```sh
$ tmux attach SESSION NAME
```


