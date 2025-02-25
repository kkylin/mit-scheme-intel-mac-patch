# mit-scheme-intel-mac-patch

* Installing MIT/GNU Scheme on macOS

These are my notes for installing <a
href="https://www.gnu.org/software/mit-scheme/">MIT/GNU
Scheme</a> on recent versions of macOS (meaning more recent
than Monterey).  These have been tested MIT/GNU Scheme 12.1
on macOS 15 running GCC-14 (installed via Homebrew).  These
*should* work on earlier versions of macOS and/or GCC, but
I've not tested them.  As usual YMMV.

Below are instructions for Intel Macs.  For Apple Silicon,
these steps work but you'll need to to install GCC targeting
Intel.  See below for instructions.

<ol>

<li> <strong>Install GCC.</strong>  I use Homebrew but you can use
   MacPorts, Fink, or install from source.
		
<li> <strong>Download <a
href="https://www.gnu.org/software/mit-scheme/">MIT/GNU
Scheme</a> and unpack it.</strong>

<li> <strong>Download [this patch](mit-scheme-12.1-patch)
and save it in `mit-scheme-12.1/src`.</strong> (The patch
(i) switches off some Apple-specific headers in a few files;
and (ii) disables `macosx-starter`, which isneeded for the
stand-alone macOS app -- and which I was never able to get
working.)
	  
<li><strong>Build MIT Scheme as usual</strong>
	  
   <ol>
   
   <li> Make sure your paths are set up correctly to use
	  your GCC (especially on Apple Silicon): in
	  `LIBRARY_PATH` and `C_INCLUDE_PATH`, the GCC versions
	  of your libraries / include dirs, respectively, should
	  come first; for Homebrew, usually these are
	  `/usr/local/lib` and `/usr/local/include`,
	  respectively.  If you have XCode installed, you may
	  also want to unset `SDKROOT` if it's defined.
	  
   <li> `./configure`
   <li> `make`
   <li> `sudo make install`
</ol>
	
</ol>

On Apple Silicon with Rosetta, you'll need to do the
following first, then follow the steps above: <ol>

<li> <strong>Install Rosetta</strong> if you don't have it
already.  `softwareupdate --install-rosetta` should work.

<li> <strong>Make yourself a Rosetta-only Terminal:</strong>
duplicate the Terminal app, rename it something informative,
e.g., Intel-Terminal, right-click on the icon, and check
"Open using Rosetta."  After this, whenever you double-click
Intel-Terminal, you'll get a terminal that runs in Rosetta
and whose subprocesses also run in Rosetta if possible.
(You don't have to do this with Terminal -- any app that
gives you a shell and ships with a fat binary should work.
I use a stock Emacs.)

<li> <strong>Install a GCC that targets Intel CPUs.</strong>
Easiest way is to follow the instructions <a
href="https://docs.brew.sh/Installation">here</a> to
download the Homebrew tarball, unpack it into
`/usr/local/homebrew`, then run this version of Homebrew in
your Intel-Terminal (see above).  You may need to create and
change ownership / permissions on a few directories under
~/usr/local~ to make Homebrew happy.  After this, use your
Intel version of ~brew~ to install GCC.  Use this version of
GCC to build MIT Scheme.

</ol>

Notes:<ol>
      <li>On Apple Silicon, a couple features are
      broken: <tt>DISK-RESTORE</tt> doesn't work for me (but
      curiously <tt>mit-scheme --band ...</tt> does),
      and `load-option` sometimes stops working after loading a
      couple things.</li>
      <li>On Intel Macs, everything seems to work just fine for me, but
      YMMV.  Use at your own risk!</li>
    </ol>

