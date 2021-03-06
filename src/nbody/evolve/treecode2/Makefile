#	TREECODE2:
include $(NEMOLIB)/makedefs
#	makefile for Lars' treecode2 version
#	This is the generic for non-recursive fortran compilers
#	but has the ascii I/O routines for use in NEMO
#
#   23-oct-91  proper Cray test

#  different OS version can be made by redefining the OS macro:
#	on sun's:	'make treecode2 OS=sun'
#	on vax:		'make treecode2 OS=vax'     (presumably)
#	on cray:	'make treecode2 OS='	    (presumably)
#		         and also all DOUBLE PRECISION should become REAL
#			 a proper tar export can be made with 'make cray'
#  make treecode2 OS= FFLAGS= CFLAGS= FC=cf77
#		(the Cray doesnt't use FC as macro)
OS=sun

FFLAGS = -O
CFLAGS = -O
L = $(NEMOLIB)/libnemo.a

ALLSRC = treecode.f treedefs.h treeutil.f treeutilsun.f treeutilvax.f \
  README Makefile runtreecode2
INC = treedefs.h
MAN = treecode2.1

SRC = treecode.f treeutil.f treeutil$(OS).f
OBJ = treecode.o treeutil.o treeutil$(OS).o

BINS = treecode0 treecode2

TAR = $(ALLSRC) $(INC) $(MAN) TREEBI TREEPAR

help:
	@echo No help yet

nemo_lib:
	@echo No nemo_lib in `pwd`

nemo_bin:	$(BINS)
	mv $? $(NEMOBIN)
	rm -f *.o

nemo_src:
	-@echo `pwd` treecode2
		
		
install:	.install_src .install_man

.install_src:	$(ALLSRC) $(INC)
	cp $? $(NEMO)/src/lars/treecode2
	@touch .install_src

.install_man:	$(MAN)
	cp $? $(NEMO)/man/man1
	@touch .install_man

treecode2: $(OBJ)
	$(FC) $(FFLAGS) -o treecode2 $(OBJ)

treecode0: treecode0.c
	$(CC) $(CFLAGS) -o treecode0 treecode0.c $(L) -lm
	
clean:
	rm -f $(BINS) core *.o TREELOG TREEBOA TREEBO 

cleanall: clean
	rm -fr TREE* tmp*

tar:
	@echo Creating some sort of TAR file with all the essentials
	@echo Plus a little test file. Assume 2 example TREEBI and TREEPAR
	@echo files are present.
	@tar cvf treecode2.tar $(TAR)

cray:
	@echo Tarfile for cray, all DOUBLE's become REAL
	@mkdir tmp
	-@for i in $(TAR); do\
	(sed "s/DOUBLE PRECISION/REAL/g" $$i > tmp/$$i);done
	@(cd tmp;tar cvf ../cray2.tar *;cd ..;rm -r tmp)
	@echo All done, file cray2.tar created
	

init:
	@echo "  ****** Standard BENCHMARK for treecode2 ******"
	@echo "Creating file with positions of the bodies in TREEBI"
	@-rm -f junk TREEBI TREEPAR
	@echo "Running hackcode1 to create standard input file ..."
	@-hackcode1 out=junk nbody=128 tstop=0 > /dev/null
	@echo "Converting it to ASCII (atos/stoa) format..."
	@-stoa junk TREEBI rformat=%22.14E
	@echo "Creating parameter file in TREEPAR"
	@echo "Standard hackcode1 benchmark" > TREEPAR
	@echo "64" >> TREEPAR
	@echo "8" >> TREEPAR
	@echo "1" >> TREEPAR
	@echo "0.03125" >> TREEPAR
	@echo "1" >> TREEPAR
	@echo "0.05" >> TREEPAR
	@echo ".FALSE." >> TREEPAR
	

bench: treecode2  
	@echo Running standard benchmark...
	@echo "[input files:  TREEPAR TREEBI]"
	@echo "[output files: TREELOG TREEBO TREEBOA]"
	@rm -f TREEBO TREEBOA TREELOG
	@time treecode2
	@tr D e < TREEBOA > tree.data

#	dependencies:

treecode.f: treedefs.h



