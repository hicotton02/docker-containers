# Cudo Worker Core

To execute the worker core simply run the `cudo-worker-core` executable in a command prompt.

## Configurable options

The following options can be applied using command line options, environment variables or config [rc](https://www.npmjs.com/package/rc) files.

For example:
```
# Command line option
./cudo-worker-core --device-name Miner5

# Environment variable
DEVICE_NAME=Miner5 ./cudo-worker-core

# RC config file
echo "device-name=Miner5" > /etc/cudo_workerrc
./cudo-worker-core
```

The available options are available:

* `ORG` - switch the device to work for another organization, accepts a username
* `CONFIG_ID` - switch the device configuration, accepts a config identifier
* `ACCESS_KEY` - authenticate the device with an organization generated access key
* `DEVICE_NAME` - friendly device name used in the console, default is the device hostname
* `DEVICE_LABELS` - comma separated device labels that are appended to the device on startup

* `SOCKET_PATH` - socket file path for the RPC API
* `EXECUTION_CONTEXT` - specify the context in which the core is being run, accepts: systemd | launchd | winsw | child | electron | standalone, default: `standalone`

* `WS_HOST` - host to listen on for the RPC API
* `WS_PORT` - port to listen on for the RPC API
* `WS_PASSWORD` - password for authentication of WebSocket requests

* `LOG_PATH` - directory path to write log files, default is no log
* `HIDE_LOG_TIMESTAMP` - do not prefix timestamp to log messages
* `DISABLE_LOG_COLOR` - do not include ANSI codes in log messages

* `STORE_PATH` - directory path for writing state files
* `BIN_PATH` - directory path for executable dependencies
* `RUNTIME_PATH` - directory path for runtime dependencies
* `CUDO_SMI_PATH` - directory path for cudo smi logs

* `FORCE_AUTO_UPDATE` - force an auto update and do not perform any semver checks
* `UPDATE_CHANNEL_URL` - url prefix for auto updates checks, can be a file path or string url
* `UPDATE_INSTALLER_PATH` - temporary path to store updater installer executables
* `MAC_UPDATER_CHOICES_CHANGES_PATH` - path for mac choice change plist xml when installing macOS pkg files
* `WIN_UPDATER_SERVICE_PATH` - windows executable path for updater service executable
* `WIN_DESKTOP_EXE_PATH` - windows executable path for desktop app
* `WIN_UPDATING_DESKTOP_EXE_PATH` - temporary windows executable path for desktop app when an update is in progress
* `INSTALLED_COMPONENTS` - installed components seperated by comma, can be a file path or plain string
* `TRIGGER_EXIT` - executes the core only to trigger a graceful shutdown over a socket connection

* `REGISTRY_PATH` - directory path for storage of workload binaries
* `DISABLE_REGISTRY_CHECKSUM` - do not validate checksum of workloads before execution
* `REGISTRY_EXPIRY_HOURS` - prune registry items that have been unused in a specific number of hours, default: `720` (30 days)

* `COOLDOWN_PERIOD` - time in milliseconds to wait after shutting down workloads, default: `3000` (3 seconds)
* `TELEMETRY_INTERVAL` - interval in milliseconds telemetry can is published when status is idle, default: `300,000` (5 minutes)
* `HARDWARE_PROCESSOR_STATS_INTERVAL` - interval in milliseconds hardware processor stats are published, default: `300,000` (5 minutes)
* `TELEMETRY_MAX_FREQUENCY` - maximum interval in milliseconds telemetry can be published, default: `5000` (5 seconds)
* `REBOOT_IDLE_TIME` - time in seconds to wait after the machine was rebooted before workloads are activated, default: `0`
* `STATS_FREQUENCY` - frequency interval in milliseconds in which stats are emitted, default: `1000` (1 second)

* `CUDO_ENV` - specify the cudo api environment, default: `production`
* `CUDO_API` - use a custom cudo api, default: `https://api.cudo.org/v1`

* `IOT_REGISTRY` - google iot registry, default: `miners`
* `IOT_PROTOCOL` - google iot connection protocol, default: `mqtts`
* `IOT_HOST` - google iot connection host, default: `mqtt.googleapis.com`
* `IOT_PORT` - google iot connection port, default: `8883`
