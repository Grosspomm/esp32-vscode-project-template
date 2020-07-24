# Visual Code Studio Template for ESP32

## Getting Started

Thank you ! you just have cloned the `esp32-vscode-project-template` project.
```bash
git clone https://github.com/fmuller-pns/esp32-vscode-project-template.git
```

#### 1. Rename the `vscode_project_template` folder
```bash
mv esp32-vscode-project-template <my_project_name>
```
#### 2. Go to the project directory
```bash
cd <my_project_name>
```
#### 3. Remove the GIT directory
```bash
rm -fR .git
```
#### 4. Open visual studio code for the new project
```bash
code .
```
#### 5. Verify paths in the `c_cpp_properties.json` file and change them if wrong.
```json
"IDF_TOOLS": "~/.espressif/tools",
"IDF_PATH": "~/esp/esp-idf"
```
#### 6. [Not required] Change the default project name called `main` in files.
This step renames the executable file. By default, the executable file is `main.elf`.
1. Open `CMakeLists.txt` and replace `main` by <my_project_name>
2. Open `Makefile` and replace `main` by <my_project_name>
3. Open `.vscode/launch.json` and replace `main` by <my_project_name> (lines 11 and 19)

#### 7. Open an terminal from vscode to perform commands
Choose an externant or internal terminal.

##### Open integrated terminal from vscode

  * using keyboard shortcut: `Ctrl+Shift+`<sup>2</sup>
  * or pressing `F1` key and typing `integrated`

##### Open external terminal from vscode

  * using keyboard shortcut: `Ctrl+Shift+C`
  * or pressing `F1` key and typing `external`

#### 8. Identify the USB serial port (usually `/dev/ttyUSB0`)
```bash
ls /dev/ttyUSB*
```
<span style="color:yellow">/dev/ttyUSB0</span>

#### 9. Building, flashing and running project
The serial port is `/dev/ttyUSB0` identified above.

```bash
idf.py -p /dev/ttyUSB0 flash monitor
```

##### Push the button on the ESP32 board when connecting

```bash
Serial port /dev/ttyUSB0
Connecting........_____....._
Detecting chip type... ESP32
Chip is ESP32-PICO-D4 (revision 1)
```

##### Flashing and monitoring
The message "`Hello ESP32 !`" appears.
```bash
...
W (290) spi_flash: Detected size(4096k) larger than the size in the binary image header(2048k). Using the size in the binary image header.
I (300) cpu_start: Starting scheduler on PRO CPU.
I (0) cpu_start: Starting scheduler on APP CPU.
Hello ESP32 !
```

To exit monitoring, typing `Ctrl+AltGr+]`

## Useful Commands 

#### Open external terminal from vscode to perform commands

  * using keyboard shortcut: `Ctrl+Shift+C`
  * or pressing `F1` key and typing `external`

#### Open integrated terminal from vscode to perform commands

  * using keyboard shortcut: `Ctrl+Shift+`<sup>2</sup>
  * or pressing `F1` key and typing `integrated`

#### Clean project
```bash
idf.py fullclean
```

#### Configuration of the ESP32 board (only in external terminal)
```bash
idf.py menuconfig
```

#### Compile and build the executable file (`.elf` extension)
```bash
idf.py build
```

#### Identify the USB serial port (usually `/dev/ttyUSB0`)
```bash
ls /dev/ttyUSB*
```

#### Compile, build, flash
```bash
idf.py -p /dev/ttyUSB0 flash
```

#### Compile, build, flash and monitor
```bash
idf.py -p /dev/ttyUSB0 flash monitor
```
To exit monitoring, typing `Ctrl+AltGr+]`

## Configure GIT for your new project

#### Go to your new project folder
```bash
cd <project_name>
```
#### Configure name and email address
```bash
git config --global user.name "your name" 
git config --global user.email "your email address"
```
#### Avoid typing your username and password in vscode each time
This is useful when connecting your GIT to GitHub.
```bash
git config credential.helper store
```

