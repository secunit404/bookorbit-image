# bookorbit-patched

BookOrbit release images rebuilt with one fix applied: the qBittorrent download client
also accepts the `QBT_SID_<port>` session cookie that qBittorrent 5.2 and newer send.
Without it every call after a successful login is answered with `403`, and BookOrbit
reports `qBittorrent answered 403 for /api/v2/app/version`.

The fix itself lives in [secunit404/bookorbit](https://github.com/secunit404/bookorbit)
on branch `BO-1306-qbittorrent-5-2-session-cookie`. This repository only carries the
one-line patch in `patches/` plus the workflow that builds it.

## Image

```text
ghcr.io/secunit404/bookorbit-patched:release   # moving tag, latest patched release
ghcr.io/secunit404/bookorbit-patched:v2.10.0   # one tag per upstream release
```

Nothing else is changed: same Dockerfile, same source, one regex widened.

## How it builds

A daily workflow looks up the newest upstream release, skips it when that image already
exists, applies the patch to the release tag, and pushes the image. It can also be run
by hand, optionally against a specific tag.

## When upstream fixes this

The workflow checks the release for `QBT_SID` before patching. Once upstream ships the
fix it stops building and says so in the run summary. At that point switch the
deployment back to `ghcr.io/bookorbit/bookorbit` and archive this repository.
