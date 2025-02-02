% SPLINTER-CIRCUIT-LIST(1) Cargill, Incorporated | Splinter Commands
<!--
  Copyright 2018-2021 Cargill Incorporated
  Licensed under Creative Commons Attribution 4.0 International License
  https://creativecommons.org/licenses/by/4.0/
-->

NAME
====

**splinter-circuit-list** — Displays the existing circuits for this Splinter node

SYNOPSIS
========
**splinter circuit list** \[**FLAGS**\] \[**OPTIONS**\]

DESCRIPTION
===========
This command lists all or some of the circuits the local node is a member of.
This command displays abbreviated information pertaining to circuits in columns,
with the headers `ID`, `NAME`, `MANAGEMENT` and `MEMBERS`. This makes it
possible to verify that circuits have been successfully created as well as being
able to access the generated circuit ID assigned to a circuit. The information
displayed will be the same for all member nodes. The circuits listed have been
accepted by all members and are currently active, meaning their `circuit_status`
is `Active`.

FLAGS
=====
`-h`, `--help`
: Prints help information

`-q`, `--quiet`
: Decrease verbosity (the opposite of -v). When specified, only errors or
  warnings will be output.

`-V`, `--version`
: Prints version information

`-v`
: Increases verbosity (the opposite of -q). Specify multiple times for more
  output.

OPTIONS
=======
`-F`, `--format` FORMAT
: Specifies the output format of the circuit. (default `human`). Possible values
  for formatting are `human` and `csv`.

`-k`, `--key` PRIVATE-KEY-FILE
: Specifies the private signing key (either a file path or the name of a
  .priv file in $HOME/.splinter/keys).

`-m`, `--member` <member>
: Filter the circuits list by a node ID that is present in the circuits’ members
  list.

`--circuit-status` CIRCUIT-STATUS
: Filter the circuit proposals list by their circuit status. Possible values
  for the `circuit-status` filter are `active`, `disbanded` and `abandoned`.

`-U`, `--url` URL
: Specifies the URL for the `splinterd` REST API. The URL is required unless
  `$SPLINTER_REST_API_URL` is set.

EXAMPLES
========
This command displays information about circuits with a default `human`
formatting, meaning the information is displayed in a table. The `--member`
and `--circuit-status` options allow for filtering the circuits.

The following command does not specify any filters, therefore all active
circuits the local node, `alpha-node-000` is a member of are displayed.
```
$ splinter circuit list \
  --url URL-of-alpha-node-splinterd-REST-API
ID            NAME      MANAGEMENT    MEMBERS
01234-ABCDE   -         mgmt001       alpha-node-000;beta-node-000
43210-ABCDE   circuit1  mgmt001       alpha-node-000;gamma-node-000
56789-ABCDE   -         mgmt002       alpha-node-000;gamma-node-000
```

The next command specifies a `--member` filter, therefore all
active circuits the local node, `alpha-node-000` is a part of including the
`gamma-node-000` node ID will be listed.
```
$ splinter circuit list \
  --member gamma-node-000 \
  --url URL-of-alpha-node-splinterd-REST-API
ID            NAME      MANAGEMENT    MEMBERS
43210-ABCDE   circuit1  mgmt001       alpha-node-000;gamma-node-000
56789-ABCDE   -         mgmt002       alpha-node-000;gamma-node-000
```

The next command specifies a `--circuit-status` filter, therefore all
circuits the local node, `alpha-node-000` is a part of that have the provided
`circuit_status` will be listed. Note: The circuits listed have a
`circuit_status` of `Disbanded`. Therefore, these circuits will not be listed
using `splinter circuit list` with no filters as only `Active` circuits are
listed by default.
```
$ splinter circuit list \
  --circuit-status disbanded \
  --url URL-of-alpha-node-splinterd-REST-API
ID            NAME      MANAGEMENT    MEMBERS
43210-GHIJK   circuit0  mgmt001       alpha-node-000;gamma-node-000
56789-GHIJK   -         mgmt001       alpha-node-000;beta-node-000
```

Since all of the active circuits listed have been accepted by each member, the
same circuit information will be displayed for member nodes.

From the perspective of the `gamma-node-000` node, this command will display the
following with no filters:
```
$ splinter circuit list \
  --url URL-of-gamma-node-splinterd-REST-API
ID            NAME      MANAGEMENT    MEMBERS
43210-ABCDE   circuit1  mgmt001       alpha-node-000;gamma-node-000
56789-ABCDE   -         mgmt002       alpha-node-000;gamma-node-000
```

From the perspective of the `beta-node-000` node, this command will display the
following with no filters:
```
$ splinter circuit list \
  --url URL-of-gamma-node-splinterd-REST-API
ID            NAME  MANAGEMENT    MEMBERS
01234-ABCDE   -     mgmt001       alpha-node-000;beta-node-000
```

ENVIRONMENT VARIABLES
=====================
**SPLINTER_REST_API_URL**
: URL for the `splinterd` REST API. (See `-U`, `--url`.)

SEE ALSO
========
| `splinter-circuit-abandon(1)`
| `splinter-circuit-disband(1)`
| `splinter-circuit-proposals(1)`
| `splinter-circuit-propose(1)`
| `splinter-circuit-purge(1)`
| `splinter-circuit-remove-proposal(1)`
| `splinter-circuit-show(1)`
| `splinter-circuit-vote(1)`
|
| Splinter documentation: https://www.splinter.dev/docs/0.5/
