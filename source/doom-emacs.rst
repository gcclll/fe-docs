========================
Doom Emacs Configuration
========================
-------------------------------------------------
The Methods, Management, and Menagerie of Madness
-------------------------------------------------

    :Author: Zhicheng Lee
    :Date: 2021-05-30 13:46 ;CST,

.. contents::

::

    用 config.org 文件来维护 doom emacs 配置。

::

    他人配置列表：

.. table::

    +-------------------------------------------------------------------------------------------------------------------------------------------------+-------+
    | name&link                                                                                                                                       | brief |
    +=================================================================================================================================================+=======+
    | `dot-doom/doom.org at master · zzamboni/dot-doom <https://github.com/zzamboni/dot-doom/blob/master/doom.org>`_                                  | -     |
    +-------------------------------------------------------------------------------------------------------------------------------------------------+-------+
    | `Sacha Chua’s Emacs configuration <http://pages.sachachua.com/.emacs.d/Sacha.html>`_                                                            | -     |
    +-------------------------------------------------------------------------------------------------------------------------------------------------+-------+
    | `daedreth/UncleDavesEmacs: My personal ~/.emacs.d <https://github.com/daedreth/UncleDavesEmacs#user-content-ido-and-why-i-started-using-helm>`_ | -     |
    +-------------------------------------------------------------------------------------------------------------------------------------------------+-------+
    | `PythonNut/quark-emacs: An incredible wonderland of code <https://github.com/PythonNut/quark-emacs>`_                                           | -     |
    +-------------------------------------------------------------------------------------------------------------------------------------------------+-------+
    | `Mastering Emacs <https://www.masteringemacs.org/>`_                                                                                            | -     |
    +-------------------------------------------------------------------------------------------------------------------------------------------------+-------+
    | `Doom Emacs Configuration <https://tecosaur.github.io/emacs-config/config.html>`_                                                               | -     |
    +-------------------------------------------------------------------------------------------------------------------------------------------------+-------+
    | `GitHub - KaratasFurkan/.emacs.d: My literate Emacs configuration <https://github.com/KaratasFurkan/.emacs.d>`_                                 | -     |
    +-------------------------------------------------------------------------------------------------------------------------------------------------+-------+

1 自定义函数
------------

