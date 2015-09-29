Akiss
=====

Akiss is a tool for checking trace equivalence of security protocols.
It works in the so-called symbolic model, representing protocols by
processes in the applied pi-calculus, and allowing the user to describe
various security primitives by an equational theory. In order to show
that two processes are trace equivalent, Akiss derives a complete set
of tests for each trace of each process, using a saturation procedure
that performs ordered resolution with selection.

As an experimental feature, this version supports one AC connective,
namely "plus". It comes with modifications of the resolution rules that
help reach saturation, mostly in the case where "plus" represents
exclusive or.


Build
-----

You will need OCaml; version 4.01 is known to work.

The AC feature of Akiss also requires two external tools:

 * [tamarin-prover](http://www.infsec.ethz.ch/research/software/tamarin.html) (branch feature-ac-rewrite-rules)
 * [maude](http://maude.cs.illinois.edu/w/index.php?title=The_Maude_System) (version 2.6 or 2.7)

You shouldn't need them if you don't use the feature.

For parallelization, Akiss needs the following library:

 * [nproc](https://github.com/MyLifeLabs/nproc)

This dependency is optional; when it is not available, Akiss will run
sequentially.

To build, just run `make`.


Usage
-----

    Usage: akiss [options] < specification-file.api
      --verbose Enable verbose output
      -verbose Enable verbose output
      -debug Enable debug output
      --debug Enable debug output
      --output-dot <file>  Output statement graph to <file>
      -j <n>  Use <n> parallel jobs (if supported)
      --ac-compatible Use the AC-compatible toolbox even on non-AC theories
                      (experimental, needs maude and tamarin)
      --check-generalizations Check that generalizations of kept statements
                              are never dropped.
      -help  Display this list of options
      --help  Display this list of options

For example:

    ./akiss --verbose < examples/strong-secrecy/blanchet.api


Source tree
-----------

Here is a quick guide to the organization of the source code:

 * `util.ml`: misc utilities
 * `ast.ml`, `parser.mly`, `lexer.mll`: parsing of API files
 * `config.ml`: detects external tools
 * `term.ml`: term structure and basic operations on them
 * `maude.ml`: interface with maude
 * `lextam.mll`, `parsetam.mly`, `tamarin.ml`: interface with tamarin
 * `rewriting.ml`: unification and variants for non-AC theories
 * `theory.ml`: process first half of API file, setting up the theory and
   appropriate rewriting toolbox
 * `base.ml`: data structure for the saturation algorithm
 * `horn.ml`: the saturation procedure itself
 * `process.ml`: processes and various operations on them, including the
   creation of protocol-related seed statements
 * `seed.ml`: generation of seed statements
 * `lwt_compat_pure.ml`: compatibility layer for systems which lack nproc
 * `main.ml`: process second half of API file, create theory-related seed
   statements and and perform queries