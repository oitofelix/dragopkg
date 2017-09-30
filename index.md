---
title: oitofelix - DragoPKG
description: >
  DragoPKG is a reincarnation of Dragora's pkgsystem, written from
  scratch, based on the idea of distributed maintenance, common
  meta-data infrastructure and user-based management.
tags: >
  Package manager, pkgsystem, pkg, Dragora, Bash, shell script
license: CC BY-SA 4.0
layout: oitofelix-homepage
base: http://oitofelix.github.io
base_local: http://localhost:4000
---
<div id="markdown" markdown="1">
## DragoPKG

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

- [DragoPKG specification](specification.html)
- [DragoPKG VCS repository](https://github.com/oitofelix/dragopkg/)

_DragoPKG_ allows the management of remote packages using any protocol
supported by _GNU wget_ and the use of any compression algorithm
supported by _GNU tar_.  Last but not least, it features a consistent
and rich user interface.  Unfortunately, _DragoPKG_ is incompatible
with _pkgsystem_'s database and packages, but scripts may be created
to ease the conversion to _DragoPKG_'s format.  From developers'
standpoint, _DragoPKG_ is by far more extensible and easier to work
with than _Dragora's pkgsystem_.  That's due to the modularization and
standardization of interfaces provided by the main library
`pkgsystem.bash` and various other helper core libraries like
`i18n.bash` and `sanity.bash`.

<h3 style="margin-bottom: 0px;">Current Status</h3>

Module | Status
--------|--------
DragoPKG specification | Done
`sysexits.bash` | Done
`i18n.bash` | Done
`sanity.bash` | In progress
`pkgsystem.bash` | In progress
`dragopkg` | Done
`dragopkg build` | To do
`dragopkg build install-dependencies` | To do
`dragopkg install` | To do
`dragopkg install warn` | To do
`dragopkg install source` | To do
`dragopkg install upgrade` | To do
`dragopkg remove` | To do
`dragopkg show` | To do
`dragopkg update` | To do


<div style="font-size: small; text-align: right" markdown="1">
[Dragora](http://www.dragora.org/en/index.html) is a GNU+Linux-libre
operating system distribution endorsed by the
[FSF](http://www.fsf.org/).
</div>


### Manifesto

_DragoPKG_ came into existence in face of numerous severe limitations
of its precursor _Dragora's pkgsystem_.  Below is a list of some
drawbacks in _pkgsystem_'s design that have motivated the creation of
_DragoPKG_.

- __Its internal package format is weak and inexpressive__.  The
  meta-information is not centralized in a main directory but
  scattered inside package's root directory, and all information that
  uniquely identifies a package is derived only from the package's
  file name.  Furthermore, it provides no way to introduce and
  retrieve relevant package information like "who is the maintainer"
  and "what is the upstream homepage".

- __Its meta-information handling mechanism is error-prone__.  For
  instance, when installing a package, _pkgsystem_ extracts all
  meta-information files immediately below the system's global root
  directory hierarchy.

- __Its database format is poorly structured__.  It scatters
  meta-information files through various directories instead of
  indexing by the triplet _name/version/arch_.

- __It doesn't support remote package management__.  Users can only
  handle packages that are virtually available from the local file
  systems.

- __It isn't designed for extensibility__.  In fact, it duplicates too
  much code as a consequence of badly designed modularization and
  internal interfaces.

- __Its internationalization support is defective__.  Its i18n support
  doesn't follow the recommended practices for internationalization,
  and it has some major known vulnerabilities like using the built-in
  `bash` interface to `gettext`.

- __Its command line UI is primitive__.  It doesn't offer to users the
  expected interface, lacking basic functionality.

- __Its treatment of errors is neither standardized nor systematic__.
  Error handling is not systematically carried out throughout its
  source code, resulting in security and stability problems.

- __It has support for only one compression algorithm__.  It has
  hard-coded support for a single, arbitrarily chosen, compression
  algorithm.

- __Its user messages are unpleasantly scarce__.  Often users don't
  know what is going on during normal or abnormal executions.

- __Its source code comments are not in English__.  Commenting in a
  language other than English is bad practice because tends to limit
  the collaboration between people and make it harder for people to
  understand how the program works.  Besides that, the source code
  isn't systematically commented.

- __It does not follow the GNU Coding Standards__.  In a GNU system,
  wherever applicable, it's advisable to follow the GNU coding
  standards.


</div>
