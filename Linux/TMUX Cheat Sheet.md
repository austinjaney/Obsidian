# TMUX Cheat Sheet
#sysadmin #linux 

Creates a new tmux session with a name

```
tmux new -s newsessionname
```

detaches the current open tmux session

```
control + b + d
```

lists the current tmux sessions

```
tmux ls
```

attaches a target tmux session

```
tmux a -t sessionname
```

shows a selector of current tmux sessions

```
control + b + s
```