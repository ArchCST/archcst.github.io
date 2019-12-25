title: org speed commands 快捷键综合  
date: 2019-02-19  
updated:  
comments: true  
tags:  

-   learn

layout: post  

---

# Outline Navigation

---

上/下/左/右 可见的标题  
n   (org-speed-move-safe (quote org-next-visible-heading))  
p   (org-speed-move-safe (quote org-previous-visible-heading))  
f   (org-speed-move-safe (quote org-forward-heading-same-level))  
b   (org-speed-move-safe (quote org-backward-heading-same-level))  

F   org-next-block  
B   org-previous-block  
u   (org-speed-move-safe (quote outline-up-heading))  
j   org-goto  
g   (org-refile t)  

Outline Visibility  

---

c   org-cycle  
C   org-shifttab  
    org-display-outline-path  
s   org-narrow-to-subtree  
=   org-columns  

Outline Structure Editing  

---

U   org-metaup  
D   org-metadown  
r   org-metaright  
l   org-metaleft  
R   org-shiftmetaright  
L   org-shiftmetaleft  
i   (progn (forward-char 1) (call-interactively (quote org-insert-heading-respect-content)))  
^   org-sort  
w   org-refile  
a   org-archive-subtree-default-with-confirmation  
@   org-mark-subtree  

Clock Commands  

---

I   org-clock-in  
O   org-clock-out  

Meta Data Editing  

---

t   org-todo  
,   (org-priority)  
0   (org-priority 32)  
1   (org-priority 65)  
2   (org-priority 66)  
3   (org-priority 67)  

    org-set-tags-command

e   org-set-effort  
E   org-inc-effort  
W   (lambda (m) (interactive "sMinutes before warning: ") (org-entry-put (point) "APPT<sub>WARNTIME</sub>" m))  

Agenda Views etc  

---

v   org-agenda  
/   org-sparse-tree  

Misc  
-&#x2014;  
o   org-open-at-point  
?   org-speed-command-help  
<   (org-agenda-set-restriction-lock (quote subtree))  
>   (org-agenda-remove-restriction-lock)  

[back]  
