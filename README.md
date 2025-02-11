# rust-tools.nvim
Extra rust tools for writing applications in neovim using the native lsp.
This plugin adds extra functionality over rust analyzer. The features mirror VsCode.

## Prerequisites

- `neovim 0.5+` (nightly)
- `nvim-lspconfig`
- `rust-analyzer`

## Installation

using `vim-plug`

```vim
Plug 'neovim/nvim-lspconfig'
Plug 'simrat39/rust-tools.nvim'

" Optional dependencies
Plug 'nvim-lua/popup.nvim'
Plug 'nvim-lua/plenary.nvim'
Plug 'nvim-telescope/telescope.nvim'
```
<b>Look at the configuration information below to get started.</b>

# Configuration + Functionality

Please configure [`nvim-lspconfig for rust`](https://github.com/neovim/nvim-lspconfig/blob/master/CONFIG.md#rust_analyzer) before using this plugin.

This plugin is more of WYSIWYG right now but more configuration options will slowly be added.

### Initial setup
```lua
local opts = {
    -- automatically set inlay hints (type hints)
    -- There is an issue due to which the hints are not applied on the first
    -- opened file. For now, write to the file to trigger a reapplication of
    -- the hints or just run :RustSetInlayHints.
    -- default: true
    autoSetHints = true,
    -- All opts that go into runnables (scroll down a bit) can also go here,
    -- these apply to the default RustRunnables command
    runnables = {
        -- whether to use telescope for selection menu or not
        -- default: true
        use_telescope = true
        -- rest of the opts are forwarded to telescope
    },
    -- All opts that go into inlay hints (scroll down a bit) can also go here,
    -- these apply to the default RustSetInlayHints command
    inlay_hints = {
        -- wheter to show parameter hints with the inlay hints or not
        -- default: true
        show_parameter_hints = true,
    },
}

require('rust-tools').setup(opts)
```

## Commands
```vim
RustSetInlayHints
RustRunnables
RustExpandMacro
RustOpenCargo 
RustParentModule
RustJoinLines
RustHoverActions
RustMoveItemDown
RustMoveItemUp

" Deprecated
RustRunnablesTelescope
```

#### Inlay Hints
![inlay hints](./images/inlay_hints.png)
```lua
-- Command:
-- RustSetInlayHints
local opts = {
    -- whether to show parameter hints with the inlay hints or not
    -- default: true
    show_parameter_hints = true,
}

require('rust-tools.inlay_hints').set_inlay_hints(opts)
```

#### Runnables
![runnables](./images/runnables.gif)
```lua
-- Command:
-- RustRunnables
local opts = {
    -- whether to use telescope for selection menu or not
    -- default: true
    use_telescope = true
    -- rest of the opts are forwarded to telescope
}
require('rust-tools.runnables').runnables(opts)

-- DEPRECATED !!!
-- The command above automatically detects if telescope is installed and uses that by default
-- Needs telescope.nvim
-- The theme part is optional
--
-- Command:
-- RustRunnablesTelescope
require('rust-tools.runnables').runnables_telescope(require('telescope.themes').get_dropdown({}))
```
#### Expand Macros Recursively 
![expand macros](./images/expand_macros_recursively.gif)
```lua
-- Command:
-- RustExpandMacro  
require'rust-tools.expand_macro'.expand_macro()
```

#### Move Item Up/Down
![move items](./images/move_item.gif)
```lua
-- Command:
-- RustMoveItemUp    
-- RustMoveItemDown    
local up = true -- true = move up, false = move down
require'rust-tools.move_item'.move_item(up)
```

#### Hover Actions
![hover actions](./images/hover_actions.gif)
```lua
-- this needs the experimental hoverActions capability set
-- while configuring your rust-analyzer:
local capabilities = vim.lsp.protocol.make_client_capabilities()

capabilities.experimental = {}
capabilities.experimental.hoverActions = true

nvim_lsp.rust_analyzer.setup({
    capabilities = capabilities,
})
------------------------------------------------------------------
-- Actual call
-- Command:
-- RustHoverActions 
require'rust-tools.hover_actions'.hover_actions()
```

#### Open Cargo.toml
![open cargo](./images/open_cargo_toml.gif)
```lua
-- Command:
-- RustOpenCargo
require'rust-tools.open_cargo_toml'.open_cargo_toml()
```

#### Parent Module
![parent module](./images/parent_module.gif)
```lua
-- Command:
-- RustParentModule 
require'rust-tools.parent_module'.parent_module()
```

#### Join Lines
![join lines](./images/join_lines.gif)
```lua
-- Command:
-- RustJoinLines  
require'rust-tools.join_lines'.join_lines()
```

## Inspiration

This plugin draws inspiration from [`akinsho/flutter-tools.nvim`](https://github.com/akinsho/flutter-tools.nvim)