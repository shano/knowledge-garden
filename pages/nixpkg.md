- Quite a handy start to using nixpg is to just to come across a package you can't install through your traditional package manager and you can make a slow start. Like for me with navi
- ```
  sh <(curl -L https://nixos.org/nix/install)
  nix-channel --add https://nixos.org/channels/nixpkgs-unstable
  nix-channel --update
  nix-env -iA nixpkgs.navi
  ```