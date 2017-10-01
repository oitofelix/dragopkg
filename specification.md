---
title: oitofelix - DragoPKG specification
description: DragoPKG specification
tags: >
  Package manager programming, pkgsystem, pkg, Dragora, Bash, shell script
license: CC BY-SA 4.0
layout: oitofelix-homepage
#base: http://oitofelix.github.io
#base_local: http://localhost:4000
---
<div id="markdown" markdown="1">
## DragoPKG specification

> ... go, mere mortal, and thus you'll find the sacred packages.
>
> --- Dragomana, the goddess

_DragoPKG_ is a reincarnation of _Dragora's pkgsystem_, written from
scratch, based on the idea of distributed maintenance, common
meta-data infrastructure and user-based management.  It allows users
to create and manage local and remote software packages comprised of
_sources_, _libraries_, _programs_, _debugging symbols_, _headers_,
_localization_, _general data_, and _documentation_.  Its main
strength is its simplicity and robustness; its goal is to provide the
tools to build a strong and collaborative community --- easy for
newcomers and powerful for experts.  It covers various aspects of
software packages deployment, from the three main technical areas of
application: _build process_, _package management_ and _repository
maintenance_ --- to the workings of community cooperation for the sake
of distribution.

This specification aims to define every relevant aspect of _the
DragoPKG package management system_.

__Table of contents__

