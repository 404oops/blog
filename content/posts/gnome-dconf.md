+++
date = '2025-08-19T19:20:12+02:00'
title = 'DConf Enterprise Deployment - a How-to and Why?'
tags = ['configuration', 'enterprise']
categories = ['computers']
+++

Have you ever wanted to transition your company to Linux? Sure you have. But seriously - Linux might be the best operating system for enterprise users because you don't have to pay GARGANTUAN prices for shit as simple as enterprise deployment.

Programs like Google Chrome, Firefox and others have their own ways of managing enterprise deployment defaults, like default installed extensions, configurations and extensions policies.

See where I'm going with this? You can also do this on GNOME.

GNOME is undeniably the best shell. Everything works, it has unified standards, and the best part of it is that it's highly customizable and has a lot of options for all users. Which actually includes enterprise deployment.

It may not seem like it, but Enterprises need defaults for NEW USERS. New users are users created by an LDAP/AD server. Think of it as a database of users that can be applied to multiple applications, unified, like Nextcloud, and of course a Linux system.

## What is my issue?

My company uses Ubuntu Linux for its computers. All computers are the same, so we can do FOG deployment to registered computers. Defaults on Ubuntu are standard now, and everyone knows how to navigate it.

What's the problem? Ubuntu Linux. It's getting worse and worse, and no attention to detail is given. X11 has weird glitches, the whole shell has issues, and it's not plausible for production use. This company had to call me several times for on-site fixes because the OS just isn't palatable for use in a company full of semi-illiterate employees.

That's why I opted into openSUSE Leap. openSUSE is an enterprise-ready distribution, particularly with YaST2, which eases the administration of a system by a TON. I can select modules and install them on-demand, like a KVM host, without having to Google and sift through tutorials.

RPM packages are a worry for many system administrators, but Zypper supports YUM/DNF repositories, so it's just a worry of accepting package signatures for external repos and being done with it.

openSUSE is also proven to be EXTREMELY stable. My experience proves it. For someone that fucks around on an Operating System, I've seen it perform better than Ubuntu. BUT it's EXTREMELY different. Particularly because the default GNOME-Shell is untouched.

## How does one fix it?

Well, by replicating Ubuntu's shell. If it ain't broke, why fix it? If you want to do something else, skip to the part where you make DConf edits

Let's start with the obvious:

1. Desktop Icons are a MUST
2. Dash-to-dock, on the left, extended
3. Application status icon support
4. Tiling helper (like on Windows)
5. Alt-F4 for Shutting down

Download all of these extensions locally. Don't worry, we'll make it global later. We just need the GNOME-Shell Extension Marketplace versions of extensions for maximal compatibility.

If DING doesn't work for you, then DING-GTK4 might. But either way, it's a must-have for replicating Ubuntu

## DConf Editing

Now, onto DConf.

Before we start, lets define our $EDITOR. This will make it easier to unify and streamline the experience for users with a specific editor preference, whether it be VSCode, Emacs, VIM, Neovim, Nano or whatever else

```bash
export EDITOR=vim
```

Let's start by making a profile folder. This'll be relevant

```bash
sudo mkdir /etc/dconf/profile/
```

and then create and edit the "user" file that'll dictate that there's a system database on the system

```bash
$EDITOR /etc/dconf/profile/user # REPLACE $EDITOR WITH YOUR PREFERRED EDITOR
```

```
user-db:user
system-db:local
```

This will dictate that the user directory belongs to the user database and that the local directory belongs to the system database. This is highly important because you have to dictate a directory where your configuration files will be stored.

Now let's make a new directory inside the profile/db directory.

```bash
sudo mkdir /etc/dconf/profile/db/local.d
```

and start by writing a file. It should start with a number to dictate order of policy.

```bash
$EDITOR /etc/dconf/profile/db/local.d/00-Default
```

And now onto making the settings. These are written in INI. Here's an example of what I have:

```ini
[org/gnome/shell]
enabled-extensions=['dash-to-dock@micxgx.gmail.com', 'appindicatorsupport@rgcjonas.gmail.com', 'tiling-assistant@leleat-on-github', 'shutdown-dialogue@subashghimire.info.np', 'gtk4-ding@smedius.gitlab.com']
```

This only dictates extension status. Extensions have their own settings.

Locations of where each setting is located need to be encapsulated in the section tag.

Configurations of extensions (like Dash-to-dock) can be exported to INI:

```bash
dconf dump /org/gnome/shell/extensions/dash-to-dock
```

IMPERATIVE FOR DASH-TO-DOCK: Do NOT copy monitor placement information. It could result in an unusable extension.

Here's an example of what should come out (when you omit monitor configuration):

```ini
[org/gnome/shell/extensions/dash-to-dock]
apply-custom-theme=true
dash-max-icon-size=48
dock-fixed=true
dock-position='LEFT'
extend-height=true
```

This replicates Ubuntu defaults.

### What about the Background?

Backgrounds are IMPORTANT for branding. Accent colors can also be modified on GNOME, so watch out.

Place your background in a globally accessible directory, like /opt/ and change it to your background.

Dump your background configuration information:

```bash
dconf dump /org/gnome/desktop/background
```

then watch out for these 4:

```ini
picture-uri
picture-uri-dark # Make this the same as picture-uri
picture-options
primary-color
secondary-color
```

Copy those over to your global configuration file (or segment sections into different files), with the section tag and everything.

### But what if I don't want my employees and clientele to change their backgrounds?

Let's start by making a directory called locks. Locks dictate which values cannot be changed.

```bash
mkdir /etc/dconf/db/local.d/locks/
```

And then put these into a file in that directory

```bash
$EDITOR /etc/dconf/db/local.d/locks/default # You can name it whatever you want
```

```ini
/org/gnome/desktop/background/picture-uri
/org/gnome/desktop/background/picture-uri-dark
/org/gnome/desktop/background/picture-options
/org/gnome/desktop/background/primary-color
/org/gnome/desktop/background/secondary-color
# as well as other values you'd like to lock
```

Let's fix permissions:

```bash
chmod 644 -R /etc/dconf/db/local.d
```

## Extensions

Let's copy the extensions into the directory. It's best to do it with Nautilus.

```bash
nautilus admin:/
```

This will request your root user password so if you don't have it set, set it now.

navigate to /usr/share/gnome-shell/extensions/ and copy your extensions from \~/.local/share/gnome-shell/extensions/ there.

Change permissions of ALL DIRECTORIES to match drw--rw--rw- because of automatic updates:

```bash
chmod 666 -R /usr/share/gnome-shell/extensions/
```

and you're essentially done for configuring the machine

# Epilogue

In this article you learned how to:

1. Replicate Ubuntu's GNOME-Shell and;
2. Globally configure the GNOME-Shell with Dconf

If you want to jump into experimenting with dconf values, you can do so by using the dconf editor built into your system (unless you uninstalled it). Otherwise, you're finished
