# Cudo Worker Core API

On startup the core will create the following Unix socket or Windows named pipe.

* `Windows` - `\\.\pipe\cudo-worker`
* `Linux & macOS` - `/var/lib/cudo-worker.sock`

An IPC API is exposed through this socket allowing actions to be executed and global events to be received.

## Messages

The protocol exposed by the core is [JSON-RPC](https://www.jsonrpc.org/specification) compliant with the addition of global event messages.

### Action

An action message consists of a request and response message tied together by a unique identifier integer.

```json
{
  "id": 1,
  "method": "setOrganization",
  "params": ["sean"]
}
{
  "id": 1,
  "result": {
    "id": 129,
    "slug": "sean",
    "...": "..."
  }
}
```

### Action Error

If there was a problem executing the action an error key will be returned in the response.

```json
{
  "id": 2,
  "method": "getBenchmarks"
}
{
  "id": 2,
  "error": {
    "code": -3200,
    "message": "a problem happened"
  }
}
```

### Event

Event messages are broadcast to all connected clients.

```json
{
  "event": "organization",
  "params": [
    {
      "id": 129,
      "slug": "sean",
      "...": "..."
    }
  ]
}
```

## Actions

```typescript
/**
 * get information about the worker process.
 */
getInfo(): IWorkerInfo

/**
 * enable the policy on the device to allow mining.
 * this will start workloads for each active architecture.
 */
enable(): void
/**
 * disable the policy on the device to disallow mining.
 * any existing workloads will be stopped.
 */
disable(): void
/**
 * check whether worker is enabled.
 */
isEnabled(): boolean
/**
 * set whether the machine is in use.
 * this may cause a status change depending on the policy.
 */
setIdle(idle: boolean): void

/**
 * get display information for the current status.
 */
getStatus(): IStatus
/**
 * get the current device policy to be applied.
 */
getPolicy(): IPolicy
/**
 * get information about the current device.
 */
getDevice(): IMiner
/**
 * get information about the configuration the device is current assigned to.
 * this may be a shared organization config or a custom device specific config.
 */
getDeviceConfig(): IDeviceConfig
/**
 * upsert the device config for this device by specifying any fields to modify.
 * the workloadConfigs property controls which workloads and settings are to be used.
 * if the device is currently assigned to a share config this will trigger creation of a custom config.
 *
 * @param deviceConfig partial device configuration
 */
setDeviceConfig(deviceConfig: Partial<IDeviceConfig>): IDeviceConfig

/**
 * get information about the current organization.
 */
getOrganization(): IOrganization | null
/**
 * get statistics for the current organization.
 */
getOrganizationStats(): any
/**
 * get referral count for the current organization.
 */
getOrganizationReferralCount(): number
/**
 * assign this device a specific organization for all future work and earnings.
 * an error will be thrown if the organization slug cannot be found.
 *
 * @param slug organization slug
 */
setOrganization(slug: string): IOrganization
/**
 * unassign this device from any organization.
 * any existing workloads will be stopped.
 */
unsetOrganization(): void

/**
 * authenticate this device using an access key.
 * if the access key is invalid an error will be emitted.
 * if authentication is successful a new device will be created and returned.
 * if null access key is specified the device will return to being un-authenticated.
 *
 * @param accessKey organization access key
 */
setOrganizationAccessKey(accessKey: string | null): IMiner

/**
 * return all active jobs running on all architectures
 */
getActiveJobs(): IActiveJob[]

/**
 * return hardware information for gpus.
 */
getHardwareInfo(): IGpuInfoInfo | null
/**
 * return all hardware processors detected for this device.
 */
getHardwareProcessors(): IAggregatedHardwareProcessor[]

/**
 * return all saved workload history items for this device.
 */
getWorkloadHistory(): IDeviceWorkloadHistory[]
/**
 * return total pool calculated revenue information for this device.
 *
 * @param resolution        accuracy for revenue data
 * @param includeTimeSeries whether to include revenue over time
 */
getRevenue(resolution: RevenueResolution, includeTimeSeries?: boolean): any
/**
 * return estimated crowd sourced data and actual history data for workload hash rates and revenues.
 */
getBenchmarks(): any

/**
 * enable AMD large pages on Windows.
 */
enableLargePages(): void
/**
 * disable AMD large pages on Windows.
 */
disableLargePages(): void

/**
 * check whether registry directory is whitelisted using Windows defender
 * only available on Windows 10 and above.
 */
isRegistryWhitelisted(): boolean
/**
 * launch the Windows defender whitelist tool for the registry directory.
 * only available on Windows 10 and above.
 */
whitelistRegistry(): void

/**
 * return a list of coins on the platform.
 */
getCoins(): ICoin[]
/**
 * return a list of exchange rates for a specific coin.
 * @param toCoinId option coin to filter exchange rates
 */
getExchangeRates(toCoinId?: string): IExchangeRate[]
/**
 * return a list of image settings on the platform.
 * @param imageId optional imageId to filter settings
 */
getImageSettings(imageId?: string | null): IImageSetting[]
/**
 * return a list of workloads on the platform.
 * @param filter optional filter for workloads
 */
getWorkloads(filter?: Partial<IWorkload>): IWorkload[]

/**
 * return a list of backed off workloads and their stats.
 */
getWorkloadBackoffs(): IBackoff[]

/**
 * return a list of backed off workloads and their stats.
 * if no key is provided all backoffs will be reset.
 * @param key backoff key to reset
 */
resetWorkloadBackoff(key?: string): void

/**
 * return a list of workloads on the platform.
 * @param feedback body
 */
submitFeedback(feedback: IFeedback): void

/**
 * shutdown the application
 */
exit(): void
```

## Events

```typescript
/**
 * the worker has launched and is ready to receive requests.
 */
start: (info: IWorkerInfo) => void
/**
 * the worker is exiting but has not yet cleaned up.
 */
stop: () => void

/**
 * the device policy was enabled.
 */
enable: () => void
/**
 * the device policy was disabled.
 */
disable: () => void

/**
 * the overall device status changed.
 * @param status
 */
status: (status: IStatus) => void
/**
 * the current device config or one of it's properties has changed.
 * @param deviceConfig
 */
'device-config': (deviceConfig: IDeviceConfig) => void
/**
 * the current device config or one of it's properties has changed.
 * @param deviceConfig
 */
policy: (policy: IPolicy) => void

/**
 * the current organization or one of it's properties has changed.
 * @param organization
 */
organization: (organization: IOrganization) => void
/**
 * the current organization has been unset and there is currently no organization.
 */
'no-organization': () => void

/**
 * a job has been created on a given architecture
 * @param id
 * @param architecture
 */
'job': (spec: IJobSpec) => void
/**
 * a job status has been updated
 * @param id
 * @param status
 */
'job-status': (id: string, status: JobStatus) => void
/**
 * a job has emitted an error
 * @param id
 * @param error
 */
'job-error': (id: string, error: string) => void
/**
 * a job has emitted updated stats data
 * @param id
 * @param stats
 */
'job-stats': (id: string, stats: IJobStats) => void
/**
 * a job has ended with a given exit code.
 * @param id
 * @param exitCode
 * @param historyItem
 */
'job-end': (id: string, exitCode: number, historyItem: Partial<IDeviceWorkloadHistory>) => void

/**
 * hardware processor stats have been refreshed.
 * @param id
 * @param adapters
 */
'hardware-processor-stats': (id: string, adapters: IGpuInfoAdapter[]) => void

/**
 * registry directory has been whitelisted
 */
'registry-whitelist': () => void

/**
 * an error has occured
 */
error: (error: string) => void
```
