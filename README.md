# Basic Odoo dev container

This is a basic sample for an Odoo dev container.

Postgress is installed and configured within the same container.

Odoo itself is not included, you must download it into the subdirs of the `odoo` folder before starting.

## Usage

1. Clone this repository
1. Download Odoo community edition into the `odoo/community` folder
1. (optional) Download Odoo enterprise edition into the `odoo/enterprise` folder
1. (optional) Download Odoo themes into the `odoo/themes` folder
1. Keep your modules in the root directory of the repository
1. In case of submodules, add the submodule path to the `addons_path` in the `odoo.conf` file
1. Start the dev container with vscode
1. Open the terminal in vscode
1. (optional) Run `odoo-bin -d some_db -i base,web --stop-after-init` to create a new db
1. Run `odoo-bin` as usual to start the Odoo server
