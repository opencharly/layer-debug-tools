# debug-tools

In-container debug toolkit for OpenCharly images — network, process, file, and
system-stats probes.

The `debug-tools` candy installs a distro-agnostic debugging toolkit so an
operator can inspect a deployed service from inside the container without
re-deploying. Every headline binary lands at a fixed path, so its presence is
directly verifiable.

- **Network** — `ip`/`ss`, `lsof`, `ping`, `dig`, `nc`, `socat`, `tcpdump`,
  `traceroute`, `mtr`, `wget`
- **Process** — `ps`/`top`, `htop`, `pgrep`, `free`, `vmstat`, `strace`, `ltrace`
- **File / text** — `file`, `tree`, `xxd`, `vim`, `nano`
- **System stats** — `iotop`, `iftop`, `sysstat`
- **Session helpers** — `tmux`, `rsync`, `yq`

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `debug-tools` |
| Packages | per-distro `distro:` arms (`fedora` / `arch` / `debian` / `ubuntu`) |
| Binaries | `/usr/bin/dig`, `/usr/sbin/ss`, `/usr/sbin/tcpdump`, `/usr/bin/ps`, `/usr/bin/htop`, `/usr/bin/strace`, `/usr/bin/vim`, `/usr/bin/tmux`, `/usr/bin/lsof`, … |
| Service / port | none |

Tool names diverge across distros (`nmap-ncat` vs `ncat` vs `gnu-netcat`), so the
package lists are authored per distro rather than as one synthetic "common" list
that would silently install nothing on the wrong distro. Debian/Ubuntu omit `yq`
(it ships via snap there); Fedora and Arch include it.

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-debug-tools:v2026.239.1629'
```

Then, inside the running container:

```bash
ss -ltnp
dig example.com
tcpdump -i any port 8080
```

The candy's `plan:` ships deterministic `check:` steps over the headline binaries
(`dig`, `ss`, `tcpdump`, `ps`, `htop`, `strace`, `vim`, `tmux`, `lsof`, `ping`,
`nc`, `nano`, `tree`, `wget`, `file`, `rsync`) and their packages, so a missing
tool fails the checks.

## Layout

- `charly.yml` — the `debug-tools:` candy entity (the per-distro `package:`
  arms and the `check:` assertions). It declares **no `skill:` entity**.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-versa:debug-tools-layer` — the debug-tools layer
  reference (the candy declares no `skill:` entity of its own; the gap is tracked
  in [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291))
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
