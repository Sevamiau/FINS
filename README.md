
---

#  **FISH INTERACTIVE NAVIGATION SCRIPT** 
**A Minimalist Terminal Productivity Suite for Linux**

###  Project Resume
**FINS** is a workflow enhancement that transforms the traditional Linux terminal into a high-speed, visual command center. Instead of manually typing long file paths or using `ls` repeatedly, **FINS** allows you to peek into your files with syntax highlighting before you even open them. 

While designed on Fedora Sway,**FINS** is shell-agnostic across the Linux ecosystem. As long as the Fish shell is present, your workflow remains identical.

**Key Benefits:**
*   **Instant Navigation:** Find any file in seconds using fuzzy logic (you don't need to remember exact names).
*   **Visual Previews:** See code, scripts, and text inside a "window" using `bat` (the modern `cat`).
*   **One-Key Actions:** Open your editor or run custom scripts with single-letter commands.

---

###  Step-by-Step Setup Guide

#### Step 1: Install the Core Four Dependencies
Open your terminal and install the necessary tools:
```bash
sudo dnf install fish 
sudo dnf isntall fzf 
sudo dnf install bat
sudo dnf install neovim
```

#### Step 2: Set Fish as your Default Shell
You need to be using the Fish shell. Run this to change your default shell:
```bash
chsh -s /usr/bin/fish
```

#### Step 3: Configure your Fish Shell
We need to tell Fish how to look beautiful and how to handle our new shortcuts. Open your config file:
```bash
nvim ~/.config/fish/config.fish
```
Paste the following block at the bottom:

```fish

# Set the look
set -gx FZF_DEFAULT_OPTS "
  --height 80% 
  --layout reverse 
  --border 
  --inline-info 
  --color='hl:yellow:bold,hl+:green:bold,fg+:white:bold,bg+:black:bold'
  --preview 'bat --color=always --style=numbers --line-range=:500 {}'
"

# Create the 'ff' command to search and open files in Neovim
function ff
    set -l file (fzf)
    if test -n "$file"
        nvim "$file"
    end
end

```


#### Step 5: Refresh and Use!
Reload your config:
```fish
source ~/.config/fish/config.fish
```

---

###  How to use **FINS**

1.  **Search & Edit:** Type `ff` in any folder. Use your arrow keys to scroll through files. You can also write the name or parts of it and it will appear with is preview.You will see a syntax-highlighted preview on the right. Press **Enter** to start editing in Neovim.
2.  **Generic Search:** Just type `fzf` to quickly find a filename and print it to the screen.

###  Tip for scripts:
If you have your **`run-qemu`** script in `~/.local/bin`, you can now run it from anywhere. You can even combine it with **FINS**:
```bash
ls ~/work/ | fzf | xargs -I {} run-qemu run {}
```
*This will let you visually pick which VM to start from a list!*
