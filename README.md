# val
PDDL 3.1 plan validator mirror

Confirmed to work with flex-2.5.39

The following is copied from the source code provided by the original VAL authors. 


VAL - The Automatic Plan Validator for PDDL3
==============================================

Derek Long and Maria Fox - PDDL2.1 and VAL, extensions to PDDL3
Stephen Cresswell - PDDL2.1 Parser
Richard Howey - Continuous Effects, derived predicates, timed initial literals and LaTeX Report in VAL
Andrew and Amanda Coles - bug fixes, parser improvements and instantiation modifications.

Strathclyde Planning Group, University of Strathclyde, UK

This code is written in C++ using the STL. It is known to compile
under Linux with Gnu C++ (3.4.0, 3.3.3, 3.2.2 and 2.96) and the associated STL. 

The current version is more ANSI-standard C++ compliant than previous
releases, but this has made it harder to compile under older compilers
(particularly where the STL is non-standard, for example). We are attempting
to construct versions that will compile under other environments. Windows
is our priority, but please contact us if you have other specific needs.
[This is currently progressing very slowly - the parser and lexer are probematic
and the best option is probably to use Cygwin. We have got a version working with Cygwin quite successfully.]


The parser requires flex++ and bison (also Gnu tools). See below for known 
problems and possible solutions.

The Makefile is automatically constructed using makemake with the
given Make.header and Make.files. It should not need to be touched
if the program is simply constructed "as is".

To build, simply "make". 

The executable is "validate". It expects three or more file name 
arguments: a domain, a problem and one or more plans. There are
also many optional arguments that must come first:

-t <n> where <n> is a (floating point format) value determining
the tolerance that is to be used in determining whether the plan
will execute correctly. In particular, this value must be set 
small enough to ensure that the time stamps on actions are
distinguishable (otherwise the validator will treat those actions
with time stamps closer than the tolerance value apart as simultaneous).
It defaults to 0.01. 

-v
This makes the output verbose - this is useful to find out more about failing plans especially now that advice is given on how to fix unsatisfied preconditions.

-h
Display the help message.

-g
Use the graphplan length where no metric is specified.

-l
Output a LaTeX report of the plan validation. This includes a Gantt chart and graphs of the values taken by numerical expressions.

-a
Do not output plan repair advice when Verbose is on.

-p <n> <m>
This option allows the Gantt chart to be split across a number of pages. The first value <n> is the number of pages to split the time axis over, the second value <m> is the number of pages to split the rows over. Zero obtains the default value.

-q <n>
This option specifies the number of points to use when drawing each LaTeX graph of PNEs. The more points used the smoother the appearance of the curves. To reduce the size of the graphs change the '\setlength{\unitlength}{100pt}' part in the .tex file to change the point size, such as: '\setlength{\unitlength}{80pt}'. If the graph is required to be really small then the number of points to draw the graph needs to reduced otherwise the LaTeX .tex file will not compile.
  
-o ... -o
Specify a list of the objects and/or types of objects to be tracked on the Gantt chart.

-c
This option continues to execute the plan even if an action precondition is unsatisfied. The plan will still, of course, be deemed to have failed. Note that a happening with a mutex pair of actions will not have its effects enacted.

-d
Do not check set of derived predicates for stratification. Actions are also not checked in case a derived predicate appears as an effect.

-i
An invariant may include comparisons with continuously changing numerical expressions in it that are too complex to verify. This option allows the validator to assume that the conditions hold and continues to validate the plan. The plan is then valid subject to these conditions holding -- which may be trivial to prove (or disprove) to a human.

-e
Produce error report for the full plan, this enables the -c option and reports advice on how to fix each unsatisfied precondition.

-m
Use makespan as metric for temporal plans (overrides any other metric).

The validator parses and typechecks domains, problems and plans before
confirming that plans are valid. Plans that will not typecheck are 
listed as failed plans. Plans that fail for reasons that are not clear
are listed as queries (verbose mode should clarify what caused this).