1. [The kingdom of packages --- DragoPKG package dynamics](dragopkg/specification.html#the-kingdom-of-packages-----dragopkg-package-dynamics)
2. [The Fate of The Knights --- The maintainer's role](dragopkg/specification.html#the-fate-of-the-knights--the-maintainers-role)
3. [The Universal Divine Flow --- Distribution Mechanics](dragopkg/specification.html#the-universal-divine-flow-----distribution-mechanics)
4. [The Nature of The Unknown --- Package Structure and Meta-data](dragopkg/specification.html#the-nature-of-the-unknown-----package-structure-and-meta-data)
5. [Royalty and Their People --- The System-Administrator and Common Users](dragopkg/specification.html#royalty-and-their-people-----the-system-administrator-and-common-users)
6. [The Treasure Map --- Packages Database Cache](dragopkg/specification.html#the-treasure-map-----packages-database-cache)
7. [The Dragorian List --- The User's Connection to Repositories](dragopkg/specification.html#the-dragorian-list-----the-users-connection-with-repositories)
8. [Class of Magic --- Command Line Interface](dragopkg/specification.html#class-of-magic-----command-line-interface)

### The kingdom of packages --- DragoPKG package dynamics

> ... and from source came life!
>
> --- Mandragorius, the dragon

There are two kinds of _DragoPKG packages_ (the so called
"super-classes"): __source__ and __live__.

- _Source packages_ are usually what _live package maintainers_ or
  _hard core users_ want and need; they contain the program's source
  code from the mainstream developers --- possibly with distribution
  specific patches --- alongside _DragoPKG meta-data_, and are ready
  to be built for any architecture by a single _DragoPKG_ incantation.
  The maintenance of this type of package is entrusted to the _source
  package maintainer_.

- _Live packages_ are usually what _common users_ want and need; they
  contain the software already built from their respective _source
  packages_, alongside _DragoPKG meta-data_, and are ready to be
  installed by a single _DragoPKG_ incantation.  The maintenance of
  this type of package is entrusted to the _live package maintainer_.

Although, there is only one maintainer for each _source package_,
_DragoPKG source packages_ can relate to more than one upstream source
packages.  In fact, any number of upstream source packages can be
mixed as far as the resulting combination makes sense and has proved
useful for users.

By contrast, several _live packages_ usually relate to the same
_source package_.  They are classified according to their content and
are divided in 7 _classes_: _library_, _program_, _debugging_,
_headers_, _l10n_, _data_, and _documentation_.  These _classes_, in
turn, are divided into specific _sub-classes_.  For instance,
_sub-classes_ might be _architectures_, _languages_ and _data
formats_, just to name a few.

One single _sub-class_ determines the smallest possible _live
package_.  Notwithstanding, there may be live packages that belong to
more than one class or sub-class.  In fact, a package can belong to
whatever combination of _classes_ and _sub-classes_ one might want.
The biggest possible live package can belong to every existing class
and sub-class.

All classes are divided into two major groups, depending on the set of
sub-classes that belong to them: the _executable_ and _non-executable_
groups.

- The _executable group_ is comprised of the _library_, _program_,
  _debugging_ and _headers_ classes.  Therefore, its _sub-classes_ are
  architectures.
- The _non-executable group_ is comprised of the _l10n_,
  _data_ and _documentation_ classes.  Therefore, its _sub-classes_
  are __not__ architectures.

For instance, consider a game engine like _PrBoom_.  Alone it can't do
much because it relies on maps, graphics and sound --- that's to say
the game's artistic work --- to offer a playable game.  For this,
there is another upstream package called _Freedoom_ which offers the
so called _IWAD file_ that contains all such data.  It could be useful
to combine both upstream packages into a single _DragoPKG source
package_, so a playable game can be distributed.  In that case, the
_PrBoom_ part would be used to make the architecture-dependent _live
packages_ --- e.g. those belonging to the _program class_ --- and the
_Freedoom_ part would be used to make the architecture independent
_live packages_ --- e.g. those belonging to the _data class_.
Actually, there are other upstream _IWAD packages_ for the same
engine, and they could be included along that _DragoPKG source
package_ in such a way that each _IWAD_ would be placed in a distinct
_sub-class_ of the _data class_.

The _executable group_ share a set of common sub-classes that are
machine architectures like `i486`, `x86_64`, `mipsel`, `arm`, and so
on; cross-compiler pseudo-architecture in the format `host-target`
like `i486-mips` and so on; and the neutral pseudo-architecture:
`no-arch`.  Thus, these sub-classes define two abstract sets of
_executable live packages_:

- __arch__: these are executable live packages that have a sub-class
  different from "no-arch" --- built for some specific machine
  architecture.

- __no-arch__: these are executable live packages that have sub-class
  "no-arch" --- they run on any kind of machine without modification
  and are written for interpreters or virtual machines.

In the executable group we have:

- The __library live packages__ are the packages containing system
  libraries.  There are numerous upstream software packages that build
  into libraries and programs.  Whenever makes sense to use a library
  without its companion programs, it might be included in a separate
  _library live package_.  If that's not the case, the libraries
  should only be included in their program's __live package__.

- The __program live packages__ are packages with commands, tools and
  applications, which can be run directly from the operating system's
  user interface.  The package may contain programs directly used by
  the user, or just helper program for other programs.

- The __debugging live packages__ are that packages which contains the
  debug symbols that were striped off from libraries and programs to
  save storage space.

- The __headers live packages__ are packages which contain the
  library headers used in software development.

The _non-executable group_ does not have sub-classes determined by
machine architectures, so they are arch-independent.  Its sub-classes
are not the same for each particular class, but like _executable live
packages_ any _non-executable live package_ can belong to more than
one sub-class in a given class.

- The __l10n live packages__ are those which contain the information
  pertinent to the localization of an internationalized program.  This
  class have a set of sub-classes that represent human languages:
  en_US, es_MX, ja_JP, pt_BR and so on.

- The __data live packages__ are those which contain whatever
  sub-class of data do not fit in any other live package class.  The
  sub-class list of this class is extendable.  Some of its predefined
  sub-classes are: _image_, _audio_, _video_ and _database_.  For any
  given source package there can be one live package maintainer for
  each of these sub-classes.  By definition all data that fits in
  packages of this class are arch-independent.  If there is an
  upstream package that builds arch-dependent data, be it executable
  or not, it should belong not to _data_ but to a _library_ or
  _program_ live package as appropriate.

- The __documentation live packages__ are those which contain the
  technical manuals, instruction texts, e-books, tutorials, how-tos,
  faqs, and whatever sort of information intended to help users and
  developers in the use of a program or library.  The sub-classes of
  this class are the formats that can be generated from the source
  documentation.  Some possible values are: _info_, _man_, _txt_,
  _html_ and _pdf_.

It is important to emphasize that a live package can theoretically
pertain to any combination of sub-classes and classes thereof; ranging
from the minimum package that has just one sub-class inside one class,
to the maximum one that has been built to every possible architecture
and holds every documentation format supported, and so on; it is a
very general design and one must use it adequately conforming to the
specific conventions of the distribution.




### The Fate of The Knights --- The maintainer's role

> ... be brave, be brief, be strong, be gentle
>
> --- Chidragoli, the old sage

The heavy, technical and centralized work inside a _DragoPKG based
distribution_ is on the source package maintainer's shoulders.  They
need to get the upstream software and couple it with _DragoPKG
meta-data_; in particular with a very powerful and handy "build"
script.  That script must be able to generate every meaningful live
package sub-class from the source package constructed by them.

The _live package maintainers_ represent an important role in the
process, but their work is not very technical, but rather distributed
and social.  Their work is not very much technical because they only
need to type one command to have the desired _live package_.  Their
work is distributed because each _live package maintainer_ cares only
with the _live package sub-classes_ that regards them and thus there
are one or more _live package maintainers_ for each other sub-classes
of the same software.  Their work is social because they need to
cooperate with other _live package maintainers_ inside that same
package class and deal directly with the distribution to users.  One
could wonder why there are _live maintainers_, if we can just give the
source packages right away for the users to make installable _live
package_ by themselves.  There are a number of reasons for this, among
them: reduce duplicated work, ease objective reaching, teach the
principle of good will by peer collaboration and facilitate the
initiation on the free software distribution process and the workings
of community cooperation.  Some of these are expressed by the
following rhetorical questions:

- If someone have already built that package, why should he not
  collaborate with the community providing his resultant work?

- If some user need right now get a job done, why should he waste
  time, space and power fulfilling build requirements, running lengthy
  compilations and spanning into more problems than they need to, if
  somebody else have already built the necessary package, and that
  user can right now start using those precious resources to do his
  own computing and (possibly) be able to benefit the community
  earlier?

- Why not give people the tools they need to help their neighbors
  without sacrificing much of themselves and making them feeling an
  important part of a whole?

- Why not build a stronger community, where more people work together
  and trace together the future of their own computing?




### The Universal Divine Flow --- Distribution Mechanics

> ... the gods play poker... and drink in the saloon...
>
> --- Grado, the drunk beggar

The following linear chart illustrates the hierarchy of a distribution
based on _DragoPKG_.

__The Free Software Distribution Pipeline__

__Lower level --- first in time__

1. There are tools for programming --- no package
   - _Developer_ writes program source code
2. There is code --- no package __(distribution starts here)__
   - _Source Package Maintainer_ makes source package
3. There is source package __(DragoPKG starts here)__
   - _Live Package Maintainer_ makes live packages
4. There is live packages __(distribution stops here)__
   - _User_ installs live packages __(DragoPKG stops here)__
5. The user's computing is being done

__Higher level --- later in time__

At the __first level__, _DragoPKG_ cannot help a developer to do his
job because there is no package (not even source packages) to manage
yet.

At the __second level__, _DragoPKG_ cannot help the source package
maintainer because there will be a source package to manage only in
the very end of the process.

At the __third level__, _DragoPKG_ can help the live package maintainer by
generating from the source package obtained in the end of the previous
step a live package that may contain: binaries for some architecture,
headers for developers, documentation for users or some architecture
independent data for programs, and so on.

At the __fourth level__, _DragoPKG_ can help the user installing and
managing the live package obtained from the previous level.

As you can see the _DragoPKG_ is designed to help at higher levels,
the live package maintainers and users.  After all, the user and its
community is the reason to make a free software distribution.




### The Nature of The Unknown --- Package Structure and Meta-data

> Finally!  Now I know how this dragorian alien machine works!
>
> --- Hogradstein, the mad scientist

Source and live packages are `tar` files, with arbitrary name and
extension, optionally compressed in any format automatically handled
by `tar`.  They differ from other common tarballs only by the
conventions held on the organization of internal files.  Basically
each of those package are divided in two parts: __data__ and
__meta-data__.

The data is the actual collection of sources, libraries, programs,
debugging symbols, headers, l10n catalogs, arch-independent general
data and documentation that is generally used by the system and user.
It is obtained from the upstream software package and is inserted in
the distribution source package by the source package maintainer, and
then will be split on its various sub-classes ending up in the live
packages by a DragoPKG automatic build process.

The meta-data is the information used exclusively by DragoPKG for
package management purposes.  The meta-data structure of a live
package is automatically derived from source package's meta-data
structure on which it is based.  The meta-data of a source package is
called "source meta-data".  There are also similar meta-data
structures for the package database and repositories.  See below.  The
meta-data, for source or live packages, is all kept in a directory
named "meta" in the package's root.

The _source meta-data_ contains information about: what are the name,
version, class (source, in this case), the upstream homepage, who is
the source package maintainer, the description of package's purpose,
possibly in any language, the list of imported and exported symbols,
scripts to build each sub-class that is supported and scripts to run
as hooks after install, removal or upgrade.  In details:

meta/: meta-data directory;

  name: this file holds the package's name, like "bash".

  version: this file holds the package's version, like "4.2p14".
           Where "4.2" is the upstream version and "14" is the
           distribution patch level.

  super-class: this file holds the package's super-class.  In this
               case, it is "source".

  maintainer.source: this file holds the source package's maintainer's
                     name, nick and email, like "Bruno Félix Rezende
                     Ribeiro (oitofelix) <oitofelix@gmail.com>"

  description/: this directory holds the package's description already
                written for any language in UTF-8 encoding.  The name
                of the default description, often in "canonical"
                English, that is the minimum requirement for any
                package, is "default".  For instance, for Brazilian
                Portuguese it would be "pt_BR".

  import/: this directory contains files, one for each language, that
           lists the symbols demanded by the software included in the
           source package for languages on which it is
           implemented. See below.

  export/: this directory contains files, one for each language, that
           lists the symbols provided by the software included in the
           source package for languages on which it implements and
           exports functionality.  See below.

  live/: this directory holds information about the construction of
         each live package sub-class.  It has an immediate directory
         for each class that can be constructed from it.  In turn,
         those class directories has one directory for each sub-class
         that regards them.  So, the executable classes set up as
         follows:

    library/, program/, debugging/: these directories has below them
      one directory for each architecture that the source package
      supports building to.

    On the other hand, the non-executable classes are configured the
    following way:

    headers/: there is no sub-class.  This class's live meta-data is
      immediately under this directory.

    l10n/: this directory has one sub-directory for each language to
           which there is localization.

    data/: this directory has one sub-directory for each format of
           arch-independent auxiliary data the source package can
           build.

    documentation/: this directory has one sub-directory for each
                    documentation format the source package's build
                    process can output.

    Under each of those sub-classes directories, that are under
    classes directories, resides the following structure:

      homepage: this file holds the package's mainstream homepage url,
                like "http://www.gnu.org/software/bash", that provides
                the original source from which this sub-class is
                derived.

      feature: this file lists the features that can be selected when
      	       building a package of this sub-class.

      dependence.build: this file lists the live packages that this
                        sub-class depends upon at build-time.

      dependence.run-time: this file lists the live packages that this
                           sub-class depends upon at run-time.

      script/: this directory holds the package's scripts to build the
	       live package of this sub-class and scripts to run
	       before and after installation, removal and update.  The
	       scripts are called with the working directory set to
	       where they reside.

        build/: On this directory resides the scripts to build this
        	live packages's sub-class.  The main script, the only
        	which is mandatory, is named "build".  That script
        	must take the appropriate actions to build the
        	necessary files and temporarily install it on the
        	stage directory, and should not require high
        	privileges.  The stage directory is found in the
        	environment variable PKGSYSTEM_STAGE_DIR and it has
        	the form "/tmp/pkgsystem.XXXXXXXXX/stage" where the
        	"X" are randomly generated characters.

        install/: this directory holds installation time scripts.

	  pre: this script runs before package installation.

    	  post: this script runs after package installation.

	removal/: this directory holds removal time scripts.

	  pre: this script runs before package removal.

	  post: this script runs after package removal.

	upgrade/: this directory holds upgrade time scripts.

	  pre: this script runs before package update.

	  post: this script runs after package update.


The import and export directories are intended to help source
maintainers in finding what libraries or programs packages exports the
desired symbols when tracking the dependencies of a new package or
building a foreign software.  Uncountable many times developers are
faced with problems in build linking stage regarding undefined
symbols.  Maybe there is not a certain flag in the compilation command
line, maybe it is trying to link against the wrong version of the
library or perhaps the demanded library is not installed at all, but
in any case, mainly when the source maintainer is not very much
familiar with the package being built, a search feature that
correlates packages and symbols would be of enormous help.

Both directories contains files, one per language, listing the
symbols, one per line, for each relevant language the software in that
package requires or provides.  So, for example, for the "GNU C"
library source package there will be a file called "c" in the
directory "export" with some line being "printf".  Analogously, there
can be a "bash" file listing the external commands required and
provided.  For example, in the "GNU coreutils" source package there
will be a file in "export" named "bash" with some line being "ls".
The same apply to C++, Perl, Python, Java and every single language
that a source package is in implemented into or for.

There are scripts and tools that automatizes the process of symbol
extraction from the source files, for interpreted languages, and from
object files, for compiled languages.

The live packages' meta-data is entirely deduced from the
correspondent source package's meta-data.  In the build process, for
every live package, the following meta-data fields (or files if you
prefer) are copied verbatim: name, homepage, maintainer.source, the
descriptions for any language, and for the mattering sub-classes, the
scripts, except build scripts, and the list of run-time dependencies.
The import and export list of symbols are included, verbatim, in the
live meta-data if, and only if, some of its classes are library or
program.

The super-class field is modified to the string "live", so
representing a live package.

The version file is modified to include a build level field and become
something like "4.2p14-7", where "7" is the build level.  As expected
the build level distinguishes between two builds, for a given
sub-class, of the same source package.

There are also one file not present in source meta-data that are added
to the live meta-data infrastructure: maintainer.live.  Supposedly,
this file contains the live package maintainer's name, nick and email
in the same format specified in the maintainer.source field.

Now, one could argue that here make no sense to preserve the live
directory structure for sub-classes of executable live packages since
there is no meaning in creating a package built for two different
architectures, and so we can remove the sub-classes directories in the
resulting live package putting the libraries, programs or debugging
symbols immediately below library, program or debugging directories as
appropriate.  But that is not true for two reasons:

Firstly because of cross compilers.  It is very common to have the
same version of the same compiler targeting different architectures
and running on the same host.  To exemplify that, consider for example
that we want to install two binary packages for GNU GCC 4.7.2, one
with the native target, let us say i486, and other with the mips
target.  How we will differ both?  We can use a simply scheme
modifying the "arch" field yielding into a somewhat "hybrid"
architecture.  So the meta-data regarding the GNU GCC targeting the
native processor for a program live package will be installed in:
"meta/live/program/i486-i486" but the meta-data regarding GNU GCC that
targets the mips architecture will be put in:
"meta/live/program/i486-mips".  So, now makes sense to create a single
executable package that has two sub-classes inside a given executable
class.

Secondly, DragoPKG is versatile enough to provide a way to build a
single executable package for more than one architecture and let the
user install only what they need.  So there can be a single executable
"GNU Bash" package that is built for the sub-classes i486 and x86_64
and, from the same package file, the user can install both if they
want to.  Alike, it can be good to package altogether when
architectures are super-sets one from another.  But even, when it is
not the case, DragoPKG do not impose an artificial restriction and
take the symmetrical approach as if executable sub-classes were
indistinct from the non-executable ones.  We hope that this feature,
which is a consequence of the design, be useful to someone on a
somewhat uncommon situation.

Whereas DragoPKG does not require a specific compression algorithm,
for exposition purposes we will use a good one: the LZMA algorithm
implementation Lzip, that offers high compression ratio, simplicity
and it is licensed under GPLv3+.

Although it is not enforced any specific file name scheme to DragoPKG
packages, one arises automatically from the package design.  It is a
good practice to follow the scheme when naming a source package and it
will be automatically done when building a live package by DragoPKG's
build command.  The convention for source packages is:

(name)-(source version)-source.(extension)

For instance, the source package for GNU Bash 4.2p14 compressed with
lzip will be:

bash-4.2p14-source.tlz

The convention for live packages is:

(name)-(live version)-(class 0)-(class 0 sub-class 0)-...-(class 0 sub-class m)-...
...-(class n)-(class n sub-class 0)-...-(class n sub-class p).(extension)

For instance, the program live package for GNU Bash 4.2p14 build level
7 for the i486 will be:

bash-4.2p14-7-program-i486.tlz

And, a super-set of that package that contains also documentation in
info, man and pdf formats will be:

bash-4.2p14-7-program-i486-documentation-info-man-pdf.tlz

As in the cross compiler example above, a program live package for GCC
4.7.2 build level 5, containing a native i486 compiler and a mips
targeting one, will be:

gcc-4.7.2-5-program-i486-i486-i486-mips.tlz

So, there is no ambiguity.

Besides having a "meta" directory, with the before mentioned files, in
its root directory, a source package does not have any other
constraint.

There is not a pkg command to construct source packages from source
files and meta-data in a directory "foo-source" because it predates the
package, so there is not a package to manage, and besides that a
simply call to "tar" suffice.  So, if you are a source package
maintainer and the program's sources and the meta-data are in "foo-source"
directory, and you want to generate a source package with a good
compression format automatically handled by the "tar" archiver, just
type:

tar --lzip -cf foo-source.txz foo-source

Soon a source package will show up in the current directory.

By the other hand, the root of a live package contains, besides the
"meta" directory, a class/subclass directory infrastructure.  So the
actual data files for class CLASS and subclass SUBCLASS will be placed
in CLASS/SUBCLASS and it will be installed at the prefix given to
pkg's install command.  For instance, using the default prefix, the
GNU Bash program for i486 architecture will be "program/i486/bin/bash"
and it will be installed in "/bin/bash".  Of course, the "meta"
directory is not installed directly inside the prefix but on the
meta-data cache database.

If you are a live package maintainer, or you are a hard core user that
fetched the source package and want to build a live package from it,
the pkg's "build" command is all you need.  Assuming your source
package is "foo-source.txz", and you want to build the class CLASS for
the sub-class SUBCLASS, you only need to type:

pkg build --class CLASS --sub-class SUBCLASS foo-source.txz

Soon a live package will show up in the current directory.




### Royalty and Their People --- The System-Administrator and Common Users

> ... Indeed, but my power is conceived to serve my people, not
> to cause pain by injure of their liberty.
>
> --- Kirlimim, the righteous king

DragoPKG distinguishes between two main operation modes to perform
persistent actions: system-wide and user-based.  This distinction is
made because DragoPKG, unlike most package management systems, does
not require that a user be privileged, a.k.a system administrator, to
perform actions such as build, install, remove, upgrade and maintain
the meta-data cache database.  In order to perform such a job,
DragoPKG has two distinct set of configuration files that regulates
its relevant behaviors: system-wide configuration, under /etc, and
user-based configuration, under user's home directory.  The former is
constituted by the file: "/etc/pkgsystem.conf", and the second by
"~/.pkgsystem.conf", where "~" is the respective user's home
directory.  To decide what file should be considered, DragoPKG
analyzes the user's id: if it is 0, the system-wide configuration is
taken; otherwise it takes the user-based one.

Those files have identical syntax and semantics and are just a list of
variable attributions in the form "VARIABLE=VALUE".  They dictates
where each other directory or file demanded by DragoPKG's non-volatile
operations are located, possibly among other things.  The majority of
those variable assumes a default value if none are supplied in the
configuration file in question.  Hereinafter, when we talk about
"configuration variable" we are referring to some variable in those
files.  Also, the active value of a configuration variable, be it
system-wide or user-based will be referred by, in the bash syntax,
$VARIABLE, where "VARIABLE" is the variable's name.




### The Treasure Map --- Packages Database Cache

> Buzzzzzzzzzz... buzzzzzzzz... buzzzzzzzz...
>
> --- Dragonfly, the annoying dragonfly

One important part of a complete package management system is the
package management itself, and it only becomes possible with a
packaging track mechanism to store information about the status of
every known package, local or remote, source or live, installed or
not.  DragoPKG implements that package tracking mechanics using a
database cache which is an extension of package's meta-data as seen in
the previous section.  That meta-data is called, hereinafter,
meta-data cache.

There is two main modes of meta-data cache database management:
system-wide and user-based.  The former is the traditional model aimed
at system administrators, that maintains the global control over all
the installed applications that are enforced upon all system's users.
The second is a per-user approach that allows that any user have its
own, and independent from system and other users, meta-data cache
database for local and remote packages.  Both models can be used in
parallel.

The DB_DIR configuration variable value sets the meta-data cache
database directory, and its default system-wide value is
"/var/db/pkgsystem", whilst its user-based value defaults to
"~/.pkgsystem-cache".

To keep track of installed packages, DragoPKG builds a database of
meta-data caches in $DB_DIR.  That database uniquely identify each
installed package by the pair "name/version".  In fact that is the
directory structure under $DB_DIR.  So, for example, every GNU Bash
4.2p14-7 live package has its meta-data cache residing on
"$DB_DIR/bash/4.2p14-7".  Notice that, naturally,
differently from package meta-data there is not a leading "meta"
directory.

The pair scheme allows, for example, that multiple versions of GNU
Bash be installed at the same time, at different prefixes, being
managed flawlessly by DragoPKG.  Generally speaking, the pair
name/version define the boundaries of a super-class live package.  The
meta-data cache for a given pair name/version is partially built from
the respective live package meta-data.  The following fields that are
present in the meta-data cache are copied verbatim from the live
package's meta-data: homepage, maintainer.source, description
directory, import and export directories, for library or program
packages, and the live structure, except install scripts directory.

The file maintainer.live is added to each pertinent sub-class
directory inside the live infrastructure.  Likewise, a file, called
"files", listing all installed files for a given live package, is
added into the correspondent sub-class directory.  That file is also a
flag indicating whether a specific sub-class of a live package is
installed.  In fact, even if you install a live package that belongs
to more than one class or sub-classes, after the installation DragoPKG
handle each single sub-class individually as if you had installed a
collection of minimum live packages.  Consequently, these minimum
"virtual" packages can be uninstalled independently from each other.
A file named "prefix" is also installed under each sub-class directory
and contains the prefix under which the respective live package was
installed.  Note that it is not possible to install two live packages
with the same name and version and of the same sub-class even in two
different prefixes.

Besides representing installed packages, the meta-data cache can also
represent live packages in a repository yet to be installed.  The
meta-data cache, in this case, is obtained by remote synchronization
from one ore more meta-data repository servers.  Inside each sub-class
directory --- where can be a dependence file, for example --- there is
a file called "url" that contains the address from where to get the
actual live package pertaining to that sub-class.  Additionally, in
the meta-data cache root --- where is the homepage file, for example
--- there is another file called "url" that points to the
correspondent source package.  Therefore, a meta-data repository is
just a remote location that can be retrieved with rsync and is a
database (collection) of meta-data cache directories with that special
"url" files.




### The Dragorian List --- The User's Connection to Repositories

> ... all our souls are linked altogether, dragorian-listed to
> reach the sacred packages...
>
> --- Niphydracia, the reptilian fairy

All information regarding the repositories of source and live packages
that the system and user can use are stored in a clear text file
called "dragorian list".  Its location is regulated by the
configuration variable DRAGORIAN_LIST and its system-wide and
user-based defaults are "/etc/pkgsystem.lst" and "~/.pkgsystem.lst",
respectively.

The Dragorian List is just a list of remote meta-data directories that
can be synchronized with local ones using rsync.  So, each entry on
that list seems something like:

rsync://USER@HOST::/META-DATA-DIR

for access via rsync daemon, and

USER@HOST:/META-DATA-DIR

for access via remote shell.

Those remote locations in the list must contains only meta-data
structures, in the format described in the last section, and not the
packages themselves.  Each meta-data directory in the repository
corresponds to one source package and all live packages that can be
obtained from it.  Immediately, below each meta-data is a "url" file
addressing the source package, and, inside each sub-class directory
there is a "url" file addressing the respective live package.  Notice,
however, that there is no need that the pointed packages be minimal,
distinct, for every sub-class; the "url" file inside a sub-class
directory is semantically constrained only for representing a package
that pertain to the respective sub-class, being irrelevant whatever
other sub-classes it might belongs too.  Thence, we can have multiple
"url" files inside different classes and sub-classes pointing to the
same live package file.

The contents of "url" files is any single uniform resource locator
that can be fetched by GNU Wget; and it will be at the user's request
to install the correlate package.  This design make it possible to not
have a centralized repository of package files; that packages can be
scattered over the net under any domains.  But, of course, it can be
centralized as well.



### Class of Magic --- Command Line Interface

> Where did I put my notes about that incantation?
>
> --- Widragordimus, the magician

DragoPKG has a standardized command line interface that allows build,
installation, removal, upgrade and analyze of packages. It has only
one command: pkg.  That command is a delivery command that unfold
various other commands, and those commands, in turn, can unfold
another sub-commands or perform an action on their own.  In principle,
there is no limit on command's nesting level.  After the last command
supplied the user can provide options and non-option arguments that
regulates the behavior of the resultant group.  Long and short options
are accepted and can be intermixed with non-option arguments.  Short
options can be grouped when any of them do not require, except
possibly the last, an argument.  Non-ambiguous abbreviation of long
options are accepted also.  That command line format is designed to
make DragoPKG's pkg command extensible.  The general format is:


pkg [command]... [option...] [non-option]...


Semantically, the command sequence must be treated, and only it must
be handled this way, as an action indication.  Analogously, options
must be treated as action modifiers.  It means that options can modify
actions as long it does not modify their nature.

For instance, to install the bash local package
bash-4.2p14-program-i486.tlz one would use:

pkg install bash-4.2p14-program-i486.tlz

But to show what system files would be overwritten if it were
installed we use:

pkg install warn bash-4.2p14-program-i486.tlz

rather than something like

pkg install --warn bash-4.2p14-program-i486.tlz

Because the resulting action of the command is not to install the
package but warn about issues with a potential posterior installation.
As already mentioned, the rule of thumb is that an option cannot
modify the nature of an action, just modify it to a certain degree.
On the other way around, to install that same package asking the user
whether he wants to overwrite conflicting files, one should use:

pkg install --ask bash-4.2p14-program-i486.tlz

instead of something like

pkg install ask bash-4.2p14-program-i486.tlz

Because the main action is still to install the package, but just with
somewhat altered behavior.

The majority of pkg commands accepts a notable and powerful option
that determines on which packages the specified action should be
applied.  That option accepts an argument that is a boolean expression
of arbitrarily complexity called "The Dragorian Expression".  With
that it is possible to apply an action to a whole group of packages
selecting by any combination, at any nesting level, of conjunctive,
disjunctive and negative expressions, with additional pattern
matching, in two flavors: globbing and regular expressions.  Those
expressions operates regarding meta-data information fields: name,
version, super-class, class, sub-class, upstream homepage, maintainer,
description, imported or exported symbols, status of installation,
dependencies, location of repository and possibly much more.  Beyond
that, the user can easily extend the list of available operators
programming arbitrary search predicates that is automatically added to
the existing available list.  The respective long and short options
are '--dragorian-expression' and '-d'.  If a dragorian expression is
argument of that options, it is tested against every package in the
meta-data cache database.  The current testing package is referred on
the following items by "the package".  The terms "GLOBEXP" and
"REGEXP" correspond to any globbing expression and regular expression
pattern, respectively.  The Dragorian Expression syntax and semantics
is defined as follow:

0. A dragorian expression assumes exclusively one of two, and only
   two, values: false or true.

1. If "EXPR" is a dragorian expression, it is true if, and only if,
   "EXPR" matches the package.

2. If "EXPR" is a dragorian expression, also it is
   "<BLANK1>EXPR<BLANK2>", where <BLANK1> and <BLANK2> are blank
   characters, and its value is the same of "EXPR".  The blanks are
   used to meliorate expression's legibility.

3. If "EXPR" is a dragorian expression, also it is "(EXPR)" and its
   value is the same of "EXPR".  The punctuation symbols "(" and ")"
   are used to override the precedence of logical operators.

4. If "EXPR" is a dragorian expression, also it is "!EXPR" and its
   value is true if, and only if, EXPR is false.  The symbol "!"
   indicates the negation logical operator and its precedence is 0.

5. If "EXPR1" and "EXPR2" are dragorian expressions, also it is
   "EXPR1&&EXPR2" and its value is true if, and only if, EXPR1 and
   EXPR2 are true.  The symbol "&&" indicates the conjunction logical
   operator and its precedence is 1.

6. If "EXPR1" and "EXPR2" are dragorian expressions, also it is
   "EXPR1||EXPR2" and its value is true if, and only if, EXPR1 or
   EXPR2 are true.  The symbol "||" indicates the disjunction logical
   operator and its precedence is 1.

7. The following predicates, with its arguments, are dragorian
   expressions:

   - _name REGEXP: true if, and only if, the package's name matches
   		   REGEXP.

   - _name_glob GLOBEXP: true if, and only if, the package's name
     		         matches GLOBEXP.

   - _version VERSION: true if, and only if, the package's version is
                       "VERSION".

   - _version_less VERSION: true if, and only if, the package's version is
                            less than "VERSION".

   - _version_greater VERSION: true if, and only if, the package's
                               version is greater than "VERSION".

   - _superclass SUPERCLASS: true if, and only if, the package's
                              super-class is SUPERCLASS.

   - _class CLASS: true if, and only if, the package's class is CLASS.

   - _subclass SUBCLASS: true if, and only if, the package's
                          sub-class is SUBCLASS.

   - _homepage REGEXP: true if, and only if, the package's
                       upstream homepage matches REGEXP.

   - _maintainer REGEXP: true if, and only if, the package's source or
                         live maintainer matches REGEXP.

   - _description LANG REGEXP: true if, and only if, the package's
                               description in language LANG matches
                               REGEXP.

   - _import REGEXP: true if, and only if, there is some package's
                     imported symbol that matches REGEXP.

   - _export REGEXP: true if, and only if, there is some package's
                     exported symbol that matches REGEXP.

   - _installed: true if, and only if, the package is installed.

   - _dependence_build REGEXP VERSION: true if, and only if, the
                 package has a build dependence which name matches
                 REGEXP and version is VERSION.

   - _dependence_runtime REGEXP VERSION: true if, and only if, the
                 package has a run-time dependence which name matches
                 REGEXP and version is VERSION.

   - _repository REGEXP: true if, and only if, the package's url to
                         the actual package file matches REGEXP.


For instance, if you want to install GNU Bash, version 4.2p14-7,
program for i486 architecture and its documentation in info format and
also fetch its source, additionally installing GNU Multiple Precision
Arithmetic library, version 5.0.4, for i486 and its headers, you can
run:

pkg install --dragorian-expression          \
"(_name bash && _version 4.2p14-7           \
  && (_superclass source                    \
      || (_class program && _subclass i486) \
      || _subclass info))                   \
|| (_name gmp && _version 5.0.4             \
    && ((_class library && subclass i486)   \
        || _class headers))"

That operation behavior, can seem to someone as very powerful and
dangerous, but actually the pkg will show all matching packages and
ask user's confirmation before installing any package, unless
overridden by the "--no-interactive" option.

The configuration variable PKGSYSTEM_EXT_DRAGOEXP_PRED names a script
that defines additional predicates to augment from built in ones.
Each function should have a name starting with "_" and must return 0
if the package matches or any other value otherwise.  The basic
testing package's meta-data are stored in an associative array with
name "PKGSYSTEM_PACKAGE" and it has the following index scheme:

[name] = package's name
[version] = package's version
[super-class] = package's super-class
[class] = package's class
[sub-class] = package's sub-class

An user defined predicate can take as many argument as the user
wishes.  Using the variables PKGSYSTEM_DB_DIR, and knowing the
meta-data cache structure conventions, as already specified, the
user's predicate can operate on any meta-data information.  It is
important that the predicates do not have side effects, because the
consequences would be unpredictable since, in a dragorian expression,
only the necessary predicates to deduce the value of an expression are
evaluated.

Often pkg commands accepts non-option arguments that indicates on
which entity to operate on.  It is either a local file or the name of
a remote package.  If it does name an actual file that file is used,
if not, it is considered a package name and looked up in the meta-data
cache database.  In this case, the whole set of live packages with
that name and highest version are taken.  You can specify files
explicitly with the `--file' option and explicitly specify installed
or remote packages with `--dragorian-expression' option.  If neither
`--file', nor `--dragorian-expression', nor non-option arguments are
given, the package to be processed is read from the standard input.

A flag option is one that only enables or disables determined
behavior.  Every flag option that starts with the word `no' is called
a "negative option flag".  For every option that is a flag there is a
configuration variable that has the name PKGSYSTEM_OPTION_NAME, where
NAME is the non-negative name of the respective long option, and it
governs what is the assumed behavior, regarding that flag, when none
correlate option has been supplied.  That configuration variables can
assume integer values representing boolean values; 0 means false and
anything else means true.  Often, the following option lists has only
options that do not imply in default flags values, but it can include
a option flag and its negative when there is worth information to
provide.  For every option flag there are a negative correspondent.
If the first word in a flag options' name is `no', then its negative
correspondent has the same name but without the leading `no-'.  On the
other hand, if the first word in a flag options' name is not `no',
then its negative correspondent has the same name but with a leading
`no-' appended.

The following options are general and common to every pkg command.  If
some of them do not make sense for a specific command, it is just
ignored.

--file=FILE: operate the selected action on the actual file FILE.
             This option can be specified multiple times.

--dragorian-expression=DRAGOEXP: operate the selected action on
            packages, installed and remote, matching the dragorian
            expression DRAGOEXP.  This option can be specified
            multiple times.

--interactive: allow DragoPKG to ask user relevant questions about the
               procedures that DragoPKG should take when its action
               imply in a irreversible permanent change, like
               overwriting files, if a interactivity option, like
               `--ask', was given.

--no-interactive: do not ask user any question.  Even if you specify a
                  specific command interactivity option none question
                  will be made.

--no-quiet: turn on output.  It is equivalent to setting the verbosity
            level to 2.

-q, --quiet: turn off output.  It is equivalent to setting the
             verbosity level to 0.

--less-verbose: be less verbose about performing actions.  This
                options can be provided multiple times and its effect
                is cumulative.  Each time it is provided the verbosity
                level decreases by 1.  The default value is 2 and you
                can change it assigning an integer by the
                configuration variable PKGSYSTEM_VERBOSITY.

-v, --verbose: be more verbose about performing actions.  This options
               can be provided multiple times and its effect is
               cumulative.  Each time it is provided the verbosity
               level increases by 1.  The default value is 2 and you
               can change it assigning an integer by the configuration
               variable PKGSYSTEM_VERBOSITY.

-d, --debug: turn on debug output.  With this option it is shown
             various information important to the developers of
             DragoPKG, what is helpful if it does not work properly.

--no-debug: turn off debug output.  This is the default.

The pkg commands summary follows:

- build: build a live package from a source package.
- install: install local or remote packages into system.
- remove: remove an installed package from system.
- show: show local or remote package information.
- update: update meta-data cache database.

Let us consider it one by one.

The _build command_ is a higher level interface designed for live
package maintainers and users that want to build from sources.  It
abstracts the build process to a unique common interface, since who
cares about the build process is the source package maintainer that
wrote the "meta/script/live/*/*/build" recipe.

The usage is as follow:

- build: build a live package from a source package.

  --class=CLASS: mark the next `--sub-class' argument as pertaining to
                 CLASS.  It is necessary because there can be two
                 sub-classes belonging to different classes but with
                 equal names.  This option can be specified multiple
                 times and need to be specified at least once.

  --sub-class=SUBCLASS: build sub-class CLASS in the resultant live
                        package.  This option can be specified
                        multiple times and need to be specified at
                        least once.

  --features=FEATURES: Build with FEATURES.  It is a comma separated
                       list, of features that are present in the
                       sub-class "features" files.

  --add-configure=CONFIGOPT: In the build process call source
                 package's configure script with CONFIGOPT added to
                 its command line options list.  This option can be
                 supplied multiple times and has a cumulative effect.

  --override-configure=CONFIGOPT: In the build process call source
             package's configure script with CONFIGOPT substituting
             its command line options list.  This option can be
             supplied multiple times and has a cumulative effect.

  --live-maintainer=IDENTITY: use IDENTITY as the live maintainer
         identity.  It should be a string with live maintainer's name,
         nick and email, like "Bruno Félix Rezende Ribeiro (oitofelix)
         <oitofelix@gmail.com>".  This will be the content of
         "meta/live/*/*/maintainer.live".  If it is not specified,
         maintainer.live will be made as a copy of maintainer.source.

  --binary-version=N: use N as the build level.  This will be added to
                      the version in "meta/version" before a dash.  If
                      it is not specified it will be assumed N=1.

  --sand-box: build in a sand-boxed environment.  All build
              dependencies must be installed.  The necessary files
              will be redirected to a temporary tree that will be
              setup properly and then chrooted to proceed with the
              build.  See `install-dependencies' sub-command for a
              handy way to install the required dependencies.

  --no-sand-box: do not build in sand-boxed environment.  All
                 prerequisites from library and headers classes must
                 be installed.  See `install-dependencies' sub-command
                 for a handy way to install the required dependencies.
                 This is the default.

  install-dependencies: install all dependencies for build a given set
                        of source package's class and sub-classes.  It
                        accepts `--class' and `--sub-class' options to
                        specify which sub-class dependencies must be
                        satisfied.  The dependencies are installed in
                        the "/" prefix.

The build command do not make use of dragorian expressions and only
accepts one source package file at a time.  All pertinent environment
variables like CC, CFLAGS, LDFLAGS, LIBS, CPPFLAGS, CPP, CXXFLAGS,
YACC, YFLAGS and so on, will be passed through to the build process.

On DragoPKG side, all temporary directories for the build process are
random generated, so there is no design inherent limitation for
parallel building.  So, theoretically, a live package maintainer can
invoke multiple instances of pkg build over a same, or different, live
package without a problem.

The _install command_ installs a local or remote live package into
system.

install: install local or remote packages into system.

  --prefix=PREFIX: install the live package under PREFIX.

  --ask: for each existing system conflicting file, ask if user wants
         to overwrite it.

  --force: Allow installation of package even if it is already
           installed.

  warn: warn about files that would be overwritten if the package were
        installed.

  source: fetch a source package.  The source package file will be put
          on the current directory.

  upgrade: this command is intended for core packages that can break
           DragoPKG if they are uninstalled without some immediate
           replacement precautions.  For all other purposes, it is
           much like a "remove" followed by an "install".  If a live
           package with the same name, sub-class and with a smaller
           version of a selected one is already installed, it is
           safely removed and the new one is installed in its place.

    --downgrade: allow a package with smaller version be installed in
                 place of a higher one.

    --fix: allow that a package with identical version of an installed
           one be installed over it.  It is useful to fix corrupt
           installations.

It is an error try to install a source package file.  An already
installed package is only re-installed if option `--force' is
supplied.

The _remove command_ removes an installed package from system.  Note
that there are elementary package that when removed can cause DragoPKG
to stop working.  If you are trying to upgrade such a package,
"install upgrade" should be used instead.

remove: remove an installed package from system.

  --ask: for each removing file that also is listed as pertaining to
         another existing package, show this package name and ask
         whether the user really wants to remove it.

The _show command_ is useful to ease the visualization of packages
meta-data.  It can show the name, version, super-class, class,
sub-class, upstream homepage, maintainer, description for the current
locale or the default if not implemented, status of installation,
dependencies, location of repository, installation prefix and possibly
more.

The _update command_ is intended to synchronize the meta-data cache
database with the remote repositories listed in $DRAGORIAN_LIST.


</div>
