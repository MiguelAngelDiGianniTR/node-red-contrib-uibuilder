# Developer documentation for `uibuilder.html`

Documents the processing of the html file that defines the Node-RED admin UI panel for uibuilder. Shown when double-clicking on a uibuilder node in a flow.

## Package List

The package list is shown when the "Manage Front-End Libraries" button is clicked.

_NOTE: Managing packages does **not** require a Node-RED deploy or restart._

The list is a [Node-RED editable list widget](https://nodered.org/docs/api/ui/editableList/).

Setup of the list is done in `RED.nodes.registerType('uibuilder', { ... oneditprepare() ... }`

The list of packages is provided by the `packageList()` function.

### packageList Function

Calls the `uibvendorpackages` API which is defined in `uibuilder.js`.

For each package in the resulting object, the editable list `addItem` method is called with the package name. The addItem method calls the `addPackageRow` function.

This adds the package name to the list without further processing.

### Add button

When the add button (just under the package list) is clicked, the `addPackageRow()` function is called.

This creates a new row in the list with a text input box and a button marked "Install".

If text is typed into the input box and then the Install button pressed, the `addPackageRow` function is called.

This results in the `installPackage` API being called to attempt to install the package using an exec call to npm. If that succeeds, the package name is added to the list.

### addPackageRow(element,index,data) Function

If the length of `data` is zero, we know that the `add` button was pressed. This adds a row containing an input text box and an "Install" button.

If `data` is non-zero, the assumption is that we are adding known entries (probably via the `packageList()` function). The string in `data` is simply added to the list widget as a new row.

If the "Install" button is pressed, the uibuilder `installPackage` API is called with the package name.