Win32::SerialPort and Win32API::CommPort
VERSION=0.19, 4 November 1999

Hello Serial Port users:

If you are not running Windows, you want the Device::SerialPort module
instead of this one. It has a compatible user interface and runs on
Operating Systems that fully support POSIX.pm. Available from your
favorite CPAN site. Since someone asked, MS-DOS does NOT support POSIX.pm.

These modules are intended for Win32 ports of Perl without requiring
a compiler or using XS. In every case, compatibility has been selected
over performance. Since everything is (convoluted but still pure) perl,
you can fix flaws and change limits if required. But please file a bug
report if you do. I have tested with the ActiveState (3xx and 5xx) and
Core (GS 5.004_02) distributions. But these modules should work on any
Win32 version that supports Aldo Calpini's Win32::API.

This is the fifth production release. All planned features are now
implemented. A few are still experimental - see the documentation.
I suspect there are still bugs - but I only know of one: "is_parity_enable"
sometimes fails on NT (test4.t #86). I have not been able to duplicate this
bug on my system. If you do get this test to fail, please let me know if
you can isolate it to some line in the module source code. If you see any
place where the code does not match the documentation, consider it a bug
and please report it. Except for those items highlighted as "experimental"
in the documentation (tied FileHandles, "lookfor" details, and stty
emulation), the modules have been very stable since Nov 8, 1998 (0.12).

INSTALLATION:

Version 0.18 adds a more Un*x-like installation for ALL Perl versions.

   perl Makefile.PL [optional PORT]
   perl test.pl
   perl install.pl

The Makefile.PL creates the test.pl, install.pl, and t/DefaultPort.pm
files. With no port specified, COM1 is used.

Since ActiveState build 3xx did not install html docs by default, I don't
install them.  You can manually copy them from the html subdirectory if
you want.

'perl options.plx' should run without errors after installing.

UPGRADE GOTCHAS:

1. The test programs and some of the demo programs have added features
   since the last beta release. The new ones won't work on earlier
   versions (0.17 and below).

2. The configuration file used by start, restart and tie must be
   regenerated (by save) for Version 0.18 and above. Sorry about that.

3. The defaults for the stty_xxx parameters have been changed to no
   processing (raw data). In earlier versions, the default matched
   the setup one would use with a dumb terminal (echo on, convert
   \r to \n on input and \n to \r\n on output).

4. The matchclear method was added to fix a bug with lookfor and
   matches at the beginning of the input (usually "empty" lines).
   This may change the return value in certain cases.

5. In many cases, streamline is a better choice than lookfor when
   receiving large quantities of data.

COMPATIBILITY NOTES: (changes since 0.15)

1. The Tied FileHandle support works fully on 5.005, mostly on 5.004,
   and essentially NOT on 5.003. This is due to the level of support
   in the underlying Perl - not to the module implementation.

2. Version 0.16 eliminated the "dummy (0, 1) list" returned by most
   of the binary methods in case they were called in list context.
   I do not think that feature was used outside the test suite.

3. A "syswrite" from a tied FileHandle now checks stty_opost and
   performs the output conversions. You can still use "write" to
   generate "raw" output even with stty_opost==1.

These modules use Aldo Calpini's Win32::API module extensively. It is
available at:

    http://www.divinf.it/dada/perl/Win32API-0_011.zip

for AS build 3xx and GS 5.004_02. For AS build 5xx, it is available as
the package Win32-API using PPM on the ActiveState repository.
Get it, install it, and test it BEFORE trying Win32API::SerialPort. The
"-w" complaints (xxx used only once) under AS 3xx are normal. CommPort
now fixes them - but other modules probably do not.

See the NOTES and KNOWN LIMITATIONS in the SerialPort documentation. The
".pod" is embedded in the ".pm". The comments on "-w" and "use strict"
are especially relevant when you start calling this module from your own
code. This module has been tested on Win95 with AS Builds 315, 500, 509 and
the GS binary 5.004_02. Thanks to Ken White for testing on NT. Also thanks
to the others who have contributed comments and suggestions.

FILES:

    Changes		- for history lovers
    Makefile.PL		- the "starting point" for traditional reasons
    MANIFEST		- file list
    README.txt		- this file (CRLF)
    README    		- same file with CPAN-friendly name (LF only)
    eg/any_os.plx	- cross-platform use and init
    eg/demo1.plx	- talks to a "really dumb" terminal
    eg/demo2.plx	- "poor man's" readline and chat
    eg/demo3.plx	- looks like a setup menu - but only looks :-(
    eg/demo4.plx	- simplest setup: "new", "required param", "restart"
    eg/demo5.plx	- "waitfor" and "nextline" using lookfor
    eg/demo6.plx	- basic tied FileHandle operations, record separators
    eg/demo7.plx	- a Perl/Tk based terminal, event loop and callbacks
    eg/demo8.plx	- command line terminal emulator with Term::Readkey
    eg/demo9.plx	- using debug on a close()
    eg/options.plx	- post-install test that prints available options
    eg/stty.plx		- first try at Unix lookalike

    lib				- install directory
    lib/Win32			- install directory
    lib/Win32/SerialPort.pm	- the reason you're reading this
    lib/Win32API		- install directory
    lib/Win32API/CommPort.pm	- the raw API calls and other internals

    html			- install directory
    html/Win32			- install directory
    html/Win32/SerialPort.html	- documentation
    html/Win32API		- install directory
    html/Win32API/CommPort.html	- documentation

    Altport.pm		- stub for inheritance test
    t			- test directory
    t/test1.t		- RUN ME FIRST, tests and creates configuration
    t/test2.t		- tests restarting_a_configuration and timeouts
    t/test3.t		- Inheritance and export version of test1.t
    t/test4.t		- Inheritance version of test2.t and "restart"
    t/test5.t		- tests to optional exports from CommPort
    t/test6.t		- stty tests
    t/test7.t		- tied FileHandle tests 5.004+ (was test6.t)

This is a genuine production release. You can complain if I don't
maintain compatibility from this point forward. The experimental
items might change a bit - but I'll try to avoid breaking any code.

TEST:

With 5.004+, the Benchmark routines are used to generate reports.
The test suite covers most of the module methods and leaves the port
set for 9600 baud, 1 stop, 8 data, no parity, no handshaking, and
other defaults. At various points in the testing, it expects
unconnected CTS and DTR lines. The final configuration is saved
as COM1_test.cfg in this directory.

Tests may also be run individually by typing:
	'perl test?.t Page_Delay [COMx]'

With no delay, the tests execute too rapidly to follow from an MS-DOS
command line. Delay may be set from 0 to 5 seconds.

All tests are expected to pass - I would be very interested in hearing
about failures ("not ok"). These tests should be run from a command
line (DOS box).

DEMO PROGRAMS:

Connect a dumb terminal (or a PC that acts like one) to COM1 and setup
the equivalent configuration. Starting demo1.plx should print a three
line message on both the terminal and the Win32 command line. The
terminal keyboard (only) now accepts characters which it prints to both
screens until a CONTROL-Z is typed. Also included is demo2.plx - a truly
minimal chat program. Bi-directional communication without an event loop,
sockets, pipes (or much utility ;-) This one uses CAPITAL-Q from the
active keyboard to quit since <STDIN> doesn't like CONTROL-Z. And each
command shell acts a little differently (Cygnus "bash", COMMAND.COM).
Try running the terminal at 4800 baud to get errors (or 300 to get
"breaks").

AltPort.pm and test3.t implement the "basic Inheritance test" discussed
in perltoot and other documentation. It also imports the :STAT constants.
It's otherwise only slightly modified from test1.t (you'll get a different
"alias" if you run test2.t or demo3.plx after test3.t). There are some
subtle functional changes between test2.t and test4.t. But test4.t also
calls CommPort methods directly rather than through SerialPort and adds
tests for lookfor and stty_xxx methods.

You can read (many of the important) settings with demo3.plx. If you
give it a (valid) configuration file on the command line, it will open
the port with those parameters (and "initialized" set - so you can test
simple changes: see the parity example at the end of demo3.plx).

Run options.plx to see the available choices for various parameters
along with the current values. If you have trouble, I will probably
ask you to save the output of options.plx in a file and send it to me.
You can specify a port name for options.plx on the command line
(e.g. 'perl options.plx COM2').

Demo4.plx is a "minimum" script showing just the basics needed to get
started.

Demo5.plx demonstrates various uses of the lookfor routine including
setups for "waitfor" and a primitive "readline". Try them out. The
default "stty" settings work with a VT-100 style terminal. You may
have to set the options by hand. Use any editor. Let me know if the
descriptions in the documentation are useable. And if any more options
are necessary.

Demo6.plx demonstrates tied FileHandles. Perl 5.005 is recommended.
It "requires" 5.004. It implements timeouts on all user inputs - so
you can run it "hands-off" to see what happens.

Demo7.plx uses Tk to create a terminal emulator. Its included to show
polling and callbacks using an event loop.

Demo8.plx is a simple command-line terminal emulator contributed by
Andrej Mikus.

Stty.plx is a wrapper around the stty method that implements a clone
of the Unix/POSIX function of the same name. It's line noise unless
you know Unix.

Bruce Winter supplied the any_os.plx cross-platform demo.

The Perl Journal #13 included an article on Controlling a Modem with
Win32::SerialPort. Examples from the article and additional demos can be
found on the website below.

Please tell me what does and what doesn't work. The module has proven
to be pretty robust. But I can't test all possible configurations.
Don't trust it for anything important without complete testing.

And watch for updates at:

%%%% http://members.aol.com/Bbirthisel/alpha.html

or CPAN under authors/id/B/BB/BBIRTH or
              Win32::SerialPort and Win32API::CommPort

Thanks,

-bill

Copyright (C) 1999, Bill Birthisel. All rights reserved. This module is
free software; you can redistribute it and/or modify it under the same
terms as Perl itself.