You can also build a parser alone, using "make parser" which will build 
the executable "parser", taking any number of files to syntax check as 
arguments. The check on problem files requires the associated domain as a 
preceding argument.

If you find any bugs, have problems compiling or running the system or
have any comments please contact:

derek.long@cis.strath.ac.uk or maria.fox@cis.strath.ac.uk

Note:
=====
By releasing this code we imply no warranty as to its reliability
and its use is entirely at your own risk.

Known Problems
==============

The original release had a bad command in the Makefile - it has now been
fixed. There was also a bug in the typechecking, which assumed domains 
would always be typed, also now corrected.

Some older installations of g++ do not use the std namespace by default
and so there is a line in Plan.h which needs to be corrected. There is
now a switch in the start of the header: NO_STD_NAMESPACE which you can 
set to try to handle this problem if it affects you. 

Note that the installation expects to have flex++ if you build it from
clean, but the distribution includes the lex.yy.cc file built from 
pddl+.lex and the FlexLexer.h header file to avoid this dependency being
too great a problem. In general, flex++ is quite variable from distribution
to distribution, so we have attempted to remove the dependency as far as
we can.

At least one person has reported a problem with the macros.h file. We are 
looking into this, but if you are included, please let us know.

Change Log
==========

1.12.09: This change log has not been updated properly for some time, but
         this version contains bug fixes, various bits of other tools and 
         improvements in parser error messages. 

16.06.07:   Significant collection of bug fixes, addition of some new tools (several
            in partial stages of development).
            
25.04.06:	Minor bug fixes. 

21.04.06:	Major revision: addition of RH's code to handle Events and Processes
		(PDDL+), extension of treatment of PDDL3.0 - this version should
		handle all new language features. There remain some outstanding
		issues in format of reports for failures.

30.03.06:	Extensions to handle PDDL3.0. There have also been extensive
		additions to the code in other areas, including handling events
		and processes (PDDL+). Peripheral tools have also been extended,
		including development of a new version of TIM.

28.04.04:	Accumulation of several key changes and bug fixes. Many of 
		these are issues raised by the competition domains, but 
		also some changes for better compiler compatibility (now
		compiles cleanly with g++ 3.4 as well as 3.3). 

04.11.03:	Now handles PDDL2.2. This means VAL now handles derived
		predicates (axioms) and timed initial literals.
 
25.03.03:	Now handles continuous effects (linear and polynomial effects)
                and provides a LaTeX report.

30.03.02:	Minor additional robustness improvement and SunOS Makefile
		created.

22.03.02:	Improved robustness and error reporting. Made behaviour
		for reporting plan length more flexible and more 
		definitive. Fixed memory leaks and made it possible to
		check domains alone or domains with problems and no plans.
		Reporting of parse errors from validator improved.

19.03.02:	Corrected bug in conflict identification with commuting
		numeric effects.

25.02.02:	Corrected bug in handling of quantified goals with 
		multiple quantified variables. [Thanks to Dan Wu for
		pointing this one out!]

04.02.02:	Corrected bug in type-checking of equality. Note that 
		equality type-checking is currently lax - it allows 
		objects of any types to be compared, regardless of whether
		they are subtypes of a common parent or not. 

27.01.02:	Minor adjustments to output and tolerance default value.

21.01.02:	Corrected a bug in handling of types for typechecking
		(another!). Corrected use of requirements flags in
		durative actions.

18.10.01:	System now handles quantified effects. The approach is not
		very pretty, because it requires multiple new environments
		to be constructed and these cannot be easily garbage
		collected. A better approach might be to make Environment
		contain a chained sequence of maps to avoid copying old
		environment maps. Certainly not an issue in the
		short-term: plans are not that big!

14.01.02:	System now handles quantified preconditions. Also 
		corrected a bug in typechecker for handling type 
		hierarchies. All language complete to level 3 of PDDL2.1.


TODO:
=====
* A better treatment of tolerance - at least an automatic determination
  of a sensible value from the domain, problem and plan. We probably need
  a way to specify this to the planner as part of a problem specification.

