# NetPath

Network topology and route tracing from device configs. Single HTML file, runs in a browser.

No server, no install, no build step, no account. Configs are parsed and stored in your browser
and never sent anywhere.

## Use

Open `netpath.html`. Click **Load example lab** to see it working, or drag your configs onto the
left panel. Enter a source and destination address and press **Trace the path**.

## Input

Cisco IOS, IOS-XE, NX-OS, ASA: hostname, interfaces, VRFs, static routes, OSPF network statements,
BGP neighbours, switchport and VLAN config.

Palo Alto set-format or XML: interfaces and sub-interfaces, zones, virtual routers, static routes,
address objects, security rules. Produce set format with:

    set cli config-output-format set
    show config running

Per device you can also paste or upload:

- `show ip route`, `show routing route` — the real routing table
- `show cdp neighbors [detail]`, `show lldp neighbors` — layer 2 adjacency

## Features

**Topology** is inferred by grouping interfaces into subnets. Five views, from one line per device
pair up to every subnet drawn.

**Tracing** builds a forwarding table per device and does longest-prefix match, per VRF and virtual
router. Each hop shows the matched prefix, egress interface, next hop and route source. On Palo
Alto it also evaluates security policy by zone, source and destination.

**Failures** name the device and, where identifiable, the config line.

**Layer 2** from CDP/LLDP output. Checks each VLAN forms one connected domain and flags a trunk
that drops it mid-path.

**WAN** clouds are detected from carrier names in interface descriptions (AT&T, T-Mobile, Vodafone,
Colt by default, editable). A trace between sites crosses the cloud by asking each far-side router
which one has the route.

**Before/after** — each device keeps its original config. Edit the candidate, get a diff, and trace
in Compare mode for a verdict: unchanged, regression, pre-existing failure, or fixed.

**Saving** — autosaves to the browser. Save/Open project writes a JSON file. Node positions persist.

## Limitations

- IGP inference is hop-count shortest path, not SPF or BGP best-path. Routes tagged `igp` are a
  guess. For migration work, turn inference off or paste real routing tables and tick "table is
  complete" — otherwise inference can mask the static-route mistake you are looking for.
- BGP and EIGRP metrics are not evaluated. Local-pref, AS-path, MED and the EIGRP composite metric
  are ignored.
- Layer 2 adjacency requires CDP or LLDP output. Configs do not record cabling.
- Spanning tree is not modelled. A blocked link still counts as a path.
- NAT, Cisco ACLs and VRRP/HSRP are not read.
- Palo Alto policy matching ignores application, service, user and URL conditions.
- Line pinning works for Cisco and Palo Alto set-format. XML exports have no usable line numbers.
- Comfortable to ~50 devices, usable to ~100.

## Requirements

A modern browser. Cytoscape.js loads from a CDN, so the first load needs internet access. For
offline use, download `cytoscape.min.js` alongside the file and update the script tag.
