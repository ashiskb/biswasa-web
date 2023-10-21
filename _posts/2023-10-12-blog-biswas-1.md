---
title: 'Ubuntu 22.04 -- Change what is displayed at Login shell / Message of the Day'
date: 2023-10-12
permalink: /posts/2023/10/biswas/blog-motd/
tags:
  - motd
  - ubuntu
  - ubuntu22.04
  - login shell
---

You can customize the CLI login shell of Ubuntu (e.g., 22.04) by updating the `motd` (message of the day) file. `MOTD`` is a simple text file in a Linux/Debian-based system that is used to display some custom text message on login using the command line, locally or via SSH.

* You can add your ASCII art /custom text --
  *  either in `/etc/motd` file, 
  *  or add a script in `/etc/update-motd.d/` directory. For instance, `00-header` file containing your text.
* You may need to reboot (!)
* It is just a normal text file, but it accepts a good number of escape sequences:

---
| Escape character | Description |
| :- | :- | 
| `\b` | Baud rate (the rate at which information is transferred in a communication channel) of the current line |
| `\d`| Current date|
| `\s` | System name, the name of the operating system |
| `\l` | Name of the current tty line |
| `\m` | Architecture identifier of the machine, eg. i486 |
| `\n` | Node name of the machine, also known as the hostname |
| `\o` | Domain name of the machine |
| `\r` | Release number of the OS, eg. 1.1.9 |
| `\t` |  Current time |
| `\u` | Number of current users logged in |
| `\U` | The string `1 user` or `x users` where `x` is the number of current users logged in |
| `\v` | Version of the OS, eg. the build-date etc. |

* You can also add command like `banner "   gpux4` to create something like this:

```
                                                        #
                          ####   #####   #    #  #    # #    #
                         #    #  #    #  #    #   #  #  #    #
                         #       #    #  #    #    ##   #######
                         #  ###  #####   #    #    ##        #
                         #    #  #       #    #   #  #       #
                          ####   #        ####   #    #      #
```
* Finally, this was the content from my server at `/etc/update-motd.d/00-header`:

```bash
echo "##########################################################################"; 
banner "   gpux4   "
echo "##########################################################################"; 

[ -r /etc/lsb-release ] && . /etc/lsb-release

if [ -z "$DISTRIB_DESCRIPTION" ] && [ -x /usr/bin/lsb_release ]; then
        # Fall back to using the very slow lsb_release utility
        DISTRIB_DESCRIPTION=$(lsb_release -s -d)
fi

printf "Welcome to %s (%s %s %s)\n" "$DISTRIB_DESCRIPTION" "$(uname -o)" "$(uname -r)" "$(uname -m)"
```
