# CallPerch support and Linux downloads

Created with love by KR4GOJ.

- [Linux source (MIT)](https://github.com/donrad64/callperch-linux)
- [Linux downloads](https://github.com/donrad64/callperch-support/releases/latest)
- [Support and getting started](https://donrad64.github.io/callperch/)
- [Privacy policy](https://donrad64.github.io/callperch/privacy.html)
- [Report a bug](https://github.com/donrad64/callperch-support/issues/new/choose)

Linux 1.2.3 adds a visible FCC sync/import progress window and collapsible assignment-history timelines with smaller orange release-estimate notes. Repeated callsign switching is a limited summary of public FCC records linked by individual FRN. It does not establish intent, improper conduct, a rule violation, former-holder eligibility, or entitlement to a callsign. Incomplete history and changed FRNs can affect results. Verify records and eligibility directly with the FCC.

An upgrade uses your existing local database, preferences and watchlist. Older snapshots need Sync FCC to add identity fields for assignment-history summaries. Close other CallPerch instances before upgrading or syncing.

## Raspberry Pi: sync reports only about 1.9 GiB available

On some Raspberry Pi systems running Debian 13 (Trixie), **Sync FCC** may report less than 12 GiB available even when the SD card or NVMe drive has plenty of free space. Debian Trixie defaults `/tmp` to a memory-backed `tmpfs`, normally capped at half of RAM. A 4 GB Pi can therefore have a roughly 2 GB `/tmp`. See the [Debian release notes](https://www.debian.org/releases/trixie/release-notes/issues.html#the-temporary-files-directory-tmp-is-now-stored-in-a-tmpfs).

CallPerch 1.2.3 checks free space on both the database filesystem and the temporary-download filesystem. **12 GiB means available space for syncing, not the final database size.** When `XDG_DATA_HOME` is unset, the database defaults to `~/.local/share/callperch`; an empty `echo $XDG_DATA_HOME` result is normal.

To check the filesystems:

```sh
df -h "$HOME" /tmp
findmnt /tmp
```

If `/tmp` is a small `tmpfs` and your home filesystem has at least 12 GiB available, close all CallPerch instances and launch the installed app with a temporary folder on your home filesystem:

```sh
mkdir -p "$HOME/.cache/callperch-tmp"
TMPDIR="$HOME/.cache/callperch-tmp" callperch
```

For a source checkout, use the same folder and replace the launch command with:

```sh
TMPDIR="$HOME/.cache/callperch-tmp" .venv/bin/python linux/callperch.py
```

The override applies to that launch only, so start from this command again for future syncs. Your existing database, settings and watchlist are reused. It does not change the system-wide `/tmp` mount or require more RAM. A home directory backed by another small filesystem will still need sufficient free space.

On a Raspberry Pi with a **64-bit** operating system, choose the **arm64** installer. Ubuntu 24.04 is the package build baseline; this note addresses the observed Trixie temporary-storage issue, rather than guaranteeing compatibility with every Raspberry Pi OS image.

Contact: kr4goj@arrl.net or hamradio.balancing125@simplelogin.com.

Issues are public. Do not upload FCC databases, personal addresses, private logs, credentials, or signing material. Email privacy/security reports instead. Use synthetic examples when reporting display problems.

This public repository contains support documents and binary releases. Linux source is published under MIT in [callperch-linux](https://github.com/donrad64/callperch-linux). GitHub’s automatically generated source archives here contain only this support repository; obtain application source from callperch-linux.
