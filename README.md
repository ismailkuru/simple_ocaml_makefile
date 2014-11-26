# simple_ocaml_makefile

A simple Makefile. Requires `ocamlfind`.

  * Make a build in directory e.g. `build`
  * Create a Makefile in `build`
  * In the Makefile, `include Makefile.include`
  * Specify the source directory: `SRC_DIR=../src` if your sources are in `../src` relative to `build`
  * The `.ml` and `.mli` files will be linked into the build dir as the first stage of the build
  * Specify the `.ml` files that should be compiled into executables, if required
  
```
MLS_TO_EXEC:=p1_examples.ml p1_gen.ml # executables
```

  * Specify the name of the library if required

```
LIB=p1 # libraries will be called p1.cma and p1.cmxa
```

  * Specify the `.ml` files you want to include in the lib e.g. 

```
IGNORE_FOR_LIB:=$(MLS_TO_EXEC) interactive.ml # don't want these in the lib
MLS_FOR_LIB:=$(shell ocamlfind ocamldep -sort *.ml)
MLS_FOR_LIB:=$(filter-out $(IGNORE_FOR_LIB), $(MLS_FOR_LIB))
```

  * If required, specify any includes using e.g.

```
CAMLCINCLUDES=str.cma
CAMLOPTINCLUDES=str.cmxa
```

  * Specify the default make target in your Makefile. `before_all` links the files and computes the `.depend` file:
  
```
CMO=$(MLS_FOR_LIB:.ml=.cmo)
all:
  make before_all
  make $(CMO) $(LIB).cma $(LIB).cmxa $(MLS_TO_EXEC:.ml=.native)
```
