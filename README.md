# dns-bash-scripts

Three small bash scripts for auditing BIND DNS zones from a `named.conf` file: two that resolve every zone and flag the ones with problems, and one that cross-references PTR-style entries against `named.conf`.

## Scripts

### `zone_audit_dig.sh`

```bash
./zone_audit_dig.sh <server-ip> <named.conf> <results.txt>
```

Extracts every zone name from `named.conf` (skips `master` lines), runs `dig @<server-ip> <zone>` for each one, and tees the output to `<results.txt>`. Then scans those results for `SERVFAIL` and `NXDOMAIN` responses and writes the offending zone names to `<results.txt>.prob_hosts`. Backs up an existing results file to `.old` before overwriting.

### `zone_audit_nslookup.sh`

```bash
./zone_audit_nslookup.sh <server-ip> <named.conf> <results.txt>
```

Same idea as `zone_audit_dig.sh`, using `nslookup <zone> <server-ip>` instead of `dig`. Flags `No answer` and `NXDOMAIN` results into `<results.txt>.prob_hosts`.

### `ptr.sh`

```bash
./ptr.sh <file-of-quoted-entries>
```

Pulls the second single-quoted field out of each line of the input file, strips anything after `/`, dedupes and sorts the result, then greps `/opt/named/external/etc/named.conf` for each one — a quick way to check whether a PTR target is actually declared in that zone file. The `named.conf` path is hardcoded.

## Origin

Written in 2013 for auditing BIND zone files by hand. Still functional for that narrow case; not maintained as a general-purpose tool.

## License

MIT — see [LICENSE](LICENSE).
