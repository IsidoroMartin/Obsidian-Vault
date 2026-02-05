
Table of Contents

1. [COMMAND LINE](#command-line)
2. [COMMANDS](#commands)
3. [MOVEMENTS](#movements)
4. [COMMANDS RANGE](#commands-range)
5. [SPLIT WINDOWS](#split-windows)
6. [TABS](#tabs)
7. [FIND](#find)
8. [HIGHLIGHTS](#highlights)
9. [COMPILATION & TAGS](#compilation--tags)
	1. [Compilation](#compilation)
	2. [Tags](#tags)
10. [ENVIRONMENT](#environment)
11. [FOLDING](#folding)
12. [REGEX & MACROS](#regex--macros)
	1. [Regex](#regex)
	2. [Macros](#macros)
13. [MISC](#misc)
14. [INDENT XML](#indent-xml)

# VIM CHEATSHEET - v1.1

## COMMAND LINE
* `-A` Arabic
* `-x` Crypting the file
* `-u vimrc` to read external config
* `-p files` to open several tabs
* `-` read from stdin

---

## COMMANDS
* `i` insert
* `a` append
* `A` append at end of line
* `o` new line below
* `O` new line above
* `c` change
* `cc` change line
* `r` replace
* `R` replace till ESC
* `J` join line with the following
* `s` delete & insert mode
* `~` change case
* `d` delete
* `dd` delete line
* `x` cut
* `y` yank
* `yy` yank line
* `p` paste the buffer after
* `P` paste the buffer before
* `<<` move shiftwidth to left
* `>>` move shiftwidth to right
* `=` indent
* `CTRL+{a,x}` `{in,de}crement` number
* `u` undo
* `CTRL+r` redo
* `.` apply last command
* `:w [>>] [file]` save
* `:q` quit
* `:wq` / `ZZ` save & quit
* `v` enable visual selection
* `CTRL+v` enable grid visual selection

---

## MOVEMENTS
* `h,j,k,l` cursors
* `g{k,j}` up/down independent from line-wrap
* `{0,$}` `{start, end}` of line
* `#` column of line
* `{0,#,$}g` `{start, line X, end}` of file
* `#%` percentage of file
* `{w,b}` `{next, prev}` word
* `z{t,z,b}` put the line at the `{top, center, bottom}`
* `H` first line of screen
* `M` middle line of screen
* `L` last line of screen
* `CTRL+{U,D}` half screen `{up, down}`
* `CTRL+{F,B}` one screen `{up, down}`
* `%` matching bracket
* `{(,)}` `{start, end}` of sentence
* `{{,}}` `{start, end}` of paragraph

---

## COMMANDS RANGE
* `% cmd` execute cmd on whole file/block
* `x:y cmd` execute cmd on line range `x={line #, . , {+,-} #}`
* `v cmd` execute cmd on selection
* `x cmd` execute cmd x times
* `cmd 'mark` execute cmd till mark included
* `cmd \`mark` execute cmd till mark excluded
* `:g/pattern/cmd` execute cmd on pattern
* `cmd/pattern` execute cmd forward till pattern
* `cmd?pattern` execute cmd backward till pattern

---

## SPLIT WINDOWS
* `:sp[lit] [file]`
* `:vsp[lit] [file]`
* `CTRL+w n` create a new file
* `CTRL+w {h,j,k,l}` to move
* `CTRL+w w` to move clockwise
* `CTRL+w {r,R}` to rotate
* `CTRL+w {+,-}` to `{add, sub}` lines
* `CTRL+w =` to assign equal size

---

## TABS
* `:tabnew [file]`
* `:tab{first,p,n,last}` to move
* `CTRL+p,n` to move
* `:tabs` to list
* `:tabn #` to move to `#` pos

---

## FIND
* `/pattern` search
* `/pattern\c` case-insensitive search
* `/pattern/*` print show search results
* `*` search char under cursor
* `n` next search
* `?` prev search
* `:set hlsearch` highlight found results
* `:set incsearch` search as you type
* `:set ignorecase` case-insensitive search
* `:set smartcase` case-insensitive if 1+ uppercase chars in pattern

---

## HIGHLIGHTS
* `:syntax on` enable syntax coloring
* `:set showmatch` highlight matching parenthesis
* `:set cursorline` highlight the current line
* `:set cursorcolumn` highlight the current column
* `:[23]?match # /pattern/` highlight with color group `#`
* `:[23]?match NONE` disable highlight
* `:so $VIMRUNTIME/syntax/hitest.vim` list color groups

---

## COMPILATION & TAGS
### Compilation
* `:make {,args}` to launch compilation with Makefile
* `:c{next,previous}` to move cursor among errors
* `:cc [#]` to display current/`[number #]` error
* `:clist` to list the errors
* `:cnfile` to go to the first error of next file

### Tags
* `:tags {,#}` search symbols using `#` `{,as regex}`
* `:tselect #` list the tags of `#`
* `CTRL+{,W}` to get to definition in the `{same, new}` window
* `CTRL+t` to get back
* `:{next,prev}` to scroll

---

## ENVIRONMENT
* `:set fileformat={dos,unix,mac}` change EoL
* `:set nu{mber}` show line numbering
* `:set numberwidth=#` format line number
* `:filetype on`
* `:set nobackup`
* `:set nowritebackup`
* `:set shiftwidth=#` indentation of `#` spaces
* `:set tabstop=#` length of a tab
* `:set autoindent` indent after `\n`
* `:set cindent` C indentation standard
* `:set expandtab` convert tab into spaces
* `:set matchtime=#` time to show the matching bracket
* `:set report=#` alert if `>#` lines has been changed
* `:set list` show invisible chars
* `:set statusline=format` custom status line
* `:set tabline=format` custom tab name
* `:set laststatus=[2-9]` always show status line
* `:set backspace=indent,eol,start`

---

## FOLDING
* `zf#` to fold using range `#`
* `zo` to open a fold
* `zR` to open all folds
* `zc` to close a fold
* `zM` to close all folds
* `zd` to delete a fold
* `zE` to delete all folds
* `:mkview [rev]` to save the folds
* `:loadview [rev]` to apply saved folds

---

## REGEX & MACROS
### Regex
* `\(...\)` capturing
* `\[1..9]` to paste captured
* `\|` alternatives
* `\r` eol

### Macros
* `q[a-z]` record into reg `#`
* `q` stop recording
* `@[a-z]` reproduce macro `#`

---

## MISC
* `:E{,x,plore}` show files list
* `:Tex{,plore}` show files list in another tab
* `:S{,ex,plore}` show files list in an horiz split window
* `:Vex{,plore}` show files list in an vertical split window
* `:changes` to list all the edits
* `CTRL+l` refresh the screen
* `CTRL+n` autocomplete
* `CTRL+g` show status line

---

## INDENT XML
1. `:set filetype=xml`
2. `:filetype indent on`
3. `:e`
4. `gg=G`