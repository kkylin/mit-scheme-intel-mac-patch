# Installing MIT/GNU Scheme on macOS

These are my notes for installing <a
href="https://www.gnu.org/software/mit-scheme/">MIT/GNU
Scheme</a> on recent versions of macOS (meaning more recent
than Monterey).  These have been tested MIT/GNU Scheme 12.1
on macOS 15 running GCC-14 (installed via Homebrew).  These
*should* work on earlier versions of macOS and/or GCC, but
I've not tested them.  As usual YMMV.

A little bit of context: for reasons given elsewhere, the
MIT Scheme compiler currently won't work on Apple Silicon.
(The Linux-ARM distribution won't run on Macs.)  So if you
want to use MIT Scheme with native code compilation on a
Mac, you will either need to run it on an Intel Mac or on
Apple Silicon with Rosetta.  Additionally, while you can
install MIT Scheme using Apple's clang-based toolchain, the
resulting native code compiler is partially broken for
reasons that remain mysterious to me.  But building using
GCC seems to solve the problem.  These notes describe how to
build MIT Scheme using GCC, which means disabling some
Apple-specific bits of code.  In particular, I don't know
how to make a stand-alone Mac app.

Another bit of clarification: these notes talk about
"building" MIT Scheme.  It is useful to know that one can
view MIT Scheme as consisting of two parts: "microcode"
written in C and providing basic runtime support, and
everything else, almost all in Scheme.  When I talk about
building MIT Scheme, it is about compiling the C part.
You'll usually want to use a distribution with the Scheme
part precompiled, because otherwise you'll really want to
have a working MIT Scheme compiler (usually an older
version).

What follows are instructions for Intel Macs.  It assumes
familiarity with working from the command-line.  For Apple
Silicon, these steps work but you'll need to to install GCC
targeting Intel; see <a href="#apple-silicon">here</a> for
instructions.

1. **Install GCC.**  I use Homebrew but you can use
   MacPorts, Fink, or install from source.

1. **Download <a
href="https://www.gnu.org/software/mit-scheme/">MIT/GNU
Scheme</a> and unpack it.**

1. **Download [this patch](mit-scheme-12.1-patch) and save
it in `mit-scheme-12.1/src`.** Note the patch just (i)
switches off Apple-specific headers in a few files; and (ii)
disables `macosx-starter`, needed for the stand-alone macOS
app (which I never got working).  Apply the patch, i.e., run
`patch < mit-scheme-12.1-patch` in a shell in the directory
`mit-scheme-12.1/src`.

1. **Build MIT Scheme as usual**
	  
   - Make sure your paths are set up correctly to use your
	  GCC (especially on Apple Silicon): in `PATH`,
	  `LIBRARY_PATH`, and `C_INCLUDE_PATH`, paths to the GCC
	  binaries / libraries / include dirs, respectively,
	  should come first; for Homebrew, usually these are
	  `/usr/local/bin`, `/usr/local/lib`, and
	  `/usr/local/include`, respectively.  If you have XCode
	  installed, you may also want to unset `SDKROOT` if
	  it's defined.

   - Then in your shell:
```
./configure
make
sudo make install
```

<a name="apple-silicon">

# Apple Silicon

On Apple Silicon, you'll need to do the following first,
then follow the steps above:

1. **Install Rosetta** if you don't have it already.
`softwareupdate --install-rosetta` should work.

1. **Make yourself a Rosetta-only Terminal:** duplicate the
Terminal app, rename it something informative, e.g.,
Intel-Terminal, right-click on the icon, and check "Open
using Rosetta."  After this, whenever you double-click
Intel-Terminal, you'll get a terminal that runs in Rosetta
and whose subprocesses also run in Rosetta if possible.
(You don't have to do this with Terminal -- any other app
that ships with a fat binary and gives you a shell will do.
I use a stock Emacs.)

1. **Install a GCC that targets Intel CPUs.** Easiest way is
to follow the instructions <a
href="https://docs.brew.sh/Installation">here</a> to
download the Homebrew tarball, unpack it into
`/usr/local/homebrew`, then run this version of Homebrew *in
your Intel-Terminal* (see above).  For example, if you
called your Intel version of Homebrew `intel-brew`, then do
`intel-brew install gcc`.  You may need to create and change
ownership / permissions on a few directories under
`/usr/local` to make Homebrew happy.  After this, use your
Intel version of `brew` to install GCC.  Use this version of
GCC to build MIT Scheme as outlined above.

Notes:

1. On Apple Silicon, a couple features are broken:
   `DISK-RESTORE` doesn't work for me (but curiously
   `mit-scheme --band ...` does), and `load-option`
   sometimes stops working after loading a couple
   things.
   
1. On Intel Macs, everything seems to work just fine for me,
   but YMMV.  Use at your own risk!

1. From David Gray: "For anybody who needs macports over
homebrew I did these extra steps to select the correct
compiler:"
```
sudo port install gcc14 
port select --list gcc
sudo port select --set gcc mp-gcc14
hash -r
```

1. Instead of installing an Intel version of GCC, you may
also try cross-compiling or using a container.  I didn't go
down either of those routes and don't have anything useful
to add.

## Sources and acknowledgements

1. The insight to use GCC rather than Apple's Clang is due
   to David Gray; see <a
   href="https://lists.gnu.org/archive/html/mit-scheme-users/2024-12/threads.html">here</a>
   and <a
   href="https://lists.gnu.org/archive/html/mit-scheme-users/2025-02/threads.html">here</a>
   for context

1. The Apple Silicon instructions are based largely on <a
   href="https://kennethfriedman.org/thoughts/2021/mit-scheme-on-apple-silicon/">Kenneth
   Friedman's post</a>, with very minor tweaks

1. The advice on installing Intel-targeting GCC comes from [here](https://www.wisdomgeek.com/development/installing-intel-based-packages-using-homebrew-on-the-m1-mac/)

1. The advice on making `Intel-Terminal` comes from various places around the web
