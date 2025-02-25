# mit-scheme-intel-mac-patch

* Installing MIT/GNU Scheme on macOS

These are my notes on installing <a
href="https://www.gnu.org/software/mit-scheme/">MIT/GNU
Scheme</a> 12 on versions of macOS more recent than
Monterey.  These have been tested MIT/GNU Scheme 12.1 on
macOS 15 running GCC-14 installed via Homebrew.  These
*should* work on earlier versions of macOS and/or GCC, but
I've not tested them.  As usual YMMV.

Below are instructions for Intel Macs.  For Apple Silicon,
these steps work but you'll need to to install GCC targeting
Intel.  See below for instructions.

<ol>
<li> Install GCC: I use Homebrew but you can use MacPorts,
   Fink, or install from source.
		
<li> Download <a
href="https://www.gnu.org/software/mit-scheme/">MIT/GNU
Scheme</a> and unpack it

<li> Download [this patch](mit-scheme-12.1-patch) and save
it in `mit-scheme-12.1/src`
	  
  - Note: the patch (1) switches off some Apple-specific
	  headers in a few files; and (2) disables
	  `macosx-starter`, needed for the stand-alone macOS app
	  (which I was never able to get working)
	  
<li>The rest is standard:
	  
   <ol>
   
   <li> Make sure your paths are set up correctly to use
	  your GCC (especially on Apple Silicon): check your
	  `LIBRARY_PATH` and `C_INCLUDE_PATH` to make sure the
	  GCC versions of your libraries / include dirs are
	  being used; for Homebrew, usually these are
	  `/usr/local/lib` and `/usr/local/include`,
	  respectively.  If you have XCode installed, you may
	  also want to unset `SDKROOT` if it's defined.
	  
   <li> `./configure`
   <li> `make`
   <li> `sudo make install`
	</ol>
	
</ol>

On Apple Silicon with Rosetta, you'll need a GCC that
outputs Intel binaries.  Easiest way is to download the
Homebrew tarball, unpack it into `/usr/local/homebrew`, then
run it in Rosetta.  You may need to create and change
ownership / permissions on a few directories under
<tt>/usr/local</tt> to make Homebrew happy.

Notes:<ol>
      <li>On Apple Silicon, a couple features are
      broken: <tt>DISK-RESTORE</tt> doesn't work for me (but
      curiously <tt>mit-scheme --band ...</tt> does),
      and `load-option` sometimes stops working after loading a
      couple things.</li>
      <li>On Intel Macs, everything seems to work just fine for me, but
      YMMV.  Use at your own risk!</li>
    </ol>

