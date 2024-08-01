# kubectl-blk

`kubectl-blk` is a simple `kubectl` plugin that helps run kubectl commands against multiple single-cluster kubeconfigs.

## Requirements

* `flock` - https://man7.org/linux/man-pages/man1/flock.1.html

## Installation

1. Place `kubectl-blk` into the directory within your `PATH` (e.g. `~/.local/bin` or `~/.krew/bin`)
2. Create the `~/.kube/configs` directory and place your kubeconfigs there

## Customization

You can export the following variables to tweak the plugin's behaviour.

| VARIABLE         | DEFAULT           | DETAILS                            |
|------------------|-------------------|------------------------------------|
| `KBLK_DIR`       | `~/.kube/configs` | directory with your kubeconfigs    |
| `KBLK_REGEX`     | `.*`              | default regex for contexts         |
| `KBLK_PROC`      | `0`               | `xargs` processes                  |
| `KBLK_FORCE`     | `false`           | do not ask for confirmation        |
| `KBLK_SEP_COLOR` | `4`               | `tput` separator color             |
| `KBLK_SEP_PAD`   | `==========`      | separator padding string           |
| `KBLK_SEP_GAP`   | `\n`              | this is printed before a separator |

## Usage

```
kubectl blk helps run kubectl commands against multiple single-context kubeconfigs

Usage:
  kubectl blk [-r '<regex>'] [-p <int>] [-f] [-h] -- <command>

Flags:
  -r <regex>    perl regex to match the contexts (default: .*)
  -p <int>      number or xargs processes (default: 0)
  -f            do not ask confirmation
  -h            show this message
```

## Example

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
```

## Why

A man had a bunch of kubeconfigs that all used `default` as the name for the cluster, user and context.
Because of that a man could not just merge those kubeconfigs and use [kubectl-mc](https://github.com/jonnylangefeld/kubectl-mc) plugin.
The proper way would be to fix the kubeconfigs by giving unique names to the clusters, users and contexts.
But a man had to satisfy his needs to shell script.