.. code:: common-lisp

    ;;;###autoload
    (defun gcl/goto-match-paren (arg)
      "Go to the matching if on (){}[], similar to vi style of % ."
      (interactive "p")
      (cond ((looking-at "[\[\(\{]") (evil-jump-item))
            ((looking-back "[\]\)\}]" 1) (evil-jump-item))
            ((looking-at "[\]\)\}]") (forward-char) (evil-jump-item))
            ((looking-back "[\[\(\{]" 1) (backward-char) (evil-jump-item))
            (t nil)))

    ;;;###autoload
    (defun gcl/string-inflection-cycle-auto ()
      "switching by major-mode"
      (interactive)
      (cond
       ;; for emacs-lisp-mode
       ((eq major-mode 'emacs-lisp-mode)
        (string-inflection-all-cycle))
       ;; for python
       ((eq major-mode 'python-mode)
        (string-inflection-python-style-cycle))
       ;; for java
       ((eq major-mode 'java-mode)
        (string-inflection-java-style-cycle))
       (t
        ;; default
        (string-inflection-all-cycle))))

    ;; Current time and date
    (defvar current-date-time-format "%a %b %d %H:%M:%S %Z %Y"
      "Format of date to insert with `insert-current-date-time' func
    See help of `format-time-string' for possible replacements")

    (defvar current-time-format "%H:%M"
      "Format of date to insert with `insert-current-time' func.
    Note the weekly scope of the command's precision.")

    (defun insert-current-date-time ()
      "insert the current date and time into current buffer.
    Uses `current-date-time-format' for the formatting the date/time."
      (interactive)
      (insert (format-time-string current-date-time-format (current-time)))
      )

    (defun insert-current-time ()
      "insert the current time (1-week scope) into the current buffer."
      (interactive)
      (insert (format-time-string current-time-format (current-time)))
      )

    (defun my/capitalize-first-char (&optional string)
      "Capitalize only the first character of the input STRING."
      (when (and string (> (length string) 0))
        (let ((first-char (substring string nil 1))
              (rest-str   (substring string 1)))
          (concat (capitalize first-char) rest-str))))
    (defun my/lowcase-first-char (&optional string)
      "Capitalize only the first character of the input STRING."
      (when (and string (> (length string) 0))
        (let ((first-char (substring string nil 1))
              (rest-str   (substring string 1)))
          (concat first-char rest-str))))

2 模块(init.el)
---------------

使用到的包：

.. code:: common-lisp
    :name: init.el

    ;;; init.el -*- lexical-binding: t; -*-

    (doom! :input
           ;;chinese
           ;;japanese
           ;;layout            ; auie,ctsrnm is the superior home row

           :completion
           company           ; the ultimate code completion backend
           ;;helm              ; the *other* search engine for love and life
           ;;ido               ; the other *other* search engine...
           (ivy               ; a search engine for love and life
            +icons
            +prescient)

           :ui
           ;;deft              ; notational velocity for Emacs
           doom              ; what makes DOOM look the way it does
           doom-dashboard    ; a nifty splash screen for Emacs
           doom-quit         ; DOOM quit-message prompts when you quit Emacs
           (emoji +unicode)  ; 🙂
           fill-column       ; a `fill-column' indicator
           hl-todo           ; highlight TODO/FIXME/NOTE/DEPRECATED/HACK/REVIEW
           hydra
           ;;indent-guides     ; highlighted indent columns
           (ligatures         ; ligatures and symbols to make your code pretty again
            +extra)
           ;;minimap           ; show a map of the code on the side
           modeline          ; snazzy, Atom-inspired modeline, plus API
           nav-flash         ; blink cursor line after big motions
           ;;neotree           ; a project drawer, like NERDTree for vim
           ophints           ; highlight the region an operation acts on
           (popup +defaults)   ; tame sudden yet inevitable temporary windows
           ;;tabs              ; a tab bar for Emacs
           treemacs          ; a project drawer, like neotree but cooler
           unicode           ; extended unicode support for various languages
           vc-gutter         ; vcs diff in the fringe
           vi-tilde-fringe   ; fringe tildes to mark beyond EOB
           window-select     ; visually switch windows
           workspaces        ; tab emulation, persistence & separate workspaces
           ;;zen               ; distraction-free coding or writing

           :editor
           (evil +everywhere); come to the dark side, we have cookies
           file-templates    ; auto-snippets for empty files
           fold              ; (nigh) universal code folding
           (format +onsave)  ; automated prettiness
           ;;god               ; run Emacs commands without modifier keys
           ;;lispy             ; vim for lisp, for people who don't like vim
           multiple-cursors  ; editing in many places at once
           ;;objed             ; text object editing for the innocent
           ;;parinfer          ; turn lisp into python, sort of
           rotate-text       ; cycle region at point between text candidates
           snippets          ; my elves. They type so I don't have to
           word-wrap         ; soft wrapping with language-aware indent

           :emacs
           (dired +icons)    ; making dired pretty [functional]
           electric          ; smarter, keyword-based electric-indent
           (ibuffer +icons)  ; interactive buffer management
           undo              ; persistent, smarter undo for your inevitable mistakes
           vc                ; version-control and Emacs, sitting in a tree

           :term
           ;;eshell            ; the elisp shell that works everywhere
           ;;shell             ; simple shell REPL for Emacs
           ;;term              ; basic terminal emulator for Emacs
           vterm             ; the best terminal emulation in Emacs

           :checkers
           syntax              ; tasing you for every semicolon you forget
           ;;spell             ; tasing you for misspelling mispelling
           ;;grammar           ; tasing grammar mistake every you make

           :tools
           ;;ansible
           ;;debugger          ; FIXME stepping through code, to help you add bugs
           ;;direnv
           ;;docker
           editorconfig      ; let someone else argue about tabs vs spaces
           ;;ein               ; tame Jupyter notebooks with emacs
           (eval +overlay)     ; run code, run (also, repls)
           ;;gist              ; interacting with github gists
           (lookup              ; navigate your code and its documentation
            +dictionary
            +docsets)
           (lsp +peek)
           (magit             ; a git porcelain for Emacs
            +forge)
           ;;make              ; run make tasks from Emacs
           ;;pass              ; password manager for nerds
           ;;pdf               ; pdf enhancements
           ;;prodigy           ; FIXME managing external services & code builders
           rgb               ; creating color strings
           ;;taskrunner        ; taskrunner for all your projects
           ;;terraform         ; infrastructure as code
           ;;tmux              ; an API for interacting with tmux
           upload            ; map local to remote projects via ssh/ftp

           :os
           (:if IS-MAC macos)  ; improve compatibility with macOS
           tty               ; improve the terminal Emacs experience

           :lang
           ;;agda              ; types of types of types of types...
           (cc +lsp)                ; C/C++/Obj-C madness
           ;;clojure           ; java with a lisp
           ;;common-lisp       ; if you've seen one lisp, you've seen them all
           ;;coq               ; proofs-as-programs
           ;;crystal           ; ruby at the speed of c
           ;;csharp            ; unity, .NET, and mono shenanigans
           data              ; config/data formats
           ;;(dart +flutter)   ; paint ui and not much else
           ;;elixir            ; erlang done right
           ;;elm               ; care for a cup of TEA?
           emacs-lisp        ; drown in parentheses
           ;;erlang            ; an elegant language for a more civilized age
           ;;ess               ; emacs speaks statistics
           ;;faust             ; dsp, but you get to keep your soul
           ;;fsharp            ; ML stands for Microsoft's Language
           ;;fstar             ; (dependent) types and (monadic) effects and Z3
           ;;gdscript          ; the language you waited for
           (go +lsp)         ; the hipster dialect
           ;;(haskell +dante)  ; a language that's lazier than I am
           ;;hy                ; readability of scheme w/ speed of python
           ;;idris             ; a language you can depend on
           (json)              ; At least it ain't XML
           ;;(java +meghanada) ; the poster child for carpal tunnel syndrome
           (javascript)        ; all(hope(abandon(ye(who(enter(here))))))
           ;;julia             ; a better, faster MATLAB
           ;;kotlin            ; a better, slicker Java(Script)
           (latex             ; writing papers in Emacs has never been so fun
            +latexmk
            +cdlatex
            +fold)
           ;;lean
           ;;factor
           ;;ledger            ; an accounting system in Emacs
           ;;lua               ; one-based indices? one-based indices
           markdown          ; writing docs for people to ignore
           ;;nim               ; python + lisp at the speed of c
           ;;nix               ; I hereby declare "nix geht mehr!"
           ;;ocaml             ; an objective camel
           (org               ; organize your plain life in plain text
            +attach
            +babel
            +capture
            +dragndrop
            +hugo
            +jupyter
            +export
            +pandoc
            +gnuplot
            +pretty
            +present
            +protocol
            +pomodoro
            ;; +roam
            )
           php               ; perl's insecure younger brother
           ;;plantuml          ; diagrams for confusing people more
           ;;purescript        ; javascript, but functional
           (python +lsp)            ; beautiful is better than ugly
           ;;qt                ; the 'cutest' gui framework ever
           ;;racket            ; a DSL for DSLs
           ;;raku              ; the artist formerly known as perl6
           rest                ; Emacs as a REST client
           ;;rst               ; ReST in peace
           ;;(ruby +rails)     ; 1.step {|i| p "Ruby is #{i.even? ? 'love' : 'life'}"}
           (rust              ; Fe2O3.unwrap().unwrap().unwrap().unwrap()
            +lsp)
           ;;scala             ; java, but good
           scheme            ; a fully conniving family of lisps
           sh                ; she sells {ba,z,fi}sh shells on the C xor
           ;;sml
           ;;solidity          ; do you need a blockchain? No.
           ;;swift             ; who asked for emoji variables?
           ;;terra             ; Earth and Moon in alignment for performance.
           (web)               ; the tubes
           yaml              ; JSON, but readable

           :email
           ;;(mu4e +gmail)
           ;;notmuch
           ;;(wanderlust +gmail)

           :app
           ;;calendar
           ;;irc               ; how neckbeards socialize
           ;;(rss +org)        ; emacs as an RSS reader
           ;;twitter           ; twitter client https://twitter.com/vnought

           :config
           ;;literate
           (default +bindings +smartparens))

3 配置(config.el)
-----------------

.. code:: common-lisp

    ;;; config.el -*- lexical-binding: t; -*-

3.1 键值绑定
~~~~~~~~~~~~

3.1.1 解绑按键
^^^^^^^^^^^^^^

.. code:: common-lisp

    ;; 解绑一些按键，待用
    (map! :niv      "C-s" nil
          :niv      "C-d" nil
          :niv      "C-i" nil
          :niv      "M-," nil
          :niv      "M-." nil
          :niv      "M-f" nil

          :leader
          "A" nil
          "X" nil
          "/" nil)

    (map! [remap swiper] #'swiper-isearch
          [remap org-capture] nil
          [remap xref-find-definitions] #'lsp-ui-peek-find-definitions
          [remap xref-find-references] #'lsp-ui-peek-find-references
          )

3.1.2 F1-F12
^^^^^^^^^^^^

.. code:: common-lisp

    (global-set-key (kbd "<f3>") 'hydra-multiple-cursors/body)
    (global-set-key (kbd "<f5>") 'deadgrep)
    (global-set-key (kbd "<M-f5>") 'deadgrep-kill-all-buffers)
    (global-set-key (kbd "<f12>") 'smerge-vc-next-conflict)
    (global-set-key (kbd "<f11>") '+vc/smerge-hydra/body)

3.1.3 SPC(空格)
^^^^^^^^^^^^^^^

.. code:: common-lisp

    (map! :leader
          :n        "SPC"   #'execute-extended-command
          :n        "bf"   #'osx-lib-reveal-in-finder
          :n        "fo"   #'crux-open-with
          :n        "fj"   #'dired-jump
          :n        "/r"   #'deadgrep

          (:prefix ("l" . "load")
           :n       "i"     #'imenu-list
           :n       "o"     #'lsp-ui-imenu
           :n       "d"     #'deft
           :n       "l"     #'+workspace/switch-to)

          (:prefix ( "v" . "view" )
           :n       "o"     #'ivy-pop-view
           :n       "p"     #'ivy-push-view)

          :n        "w -"   #'split-window-below
          )

3.1.4 s-(Command)
^^^^^^^^^^^^^^^^^

.. code:: common-lisp

    (map! "s-<"     #'move-text-up
          "s->"     #'move-text-down
          "s-i"     #'gcl/string-inflection-cycle-auto
          "s-("     #'sp-backward-barf-sexp
          "s-)"     #'sp-forward-barf-sexp

          )

3.1.5 C-(Control)
^^^^^^^^^^^^^^^^^

.. code:: common-lisp

    (map!
     "C-'"     #'imenu-list-smart-toggle
     "C-d"     (cmd! (previous-line)
                     (kill-line)
                     (forward-line))
     "C-s"     #'+default/search-buffer

     ;; smartparen
     "C-("     #'sp-backward-slurp-sexp
     "C-)"     #'sp-forward-slurp-sexp


     ;; multiple cursors
     "C->"     #'mc/mark-next-like-this
     "C-<"     #'mc/mark-previous-like-this
     "C-c C-<" #'mc/mark-all-like-this
     "C-S-c C-S-c" #'mc/edit-lines
     "C-S-c 0" #'mc/insert-numbers
     "C-S-c 1" #'mc/insert-letters
     "C-S-c s" #'mc/mark-all-in-region
     "C-S-c S" #'mc/mark-all-in-region-regexp

     ;; prefix C-c
     "C-c a c"     #'org-mac-chrome-insert-frontmost-url
     "C-c d"       #'insert-current-date-time
     "C-c t"       #'insert-current-time
     "C-c o"       #'crux-open-with
     "C-c r"       #'vr/replace
     "C-c q"       #'vr/query-replace
     "C-c u"       #'crux-view-url
     "C-c y"       #'youdao-dictionary-search-at-point+

     "C-c C-f"     #'json-mode-beautify

     :niv      "C-e"     #'evil-end-of-line
     :niv      "C-="     #'er/expand-region

     )

3.1.6 M-(Alt/Option)
^^^^^^^^^^^^^^^^^^^^

.. code:: common-lisp

    (map! "M--"     #'gcl/goto-match-paren
          "M-i"     #'parrot-rotate-next-word-at-point
          "M-f"     #'scrollkeeper-contents-up)

3.1.7 evil bindings
^^^^^^^^^^^^^^^^^^^

.. code:: common-lisp

    (map!
     :desc "Go function header"     :n "g[" #'beginning-of-defun
     :desc "Go function end"        :n "g]" #'end-of-defun
     :desc "Find definition"        :n "gd" #'xref-find-definitions
     :desc "Find reference"         :n "gD" #'xref-find-references
     :desc "Go back find piont"     :n "gb" #'xref-pop-marker-stack
     :desc "Delete parens"          :n "z-" #'sp-splice-sexp
     :desc "Wrap with markup"       :nv "z." #'emmet-wrap-with-markup
     :desc "Increase number"        :n "+"  #'evil-numbers/inc-at-pt
     :desc "Decrease number"        :n "-"  #'evil-numbers/dec-at-pt)

3.1.8 指定模式按键
^^^^^^^^^^^^^^^^^^

.. code:: common-lisp

    (map! :map web-mode-map
          "<f2>"    #'hydra-web-mode/body

          :map org-mode-map
          :n       "tt" #'org-todo
          :n       "tc" #'org-toggle-checkbox
          :n       "tpp" #'org-priority
          :n       "tpu" #'org-priority-up
          :n       "tpd" #'org-priority-down
          )

3.2 基本配置(Basic)
~~~~~~~~~~~~~~~~~~~

3.2.1 个人信息
^^^^^^^^^^^^^^

.. code:: common-lisp

    ;; 个人信息配置
    (setq user-full-name "Zhicheng Lee"
          user-mail-address "gccll.love@gmail.com"
          user-blog-url "https://www.cheng92.com")

3.2.2 基本变量设置
^^^^^^^^^^^^^^^^^^

.. code:: common-lisp

    ;; setq, set-default 统一配置的地方
    (setq read-process-output-max (* 1024 1024)) ;; 1mb
    (setq org-directory "~/github/documents/org")
    (setq display-line-numbers-type t)

    (setq-default
     fill-column 80
     undo-limit 80000000
     delete-by-moving-to-trash t
     window-combination-resize t
     delete-trailing-lines t
     x-stretch-cursor t)

    (setq-default custom-file (expand-file-name ".custom.el" doom-private-dir))
    (when (file-exists-p custom-file)
      (load custom-file))

3.2.3 模式开启
^^^^^^^^^^^^^^

.. code:: common-lisp

    ;; 全局开启一些模式
    (setq-default abbrev-mode t)
    (display-time-mode 1)                           ; 在 mode-line 中显示时间
    (unless (equal "Battery status not available"
                   (battery))
      (display-battery-mode 1))                     ; 显示电量
    (global-subword-mode 1)                         ; Iterate through CamelCase words
    ;; (prettier-js-mode 1)
    ;; (delete-selection-mode 1)

3.2.4 abbrev 缩写表
^^^^^^^^^^^^^^^^^^^

.. code:: common-lisp

    ;; ------------------- 缩写表 ---------------------------------------------
    (define-abbrev-table 'global-abbrev-table '(
                                                ("8ireq" "import '../require'")
                                                ("8imark" "import { marker } from '@commons/sunlight/marker'")
                                                ("8ilib" "import { isArray } from '@commons/sunlight/lib'")
                                                ("81com" "@import '~@commons/styles/common';")
                                                ;; for blog
                                                ("82sfc" "
    const { compileScript, parse } =
      require(process.env.VNEXT_PKG_SFC + '/dist/compiler-sfc.cjs.js')
    const { log } = require(process.env.BLOG_JS + '/utils.js')
    const compile = (src, options) => {
      const { descriptor } = parse(src)
      return compileScript(descriptor, { ...options, id: 'xxxx' })
    }
    ")
                                                ("82lib" "
    // 源文件：/js/vue/lib.js
    const { compileSFCScript, compileStyle, getCompiledSSRString: ssr, compileSSR, log } = require(process.env.BLOG_JS + '/vue/lib.js')")
                                                ("82rc" "
    // 源文件：/js/vue/lib.js
    const { rc: { h, createVNode: c }, f, log } = require(process.env.BLOG_JS + '/vue/lib.js')
    const _h = (...args) => f(h(...args))
    ")


                                                ;; markdown
                                                ("8font" "<font color='red' size='2'>xx</font>")
                                                ;; html template for hugo
                                                ("8red" "@@html:<font color='red'>@@text@@html:</font>@@")
                                                ("8sup" "@@html:<sup><font color='red'>@@官方@@html:</font></sup>@@")
                                                ("8sub" "@@html:<sub><font color='red'>@@官方@@html:</font></sub>@@")
                                                ("8img" "
    #+BEGIN_EXPORT html
    <img src='' alt='some picture'/>
    #+END_EXPORT")
                                                ))

3.2.5 add-hook
^^^^^^^^^^^^^^

.. code:: common-lisp

    (add-to-list 'initial-frame-alist '(fullscreen . maximized))
    (add-hook 'org-mode-hook 'turn-on-auto-fill)

    (defun maybe-use-prettier ()
      "Enable prettier-js-mode if an rc file is located."
      (if (locate-dominating-file default-directory ".prettierrc")
          (prettier-js-mode +1)))
    (add-hook 'typescript-mode-hook 'maybe-use-prettier)
    (add-hook 'js2-mode-hook 'maybe-use-prettier)
    (add-hook 'web-mode-hook 'maybe-use-prettier)
    (add-hook 'rjsx-mode-hook 'maybe-use-prettier)

3.3 主题配置
~~~~~~~~~~~~

.. code:: common-lisp

    ;; 主题配置

    (setq doom-theme 'doom-vibrant) ; doom-one
    ;; (delq! t custom-theme-load-path)

    ;; (setq doom-font (font-spec :family "Source Code Pro" :size 16))
    (setq doom-font (font-spec :family "Fira Code" :size 16))

3.4 Package 配置
~~~~~~~~~~~~~~~~

3.4.1 TODO
^^^^^^^^^^

1. anzu

2. `color-rg <https://github.com/manateelazycat/color-rg>`_

3.4.2 valign
^^^^^^^^^^^^

表格插件： valign

`GitHub - casouri/valign: Pixel-perfect visual alignment for Org and Markdown
tables. <https://github.com/casouri/valign>`_

.. code:: common-lisp

    (use-package! valign
      :custom
      (valign-fancy-bar t)
      :hook
      (org-mode . valign-mode))

3.4.3 Company
^^^^^^^^^^^^^

补全配置

.. code:: common-lisp

    (after! company
      (setq company-idle-delay 0.2
            company-minimum-prefix-length 2)
      (setq company-show-numbers t)
      (add-hook 'evil-normal-state-entry-hook #'company-abort)) ;; make aborting less annoying.

    (setq-default history-length 1000)
    (setq-default prescient-history-length 1000)

3.4.4 deadgrep，支持正则
^^^^^^^^^^^^^^^^^^^^^^^^

正则搜索要在搜索的结果中，选中 regexp 来筛选。

按键绑定：

.. table::

    +-------------+-------------------------------+
    | key         | func                          |
    +=============+===============================+
    | <f5>        | ``deadgrep``                  |
    +-------------+-------------------------------+
    | M-<f5>      | ``deadgrep-kill-all-buffers`` |
    +-------------+-------------------------------+
    | ``RET``     | 查看结果                      |
    +-------------+-------------------------------+
    | ``o``       | 在另一个窗口打开              |
    +-------------+-------------------------------+
    | ``n/p``     | 结果中上下移动                |
    +-------------+-------------------------------+
    | ``M-n/M-p`` | 文件头尾之间移动              |
    +-------------+-------------------------------+
    | ``g``       | 重新搜索                      |
    +-------------+-------------------------------+
    | ``TAB``     | 展开/闭合结果                 |
    +-------------+-------------------------------+
    | ``C-c C-k`` | 停止正在执行的搜索            |
    +-------------+-------------------------------+

3.4.5 deft
^^^^^^^^^^

``SPC l d``

.. code:: common-lisp

    (use-package deft
      :after org
      :custom
      (deft-recursive t)
      (deft-use-filter-string-for-filename t)
      (deft-default-extension '("org" "md" "txt"))
      (deft-directory "~/github/documents"))

3.4.6 evil 配置
^^^^^^^^^^^^^^^

.. code:: common-lisp


    (defalias 'ex! 'evil-ex-define-cmd)

    ;; 快捷操作，通过 : 冒号进入 evil 命令模式
    ;; File operations
    (ex! "cp"          #'+evil:copy-this-file)
    (ex! "mv"          #'+evil:move-this-file)
    (ex! "rm"          #'+evil:delete-this-file)

    ;; window 操作
    (setq evil-split-window-below t
          evil-vsplit-window-right t)

3.4.7 hydra
^^^^^^^^^^^

3.4.8 leetcode
^^^^^^^^^^^^^^

.. code:: common-lisp

    (after! leetcode
      (setq leetcode-prefer-language "javascript"
            leetcode-prefer-sql "mysql"
            leetcode-save-solutions t
            leetcode-directory "~/github/make-leetcode"))

3.4.9 lsp
^^^^^^^^^

.. code:: common-lisp

    (use-package! lsp-ui
      :commands
      lsp-ui-mode
      :config
      (setq lsp-headerline-breadcrumb-enable t ; 左上角显示文件路径
            lsp-lens-enable t                  ; 显示被引用次数
            ))

    (use-package! company-lsp
      :commands company-lsp)

    (use-package! lsp-mode
      :hook (
             (web-mode . lsp)
             (typescript-mode . lsp)
             (rjsx-mode . lsp)
             (java-mode . lsp)
             (javascript-mode . lsp)
             (js2-mode . lsp)
             (python-mode . lsp)
             (go-mode . lsp)
             (css-mode . lsp)
             )
      :commands lsp
      :config
      (setq lsp-idle-delay 0.500
            lsp-enable-file-watchers nil))

    (use-package! lsp-java
      :config (add-hook 'java-mode-hook 'lsp))
    (use-package! dap-mode
      :after lsp-mode
      :config (dap-auto-configure-mode))
    (use-package! dap-java
      :ensure nil)

    ;; 关闭自动格式化，全局关闭
    ;; (setq +form-with-lsp nil)
    ;; 指定模式
    (setq-hook! 'typescript-mode-hook +format-with-lsp nil)
    (setq-hook! 'typescript-tsx-mode-hook +format-with-lsp nil)

2021-02-23 11:17:14

增加 vls:

``$ npm install -g vls``

``emacs: lsp-install-server -> vls``

3.4.10 org-mode
^^^^^^^^^^^^^^^

.. code:: common-lisp

    (add-hook 'org-mode-hook
              (lambda () (display-line-numbers-mode -1)))

    (use-package! org-fancy-priorities
      :diminish
      :ensure t
      :hook (org-mode . org-fancy-priorities-mode)
      :config
      (setq org-fancy-priorities-list '("🅰" "🅱" "🅲" "🅳" "🅴")))

    (use-package! org-pretty-tags
      :diminish org-pretty-tags-mode
      :ensure t
      :config
      (setq org-pretty-tags-surrogate-strings
            '(("work"  . "⚒")))

      (org-pretty-tags-global-mode))

3.4.11 parrot, 多单词切换
^^^^^^^^^^^^^^^^^^^^^^^^^

开启全局模式：

.. code:: common-lisp

    (use-package! parrot
      :config
      (parrot-mode))

具体切换数据配置：

.. code:: common-lisp

    (setq parrot-rotate-dict
          '(
            (:rot ("alpha" "beta") :caps t :lower nil)
            ;; => rotations are "Alpha" "Beta"

            (:rot ("snek" "snake" "stawp"))
            ;; => rotations are "snek" "snake" "stawp"

            (:rot ("yes" "no") :caps t :upcase t)
            ;; => rotations are "yes" "no", "Yes" "No", "YES" "NO"

            (:rot ("&" "|"))
            ;; => rotations are "&" "|"
            ;; default dictionary starts here ('v')
            (:rot ("begin" "end") :caps t :upcase t)
            (:rot ("enable" "disable") :caps t :upcase t)
            (:rot ("enter" "exit") :caps t :upcase t)
            (:rot ("forward" "backward") :caps t :upcase t)
            (:rot ("front" "rear" "back") :caps t :upcase t)
            (:rot ("get" "set") :caps t :upcase t)
            (:rot ("high" "low") :caps t :upcase t)
            (:rot ("in" "out") :caps t :upcase t)
            (:rot ("left" "right") :caps t :upcase t)
            (:rot ("min" "max") :caps t :upcase t)
            (:rot ("on" "off") :caps t :upcase t)
            (:rot ("prev" "next"))
            (:rot ("start" "stop") :caps t :upcase t)
            (:rot ("true" "false") :caps t :upcase t)
            (:rot ("&&" "||"))
            (:rot ("==" "!="))
            (:rot ("===" "!=="))
            (:rot ("." "->"))
            (:rot ("if" "else" "elif"))
            (:rot ("ifdef" "ifndef"))
            ;; javascript
            (:rot ("var" "let" "const"))
            (:rot ("null" "undefined"))
            (:rot ("number" "object" "string" "symbol"))

            ;; c/...
            (:rot ("int8_t" "int16_t" "int32_t" "int64_t"))
            (:rot ("uint8_t" "uint16_t" "uint32_t" "uint64_t"))
            (:rot ("1" "2" "3" "4" "5" "6" "7" "8" "9" "10"))
            (:rot ("1st" "2nd" "3rd" "4th" "5th" "6th" "7th" "8th" "9th" "10th"))

            ;; org
            (:rot ("DONE" "DOING" "WAITING" "PENDING"))
            (:rot ("increment", "decrement"))

            ))

3.4.12 Plaintext
^^^^^^^^^^^^^^^^

.. code:: common-lisp

    ;; 出文本模式下，开启拼写检查
    (set-company-backend!
      '(text-mode
        markdown-mode
        gfm-mode)
      '(:seperate
        company-ispell
        company-files
        company-yasnippet))

    (after! text-mode
      (add-hook! 'text-mode-hook
                 ;; Apply ANSI color codes
                 (with-silent-modifications
                   (ansi-color-apply-on-region (point-min) (point-max)))))

3.4.13 ranger
^^^^^^^^^^^^^

.. code:: common-lisp

    ;; ranger
    (after! ranger
      :config
      (setq ranger-show-literal nil))

3.4.14 search & replace
^^^^^^^^^^^^^^^^^^^^^^^

``C-c r``

.. code:: common-lisp

    (use-package! visual-regexp
      :commands (vr/select-replace vr/select-query-replace))

    (use-package! visual-regexp-steriods
      :commands (vr/select-replace vr/select-query-replace))

3.4.15 smartparen
^^^^^^^^^^^^^^^^^

.. code:: common-lisp

    (sp-local-pair
     '(org-mode)
     "<<" ">>"
     :actions '(insert))

3.4.16 treemacs
^^^^^^^^^^^^^^^

忽略某些类型的文件：

.. code:: common-lisp

    (setq treemacs-file-ignore-extensions
          '(;; LaTeX
            "aux"
            "ptc"
            "fdb_latexmk"
            "fls"
            "synctex.gz"
            "toc"
            ;; LaTeX - glossary
            "glg"
            "glo"
            "gls"
            "glsdefs"
            "ist"
            "acn"
            "acr"
            "alg"
            ;; LaTeX - pgfplots
            "mw"
            ;; LaTeX - pdfx
            "pdfa.xmpi"
            ))
    (setq treemacs-file-ignore-globs
          '(;; LaTeX
            "*/_minted-*"
            ;; AucTeX
            "*/.auctex-auto"
            "*/_region_.log"
            "*/_region_.tex"))

3.4.17 web-mode
^^^^^^^^^^^^^^^

prettier-js 配置

hydra 配置

.. code:: common-lisp

    ;; web-mode hydra
    (defhydra hydra-web-mode (:color blue :quit-key "q" :hint nil)
      "
    ^Element^                       ^Element^                       ^Attribute^             ^Block
    ^^^^^^^^---------------------------------------------------------------------------------------------
    _a_ : Select content            _r_ : Rename                    _0_ : Start             _<_ : Begin
    _b_ : Start                     _s_ : Select                    _9_ : End               _>_ : End
    _c_ : Clone                     _t_ : Move Down                 _*_ : Insert            _-_ : Select
    _e_ : End                       _u_ : Parent                    _N_ : Next
    _f_ : Fold/unfold children      _v_ : Delete without content    _P_ : Previous                  _k_
    _i_ : Insert                    _w_ : Wrap Element              _S_ : Select                _h_      _l_
    _I_ : Insert cursor             _t_ : Last(open/close)          _X_ : Delete                    _j_
    _K_ : Delete                    _T_ : Next(open/close)          _M_ : Match tag
    _n_ : Next                      _._ : Wrap Markup               _A_ : Sort
    _p_ : Previous
    "
      ("a" web-mode-element-content-select)
      ("b" web-mode-element-beginning :exit nil)
      ("c" web-mode-element-clone)
      ("e" web-mode-element-end :exit nil)
      ("f" web-mode-element-children-fold-or-unfold :exit nil)
      ("F" web-mode-fold-unfold :exit nil)
      ("i" web-mode-element-insert)
      ("I" web-mode-element-insert-at-point)
      ("K" web-mode-element-kill)
      ("m" web-mode-element-mute-blanks)
      ("n" web-mode-element-next :color "pink" :exit nil)
      ("p" web-mode-element-previous :color "pink" :exit nil)
      ("r" web-mode-element-rename)
      ("s" web-mode-element-select)
      ("t" web-mode-element-transpose)
      ("u" web-mode-element-parent :color "pink" :exit nil)
      ("v" web-mode-element-vanish)
      ("w" web-mode-element-wrap)
      ("t" web-mode-tag-previous :color "pink" :exit nil)
      ("T" web-mode-tag-next :color "pink" :exit nil)
      ("." emmet-wrap-with-markup)
      ("q" nil "quit" :exit t)
      ;; attribute
      ("0" web-mode-attribute-beginning :exit nil)
      ("9" web-mode-attribute-end :exit nil)
      ("*" web-mode-attribute-insert)
      ("X" web-mode-attribute-kill)
      ("A" web-mode-tag-attributes-sort :exit nil)
      ("K" web-mode-element-kill)
      ("M" web-mode-tag-match :exit nil :color "pink")
      ("N" web-mode-attribute-next :exit nil :color "pink")
      ("P" web-mode-attribute-previous :exit nil :color "pink")
      ("S" web-mode-attribute-select)
      ;; block
      ("<" web-mode-block-next :exit nil :color "pink")
      (">" web-mode-block-previous :exit nil :color "pink")
      ("-" web-mode-block-select)
      ;; movement
      ("j" next-line :exit nil :color "blue")
      ("k" previous-line :exit nil :color "blue")
      ("h" backward-char :exit nil :color "blue")
      ("l" forward-char :exit nil :color "blue")
      )

3.4.18 which-key
^^^^^^^^^^^^^^^^

过滤掉 ``evilem--`` 开头的指令：

.. code:: common-lisp

    (setq which-key-idle-delay 0.5)

    (setq which-key-allow-multiple-replacements t)
    (after! which-key
      (pushnew!
       which-key-replacement-alist
       '(("" . "\\`+?evil[-:]?\\(?:a-\\)?\\(.*\\)") . (nil . "◂\\1"))
       '(("\\`g s" . "\\`evilem--?motion-\\(.*\\)") . (nil . "◃\\1"))
       ))

3.4.19 yasnippets
^^^^^^^^^^^^^^^^^

参考链接：

::

    1. `snippet-development <https://joaotavora.github.io/yasnippet/snippet-development.html>`_


.. code:: common-lisp

    (setq yas-triggers-in-field t)


    (use-package! doom-snippets             ; hlissner
      :after yasnippet)

    (use-package! yasnippet-snippets        ; AndreaCrotti
      :after yasnippet)

3.4.20 smart-hungry-delete
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: common-lisp

    ;; hungry delete
    (use-package! smart-hungry-delete
      :ensure t
      :bind (("<backspace>" . smart-hungry-delete-backward-char)
             ("C-d" . smart-hungry-delete-forward-char))
      :defer nil ;; dont defer so we can add our functions to hooks
      :config (smart-hungry-delete-add-default-hooks)
      )

3.4.21 multiple cursors
^^^^^^^^^^^^^^^^^^^^^^^

.. code:: common-lisp

    ;; multiple cursors hydra
    (defhydra hydra-multiple-cursors (:color blue :hint nil)
      "
     Up^^             Down^^           Miscellaneous           % 2(mc/num-cursors) cursor%s(if (> (mc/num-cursors) 1) \"s\" \"\")
    ------------------------------------------------------------------
     [_p_]   Next     [_n_]   Next     [_l_] Edit lines  [_0_] Insert numbers
     [_P_]   Skip     [_N_]   Skip     [_a_] Mark all    [_A_] Insert letters
     [_M-p_] Unmark   [_M-n_] Unmark   [_s_] Search
     [Click] Cursor at point       [_q_] Quit"
      ("l" mc/edit-lines :exit t)
      ("a" mc/mark-all-like-this :exit t)
      ("n" mc/mark-next-like-this)
      ("N" mc/skip-to-next-like-this)
      ("M-n" mc/unmark-next-like-this)
      ("p" mc/mark-previous-like-this)
      ("P" mc/skip-to-previous-like-this)
      ("M-p" mc/unmark-previous-like-this)
      ("s" mc/mark-all-in-region-regexp :exit t)
      ("0" mc/insert-numbers :exit t)
      ("A" mc/insert-letters :exit t)
      ("<mouse-1>" mc/add-cursor-on-click)
      ;; Help with click recognition in this hydra
      ("<down-mouse-1>" ignore)
      ("<drag-mouse-1>" ignore)
      ("q" nil))

3.4.22 scrollkeeper
^^^^^^^^^^^^^^^^^^^

.. code:: common-lisp

    (use-package! scrollkeeper)
    (global-set-key [remap scroll-up-command] #'scrollkeeper-contents-up)
    (global-set-key [remap scroll-down-command] #'scrollkeeper-contents-down)

3.5 开发配置
~~~~~~~~~~~~

3.5.1 WEB 开发
^^^^^^^^^^^^^^

设置 js/html/css 模式缩进

.. code:: common-lisp

    ;; web 开发配置
    (setq css-indent-offset 2
          js2-basic-offset 2
          js-switch-indent-offset 2
          js-indent-level 2
          js2-mode-show-parse-errors nil
          js2-mode-show-strict-warnings nil
          web-mode-attr-indent-offset 2
          web-mode-code-indent-offset 2
          web-mode-css-indent-offset 2
          web-mode-markup-indent-offset 2
          web-mode-enable-current-element-highlight t
          web-mode-enable-current-column-highlight t)
    (setq-default typescript-indent-level 2)

    (use-package! rjsx-mode)
    (use-package! import-js
      :defer t
      :init
      (add-hook! (js2-mode rjsx-mode) (run-import-js))
      (add-hook! (js2-mode rjsx-mode)
        (add-hook 'after-save-hook #'import-js-fix nil t)))
    (advice-add '+javascript|cleanup-tide-processes :after 'kill-import-js)

3.6 FIXs
~~~~~~~~

.. code:: common-lisp

    ;; fix true/false symbols for javascript
    ;; (setq +pretty-code-enabled-modes nil)
    ;; or
    ;; (remove-hook 'after-change-major-mode-hook #'+pretty-code-init-pretty-symbols-h)
    ;; (defun setup-js2-prettify-symbols ()
    ;;   "Set prettify symbols alist."
    ;;   (interactive)
    ;;   (setq prettify-symbols-alist '(("lambda" . "λ")
    ;;                                  ("->" . "→")
    ;;                                  ("!=" . "≠")
    ;;                                  ("<=" . "≤")
    ;;                                  (">=" . "≥")
    ;;                                  ("=<<" . "=≪")
    ;;                                  ("!" . "￢")
    ;;                                  ("null" . "∅")
    ;;                                  ("function" . "ƒ")
    ;;                                  (">>=" . "≫=")))
    ;;   (delete '("false" . "𝔽") prettify-symbols-alist)
    ;;   (delete '("true" . "𝕋") prettify-symbols-alist)
    ;;   (prettify-symbols-mode -1)
    ;;   )

    ;; (add-hook! 'js2-mode-hook 'setup-js2-prettify-symbols)

4 包管理(packages.el)
---------------------

This file shouldn’t be byte compiled.

.. code:: common-lisp

    ;; -*- no-byte-compile: t; -*-

辅助/管理类：

.. code:: common-lisp

    (package! smart-hungry-delete)
    (package! move-text)
    (package! parrot)
    ;; fast, friendly searching with ripgrep and Emacs
    (package! deadgrep)
    (package! ranger)
    (package! youdao-dictionary)
    (package! link-hint)
    (package! deft)
    (package! anzu)
    (package! visual-regexp)
    (package! visual-regexp-steriods
      :recipe (:host github :repo "benma/visual-regexp-steroids.el"))
    (package! osx-lib)
    (package! crux)
    (package! string-inflection)
    (package! pangu-spacing)
    (package! cnfonts)
    (package! valign)

    ;; org
    (package! org-fancy-priorities)
    (package! org-pretty-tags)
    ;; (package! org-special-block-extras)

编程类：

.. code:: common-lisp

    ;; development
    (package! leetcode)
    (package! instant-rename-tag
      :recipe (:host github :repo "manateelazycat/instant-rename-tag"))
    (package! js-doc)
    (package! imenu-list)
    (package! yasnippet-snippets)

    ;; web dev
    ;; (package! ob-typescript)
    (package! web-beautify)
    (package! prettier-js)
    (package! import-js :disable t)
    (package! tide :disable t)
    (package! eldoc :disable t)

    ;; java
    (package! lsp-java)
    (package! dap-mode)

    (package! ox-rst
      :recipe (:host github :repo "msnoigrs/ox-rst"))
    (package! scrollkeeper
      :recipe (:host github :repo "alphapapa/scrollkeeper.el"))

5 问题
------

1. lsp 不能识别 webpack/vite 别名？

   `Default FAQ js(ts)config for webpack aliases doesn’t work. · Issue #890 ·
   vuejs/vetur <https://github.com/vuejs/vetur/issues/890>`_

   .. code:: diff

          {
           "compilerOptions": {
               "target": "esnext",
               "module": "esnext",
               "moduleResolution": "node",
               "strict": true,
               "jsx": "preserve",
               "sourceMap": true,
               "resolveJsonModule": true,
               "esModuleInterop": true,
               "lib": ["esnext", "dom"],
       +        "paths": {
       +          "@/*": ["src/*"]
       +        }
             },
           "include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"]
           }

   在 ``tsconfig.json`` 中增加一项配置 ``paths`` 告诉 lsp 别名含义。
