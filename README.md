# kubectl-blk

`kubectl-blk` is a simple `kubectl` plugin that helps run `kubectl` (or `flux`) commands against multiple single-cluster kubeconfigs.

## Requirements

The plugin relies on `flock` command from `util-linux`. So `darwin` users will need to install https://github.com/discoteq/flock

## Installation

1. Place `kubectl-blk` into the directory within your `PATH` (e.g. `~/.local/bin` or `~/.krew/bin`)
2. Create the `~/.kube/configs` directory and place your kubeconfigs there

## Customization

You can export the following variables to tweak the plugin's behaviour.

| VARIABLE          | DEFAULT           | DETAILS                                          |
|-------------------|-------------------|--------------------------------------------------|
| `KBLK_BIN`        | `kubectl`         | binary to run commands with                      |
| `KBLK_DIR`        | `~/.kube/configs` | directory with your kubeconfigs                  |
| `KBLK_REGEX_TYPE` | `extended`        | `grep` regex type: `basic`, `extended` or `perl` |
| `KBLK_REGEX`      | `.*`              | default regex for contexts                       |
| `KBLK_PROC`       | `0`               | `xargs` processes                                |
| `KBLK_FORCE`      | `0`               | do not ask for confirmation                      |
| `KBLK_SEP_COLOR`  | `4`               | `tput` separator color                           |
| `KBLK_SEP_PAD`    | `==========`      | separator padding string                         |
| `KBLK_SEP_GAP`    | `\n`              | this is printed before a separator               |

## Usage

```
kubectl blk helps run kubectl or flux commands against multiple single-context kubeconfigs

Usage:
  kubectl blk [-r '<regex>'] [-p <int>] [-b <binary>] [-f] -- <command>

Flags:
  -r <regex>    perl regex to match the contexts (default: .*)
  -p <int>      number or xargs processes (default: 0)
  -b <binary>   binary to run commands with (default: kubectl)
  -f            do not ask for confirmation
  -v            show plugin version and exit
  -h            show this message and exit
```

## Examples

```sh
❯ k blk -r 'prod.*eu' -- version
⦿ matched contexts:
prod-cluster01-eu
prod-cluster02-eu
prod-cluster03-eu
⦿ type 'y' to run 'kubectl version' command: y

==========[ prod-cluster01-eu ]==========
Client Version: v1.30.2
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
Server Version: v1.25.14+rke2r1
WARNING: version difference between client (1.30) and server (1.25) exceeds the supported minor version skew of +/-1

==========[ prod-cluster02-eu ]==========
Client Version: v1.30.2
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
Server Version: v1.25.14+rke2r1
WARNING: version difference between client (1.30) and server (1.25) exceeds the supported minor version skew of +/-1

==========[ prod-cluster03-eu ]==========
Client Version: v1.30.2
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
Server Version: v1.25.14+rke2r1
WARNING: version difference between client (1.30) and server (1.25) exceeds the supported minor version skew of +/-1

❯ k blk -r 'prod.*eu' -b flux -f -- get hr
==========[ prod-cluster01-eu ]==========
NAME               REVISION        SUSPENDED       READY   MESSAGE
my-operator        0.9.1           False           True    Release reconciliation succeeded

==========[ prod-cluster02-eu ]==========
NAME               REVISION        SUSPENDED       READY   MESSAGE
my-operator        0.9.1           False           True    Release reconciliation succeeded

==========[ prod-cluster03-eu ]==========
NAME               REVISION        SUSPENDED       READY   MESSAGE
my-operator        0.9.1           False           True    Release reconciliation succeeded
```
