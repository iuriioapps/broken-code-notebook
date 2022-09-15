# Tools

### Azure CLI

* **Stable** - Text commands don't change and the CLI is in the stable state
* **Structure** - CLI commands are structured very logically and all follow the same pattern
* **Cross-Platform** - CLI works on Windows, Mac, Linux
* **Automation** - It is simple to automate the CLI commands for future use
* **Logging** - Keep track of who run what command and when in various ways

### PowerShell

* **Cmdlet** - A script that performs a specific task. "New-AzVm" is a task that creates new virtual machine
* **Azure Resource Manager** - PowerShell also uses the Resource Manager, like the Portal and CLI, to manipulate Azure resources
* **Versatile** - You can use PowerShell for many other tasks and areas, not just for Azure.&#x20;

### CloudShell

* **Access** - Access from anywhere using a web browser or mobile app. Authenticated and secure.
* **Shell** - Choose between Bash (Azure CLI) or PowerShell
* **Tools** - Included tools are: interpreters, modules, Azure tools. Language support for NodeJS, .NET, and Python
* **Storage** - Dedicated storage to persist data between sessions
* **File Editor** - A complete file editor is available.

### &#x20;ARM (Azure Resource Manager) Templates

* **Describe resource usage** - what are you creating, updating, deleting?
* **Common Syntax** - Defined language for all ARM templates, making it easier to formalize and learn
* **Idempotent** - Every ARM template can be applied multiple times, and the result is always the same
* **Source Control** - Can be checked-in into your source control system. Keep track of all changes to the ARM templates
* **Reuse** - Use a combination of multiple partial ARM templates to achieve the end result.
* **Declarative** - Specify what you want, not how it's done
* **No Human Errors** - Automation means humans don't repeat the same mistakes

