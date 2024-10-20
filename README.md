# nvimconfig

Why you may want to learn neovim -> linkedin post.

This is my personal neovim configuration based on ThePrimeagen's video https://youtu.be/w7i4amO_zaE?si=MmjuNG6dObuE-CWc tutorial on neovim configuration.

It features harpoon, telescope, mason lsp, fuzzy finder, & more (link everything later).


## Setup
Assume debian based distro ie ubuntu
```sudo apt-get neovim
cd 
cd .config/
mkdir nvim/
cd nvim
git clone repo
# --optional if you need to work in python
python3 -m venv lspvenv
source lspvenv/bin/activate
pip install <your-packages>
# --replace <your-packages> with whatever you need, this environment is the one that neovim uses for syntax correction

nvim .
:PackerSync
:q
nvim .
```

Neovim is now ready to be used. Type :Tutor and follow its instructions. 

At the beginning it's going to be extremely cumbersome to learn how to use vim movements and commands. 
You will continue to grab your mouse and be sad because you can't use ctrl+c to copy text from nvim to external sources (use <"+y>), you will use arrows instead of hjkl to navigate files, you will forget keybinds you just looked up and will do many more mistakes. Note that you WILL BE SLOWER at the beginning, it's a small price to pay for (in my opinion) many benefits.
Note that vim works on a completely different paradigm compared to other editors, it's like going from GUIs to command line, but what do people end up sticking with?
What will also happen, is that you will feel more comfortable using your keyboard, you will start typing faster, get mad at when you need to grab the mouse to do anything and start enjoying doing these commands that make you feel like you are doing combos in a fighting game. The point is that you will have fun writing and working on a project, which is good, because working and studying while having fun and being insterested is good!

My tip is to avoid using vim perfectly, avoid trying to become good at all the crazy commands that exist, it's impossible, start looking stuff up as you need it, if you feel like there is something that you need to work better, I'm 99% confident it exists.Maybe keep a personal cheat sheet or use the one I will provide later on the guide. I will also provide certain levels of mastery that you will need to go through, stay on a level until you feel like you mastered it and can do stuff easily.  



## Overview

This is the tree structure of the repo: 
nvim
├── init.lua
├── after
│   └── plugin
│       ├── harpoon.lua
│       ├── lsp_config.lua
│       ├── lualine.lua
│       ├── nvim-cmp.lua
│       ├── nvim-tree.lua
│       ├── telescope.lua
│       ├── tokyonight.lua
│       ├── treesitter.lua
│       └── undotree.lua
└── lua
    └── savconfig
        ├── packer.lua
        └── remap.lua

`init.lua`: the file that will call `packer.lua` and `remap.lua`. `packer.lua` will ensure that all required packages are correctly installed (if they aren't, it will install them), `remap.lua` will do some required remaps (a remap consists in taking the original function of a keybinding and switch it into another custom one).

Every `.lua` file inside `after/plugin` is a custom configuration for the corresponding package. I suggest you exploring those in order to understand the main key combinations used in each package. FYI: the folders are called `after/plugin` because nvim automatically looks for that folder to understand how to setup packages correctly, so the names are "special".



Rather, I will tell what are the needed extentions to learn and how important they are (imo).

## Learning path

### Introduction
Vim works in "modes". There are different modes (https://vimhelp.org/intro.txt.html#vim-modes), but the tree main ones are 3. You can think about these as different keyboard layouts stacked on top of each other, based on what you want to do you will use the corresponding one. You should already be familiar with this concept if you completed the `:Tutor` as I suggested before. I can't stress it enough, please complete it as it explains everything related to movements and commands and how to concatenate the two. I assume that you completed it. 

1. Normal mode you can enter all editor commands, including those from plugins, as well as navigate through text. Note: when using commands such as <dd>, that is actually a "cut" command, not a "del". Make sure to paste first, delete later. 
2. Visual mode allows you to highlight text, you can use either <v> for cursor highlight or <V> for line highlight. You can also use certain commands, such as delete or yank (more on this later).
3. Insert mode turns your keyboard in your regular keyboard, just note that since neovim runs on terminal, combinations such as <Ctrl+c> will not work, you need to use shift as well (just like your regular terminal). Speaking of terminal, depending on which terminal you are using you may have more or less features, such as being able to navigate through words using ctrl+leftarrow (which is BAD habit in nvim, that I still have). If you care about being able to do this, I would suggest you to install xfce terminal.  

All the following commands are done in normal mode. 
- <:q> quits the current nvim window. Notice that a window in nvim could be the file explorer located at the left (assuming you are using my configuration). If there is only a single window, it will exit neovim, note that if you get an error it most likely means you haven't saved a file you worked on and nvim is telling you that.
- <:w> saves the current file you are editing.
- <:wq> saves and quit.
- <Ctrl+w> is used and the file you are editing. 
- In the nvim-tree window: <a> lets you create a file, make sure to specify the extension (if you want a folder, use /)
- In the nvim-tree window: <d> lets you delete a file or folder.

Make sure you get comfortable using these and the following commands that you already learned completing `:Tutor`:
- <w>
- <b>
- <e>
- <d>
- <dd>
- <p>
- <esc>
- </>
- <v>
- <V> -> <<> or <>>, this will indent a highlighted line or portion of text if you are using <v> instead of <V>
- <u>
- <Ctrl+r>

All of this will already make you faster than using a regular IDE when working on a singular file, but what if we work on a codebase?

## Getting comfortable with Telescope and harpoon
Telescope and harpoon are both extremely powerful extensions, they will introduce a bit of an overhead, but when working on an actual codebase. Say you need to quickly switch between two files, maybe three or even more, normally you would have to cycle between tabs, or click them, maybe you close one and you have to open it again, maybe you have to navigate the whole folder structure in order to find it, Harpoon got you covered. 

Harpoon allows you to "mark", or pin, certain files. It means that through a two key combination you instantly switch to that file without any mental overhead, you know that on <Ctrl+h> you have file A and that on <Ctrl+j> you have file B. I have configured harpoon in order to navigate to up to 6 files, but more can be added.

In order to mark a file, you have to open it, then (in normal mode):
- <leader+a> a = add
If you have done this correctly, using
- <Ctrl+e> 
you should see your harpooned file. Now try adding another one and see if you can see it correctly from the menu.
In order to navigate into them use:
- <Ctrl+h/j/k/l/ò/à>
To remove an entry, enter in the menu and navigate to the desired file to remove and hit it with <dd>.

Now, what if we don't know where a file is, if it's git tracked or not, or maybe we are looking for a function that we call somewhere in the codebase that is giving us problems, this is where telescope rescues us. Telescope is a fuzzy finder for files or text, which works wonderfully in combination with harpoon. We have three main functionalities to address these issues:



- <leader+p+f> this is the fuzzy finder, it looks in the entire folder for a file that approximately matches the searched text.
- <leader+p+g> this is the same, but only works on a subset of the folder that belongs to git. (useful if you have bloat such as __pycache__ or a virtual environment) 
- <leader+p+s> this calls the bash Grep function, which will look for a matching text in every file inside the opened folder. 
- TODO combo of both

















