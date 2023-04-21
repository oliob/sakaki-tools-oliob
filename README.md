# Fork of sakaki-tools Gentoo Overlay

Overlay containing various utility ebuilds for Gentoo on EFI.

> 31 Oct 2020: sadly, due to legal obligations arising from a recent change in my 'real world' job, I must announce I am **standing down as maintainer of this project with immediate effect**. For the meantime, I will leave the repo up (for historical interest, and since the ebuilds etc. may be of use to others); however, I plan no further updates, nor will I be accepting / actioning further pull requests or bug reports from this point. Email requests for support will also have to be politely declined, so, **please treat this as an effective EOL notice**.<br><br>For further details, please see my post [here](https://forums.gentoo.org/viewtopic-p-8522963.html#8522963).<br><br>If you have used my [EFI Guide](https://wiki.gentoo.org/wiki/User:Sakaki/Sakaki%27s_EFI_Install_Guide) (and this repo) to install your PC-based Gentoo system, it *should* still continue to work for some time, **but** you should now take steps to migrate to a baseline [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64) install (since the underlying tools, such as [`buildkernel`](https://github.com/sakaki-/buildkernel), will also now no longer be supported and may eventually fail as more modern kernels etc. are released).<br><br>With sincere apologies, sakaki ><

Required for the tutorial ["**Sakaki's EFI Install Guide**"](https://wiki.gentoo.org/wiki/Sakaki's_EFI_Install_Guide) on the Gentoo wiki.

## List of ebuilds

* **app-portage/showem** [source](https://github.com/sakaki-/showem)
  * Provides a simple utility script (**showem**(1)), which enables you to monitor the progress of a parallel **emerge**(1). A manpage is included.
* **sys-kernel/buildkernel** [source](https://github.com/sakaki-/buildkernel)
  * Provides a script (**buildkernel**(8)) to build a (stub EFI) kernel (with integral initramfs) suitable for booting from a USB key on UEFI BIOS PCs. Automatically sets the necessary kernel configuration parameters, including the command line, and signs the resulting kernel if possible (for secure boot). Has a interactive and non-interactive (batch) mode. Manpages for the script and its configuration file (_/etc/buildkernel.conf_) are included.
* **app-portage/genup** [source](https://github.com/sakaki-/genup)
  * Provides the **genup**(8) script, to simplify the process of keeping your Gentoo system up-to-date. **genup**(8) can automatically update the Portage tree, all installed packages, and kernel. Has interactive and non-interactive (batch) modes. A manpage is included.
* **app-crypt/staticgpg**
  * A simple ebuild, derived from **app-crypt/gnupg** version 1.4.16, which creates a statically linked, no-pinentry version of **gpg**(1) suitable for use in an initramfs context. It can safely be installed beside a standard 2.x version of **app-crypt/gnupg** (which isn't SLOTted). Deploys its executable to _/usr/bin/staticgpg_. A placeholder manpage is included.

## Installation

As of version >= 2.2.16 of Portage, **sakaki-tools-oliob** is best installed (on Gentoo) via the [new plug-in sync system](https://wiki.gentoo.org/wiki/Project:Portage/Sync).
Full instructions are provided on the [Gentoo wiki](https://wiki.gentoo.org/wiki/Sakaki's_EFI_Install_Guide/Building_the_Gentoo_Base_System_Minus_Kernel#Preparing_to_Run_Parallel_emerges).

The following are short form instructions. If you haven't already installed **git**(1), do so first:

    # emerge --ask --verbose dev-vcs/git 

Next, create a custom `/etc/portage/repos.conf` entry for the **sakaki-tools-oliob** overlay, so Portage knows what to do. Make sure that `/etc/portage/repos.conf` exists, and is a directory. Then, fire up your favourite editor:

    # nano -w /etc/portage/repos.conf/sakaki-tools-oliob.conf

and put the following text in the file:
```
[sakaki-tools-oliob]
 
# Fork and reduced from Sakaki-tools: Various utility ebuilds for Gentoo on EFI
# Maintainer: oliob (sonst+sakaki-tools@o-oberst.de)
 
location = /usr/local/portage/sakaki-tools-oliob
sync-type = git
sync-uri = https://github.com/oliob/sakaki-tools-oliob.git
priority = 50
auto-sync = yes
```

Then run:

    # emaint sync --repo sakaki-tools-oliob

If you are running on the stable branch by default, allow **~amd64** keyword files from this repository. Make sure that `/etc/portage/package.accept_keywords` exists, and is a directory. Then issue:

    # echo "*/*::sakaki-tools ~amd64" >> /etc/portage/package.accept_keywords/sakaki-tools-repo
    
Now you can install packages from the overlay. For example:

    # emerge --ask --verbose app-portage/genup

## Maintainers

* [oliob](mailto:sonst+sakaki-tools@o-oberst.de)
