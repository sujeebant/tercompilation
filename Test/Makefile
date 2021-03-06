#
# Mettre la liste des fichiers .ml et .mli
# constituant le projet. Si un fichier b.ml dépend d'un fichier
# a.ml, a.ml doit se trouver avant dans la liste.
#

SOURCES = parser.mly lexer.mll main.ml

# Répertoires contenant les tests

# TEST_DIRECTORIES = print arith let letin if

# Nom du binaire

EXEC = deca

# Compilateurs

CAMLC = ocamlc -g -annot
CAMLOPT = ocamlopt
CAMLDEP = ocamldep
CAMLLEX = ocamllex
CAMLYACC = ocamlyacc


# Sources

SRC_MLL = $(filter %.mll, $(SOURCES))
SRC_MLY = $(filter %.mly, $(SOURCES))
SMLIY = $(SOURCES:.mly=.ml)
SMLIYL = $(SMLIY:.mll=.ml)
SMLYL = $(filter %.ml,$(SMLIYL))
OBJS = $(SMLYL:.ml=.cmo)
OPTOBJS = $(OBJS:.cmo=.cmx)

# Tests

# TEST_SUBDIRECTORY = $(DIR)
# TEST_SRC = $(shell ls tests/$(TEST_SUBDIRECTORY)/*.ml)
# 
# TEST_SRC_ALL = $(foreach test_dir, $(TEST_DIRECTORIES), \
	         $(shell ls tests/$(test_dir)/*.ml))


# Cibles

all: depend $(EXEC)

opt: depend $(EXEC).opt


.SUFFIXES: .ml .mli .cmo .cmi .cmx .mll .mly

$(EXEC): $(OBJS)
	$(CAMLC) $(CUSTOM) -o $(EXEC) $(OBJS)

$(EXEC).opt: $(OPTOBJS)
	$(CAMLOPT) -o $(EXEC) $(OPTOBJS)


.ml.cmo:
	$(CAMLC) -c $<

.mli.cmi:
	$(CAMLC) -c $<

.ml.cmx:
	$(CAMLOPT) -c $<

.mll.cmo:
	$(CAMLLEX) $<
	$(CAMLC) -c $*.ml

.mll.cmx:
	$(CAMLLEX) $<
	$(CAMLOPT) -c $*.ml

.mly.cmo:
	$(CAMLYACC) $<
	$(CAMLC) -c $*.mli
	$(CAMLC) -c $*.ml

.mly.cmx:
	$(CAMLYACC) $<
	$(CAMLOPT) -c $*.mli
	$(CAMLOPT) -c $*.ml

.mly.cmi:
	$(CAMLYACC) $<
	$(CAMLC) -c $*.mli

.mll.ml:
	$(CAMLLEX) $<

.mly.ml:
	$(CAMLYACC) $<

test: all
	@$(foreach file, $(TEST_SRC), \
	echo "$(file)"; $(INTERPRET) $(file); echo "------ ------ ------ ------\n";)

test_all: all
	@$(foreach file, $(TEST_SRC_ALL), \
	echo "$(file)"; $(INTERPRET) $(file); echo "------ ------ ------ ------\n";)

clean:
	rm -f *.cm[iox] *~ .*~ *.o *.annot
	rm -f $(EXEC)
	rm -f $(EXEC).opt
	rm -f lexer.ml
	rm -f parser.ml
	rm -f parser.mli
	rm -f parser.output

depend: $(SML)
	$(CAMLDEP) $(SMLIY) $(SMLIY:.mly:.mli) > .depend

-include .depend
