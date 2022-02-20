# my `neovim` setup using nix flakes

This configuration is was forked, but heavily edited from [Quoteme/neovim-flake](https://github.com/Quoteme/neovim-flake).

Add plugins into the `inputs` section of `flake.nix` and overwrite `init.vim`.

### Run over the internet

`nix run github:latbauerdick/neovim-flake /some/file`

### Run from a folder

Clone the repo and run `nix run /some/file` inside the new folder.

### Install system-wide

Include in the following:

```
inputs = {
    # ...blabla...
    neovim-flake.url = "github:LATBauerdick/neovim-flake";
}

outputs = {self, nixpkgs, ...}@attr: {
    nixosConfigurations.nixos = nixpkgs.lib.nixosSystem {
        system = "x86_64-linux";
        specialArgs = attrs;
        modules = [
            ({ config, nixpkgs, ...}@inputs:
                # ...blabla...
                environment.systemPackages = with pkgs; [
                    # ...blabla...
                    inputs.neovim-flake.defaultPackage.x86_64-linux
                ];
            )
        ];
    }
}
```

Note that `...blabla...` is a placeholder for any other configuration
you might have set inside your `flake.nix`!
