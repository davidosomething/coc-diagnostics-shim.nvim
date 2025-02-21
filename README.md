# coc-diagnostics-shim.nvim

**PLUGIN IS ARCHIVED** in favor of `diagnostic.displayByVimDiagnostic` [https://github.com/neoclide/coc.nvim/pull/5236]

This neovim plugin provides
[ale#other_source#ShowResults](https://github.com/neoclide/coc.nvim/blob/master/src/diagnostic/buffer.ts#L40)
so [coc.nvim](https://github.com/neoclide/coc.nvim)
with [diagnostic.displayByAle](https://github.com/neoclide/coc.nvim/blob/master/doc/coc-config.txt#L153-L158)
can pipe diagnostics directly
to [vim.diagnostic](<https://neovim.io/doc/user/diagnostic.html#vim.diagnostic.set()>)
without installing the entirety of [dense-analysis/ale](https://github.com/dense-analysis/ale)

This plugin conflicts with [dense-analysis/ale](https://github.com/dense-analysis/ale)
so make sure you don't have both!

## Installation

**Lazy.nvim** add:

```lua
{
    "davidosomething/coc-diagnostics-shim.nvim"
}
```

**coc-settings.json** add:

```lua
{
  ...
  "diagnostic.displayByAle": true,
  ...
}
```

## Tips

You can configure the plugin with formatters, i.e. with

```lua
  {
    "neoclide/coc.nvim",
    branch = "release",
    dependencies = {
      "davidosomething/format-ts-errors.nvim",
      "davidosomething/coc-diagnostics-shim.nvim",
    },
    init = function()
      vim.g.coc_start_at_startup = true
      vim.g.coc_global_extensions = {
        "coc-json",
        "coc-tsserver",
        -- "coc-pretty-ts-errors" -- using format-ts-errors instead, remove and uninstall this!
      }

      require("coc-diagnostics-shim").setup({
        formatters = {
          coctsserver = {
            ---@diagnostic disable-next-line: unused-local
            function(linter_name, item, formatted)
              local prettifier = require("format-ts-errors")[item.code]
              if not prettifier then
                vim.print("no prettifier for " .. item.code)
                return formatted
              end
              local prettified = prettifier(item.text)
              return ("%s%s"):format(
                prettified,
                "ꜰᴏʀᴍᴀᴛᴛᴇᴅ ᴡɪᴛʜ ꜰᴏʀᴍᴀᴛ-ᴛs-ᴇʀʀᴏʀs.ɴᴠɪᴍ"
              )
            end,
          },
        },
      })
    end,
  },
```

## License

MIT