## Using GITHUB with visual studio code

We consider you have followed the sections above:
* Getting Started
* Configure GIT for your new project

Now, how to communicate with GitHub ?

1. Open visual code studio.
2. Click on the `Source Control` icon on your left side or use `Ctrl+Shift+G` shortcut. 
3. For the first time, click on `Initialize Repository` button
4. Enter a message for your first commit (ex: first commit) and click on Commit icon
5. Press `F1` and typing `git add remote` and entering :
   * *remote name* : your github repository previously created
   * *remote url* : https://github.com/xxx/your_project.git
   * *username* and *password*
6. Push to the GitHub server (master branch)

See https://code.visualstudio.com/docs/editor/versioncontrol for more details.

## Debugging with JTAG FT2232

You must install FTDI FT2232 driver.

### Step 1: From external terminals

1. Connect the ESP32 board (USB)
2. Open an external terminal for building, flashing and running project
The serial port is `/dev/ttyUSB0` identified above.
```bash
idf.py -p /dev/ttyUSB0 flash monitor
```
3. Connect the JTAG FT2232 (USB)
4. Open another external terminal for running `openocd` with configuration file (`ftdi_ft2232.cfg`) located in the project root.
```bash
openocd -f ftdi_ft2232.cfg
```

5. Result on openocd terminal
```bash
Open On-Chip Debugger  v0.10.0-esp32-20190313 (2019-03-13-09:52)
Licensed under GNU GPL v2
adapter speed: 20000 kHz
Info : Configured 2 cores
esp32 interrupt mask on
Info : Listening on port 6666 for tcl connections
Info : Listening on port 4444 for telnet connections
Info : ftdi: if you experience problems at higher adapter clocks, try the command "ftdi_tdo_sample_edge falling"
Info : clock speed 20000 kHz
Info : JTAG tap: esp32.cpu0 tap/device found: 0x120034e5 (mfg: 0x272 (Tensilica), part: 0x2003, ver: 0x1)
Info : JTAG tap: esp32.cpu1 tap/device found: 0x120034e5 (mfg: 0x272 (Tensilica), part: 0x2003, ver: 0x1)
Info : esp32: Debug controller 0 was reset (pwrstat=0x5F, after clear 0x0F).
Info : esp32: Core 0 was reset (pwrstat=0x5F, after clear 0x0F).
Info : esp32: Debug controller 1 was reset (pwrstat=0x5F, after clear 0x0F).
Info : esp32: Core 1 was reset (pwrstat=0x5F, after clear 0x0F).
Info : Detected debug stubs @ 3ffb2950 on core0 of target 'esp32'
Info : Listening on port 3333 for gdb connections
```

### Step 2: From Visual Studio Code
1. Click on the left on the line you want to set a breakpoint. A red bullet appears.

2. Click on debug Icon

3. Click on RUN `ESP32 OpenOCD`. If an error arises, click again.

4. The programm stops at the breakpoint and you can see variables and more

### Step 3: When you modify the code

Do not touch the terminal with `openocd` command. 

1. Stop the program into the terminal, typing `Ctrl+AltGr+]`
2. Build, flash and run program
The serial port is `/dev/ttyUSB0` identified above.
```bash
idf.py -p /dev/ttyUSB0 flash monitor
```
3. Click on RUN `ESP32 OpenOCD`. If an error arises, click again.
4. The programm stops at the breakpoint and you can see variables and more 

## Generate Doxygen documentation

1. Open external terminal from vscode, using keyboard shortcut: `Ctrl+Shift+C`, or pressing `F1` key and typing `external`
2. Generate HTML documentation

  * From the User interface (allow you updating the `Doxyfile` configuration file)

```bash
doxywizard
```

  * Directly from `Doxyfile` configuration file


```bash
doxygen
```
3. A new `html` folder  is created, the entry file is `index.html`

