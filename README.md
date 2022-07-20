
<div id="top"></div>

<div align="center">

<img src="https://svg-rewriter.sachinraja.workers.dev/?url=https%3A%2F%2Fcdn.jsdelivr.net%2Fnpm%2F%40mdi%2Fsvg%406.7.96%2Fsvg%2Fmessage-bulleted.svg&fill=%23334155&width=200px&height=200px" style="width:200px;"/>

<h3 align="center">MOTD</h3>

<p align="center">
Modular Message of the Day (motd) Script built on NeoFetch. With additional PUPPET and SYSTEMD plugins.
</p>    
</div>

##  1. <a name='TableofContents'></a>Table of Contents

* 1. [Table of Contents](#TableofContents)
* 2. [About The Project](#AboutTheProject)
	* 2.1. [Built With](#BuiltWith)
	* 2.2. [Installation](#Installation)
* 3. [Usage](#Usage)
* 4. [ Customising](#Customising)
	* 4.1. [Adding Plugins](#AddingPlugins)
		* 4.1.1. [Filename](#Filename)
		* 4.1.2. [Your plugin](#Yourplugin)
		* 4.1.3. [motd](#motd)
		* 4.1.4. [Output](#Output)
		* 4.1.5. [Custom Branding](#CustomBranding)
* 5. [Troubleshooting](#Troubleshooting)
* 6. [Contributing](#Contributing)
* 7. [License](#License)
* 8. [Contact](#Contact)
* 9. [Changelog](#Changelog)


##  2. <a name='AboutTheProject'></a>About The Project

This is a heavily cut-down and modularised version of NeoFetch [https://github.com/dylanaraps/neofetch](https://github.com/dylanaraps/neofetch)

Built to be customisable on the display with a plugin system.

The notable differences are as follows:

1. Most functions are now in separate files in the `plugin` folder. Easier to read and debug.
1. Functions for formatting output are separate files in the `components` folder.
1. Additional custom PUPPET-based plugin for displaying lock messages, status, etc...
1. Additional systemd plugin for displaying the status of services.

```bash
❯ motd

███████████████  ████████████████████████     ------------- Andys-MacBook-Air.local -------------
████████████████  ███████████████████████     
█████████████████  ██████████████████████     HOST       Andys-MacBook-Air.local 
██████████████████  █████████████████████     OS         macOS 
███████████████████  ████████████████████     MODEL      MacBookAir8,1 
████████████████████  ███████████████████     LOCAL_IP   192.168.1.15 
█████████████████████  ██████████████████     PUBLIC_IP  79.79.210.236 
██████████████████████  █████████████████     
███████████████████████  ████████████████     ---------------------- NODE ----------------------
████████████████████████  ███████████████     
                                              UPTIME     -----------------------======== 156% 
                                              MEMORY     ---------====== 10681MiB / 16384MiB (65%) 
                                              DISK       -============== 14G / 466G (9%) 
                                              
                                              ------------------ HELP COMMANDS ------------------
                                              
                                              help       List custom scripts. 
                                              cheat      command cheatsheet. 
                                              
                                              ----------------------- ◣◥ -----------------------
```

<p align="right">(<a href="#top">back to top</a>)</p>


###  2.1. <a name='BuiltWith'></a>Built With

This project was built with the following frameworks, technologies and software.

- [https://github.com/dylanaraps/neofetch](https://github.com/dylanaraps/neofetch)
- [BASH](https://www.gnu.org/software/bash/)

<p align="right">(<a href="#top">back to top</a>)</p>


###  2.2. <a name='Installation'></a>Installation

These are the steps to get up and running.

1. Clone the repo in home directory.
    ```sh
    git clone https://github.com/IORoot/motd ~/motd
    ```
2. Link.
    ```sh
    ln -s ~/motd/motd /usr/local/bin/motd 
    ```
3. Add to your .bashrc / .zshrc file so it displays when you login.
    ```sh
    echo "motd" >> ~/.bashrc
    ```

<p align="right">(<a href="#top">back to top</a>)</p>


##  3. <a name='Usage'></a>Usage

You can run the script whenever you like by using `motd` on the command line.

##  4. <a name='Customising'></a> Customising

###  4.1. <a name='AddingPlugins'></a>Adding Plugins

####  4.1.1. <a name='Filename'></a>Filename

If you wish to add a plugin, create a new file in the `plugins` folder. The name of the file *MUST* be prefixed with `get_` and the name of the function.

So, if you were creating a `foobar` funciton, the name of the plugin file would be `get_foobar` (with NO `.sh` on the end)

####  4.1.2. <a name='Yourplugin'></a>Your plugin

Your plugin *MUST* have a function must be the same as the filename. `get_foobar() { }` for instance.
Your plugin *MUST* return a variable called by the name of your function (e.g. $FOOBAR). You can add additional variables, but these will be used in the output later.

```bash
get_foobar() { 
    FOOBAR="This is FOOBARed!" 
}
```

####  4.1.3. <a name='motd'></a>motd

Next, within the `motd` file, there is a `plugins()` function with a `PLUGINS` variable. Add your new plugin here at the bottom of the list. (Without the `get_` prefix)

e.g.

```bash
    PLUGINS=(

        # Essentially Ordered Plugins.
        "kernel"        # KERNAL MUST BE FIRST PLUGIN. (OS Relies on it)
        "os"            # OS MUST BE THE SECOND PLUGIN. (Others rely on it)

        # All other Plugins
        "hostname"
        "local_ip"
        "public_ip"
        "uptime"
        "cpu_usage"
        "memory"
        "disk"
        "model"
        "systemctl:mysql"   
        "systemctl:php"   
        "systemctl:apache"   
        "systemctl:ssh"   
        "puppet"
        "logo"
        "foobar"        # <------- Your New Plugin.
    )
```

####  4.1.4. <a name='Output'></a>Output

Lastly, Add your new plugin to one of the output sections at the bottom (or create your own)

They ustilise the `row` function to display the output in columns.

So to display in the system section, you can add your function like so:

```bash
system()
{
    # SYSTEM
    printf "${LOGO}\n"
    title "${HOSTNAME}"

    row "HOST"      "${HOSTNAME}"   "APACHE" "${APACHE}"    "UPTIME" "${CPU_USAGE}"
    row "OS"        "${OS}"         "PHP"    "${PHP}"       "MEMORY" "${MEMORY}"
    row "MODEL"     "${MODEL}"      "MySQL"  "${MYSQL}"     "DISK" "${DISK}"
    row "LOCAL IP"  "${LOCAL_IP}"   "SSH"    "${SSH}"       "" ""
    row "PUBLIC IP" "${PUBLIC_IP}" 
    row "FOOBAR"    "${FOOBAR}"

}
```

####  4.1.5. <a name='CustomBranding'></a>Custom Branding

Change the logo in the `config.conf` file.

##  5. <a name='Troubleshooting'></a>Troubleshooting

None

<p align="right">(<a href="#top">back to top</a>)</p>


##  6. <a name='Contributing'></a>Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue.
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#top">back to top</a>)</p>



##  7. <a name='License'></a>License

Distributed under the MIT License.

MIT License

Copyright (c) 2022 Andy Pearson

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

<p align="right">(<a href="#top">back to top</a>)</p>



##  8. <a name='Contact'></a>Contact

Author Link: [https://github.com/IORoot](https://github.com/IORoot)

<p align="right">(<a href="#top">back to top</a>)</p>

##  9. <a name='Changelog'></a>Changelog

v1.1.0 - Changed to using simple double columns. Basically the same as watch Neofetch originally did. Doh!

v1.0.0 - First version.
