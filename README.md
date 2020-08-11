# emacs-config
Emacs init.el
===

(require 'package)
(add-to-list 'package-archives '("melpa" . "https://melpa.org/packages/") t)
(package-initialize)

(require 'use-package)

;; backup files
(let ((backup-dir "~/.emacstmp/emacs/backups")
      (auto-saves-dir "~/.emacstmp/emacs/auto-saves/"))
  (dolist (dir (list backup-dir auto-saves-dir))
    (when (not (file-directory-p dir))
      (make-directory dir t)))
  (setq backup-directory-alist `(("." . ,backup-dir))
        auto-save-file-name-transforms `((".*" ,auto-saves-dir t))
        auto-save-list-file-prefix (concat auto-saves-dir ".saves-")
        tramp-backup-directory-alist `((".*" . ,backup-dir))
        tramp-auto-save-directory auto-saves-dir))
(setq backup-by-copying t    ; Don't delink hardlinks                           
      delete-old-versions t  ; Clean up the backups                             
      version-control t      ; Use version numbers on backups,                  
      kept-new-versions 5    ; keep some new versions                           
      kept-old-versions 2)   ; and some old ones, too 

;; docker
(require 'docker-compose-mode)
(require 'dockerfile-mode)
(add-to-list 'auto-mode-alist '("Dockerfile\\'" . dockerfile-mode))

;; solarized
(load-theme 'solarized-zenburn t)

;; rainbow mode
;;(require 'rainbow-delimiters)
(add-hook 'clojure-mode-hook 'prism-mode)

;; smartparens
(use-package smartparens
  :defer t
  :diminish ""
  :bind (("M-9" . sp-backward-sexp)
         ("M-0" . sp-forward-sexp))
  :init
  (progn
    (add-hook 'prog-mode-hook #'turn-on-smartparens-mode)
    (add-hook 'clojure-mode-hook #'turn-on-show-smartparens-mode))
  :config
  (progn
    (add-to-list 'sp-sexp-suffix '(json-mode regex ""))
    (add-to-list 'sp-sexp-suffix '(es-mode regex ""))
    (use-package smartparens-config)
    (define-key sp-keymap (kbd "C-(") 'sp-forward-barf-sexp)
    (define-key sp-keymap (kbd "C-)") 'sp-forward-slurp-sexp)
    (define-key sp-keymap (kbd "M-(") 'sp-forward-barf-sexp)
    (define-key sp-keymap (kbd "M-)") 'sp-forward-slurp-sexp)
    (define-key sp-keymap (kbd "C-<right>") 'sp-forward-sexp)
    (define-key sp-keymap (kbd "C-<left>") 'sp-backward-sexp)
    (sp-with-modes '(html-mode sgml-mode)
      (sp-local-pair "<" ">"))
    (sp-with-modes sp--lisp-modes
		   (sp-local-pair "(" nil :bind "C-("))))

;; web tools
(require 'web-mode)
(add-to-list 'auto-mode-alist '("\\.phtml\\'" . web-mode))
(add-to-list 'auto-mode-alist '("\\.tpl\\.php\\'" . web-mode))
(add-to-list 'auto-mode-alist '("\\.[agj]sp\\'" . web-mode))
(add-to-list 'auto-mode-alist '("\\.as[cp]x\\'" . web-mode))
(add-to-list 'auto-mode-alist '("\\.erb\\'" . web-mode))
(add-to-list 'auto-mode-alist '("\\.mustache\\'" . web-mode))
(add-to-list 'auto-mode-alist '("\\.djhtml\\'" . web-mode))
(add-to-list 'auto-mode-alist '("\\.html?\\'" . web-mode))

;; ui customise
(tool-bar-mode -1)
(menu-bar-mode -1)
(add-to-list 'default-frame-alist
             '(vertical-scroll-bars . nil))
(add-to-list 'default-frame-alist '(fullscreen . maximized))

;; helm
(require 'helm-config)
(helm-mode 1)

(custom-set-variables
 ;; custom-set-variables was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 '(package-selected-packages
   (quote
    (groovy-mode docker-compose-mode dockerfile-mode prism bind-key cider web-mode mustache-mode helm smartparens clojure-mode solarized-theme))))
(custom-set-faces
 ;; custom-set-faces was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 )
