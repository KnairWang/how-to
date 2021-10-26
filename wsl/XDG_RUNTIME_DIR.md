on WSL, as no systemd, by default the $XDG_RUNTIME_DIR is not set.
here is a code snip for setting up $XDG_RUNTIME_DIR:
```bash
if [[ -z "$XDG_RUNTIME_DIR" ]]; then
	export XDG_RUNTIME_DIR=/run/user/$UID
	if [[ ! -d "$XDG_RUNTIME_DIR" ]]; then
		export XDG_RUNTIME_DIR=/tmp/$USER-runtime
		if [[ ! -d "$XDG_RUNTIME_DIR" ]]; then
			mkdir -m 0700 "$XDG_RUNTIME_DIR"
		fi
	fi
fi
```
