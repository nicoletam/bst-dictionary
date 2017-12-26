# bst-dictionary
BST Dictionary to correct spelling errors


; A Token is a Str that contains only lowercase letters.

(define-struct wnode (rank word misspellings left right))
; A WNode is a (make-wnode Nat Token (listof Token) BST-Dictionary BST-Dictionarye)
; Requires: rank is a positive integer representing the frequency of word as it is used in the English language
; misspellings is a list of common misspellings of word

; A BST-Dictionary is one of
; * empty
; * (make-wnode Nat Token (listof Token) BST-Dictionary BST-Dictionary)
;	where the rank of all tokens in left are less than rank of root
;	and the rank of all tokens in right are greater than rank of root
;	and all rank values are unique (no duplicates)
;	and no Token appearing as a word in BST-Dictionary will occur
;	in a misspelling list. 


;; Sample data:
(define bst-a
  (make-wnode 46 "yes" (list "eyes" "eys") empty empty))
(define bst-b
  (make-wnode 88 "waterloo" (list "meme" "wtarloo") empty
	(make-wnode 3 "zzz" (list "z")
	empty
	empty)))
(define bst-c
  (make-wnode 29 "sleep" (list "slep")
	(make-wnode 44 "snooze" (list "sz")
	empty
	empty)
	empty))
(define bst-d
  (make-wnode 38 "there" (list "ther" "theer" "th")
	(make-wnode 1 "the" (list "teh" "th")
	empty
	empty)
	(make-wnode 72 "then" (list "thn" "ther")
	empty
	empty)))

;; (reverse-rank) consumes a BST
;; and outputs a list that prints the BST
;; in reverse order by appending
;; a list from the inside out
;; Requires: a valid BST
;; reverse-rank: BST-Dictionary -> (listof Str)
;; Examples:
(check-expect (reverse-rank-order bst-a) (list "yes"))

(define (reverse-rank-order BST-dictionary)
  (cond [(empty? BST-dictionary) empty]
	[else (append (reverse-rank-order (wnode-right BST-dictionary))
	(list (wnode-word BST-dictionary))
	(reverse-rank-order (wnode-left BST-dictionary)))]))

;; Tests:

(check-expect (reverse-rank-order empty) empty)
(check-expect (reverse-rank-order bst-a) (list "yes"))
(check-expect (reverse-rank-order bst-b) (list "zzz" "waterloo"))
(check-expect (reverse-rank-order bst-c) (list "sleep" "snooze"))
(check-expect (reverse-rank-order bst-d) (list "then" "there" "the"))

