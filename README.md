# Erlyvideo and Background Information

This was originally an open source project but it went closed source and can be licensed here: [Flussonic](https://flussonic.com). Since I am working on a scientific project that will utilize video streaming, I wanted an open source version. Hence I started hacking around with this for video streaming as well as exploring other alternatives. Otherwise, for others,  the simplest thing is simply to pay for a Flussonic license. I have no idea what the relationship of this code base is to the current commercial version. You can also see the old README file and do some Google searching.


## Fixing the Bit Rot
I forked this from another repository (see above) that was one of a number of forked remnants of the original Erlyvideo. Probably a number of people forked this before it was closed up and eventually become the precursor of Flussonic. 

The _bit rot_ was in a number of areas:
1. The code used parameterized modules that were no longer supported in erlang (version 19). This was fixed by using a work around designed to deal with this. It is called [pmod_transform](https://github.com/erlang/pmod_transform). See their examples on how to use it. I had to also change its Makefile and the rebar.config to get things to compile.
2. `erlang:now/0` is depreciated and there are a number of replacements for it. I generally used the function `erlang:timestamp/0` since that returned a tuple compatible with the one returned by `erlang:now/0`. See the erlang documentation. It could be that some of code should have been rewritten to use a different function. 
3. There were depreciated functions from `crypto`. The highlighting in emacs usually gave good clues on what to replace it with.
4. Some of the nif's did not compile probably due to changes in rebar. It thought that all of the c-code in should go into one library and since there were two or more object files with a init files with the exact same name in them, this did not work. Fixed by changing the rebar.config file. 
5. In `elixer_tracker.erl` module the code assumed that `Module.info_available(compile)` returned an association with the atom time as a key. This no longer the case in the version of erlang I am using (v 19). This module needs some major rewriting since but I fixed it by simply putting some guard code around the offending pattern matching. This is a hack since this module basically no longer works. I believe it was used to detect when new code was added and automatically recompile and load it. 
6. Numerous include files pointing at the wrong directory were fixed. 

## Compiling and Running

### Compiling
The Makefile needs some work. For now simply clone the repository using git and type the command `rebar compile` in the main directory (erlyvideo). This assumes you have a recent version of Erlang properly installed. 

### Running
In the main directory (erlyvideo) run the command `make run`. It should run with a sample config file. I have yet to test what needs to be changed to get it to do something useful. (TO-DO)

# License

Erlyvideo is distributed under the GNU General Public License version 3 and is included in this source tree in the file COPYING. It was originally written by Max Lapshin. 

Erlyvideo has runtime dependencies from other packages:

* [amf](http://github.com/maxlapshin/eamf) distributed under MIT License and packaged inside Erlyvideo
* [erlydtl](http://github.com/erlyvideo/erlydtl) distributed under MIT License and packaged inside Erlyvideo
* [log4erl](http://github.com/erlyvideo/log4erl) distributed under MIT License and packaged inside Erlyvideo
* [misultin](http://github.com/ostinelli/misultin) distributed under BSD license and packaged inside Erlyvideo
* [pmod_transform](https://github.com/erlang/pmod_transform) distributed under the Erlang Public License
* src/mochijson2.erl distributed under MIT license and packaged inside Erlyvideo

