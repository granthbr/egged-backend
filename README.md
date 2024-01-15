[![Unix build](https://img.shields.io/github/actions/workflow/status/Kong/kong-plugin/test.yml?branch=master&label=Test&logo=linux)](https://github.com/Kong/kong-plugin/actions/workflows/test.yml)
[![Luacheck](https://github.com/Kong/kong-plugin/workflows/Lint/badge.svg)](https://github.com/Kong/kong-plugin/actions/workflows/lint.yml)

Kong plugin template for Egged
====================

This repository contains a very simple Kong plugin template to get you
up and running quickly for developing your own plugins.
Follow the instructions below to get started.


This template was designed to work with the
[`kong-pongo`](https://github.com/Kong/kong-pongo) testing framework,

Please check out those repos `README` files for usage instructions. For a complete
walkthrough check [this blogpost on the Kong website](https://konghq.com/blog/custom-lua-plugin-kong-gateway).

[Naming and Versioning Details ](NamingVersioning.md)

## Getting started
First, after cloning this repo, you need to install the dependencies:

```shell
git clone https://github.com/granthbr/egged-backend.git
cd egged-backend
```
once in the directory, change these directory names to match your plugin name
```shell
mv kong-plugin-egged-backend <kong-plugin-your-plugin-name>
mv kong-plugin-egged-backend-0.1.0-1.rockspec <kong-plugin-your-plugin-name-0.1.0-1.rockspec>
mv kong/plugins/egged-backend <kong/plugins/your-plugin-name>
```
then, change the name of the plugin in the rockspec file
```shell
sed -i 's/myplugin/backend-egged/g' <kong-plugin-your-plugin-name-0.1.0-1.rockspec>
```
Then change the package name in the rockspec file
```shell
sed -i 's/kong-plugin-/backend-egged-/g' <kong-plugin-your-plugin-name-0.1.0-1.rockspec>
```
Optional: the github account, repo name, package name if you want to host the plugin on luarocks

Next, change the name of the plugin in the schema.lua file
```shell
sed -i 's/myplugin/backend-egged/g' <kong/plugins/your-plugin-name/schema.lua>
```
Next, the version on linke 17 in the handler must match the version in the rockspec file name
```shell
  VERSION = "0.1.0-1", -- this must match the version in the rockspec file name
```
rockspec file name
 <kong-plugin-your-plugin-name-0.1.0-1.rockspec> -- the version is 0.1.0-1 and this version must match the version in the handler file. 

Next, inside the spec folder, change the name of the plugin in the spec_helper.lua file
```shell
mv spec/myplugin <spec/your-plugin-name>
sed -i 's/myplugin/backend-egged/g' <spec/your-plugin-name/spec_helper.lua>
```
Change the directory name in the spec file
```shell
mv spec/myplugin <spec/your-plugin-name>
```
Next, change the name of the plugin in the spec file inside the spec folder
```shell
sed -i /myplugin/backend-egged/g' <spec/your-plugin-name/01-unit_spec.lua>
```

From here there are two options:
1. run the Luarocks make command to install the plugin locally
2. run the Luarocks pack command to create a rock file to install the plugin locally
3. run the Luarocks upload command to upload the plugin to luarocks
4. run the Luarocks pack command to create a rock file to upload the plugin to Kong Konnect. 

Or option two:
1. Make sure that Pongo is installed on the computer you are working on. 
2. From the directory you have the plugin in, run the command:
```shell
pongo run
```
This will run the tests in the spec folder. If they are no aligned with the plugin handler and schema, they will fail. And the package will not be able to be installed.
If they do pass, then you can install the plugin locally with the command:
```shell
pongo install
```
This will install the plugin locally.
If you want to upload the plugin to KOng Konnect, you can run the command:
```shell  
pongo pack
```
This will create a rock file that you can upload to Kong Konnect.

At this point, you can install the plugin in Kong Konnect and test it out.
There are two ways to install the plugin in Kong Konnect.
First, through the UI. This is a good place to start so that you can see the plugin in the UI. Once there, you can enable the plugin on a service or route. Then run a test on it. 
In the UI go to your Gateway manager, choose a control plane, go to plugins,and choose the custom plugin option. From the you can upload your custom code. 
You will need to load the handler.lua file. and the schema.lua file.
After that, the plugin can be made available to the data plane on the next screen. 

Once comfortable with the process, you can use the Kong Konnect API to install the plugin. 

```shell
https://docs.konghq.com/konnect/gateway-manager/plugins/add-custom-plugin/
```