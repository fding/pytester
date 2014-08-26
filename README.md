pytester
========

Pytester: runs unittest-based tests.  It requires TestCases to state all
dependencies (python files as well as data files).  Pytester caches results, so
that tests with no modified dependencies are not run.  However, a test might
succeed even if its dependencies are not correctly declared, if another test
has the same dependencies and ran first.  This is incorrect behavior, as we
might incorrectly skip a test even though its dependencies were modified. In
the future, a "check" command should be added that checks to see all
dependencies are stated correctly.

Internally, Pytester copies all required files into the .test_cache directory,
and runs all tests in that directory. If a file is not declared as a dependency
by any tests, then an ImportError or FileNotFound error would result; this helps
reduce some errors with dependency specification. To determine if a test needs
to be run, Pytester checks the modification date of all dependent files, and if
every file's modification time is more recent than that of the file in
.test_cache, pytester skips the corresponding test.

If the -p/--profile option is specified, Pytester profiles all the tests, which
can be viewed by "pytester view path/to/file:class".  If the -d/--debug option
is specified, Pytester will invoke the pdb debugger if an exception occured
(including Keyboard Interrupts).
