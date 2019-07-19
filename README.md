# ESP 32 - Hello World

## Getting Started

### Preparing the IDE

Talking to @bb I have been pointed to [Platform IO](https://platformio.org/) as a potential IDE for micro controller development, which is based on VS Code.

To not mix up my existing VS Code instance (with settings and set of extensions) I created an `alias` to install the Platform IO extension seperately:

```bash
alias pio='code --extensions-dir ~/.vscode-pio/extensions --user-data-dir ~/.vscode-pio'
```

Now I was able to start VS Code with an isolated environment and [installed Platform IO extension](https://platformio.org/install/ide?install=vscode).

### Creating a New Project

After restarting VS Code with proper PIO extension installed I used the *home* screen to create a new project "*esp32-hello-world*". My ESP32 development board was a **ESP32-WROOM-32** from Espressif.

![create project dialog](create-project.png)

### Writing the Application

The first straight forward (google-copy-paste-approach) implementation failed due to missing definition of `LED_BUILTIN`:

```cpp
#include <Arduino.h>

void setup() {
  pinMode(LED_BUILTIN, OUTPUT);
}
 
void loop() {
  digitalWrite(LED_BUILTIN, HIGH);
  delay(500);
  digitalWrite(LED_BUILTIN, LOW);
  delay(500);
}
```

But a quick search brought the fix:

```cpp
#define LED_BUILTIN 2
```

### Compiling

Pressing `F5` in VS Code successfully compiled the sources. Now it was time to plug in the ESP32 via USB connector.

### Uploading to the Flash

Through the PIO tab in the activity bar of VS Code I found the project task *Upload* (also available with `Ctrl`+`Alt`+`T`). Running it resulted in an error:

    ...
    could not open port /dev/ttyACM0 [Errno 2] No such file or directory

Quick research on the internet again brought this fix:

```bash
$ dmesg | grep tty
[ 2311.806072] usb 1-1.2: cp210x converter now attached to ttyUSB0
```

Which has to be set into `./platformio.ini` in the `esp32-hello-world` project root:

```diff
[env:esp32dev]
platform = espressif32
board = esp32dev
framework = arduino
+ upload_port = /dev/ttyUSB0
```

Another try via VS Code task run failed again:

```
can't open device "/dev/ttyUSB0": Permission denied
```

This time my user wasn't in the `dialout` group. The fix looks like that:

```bash
$ sudo gpasswd --add ${USER} dialout
```

After a re-login, it finally worked and the LED was blinking. :tada:
