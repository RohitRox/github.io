---
layout: post
title: "VIM Quick Tips"
date: 2012-12-19 11:50
comments: true
categories: [DevTips]
---
Vim quick tips and tricks, listing some useful, essential and most often used Vim commands.

## Moving Around

| Command                             | Action                          |
| ----------------------------------  | ------------------------------- |
| j or Up Arrow                       | Move the cursor up one line.
| k or Down Arrow                     | Down one line.
| h or Left Arrow                     | Left one character.
| l or Right Arrow                    | Right one character.
| e                                   | To the end of a word.
| E                                   | To the end of a whitespace-delimited word.
| b                                   | To the beginning of a word.
| B                                   | To the beginning of a whitespace-delimited word.
| 0                                   | To the beginning of a line.
| $                                   | To the end of a line.
| H                                   | To the first line of the screen.
| M                                   | To the middle line of the screen.
| L                                   | To the the last line of the screen.
| :n                                  | Jump to line number n. For example, to jump to line 42, you'd type :42
| gg                                  | Goto start of file
| G                                   | Goto end of file
| I                                   | go to the beginning of the line and go into insert
| A                                   | go to the end of the line and go into insert
| Ctrl+g                              | Show file info  including your position in the file
| i                                   | Insert at cursor
| a                                   | Append after cursor
| A                                   | Append at end of line
| ESC                                 | Terminate insert mode
| u                                   | Undo last change
| U                                   | Undo all changes to entire line
| dd                                  | Delete line
| 3dd                                 | Delete 3 lines.
| D                                   | Delete contents of line after cursor


<!--more-->


## Windows

| Command                             | Action                          |
| ----------------------------------  | ------------------------------- |
| :sp                                 | new window above
| :vs                                 | new window to left
| :q                                  | close current window
| :qa                                 | close all windows add trailing ! to force
| Ctrl+w                              | {left,right,up,down} move to window
| Ctrl+w                              | Ctrl+w toggle window focus


## Vi/Vim modes

Turn on autoindent:   __:set autoindent__

Turn off autoindent:  __:set noautoindent__

Set indent width:   __set shiftwidth=4__
Intelligent auto-indent:  __set smartindent__
Toggle autoindent on/off when pasting text (press F2 key to toggle mode after one is in "insert" mode): set pastetoggle=<F2>

__:set tabstop=8__
Tab key displays 8 spaces

__:set syntax on__
__:set syntax off__
Set syntax highlighting and color highlighting for a file type (eg XML, HTML, C++, Java, etc). Also cursor shows matching ")" and "}"
Also can set syntax highlighting explicitly: __:set syntax=html__
Syntax definition files: /usr/share/vim/vim73/syntax/

__:set number__
Show line number

__:set bg=dark__
__:set bg=light__
VIM: choose color scheme for "dark" or "light" console background.


__:help__
Vim has a extensive on-line help system. You can access this by using :help.

__:set cindent__
This turns on C style indentation. Each new line will be automatically indented the correct amount according to the C indentation standard.

Word Completion
When your typing and you enter a partial word, you can cause Vim to search for a completion by using the ^P (search for previous marching word) and ^N (search for next match).
