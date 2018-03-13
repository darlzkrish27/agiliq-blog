Django emacs setup
##################
:date: 2010-12-26 23:11:25
:modified: 2010-12-27 10:41:25+05:30
:slug: django-emacs-setup
:authors: thejaswi
:tags: emacs
:summary: My colleague Javed had written an article a short while back on `"Seven reasons why you should switch to Vim"`_. One of the coolest things at Agiliq is that we are free to use the development tools we are comfortable with. As a case to explain the point, everyone at team agiliq uses a different editor and if someday a flame war would escalate to the third world war, I assure you it would start from Agiliq ;-) Though I have been using emacs for more than two years now, I still consider myself an amateur. In this post, I

My colleague Javed had written a fantastic article a short while back on `"Seven reasons why you should switch to Vim"`_ and as an emacs_ user I thought I should share my setup too. One of the coolest things at Agiliq_ is that we are free to use the development tools of our choice. As a case to explain the point, everyone at agiliq uses a different editor and if someday a flame war would escalate to the third world war, I assure you it would start from Agiliq ;-)

Though I have been using emacs_ for more than two years now, I still am an amateur. In this post, I am not going to exhort you to switch to emacs but explain my setup.

If you are planning to give emacs a try, I would suggest you start with the inbuilt emacs tutorial (C-h t). After you've gone through the tutorial, start using emacs while noting your most repetitive actions. Later, bind these repetitive actions to suitable shortcuts so that you can maximize your productivity and reduce keystrokes. Whenever in doubt, refer to emacswiki_ for clarifications. Another wonderful reference would be `Steve Yegge's`_ articles on emacs.

Before I explain my setup, let me make it clear that most editors already have a lot of the functionality and I am not being judgemental in this post. If you've come here to see me bash other editors or proclaim that emacs is the best editor, you are going to be disappointed. Emacs has worked for me and a lot of other people and may work for you. Give a couple of editors out there in the market a try (atleast for a fortnight for each editor) and decide the one that you are most comfortable with.

I use emacs without a window system (emacs -nw) and have an alias entry in my .bashrc to load it in this mode by default.

Here are the contents on my .emacs.d/init.el file and I will explain them below line by line::

  ;; .emacs

  ;; enable visual feedback on selections
  (setq transient-mark-mode t)

  ;;; interfacing with ELPA, the package archive.
  ;;; Move this code earlier if you want to reference
  ;;; packages in your .emacs.
  (when
      (load
       (expand-file-name "~/.emacs.d/elpa/package.el"))
    (package-initialize))

  ;; My customizations
  (ido-mode 1)
  (icomplete-mode 1)
  (column-number-mode 1)
  (menu-bar-mode 1)

  (setq tags-file-name "~/TAGS")
  (global-set-key (kbd "<f5>") 'find-tag)
  (global-set-key (kbd "<f6>") 'rgrep)
  (global-set-key (kbd "<f7>") 'magit-status)

  (setq yas/root-directory "~/.emacs.d/snippets")
  (yas/load-directory yas/root-directory)

  (load (expand-file-name "~/.emacs.d/multi-term.el"))
  (require 'multi-term)
  (setq multi-term-program "/bin/bash")
  (multi-term)
  (defalias 'term 'multi-term)

  (defun build-tags ()
    (interactive)
    (call-process-shell-command 
        "find ~/django_projects/ -name '*.py' -print \" \
        "|xargs etags -o ~/TAGS" 
        nil (get-buffer-create "Etags Compile") 1)
    )
  (defalias 'etb 'build-tags)

  (defalias 'yes-or-no-p 'y-or-n-p)


The transient-mark-mode symbol when enabled gives feedback while marking regions (highlighting selected regions).

I use ELPA_ (Emacs Lisp Package Archive) for installing and managing my packages. Though currently, there are a few packages only in ELPA, they've been sufficient for me. There's another emacs package manager that I am looking forward to called epkg_ which will install packages from the EmacsMirror_.

I have a few modes enabled by default like ido-mode_ (for intelligent auto completion in the minibuffer), icomplete-mode_ (for helping me complete commands in the minibuffer without remembering the full names), column-number-mode (to display the column number at which the point is currently located, useful if you follow PEP-8 for python) and disable the menu-bar as I hardly use it. The menu-bar is a distraction especially when you are using emacs on a separate display in a maximized state.

I use `emacs tags`_ to jump between function calls and definitions very frequently and hence bound this invocation to the F5 key. One thing to note is that F5-F9 are generally unassigned and can be bound to your most frequently used actions.

I also search very often for files or content within files and use rgrep_ and this has been bound to the F6 key.

I use git a lot and hence use magit_, a fantastic emacs mode to interact with git_. This mode has been bound to the F7 key of the keyboard.

I am not a big fan of auto-complete but use some snippet based auto-completion using yasnippets_. It's very easy to write your own snippets and I've written lots of them for django and some dom elements.

Whenever I start emacs, I also start a shell and multi-term_ is very useful in this regard. It handles unicode and ncurses based output very well. This is where I run the django development server or deploy using the fabric scripts.

Apart from these elisp packages, I run erc_ (an IRC client written for emacs) occasionally to lurk on `#django`_. I manage my tasks using the org-mode_ and synchronize these tasks with my home workstation using the mobileorg_ android app and dropbox_.

As you would've observed my emacs setup is fairly minimal but suffices most of my django and python development. If you are an emacs user and have tips for me and other users, leave your comments. There's always a lot to learn in emacs :)

.. _`"Seven reasons why you should switch to Vim"`: http://agiliq.com/blog/2010/11/seven-reasons-why-you-should-switch-to-vim-for-dja/
.. _Agiliq: http://agiliq.com/
.. _emacs: http://en.wikipedia.org/wiki/emacs
.. _emacswiki: http://emacswiki.org
.. _`Steve Yegge's`: http://duckduckgo.com/?q=steve+yegge+emacs
.. _ELPA: http://tromey.com/elpa/
.. _epkg: https://github.com/emacsmirror/epkg
.. _EmacsMirror: https://github.com/emacsmirror
.. _ido-mode: http://www.emacswiki.org/emacs/InteractivelyDoThings
.. _icomplete-mode: http://www.emacswiki.org/emacs/IcompleteMode
.. _`emacs tags`: http://www.emacswiki.org/emacs/EmacsTags
.. _rgrep: http://www.emacswiki.org/emacs/GrepMode
.. _magit: http://www.emacswiki.org/emacs/Magit
.. _git: http://git-scm.com
.. _yasnippets: http://code.google.com/p/yasnippet/
.. _multi-term: http://www.emacswiki.org/emacs/download/multi-term.el
.. _erc: http://www.emacswiki.org/emacs/ERC
.. _org-mode: http://orgmode.org/org.html
.. _mobileorg: http://mobileorg.ncogni.to/
.. _dropbox: http://www.dropbox.com
.. _`#django`: irc://irc.freenode.net/django

