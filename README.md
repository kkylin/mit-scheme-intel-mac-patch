# mit-scheme-intel-mac-patch

* Installing MIT/GNU Scheme on macOS

These are notes on installing MIT/GNU Scheme 12 on versions
of macOS more recent than Monterey.  These have been tested
MIT Scheme 12.1 on macOS 15 running GCC-14 installed via
Homebrew.  As usual YMMV.

Below are instructions for Intel Macs.  For Apple Silicon,
these steps work but you'll need to to install GCC targeting
Intel.  See below for instructions.

1. Install GCC: I use Homebrew but you can use MacPorts,
   Fink, or install from source.
		
1. Download MIT/GNU Scheme 12.1

1. Download and apply [this patch](mit-scheme-12.1-patch).
	  
  - The patch switches off some Apple-specific headers
	  in a few files, and disables the Mac-specific
	  <tt>macosx-starter</tt>.
	  
1. The rest is standard:
	  
   a. Make sure your paths are set up correctly to use your
	  GCC (especially on Apple Silicon): check your
	  LIBRARY_PATH and C_INCLUDE_PATH to make sure the GCC
	  versions of your libraries / include dirs are being
	  used.  I also set LD_LIBRARY_PATH just in case.  (For
	  Homebrew, usually these are /usr/local/lib and
	  /usr/local/include.)  If you have XCode installed, you
	  may also want to unset SDKROOT.
	  
   a. <tt>./configure</tt>
   a. <tt>make</tt>
   a. <tt>sudo make install</tt>

1. On Apple Silicon with Rosetta, you'll need a GCC that
   outputs Intel binaries.  Easiest way is to download the
   Homebrew tarball, unpack it into
   <tt>/usr/local/homebrew</tt>, then run it in Rosetta.
   You may need to create and change ownership / permissions
   on a few directories under <tt>/usr/local</tt> to make
   Homebrew happy.</li>

Notes:<ol>
      <li>On Apple Silicon, a couple features are
      broken: <tt>DISK-SAVE</tt> doesn't work for me (but
      curiously <tt>mit-scheme --band ...</tt> does),
      and <tt>load-option</tt> sometimes stops working after loading a
      couple things.</li>
      <li>On Intel Macs, everything seems to work just fine for me, but
      YMMV.  Use at your own risk!</li>
    </ol>
    <address>
      Last updated on January 16, 2025
      by <a href="kkylin.github.io">KKL</a>.
    </address>
  </body>
</html>

