## How to copy text from remote tmux to local clipboard
There are two posible methods: https://github.com/tmux/tmux/wiki/Clipboard#the-clipboard
- OSC 52 and the set-clipboard option.
- Piping to an external tool like xsel

### Using external tools: genome-terminal dont support the OSC 52 escape sequence
The available tools are:
- On Linux and *BSD, there are the xsel(1) and xclip(1) tools, usually available as packages.
- macOS has a builtin tool called pbcopy(1).

These tools talk to the X(7) server (or equivalent) directly so without additional configuration they **only work on the local computer**
So copy text from ssh remote tmux, do below additional configuration
#### Bash script, `tmux-copy.sh`
#!/bin/bash
mkdir -p ~/tmp
tee ~/tmp/tmux-clipboard.txt | xclip -in -selection clipboard > /dev/null

#### tmux bind command - be sure ${_DOTFILES} is set, or use something else
bind -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel '${_DOTFILE}/tmux-copy.sh'
