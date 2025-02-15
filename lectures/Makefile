DEDUCE_DIR=/Users/jsiek/deduce
LIB_DIR=$(DEDUCE_DIR)/lib
PYTHON=/opt/homebrew/bin/python3.11
DEDUCE = $(DEDUCE_DIR)/deduce.py

%.png: %.dot
	dot $< -Tpng > $@

%.pdf: %.dot
	dot $< -Tpdf > $@

%.html: %.md
	pandoc --mathml $< -o $@

DOT = digraph1.dot digraph2.dot \
    digraph3.dot digraph4.dot digraph5.dot \
	digraph6.dot digraph7.dot digraph8.dot digraph9.dot digraph10.dot \
	digraph11.dot digraph12.dot digraph13.dot digraph14.dot \
	graph1.dot graph2.dot graph3.dot graph4.dot graph5.dot digraph15.dot


PNG = $(DOT:%.dot=%.png)

PDF = $(DOT:%.dot=%.pdf)

graphs: ${PNG}

all: graphs countingsort bubblesort activity_selection quiz2 SCC

countingsort:
	~/bin/lmt lecture-notes.md
	javac src/CountingSort.java
	cd src ; java -ea -cp . CountingSort

bubblesort:
	~/bin/lmt lecture-notes.md
	javac src/BubbleSort.java
	cd src ; java -ea -cp . BubbleSort

activity_selection:
	~/bin/lmt lecture-notes.md
	javac src/ActivitySelection.java
	cd src ; java -ea -cp . ActivitySelection

quiz2:
	~/bin/lmt lecture-notes.md
	javac src/Quiz2.java
	cd src ; java -ea -cp . Quiz2

SCC:
	~/bin/lmt lecture-notes.md
	javac src/StronglyConnectedComponents.java
	cd src ; java -ea -cp . StronglyConnectedComponents

BFS:
	~/bin/lmt lecture-notes.md
	javac src/BreadthFirstSearch.java
	cd src ; java -ea -cp . BreadthFirstSearch

BellmanFord:
	~/bin/lmt lecture-notes.md
	javac src/BellmanFord.java
	cd src ; java -ea -cp . BellmanFord

rev_rev:
	cat Revrev.md | sed 's/deduce\^/deduce /' > RevRev.en
	/Users/jsiek/Library/Python/3.11/bin/entangled tangle 
	$(PYTHON) $(DEDUCE_DIR)/deduce.py RevRev.pf --dir $(LIB_DIR)

deduce_programming:
	cat deduce-programming.md | sed 's/deduce\^/deduce /' > deduce-programming.en
	/Users/jsiek/Library/Python/3.11/bin/entangled tangle 
	$(PYTHON) $(DEDUCE) DeduceProgramming1.pf --dir $(LIB_DIR)
	$(PYTHON) $(DEDUCE) DeduceProgramming2.pf --dir $(LIB_DIR)

deduce_intro_proof:
	cat deduce-intro-proof.md | sed 's/deduce\^/deduce /' > deduce-intro-proof.en
	cat deduce-more-proof.md | sed 's/deduce\^/deduce /' > deduce-more-proof.en
	/Users/jsiek/Library/Python/3.11/bin/entangled tangle 
	$(PYTHON) $(DEDUCE) DeduceIntroProof.pf --dir $(LIB_DIR)
	$(PYTHON) $(DEDUCE) DeduceIntroProof2.pf --dir $(LIB_DIR)

logic_and_sets:
	cat LogicAndSets.md | sed 's/deduce\^/deduce /' > LogicAndSets.en
	/Users/jsiek/Library/Python/3.11/bin/entangled tangle 
	$(PYTHON) $(DEDUCE_DIR)/deduce.py LogicAndSets.pf --dir $(LIB_DIR)

