---
title: Boost Command Line Productivity and Maintain Remote Sessions with tmux
date: 2023-01-23
categories: [linux,terminal]
tags: [productivity,tools,server]
---

# Boost Command Line Productivity and Maintain Remote Sessions with tmux

## What is a Terminal Multiplexer?
A terminal multiplexer, such as tmux, allows you to run multiple terminal sessions within a single terminal window. This means you can have multiple shells open, each with its own set of processes and command history, all within a single terminal window. This can be extremely useful for developers who often need to run multiple commands or processes at the same time.

## Getting Started with tmux
### Installation
```bash
sudo apt-get install tmux
```

### Start a new session
```bash
tmux
```

### Start a new named session
```bash
tmux new -s my_session
```

### List sessions
```bash
tmux ls
```

### Attach to the last created session
Either one of these options:
```bash
tmux a
tmux at
tmux attach
tmux attach-session
```

### Attach to a named session
```bash
tmux a -t my_session
```

### Kill a session
```bash
tmux kill-session -t myname
```

### Adding new windows
Press `CTRL+b` and then `c`. 

### Switching between windows
Use `CTRL+b` followed by `n` for the next window or `p` for the previous window. You can also use `CTRL+b` followed by 

### Splitting windows into panes
Press `CTRL+b` and then `%` for a vertical split or `"` for a horizontal split. You can then switch between panes using `CTRL+b` followed by the `arrow keys`.

### Detaching from a session
Press `CTRL+b` and then `d`.

Additionally, tmux can also be customized to suit your specific needs and preferences. There are many resources available online that provide tips and tricks for customizing tmux, such as customizing key bindings, creating custom scripts, and more.

## Using tmux when working with servers
It allows you to easily manage and switch between multiple terminal sessions within a single window, making it a great option for running multiple commands or processes on a server. Additionally, tmux's persistent sessions feature allows you to **keep your terminal sessions running even when you disconnect from the server**, so you can continue right where you left off. 

You can also configure your ssh to enter directly by tmux, all you have to do is:

- adding the following line to your `~/.bashrc` or `~/.bash_profile` file:
  ```bash
  alias ssh = "tmux new-session -A -s my_session"
  ```

- or adding the following to your `~/.ssh/config` file. In this example a proxy is used to access the server, and either a new session will be either or you will be attached to the one already existing with the specified name:
  ```bash
  Host server1
    ProxyCommand ssh -q server-proxy nc -q0 server-ip server-port  
    User user1
    RequestTTY yes
    RemoteCommand tmux attach -t my_session || tmux new -s my_session
  ```

  - You can keep your normal ssh configuration to access the same server:
    ```bash
    Host server1-no-tmux
      ProxyCommand ssh -q server-proxy nc -q0 server-ip server-port
      User user1
    ```

  This will create a new tmux session named "my_session" each time you connect to a server via ssh. This way, you will be able to access your previous sessions directly, without having to start a new one.

## Why tmux instead of other tools?
- tmux is more configurable and customizable.
- It also has better support for copy and paste, as well as the ability to split windows and panes.
- Additionally, tmux is more actively developed, with new releases and updates coming out regularly.

## Alternatives
- [Terminator](https://gnome-terminator.org/)
- [GNU Screen](https://www.gnu.org/software/screen/)
- [byobu](https://www.byobu.org/)

In conclusion, tmux is a powerful and versatile tool that can greatly enhance your productivity and workflow when working with servers or in general. Whether you're a developer, system administrator, or simply someone who frequently uses the command line, tmux is definitely worth considering as a tool to include in your day by day work.

# References:
- [Tmux Official Website](https://github.com/tmux/tmux/wiki)
- [Tmux Tutorial](https://www.sitepoint.com/tmux-a-simple-start/)
- [Tmux Cheat Sheet](https://tmuxcheatsheet.com/)
