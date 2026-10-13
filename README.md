# CIDR Route Summarization

> [!IMPORTANT]
> **Migrate to [route-summarization-rs](https://github.com/nabbi/route-summarization-rs).**
> `aggregate-cidr` is a Rust rewrite of this script with the same options, no
> Perl or module dependencies, and this version's quirks fixed. It also adds
> `--exclude` for removing addresses from the output. New deployments should
> use it, and existing ones can switch with a symlink:
> [Migrating from the Perl version](https://github.com/nabbi/route-summarization-rs/blob/master/docs/perl-differences.md#switching-over).
> This script is kept for existing installs and as the fallback in the
> projects below.

A lightweight CIDR aggregation utility built in Perl using the [`Net::CIDR::Lite`](https://metacpan.org/dist/Net-CIDR-Lite/view/Lite.pm) module. This tool summarizes IPv4 and IPv6 prefixes from standard input and optionally formats output for SPF record usage.

Inspired by the original concept described by Adrian Popa in [Random Thoughts](http://adrianpopagh.blogspot.com/2008/03/route-summarization-script.html); see [LICENSE](LICENSE) for how that attribution is handled.

---

## Features

* Summarizes IPv4 and IPv6 CIDR prefixes
* Accepts plain IPs (auto-converted to `/32` or IPv6 host equivalents)
* Supports SPF-style `ip4:` / `ip6:` prefix stripping and output
* Gracefully ignores invalid prefixes
* Simple STDIN-driven workflow for pipelines and scripts

---

## Usage

Run interactively:

```bash
$ ./aggregateCIDR.pl
# One IP/CIDR per line, e.g. 1.2.3.0/24 -- press CTRL+D when done.
```

Or pipe a file into it:

```bash
$ ./aggregateCIDR.pl < cidr.txt
```

### Options

```text
--help        Display usage information
--quiet       Suppress non-essential messages
--spf         Enable SPF input parsing and SPF-style output
```

Example with SPF mode:

```bash
$ ./aggregateCIDR.pl --spf < spf-list.txt
ip4:203.0.113.0/24
ip6:2001:db8::/48
```

---

## Input Examples

```text
192.0.2.1
192.0.2.0/24
2001:db8:10::/48
ip4:198.51.100.0/25   (with --spf)
```

Invalid or non-CIDR lines are ignored.

---

## Output

The script prints an aggregated list of CIDRs with redundant or overlapping ranges collapsed.
IPv4 host addresses (`/32`) are printed without the mask unless explicitly required.

Example:

```text
192.0.2.0/24
198.51.100.0/24
2001:db8:10::/48
```

---

## Project Integrations

Because this tool consumes input exclusively from STDIN, it embeds cleanly into other automation frameworks such as Bash, Tcl, Python, CI pipelines, Makefile workflows, or any scripting environment.

### Projects using or compatible with this tool

* **Postwhite Fork** — [https://github.com/nabbi/postwhite](https://github.com/nabbi/postwhite)
* **TCL IPSet** — [https://github.com/nabbi/tcl-ipset](https://github.com/nabbi/tcl-ipset)

Both now run `aggregate-cidr` when it is on `PATH` and fall back to this
script, so installing the Rust version is all the migration they need.

---

## Requirements

* Perl 5
* `Net::CIDR::Lite` module
* `Getopt::Long` (core Perl)

Install module via CPAN:

```bash
cpan install Net::CIDR::Lite
```

---

## License

MIT — see [LICENSE](LICENSE). The original blog post that inspired this
script's control flow carries no license of its own; see the LICENSE file
for how that's handled.

---

