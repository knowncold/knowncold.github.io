---
title : Scheme笔记
layout : wiki_page
category : wiki
---

### 安装ChezScheme

	git clone
	sudo apt-get install libncurses5-dev
	./configure
	make
	sudo make install

##### 定义atom?
	;
	(define atom?
		(lambda (x)
			(and (not (pair? x)) (not (null? x)))))
	(defun atom? (x)
		(not (listp x)))	
	
() is a list not an atom

##### (car x)

	argument 
		non-empty lists
	result
		the first S-exp of the list

##### (cdr x)

	argument
		non-empty lists
	result
		a list

##### (cons s l)

	argument
		any S-exp
		any list
	result
		a list
		



