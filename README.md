# vm2k8s

Exports application metrics from WMI queries. The default export location is the installation folder.

### Run on Terminal

This section explains how to run vm2k8s simply on your terminal. To using as Window Service, skip this step.

Name | Description
-----|------------
`--collectors-enabled` | Provide a comma-separated list of enabled collectors. `Defaults are process,netframework_clrmemory`
`--port`               | The port to bind to. `Defaults to 9293`
`--process-white-list` | Regexp of processes to include. Process name must both match whitelist and not match blacklist to be included. `Default is ".*"`
`--process-black-list` | Regexp of processes to include. Process name must both match whitelist and not match blacklist to be included. `Default is ""`
`--interval`           | Scrape interval seconds. `Default is 60 seconds`

Example invocations:

```powershell
<path-to-exe-file> --process-white-list <your-app-pool-names-regex>
```

### Run as Windows Service

If the installer is run without any parameters, the exporter will run with default settings for enabled collectors, ports, etc. The following parameters are available:

Name | Description
-----|------------
`ENABLED_COLLECTORS` | As the `--collectors-enabled` flag.
`PROCESS_WHITE_LIST` | As the `--process-white-list` flag.
`PROCESS_BLACK_LIST` | As the `--process-black-list` flag.
`INTERVAL` | As the `--interval` flag.
`EXTRA_FLAGS` | Allows passing full CLI flags. `Defaults to an empty string.`

Parameters are sent to the installer via `msiexec`. Example invocations:

```powershell
msiexec /i <path-to-msi-file> ENABLED_COLLECTORS=process,netframework_clrmemory PORT=5000
```

Example service collector with a custom query.
```powershell
msiexec /i <path-to-msi-file> ENABLED_COLLECTORS=process --% EXTRA_FLAGS="--interval ""60"""
```

On some older versions of Windows you may need to surround parameter values with double quotes to get the install command parsing properly:
```powershell
msiexec /i <path-to-msi-file> ENABLED_COLLECTORS="process"
```