# ifcp -  Cache Poisoning Fuzzer

`ifcp` is an advanced, high-efficiency Cache Poisoning Fuzzer optimized for large-scale bug bounty reconnaissance. It automatically injects deterministically isolated payloads across dozens of unkeyed HTTP headers concurrently to identify cache poisoning, redirect hijackings, and header reflections with 0% false positive rates.

## Features

- **Bulk Header Execution:** Injects **28+** unkeyed headers (`X-Forwarded-Host`, `X-Forwarded-Scheme`, etc.) simultaneously in a single HTTP request to exponentially cut bandwidth overhead by ~90%.
- **Deterministic Payload Tracking:** Generates hyper-isolated strings completely detached from the cache-buster token to guarantee that organic reflections (like `<link rel="canonical" href="?cp_probe=X">`) never trigger false positives.
- **Multi-Strategy Cache Routing:** Recursively checks against varying cache normalization structures safely utilizing:
  - Standard Query: `?cp_probe=X`
  - Unkeyed Query Parameter: `?X=1`
  - Path Routing: `/X`
- **Smart Severity Analysis:** Autonomously validates `X-Cache`, `CF-Cache-Status`, `Age`, and `X-Served-By` to rank bugs intelligently between **HIGH** severity (direct Cache hit) and **MEDIUM/LOW** severity (isolated Header Reflection).
- **Subdomain Consolidation Output:** Parses and groups outputs natively by `Target: subdomain`, securely eliminating output repetition when fuzzing substantial `urls.txt` arrays.
- **Aggressive Proxy Routing:** Features explicit interceptor support (`-x` / `--proxy`), instantly dropping SSL validation dependencies so `ifcp` seamlessly tunnels data through tools like Burp Suite and Caido.

## Installation

```bash
git clone https://github.com/your-username/ifcp
cd ifcp
chmod +x ifcp
sudo ln -s /usr/bin/ifcp
```

## Quick Start

You can pipe data directly into the application securely via `stdin`.

Test a single domain:
```bash
echo "https://example.com" |ifcp
```

Test a massive bug bounty footprint:
```bash
cat subdomains.txt |ifcp
```

## Flags & Arguments

| Flag | Argument | Description |
| ---- | -------- | ----------- |
| `-H` | `--header` | Define custom headers needed for authentication (e.g., `-H "Cookie: session=123"`). |
| `-t` | `--timeout`| Set timeout interval dropping stalled requests. (Default: 10s) |
| `-x` | `--proxy`  | Route testing traffic explicitly towards an intercepting proxy (e.g., `http://127.0.0.1:8080`). |
| `-j` | `--json`   | Format the output purely in JSON Lines structure to stream natively into external pipelines. |
|      | `--no-color` | Prints results to standard output stripped of ANSI styling. |

## Example of usage

![Image](https://github.com/user-attachments/assets/b5048558-bb4b-4c44-a3c9-3177629ad1d2)

## Disclaimer & Legal

This script is purely produced for authorized security auditing purposes and legal bug bounty operations. By executing `ifcp`, you explicitly adhere to all applicable cybersecurity protocols mapping your targets.
