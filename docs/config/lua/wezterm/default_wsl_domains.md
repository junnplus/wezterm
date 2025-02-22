# wezterm.default_wsl_domains()

*Since: nightly builds only*

Computes a list of [WslDomain](../WslDomain.md) objects, each one
representing an installed
[WSL](https://docs.microsoft.com/en-us/windows/wsl/about) distribution
on your system.

This list is the same as the default value for the
[wsl_domains](../config/wsl_domains.md) configuration option, which is to make
a `WslDomain` with the `distribution` field set to the name of WSL distro and the
`name` field set to name of the distro but with `"WSL:"` prefixed to it.

For example, if:

```
; wsl -l -v
  NAME            STATE           VERSION
* Ubuntu-18.04    Running         1
```

then this function will return:

```lua
{
  { name: "WSL:Ubuntu-18.04",
    distribution: "Ubuntu-18.04",
  },
}
```

The purpose of this function is to aid in situations where you might want set
`default_prog` for the WSL distributions:

```lua
local wezterm = require 'wezterm'

local wsl_domains = wezterm.default_wsl_domains()

for idx, dom in ipairs(wsl_domains) do
  if dom.name == "Ubuntu-18.04" then
    dom.default_prog = {"fish"}
  end
end

return {
  wsl_domains = wsl_domains,
}
```

However, wez strongly recommends that you use `chsh` inside the WSL domain to make
that the default shell if possible, so that you can avoid this additional configuration!

