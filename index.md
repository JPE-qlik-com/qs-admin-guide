## Qlik Sense Admin Playbook

A repository of administrative best practices, organized by time and type.

| Daily                                                    | Weekly                                | Monthly                        | Quarterly                      | Yearly                          |
|----------------------------------------------------------|---------------------------------------|--------------------------------|--------------------------------|---------------------------------|
| [Review Tasks](docs/system_spot_check/tasks.md) | [Check for New Apps (QMC)](docs/asset_management/apps/new_apps.md)        | Remove/Quarantine Unused Apps  | Review/Optimize QVD Structures | Review Architecture Scale Plan  |
| [Review Node<br>Health](docs/system_spot_check/nodes.md) | [Check for New Data Connections (QMC)](docs/asset_management/data_connections.md)        | Remove Unused Tasks            | Archive old Archive Logs       | Review Hardware for Replacement |
| [Review 24<br>Hour Summary<br>(Ops Monitor)](docs/system_spot_check/24_hour_summary.md)  | [Check for New Streams (QMC)](docs/asset_management/streams.md)      | Remove Unused Connections      | Update Capacity Plan           | Practice Recovery Processes     |
| [Review<br>Expensive<br>Objects/Apps<br>(Telemetry)](docs/system_spot_check/telemetry.md)        | [Check for New Tasks (QMC)](docs/asset_management/tasks.md)          | Remove Unused Streams          | Optimize Batch Window          |                                 |
|                                                          | [Check for New/Modified Security Rules (QMC)](docs/asset_management/security_rules.md)   | Remove Unused Sheets           | Upgrade Planning/Evaluation    |                                 |
|                                                          | [Check for New/Modified Custom Properties (QMC)](docs/asset_management/custom_properties.md)         | Eliminate Unneeded Extensions  |                                |                                 |
|                                                          | [Analyze/Audit License Allocations](docs/asset_management/license_allocations.md)  | Optimize License Allocations   |                                |                                 |
|                                                          | [Check for New Extensions (QMC)](docs/asset_management/extensions.md)        | Archive Logs - Check for Space |                                |                                 |
|                                                          | Review Ops Mon - Task Details         | Verify Backups are Being Done  |                                |                                 |
|                                                          | Review Ops Mon - Log Details (Errors) | Healthcheck Dashboard Review   |                                |                                 |
|                                                          | Review App Metadata for Anamolies     | Review Ops Mon - Exports       |                                |                                 |