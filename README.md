# ESP 32 - Hello World

## Getting Started

### Preparing the IDE

Talking to @bb I have been pointed to [Platform IO](https://platformio.org/) as a potential IDE for micro controller development, which is based on VS Code.

To not mix up my existing VS Code instance (with settings and set of extensions) I created an `alias` to install the Platform IO extension seperately:

```bash
alias pio='code --extensions-dir ~/.vscode-pio/extensions --user-data-dir ~/.vscode-pio'
```

Now I was able to start VS Code with an isolated environment and [installed Platform IO extension](https://platformio.org/install/ide?install=vscode).
