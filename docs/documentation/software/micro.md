---
tags:
    - micro
    - software
    - documentation
    - text editor
---
# Micro
I wish I could say I loved vim, but I don't.  Like with most things, I have a genuine distaste for anything I stink at. I am not kidding I am embarrassingly bad at vim. Why do you think I avoid using vim on my channel?

I remember when I worked at a civil engineering software company in the mid 1990s, I edited files using vi.  I got better.  Definitely, repetition was working, but that is the extent of my recall.  I could not even tell you what kind of files they were.   

I recognize the power vim and in my case, neovim, possesses.  I have a desire to be able to use vim as proficiently as I see YouTube creators that I respect using the unquestioned first choice for a terminal text editor.  But, after several years of struggling, I am no better.  

I try.  In fact, I have an se

My current nvim configuration can be found in my dotfiles on [github](https://github.com/drewgrif/nvim).

## More my speed - the micro text editor

```
sudo apt install micro
```

Granted, Linux users make fun of nano.  It is very simplistic, not very good with regard to keybindings and, generally, ill-suited for anything other than minor text edits.

Where nano leaves you wanting more, micro picks up the middle ground between nano and vim.  I believe users would agree that micro is closer to vim than it is to nano - and that's a good thing.  

For me, I install a few plugins, but it's really unnecessary to get good use out of it.  

To accomplish what I do, I install gotham-colors, filemanager, and wc (for word counting)

```
micro -plugin install gotham-colors
micro -plugin install filemanager
micro -plugin install wc
```


## Things I love about micro:

1. Keybindings:  Like with most GTK text editors (including geany and l3afpad), Ctrl-S will save the document.  Ctrl-q will close the document.  
2. Command line: Ctrl-e is very powerful.  For example, Ctrl-e, type 'set' then tab key to autocomplete.
3. Using mouse if wanted.  You can turn mouse off if you want too.
4. Simple. Simple. Simple.

Common usages for Ctrl-e

```
> set colorscheme gotham
> set softwrap on
> set autosave 120 (autosaves every 120 second)
> wc (for word counter)
> tree (for filemanager)
> vsplit ~/.bashrc (to open a second file)
> replaceall alt super (replaces all 'alt' with 'super')

```

## Things I don't love about micro:

1. The plugins are sparse.  Not like vim.
2. The filemanager plugin is not being developed needs reworked.
3. There is no plugin for colorpreview

## Conclusion
With regard to the filemanager, I am wondering if I can/should use something like lf/ranger in terminal to navigate in place of the filemanager plugin which will still open the editor in a terminal window.

I recognize the vimtutor is something I should utilize if I want to get better and more efficient at using neovim. Furthermore, geany (WHICH I LOVE) has a plugin for vim mode which would allow me to practice as well as be in an environment that I can use really well.

But, I guess the question is do I need to be good at vim?  Not sure I do. Meanwhile, micro is darned good.  Lightweight and much more conducive for my extremely small needs.  

<script data-name="BMC-Widget" data-cfasync="false" src="https://cdnjs.buymeacoffee.com/1.0.0/widget.prod.min.js" data-id="justaguylinux" data-description="Support me on Buy me a coffee!" data-message="" data-color="#FF5F5F" data-position="Right" data-x_margin="18" data-y_margin="18"></script>
