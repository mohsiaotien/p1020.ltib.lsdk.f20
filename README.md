p1020.ltib.lsdk.f20
===================

Freescale P1020 e500v2 LSDK on Fedora 20

FreeScale P1020wlan-v0002 SDK is ready on Fedora 7
But it will need a little effort to modify some files to build it on Fedora 20

(1) Downgrade gmake version to 3.81

(2) patch all patches under ubuntu-ltib-patch directory  from
      https://github.com/fwmiller/Conserver-Freescale-Linux-U-boot.git

(3) Modify dist/lfs-5.1/base_libs/base_libs.spec

perl -w -e '
    @ARGV = grep { `file $_` =~ m,ASCII\s+.*text, } @ARGV;
    exit(0) unless @ARGV;
    $^I = ".bak";
.....


(4) Modify dist/lfs-5.1/glibc/glibc.spec & glibc-2.3.2.spec

perl -w -e '
    @ARGV = grep { `file $_` =~ m,ASCII\s+.*text, } @ARGV;
    exit(0) unless @ARGV;
    $^I = ".bak";
......

(5) -ltinfo not found issue in LVM2.2.02.45
In LVM2.2.02.45 configure script, Locate following line

    for ac_lib in '' tinfo ncurses curses termcap termlib;

then remove tinfo from the list

(6) dhcp-3.0.3b1 Linux Major version is not matched in configure script
In configure script, Locate

 Linux)
      release=`uname -r`
      minor=`echo $release |sed -e 's/[0-9]*\.\([0-9][0-9]*\)\(\..*\)*$/\1/'`
      major=`echo $release |sed -e 's/\([0-9][0-9]*\)\..*$/\1/'`
   
      case $major in
 1) sysname=linux-1 ;;
 2) case $minor in         ====> Change $major to 3
      0) sysname=linux-2.0 ;;
      1) sysname=linux-2.1 ;;
      2) sysname=linux-2.2 ;;
      *) sysname=linux-2.2 ;;
    esac;;
      esac;;

After fixing these, I can successfully buid FreeScale P1020wlan-v0002 SDK on Fedora 20
