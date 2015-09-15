# 10、2015-08-14
* [kedebug/LispEx](https://github.com/kedebug/LispEx)

  A dialect of Lisp extended to support concurrent programming, written in Go.

# 9、2015-08-13
* [YScheme - an experimental compiler for Scheme](https://github.com/yinwang0/yscheme)

  This is the final submission for a compiler course I took from Kent Dybvig at Indiana University. The compiler compiles a significant subset of Scheme into X64 assembly and then links it with a runtime system written in C. I made attempts to simplify and innovate the compiler, so it is quite different from Kent's original design.

  > compiler

# 8、2015-07-20
* [ikarus-scheme](https://web.archive.org/web/20100924103513/http://ikarus-scheme.org/)

  Ikarus Scheme is a free optimizing incremental native-code compiler for Scheme as specified in the Revised^6 Report on the Algorithmic Language Scheme.

  > history ikarus

* [What Scheme Does Ghuloum Use?](http://stackoverflow.com/questions/2165184/what-scheme-does-ghuloum-use)

  I'm trying to work my way through Compilers: Backend to Frontend (and Back to Front Again) by Abdulaziz Ghuloum. It seems abbreviated from what one would expect in a full course/seminar, so I'm trying to fill in the pieces myself.

  > ikarus

* [namin/inc](https://github.com/namin/incan incremental approach to compiler construction)

  an incremental approach to compiler construction

  > compiler

* [Ikarus (Scheme implementation)](https://en.wikipedia.org/wiki/Ikarus_(Scheme_implementation))

  Ikarus Scheme is a free software optimizing incremental compiler for R6RS Scheme that compiles directly to the x86 architecture. Ikarus is the first public implementation of a large part of R6RS, the most recent Scheme standard.

  > ikarus compiler

# 7、2015-07-14
* [Does the Scheme standard still matter? (2015 February 16)](http://marcomaggi.github.io/weblog/2015-February-16.html#g_t2015-February-16)

  In the history of the Scheme language there are documents defining the “standard Scheme”: the report, the revised report, R4RS, R5RS, R6RS, R7RS. Vicare is an R6RS implementation. R6RS is hated. Some people say that it must die. I guess that: because some People That Matter hate it, other people just hate it because it is cool to do so; this is pretty normal human behaviour.

* [reddit scheme](http://www.reddit.com/r/scheme)

  Scheme Programming Language articles.

  Scheme is one of the lingua franca of programming language theory (PLT) so PLT articles are also welcome!

  > reddit scheme

# 6、2015-07-09
* [9 Guile Implementation](https://www.gnu.org/software/guile/docs/master/guile.html/Guile-Implementation.html#Guile-Implementation)

  > guile 2.2 implementation

# 5、2015-07-04
* [guile 6.17.5 Compiling Scheme Code](https://www.gnu.org/software/guile/manual/html_node/Compilation.html)

  The eval procedure directly interprets the S-expression representation of Scheme. An alternate strategy for evaluation is to determine ahead of time what computations will be necessary to evaluate the expression, and then use that recipe to produce the desired results. This is known as compilation.

  > compile

* [Please how to use the Scheme debugger](http://comments.gmane.org/gmane.comp.gnu.lilypond.general/93498)

  Resolve the problem：
  scheme@(guile-user)> (use-modules (ice-9 readline))
  While compiling expression:
  ERROR: no code for module (ice-9 readline)

  > guile readline

* [The GNU Readline Library](https://cnswww.cns.cwru.edu/php/chet/readline/rltop.html)

  The GNU Readline library provides a set of functions for use by applications that allow users to edit command lines as they are typed in. Both Emacs and vi editing modes are available. The Readline library includes additional functions to maintain a list of previously-entered command lines, to recall and perhaps reedit those lines, and perform csh-like history expansion on previous commands.

  The history facilites are also placed into a separate library, the History library, as part of the build process. The History library may be used without Readline in applications which desire its capabilities.

* [Tweak your Guile REPL](http://hacks-galore.org/aleix/blog/archives/2013/04/30/tweak-your-guile-repl)

  If you are new to Guile and start the REPL right away you will find yourself with no history, no completions, no colors and even the cursor keys not working as expected.

  > guile repl readline

# 4、2015-07-03
* [How Guile (and Scheme) Could Really Win the Language Selection War](http://www.blogbyben.com/2011/09/how-guile-and-scheme-could-really-win.html)

  While I enjoyed reading The GNU Extension Language, a case for using Guile (a Scheme implementation) as GNU's extension language, I couldn't help but think: oh, if it was only that simple. If only you could make a reasoned and intelligent argument and that would make it so. But alas, when programming languages are involved, inevitably, these things boil down to a popularity contest. And one thing Scheme doesn't have going for it is Popularity.

  > guile

* [GNU Guile](https://en.wikipedia.org/wiki/GNU_Guile)

GNU Guile is the preferred extension system for the GNU Project, which features an implementation of the Scheme programming language. Its first version was released in 1993. In addition to large parts of Scheme standards, Guile Scheme includes modularized extensions for many different programming tasks.

# 3、2015-06-28
* [Guile git repository](http://www.gnu.org/software/guile/download.html#git)

Since March 27th, 2008, Guile's source code is stored in a Git repository at Savannah. You can access it with the following command:

    git clone git://git.sv.gnu.org/guile.git

If the port used by the git protocol is not available, you can instead use the (slower) http method:

    git clone http://git.sv.gnu.org/r/guile.git

Developers with a Savannah account can access repository over SSH:

    git clone ssh://git.sv.gnu.org/srv/git/guile.git

The repository can also be [browsed on-line](http://git.sv.gnu.org/gitweb/?p=guile.git).

# 2、2015-06-27
* [Mal](https://github.com/kanaka/mal)

  Mal is a Clojure inspired Lisp interpreter.

  [JavaScript (Online Demo)](http://kanaka.github.io/mal/)

  > lisp clojure

* [Simple, but not so simple](http://nalaginrut.com/archives/2014/04/15/simple%2C-but-not-so-simple)

  In spite of a Scheme implementation, Guile is also an extension language platform. This means you can write new language on it, which could be totally different from Scheme. Say, PHP/Lua/Ecmascript...and all these front-end will take advantage of the compiling optimization machenism of Guile.

* [Samson's machete](http://nalaginrut.com/)

  The author of artanis

  [github](https://github.com//NalaGinrut)

  A Chinese in Shenzhen

  > scheme guile

* [Projects List](http://www.gnu.org/software/guile/old-gnu-guile-projects.html)

  This page lists free software projects that use or enhance Guile.

  Right now, the list is only semi-maintained and may contain outdated information. We will move this list over to the Free Software Directory soonish.

  > guile

* [The GNU extension language Projects List](http://www.gnu.org/software/guile/gnu-guile-projects.html)

  This page lists free software projects that use or enhance the current stable version of Guile. To have your project listed here or to contribute, ask for instructions on the guile-user mailing list.

  > guile

* [guile](http://www.gnu.org/software/guile/)

  * What is Guile? What can it do for you?

    Guile is the GNU Ubiquitous Intelligent Language for Extensions, the official extension language for the GNU operating system.

    Guile is a library designed to help programmers create flexible applications. Using Guile in an application allows the application's functionality to be extended by users or other programmers with plug-ins, modules, or scripts. Guile provides what might be described as "practical software freedom," making it possible for users to customize an application to meet their needs without digging into the application's internals.

    There is a long list of proven applications that employ extension languages. Successful and long-lived examples of Free Software projects that use Guile are TeXmacs, LilyPond, and GnuCash.

  * Guile is a programming language

    Guile is an interpreter and compiler for the Scheme programming language, a clean and elegant dialect of Lisp. Guile is up to date with recent Scheme standards, supporting the Revised5 and most of the Revised6 language reports (including hygienic macros), as well as many SRFIs. It also comes with a library of modules that offer additional features, like an HTTP server and client, XML parsing, and object-oriented programming.

  * Guile is an extension language platform

    Guile is an efficient virtual machine that executes a portable instruction set generated by its optimizing compiler, and integrates very easily with C and C++ application code. In addition to Scheme, Guile includes compiler front-ends for ECMAScript and Emacs Lisp (support for Lua is underway), which means your application can be extended in the language (or languages) most appropriate for your user base. And Guile's tools for parsing and compiling are exposed as part of its standard module set, so support for additional languages can be added without writing a single line of C.

  * Guile gives your programs more power

    Using Guile with your program makes it more usable. Users don't need to learn the plumbing of your application to customize it; they just need to understand Guile, and the access you've provided. They can easily trade and share features by downloading and creating scripts, instead of trading complex patches and recompiling their applications. They don't need to coordinate with you or anyone else. Using Guile, your application has a full-featured scripting language right from the beginning, so you can focus on the novel and attention-getting parts of your application.

# 1、2015-06-26
* [Learn Scheme in 15 minutes](http://web-artanis.com/scheme.html)

  This gives an introduction to Scheme in 15 minutes

  First make sure you read this text by Peter Norvig:
  http://norvig.com/21-days.html

* [9.2.4 Conservative Garbage Collection](https://www.gnu.org/software/guile/manual/html_node/Conservative-GC.html)

  Aside from the latent typing, the major source of constraints on a Scheme implementation’s data representation is the garbage collector. The collector must be able to traverse every live object in the heap, to determine which objects are not live, and thus collectable.

* [GNU Artanis web-framework Manual](http://www.gnu.org/software/artanis/manual/manual.html)

  GNU Artanis is a web application framework(WAF) written in Guile Scheme.
