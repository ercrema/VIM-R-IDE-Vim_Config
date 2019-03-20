# perfectRVimConfiguration

A perfect R integration with vim. Make vim a better R IDE than Rstudio !!!

## Vim 

neovim with python 3 support

## Plugins (using [vim-plug](https://github.com/junegunn/vim-plug))

Plug 'rafaqz/citation.vim'
Plug 'Shougo/unite.vim'
Plug 'jalvesaq/zotcite
" snippet frameword beginning
Plug 'ncm2/ncm2'
Plug 'roxma/nvim-yarp'
Plug 'ncm2/ncm2-bufword'
Plug 'ncm2/ncm2-path'
Plug 'jalvesaq/Nvim-R'
Plug 'gaalcaras/ncm-R'
Plug 'ncm2/ncm2-ultisnips'
Plug 'SirVer/ultisnips'  
" snippets framework end
Plug 'chrisbra/csv.vim " for viewing data directly in vim R (Nvim-R)
Plug 'junegunn/goyo.vim'  "for nice zoom effet when editing
Plug 'ferrine/md-img-paste.vim' " paste directly image in system clipboard to rmarkdown by putting images in an /img folder (created automatically)

## Configurations

### filetype

filetype plugin indent on
" set rmarkdown file type for safety
au BufNewFile,BufRead *.Rmd set filetype=rmd

### ability to paste directly image in system clipboard to rmarkdown by putting images in an /img folder (created automatically)

" use leader+p to paste image to markdown and rmarkdown
autocmd FileType markdown nmap <silent> <leader>p :call mdip#MarkdownClipboardImage()<CR>
autocmd FileType rmd nmap <silent> <leader>p :call mdip#MarkdownClipboardImage()<CR>


### autocompletion

**This is the tricky part, I'll explain step by step**
" First use <TAB> and <shift tab> to browse the popup menu and use enter to expand:
inoremap <silent> <expr> <CR> ncm2_ultisnips#expand_or("\<CR>", 'n')
inoremap <expr> <Tab> pumvisible() ? "\<C-n>" : "\<Tab>"
inoremap <expr> <S-Tab> pumvisible() ? "\<C-p>" : "\<S-Tab>"

" However the previous lines alone won't work, we must disable the UltiSnips Expand Trigger, I set it to ctrl-0
let g:UltiSnipsExpandTrigger="<c-0>"

  
### tricks

let maplocalleader = ","   " the default is \, you can also set it to <\space> if you don't like ,

" make R starts automatically when .R or .Rmd file open and only starts one time

autocmd FileType r if string(g:SendCmdToR) == "function('SendCmdToR_fake')" | call StartR("R") | endif
autocmd FileType rmd if string(g:SendCmdToR) == "function('SendCmdToR_fake')" | call StartR("R") | endif

" make R vertical split at start
let R_rconsole_width = 57
let R_min_editor_width = 18

" some nice keybindding, D = cursor down one line when finished, by the way localleader+rv = view data, +rg = plot(graphic), +rs = summary, all without sending lines to R buffer, very useful
" Other useful features like Rformat and R RBuildTags aren't covered here, see Nvim-R for more info.

nmap <LocalLeader>sc <Plug>RDSendChunk   " useful when in Rmarkdown, send chunk
nmap <LocalLeader>ss <Plug>RDSendLine    " directly send line to R buffer when nothing selected    
nmap <LocalLeader>st <Plug>RDSendLineAndInsertOutput  
" st = send test, this function shows the output in comment, since it's in vim we can simply press u to make the output disappearsee figure 3333
vmap <LocalLeader>ss <Plug>REDSendSelection " send selection in visual mode
vmap <LocalLeader>test <Plug>RClearConsole   " rq would be mapped to RClose so we replace RClearConsole by some random strings
nmap <LocalLeader>test <Plug>RClearConsole " idem
nmap <LocalLeader>rr <Plug>RStart  " rr is easier than rf
vmap <LocalLeader>rr <Plug>RStart " idem
nmap <LocalLeader>rq <Plug>RClose " rq = rquit, easier to remember
vmap <LocalLeader>rq <Plug>RClose " idem
  
### Some screenshots
Figure 1 (R and rmarkdown file, the img folder and the image-591 is inserted automatically using ferrine/md-img-paste.vim)
Figure 2 (leader s s on line)
Figure 3 (leader s s on selection)
Figure 4 (leader s t on line 7)

To be continued with citation...


