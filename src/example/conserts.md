<!--
SPDX-FileCopyrightText: 2022 Andreas Schmidt <andreas.schmidt@iese.fraunhofer.de>

SPDX-License-Identifier: CC-BY-SA-4.0
-->

# ConSerts

At the system level, we have the following composition of services:

<div style="text-align: center">
<a href="./ServicesComposition.svg">
<img alt="Service Composition" src="./ServicesComposition.svg" width="400em" />
</a>
</div>

## Bin-Picking Application

At the application service level, we have the bin-picking application:

```dot process
digraph consert_binpicking { rankdir = BT; node [fontsize=16 shape=box fontname="Verdana"];
    Gate0[label="<Gate>\n||"];
    HighSpeedPicking[label="<Demand>\n\nHighSpeedPicking\n\nVelocity : meter per second\n2.0..=3.0 (G <= D)\n\nPL\n[PL d] (D <= G)\n\nSIL\n[SIL 2] (D <= G)"];
    MediumSpeedPicking[label="<Demand>\n\nMediumSpeedPicking\n\nVelocity : meter per second\n0.0..=2.0 (G <= D)\n\nPL\n[PL d] (D <= G)\n\nSIL\n[SIL 2] (D <= G)"];
    TLG_HighPerformance[label="<Gate>\n&"];
    TLG_MediumPerformance[label="<Gate>\n&"];
    TLG_SafeStop[label="<Gate>\n&"];
    TLG_SafeStop -> SafeStop[label=""];
    MediumSpeedPicking -> Gate0[label=""];
    HighSpeedPicking -> Gate0[label=""];
    Gate0 -> TLG_MediumPerformance[label=""];
    TLG_MediumPerformance -> MediumPerformance[label=""];
    HighSpeedPicking -> TLG_HighPerformance[label=""];
    TLG_HighPerformance -> HighPerformance[label=""];{rank=same;
    HighPerformance[label="<Guarantee>\n\nHighPerformance\n\nHighPerformance\n\nPL\n[PL d] (D <= G)\n\nSIL\n[SIL 2] (D <= G)"];
    MediumPerformance[label="<Guarantee>\n\nMediumPerformance\n\nMediumPerformance\n\nPL\n[PL d] (D <= G)\n\nSIL\n[SIL 2] (D <= G)"];
    SafeStop[label="<Guarantee>\n\nSafeStop\n\nSafeStop\n\nPL\n[PL d] (D <= G)\n\nSIL\n[SIL 2] (D <= G)"];}{rank=same;}}
```

Depending on the provided performance of the robot, the high-level service can be provided with a different performance.
Concretely, a robot moving at full speed can process more items in the same time than if it runs at reduced speed.
In this use case, this is a 1:1 mapping, but more complex scenarios could have more intricate interactions between the performance of collaborating systems and the overall service performance.

## Robot

The high-level performance is primarily impacted by the robot arm:

```dot process
digraph consert_robot { rankdir = BT; node [fontsize=16 shape=box fontname="Verdana"];
    EnvironmentUnoccupiedLong[label="<Demand>\n\nEnvironmentUnoccupiedLong\n\nTime : second\n0.0..=1.98 (D <= G)\n\nPL\n[PL d] (D <= G)\n\nSIL\n[SIL 2] (D <= G)"];
    EnvironmentUnoccupiedMedium[label="<Demand>\n\nEnvironmentUnoccupiedMedium\n\nTime : second\n0.0..=1.48 (D <= G)\n\nPL\n[PL d] (D <= G)\n\nSIL\n[SIL 2] (D <= G)"];
    Gate0[label="<Gate>\n&"];
    Gate1[label="<Gate>\n&"];
    MeasuredForce[label="<Evidence>\n\nMeasuredForce\n\nForce : newton\n0.0..=2.0 (G <= D)"];
    ModesConfigured[label="<Evidence>\n\nModes properly configured\n\nModesConfigured"];
    MountedToolSafe[label="<Evidence>\n\nMounted Tool is safe\n\nMountedToolSafe"];
    PowerAndForceReduction[label="<Evidence>\n\nPower & Force Reduction active\n\nPowerAndForceReduction"];
    SpeedModeFast[label="<Evidence>\n\nSpeed Mode fast active\n\nSpeedModeFast"];
    SpeedModeReduced[label="<Evidence>\n\nSpeed Mode reduced active\n\nSpeedModeReduced"];
    TLG_HighSpeedPicking[label="<Gate>\n&"];
    TLG_MediumSpeedPicking[label="<Gate>\n&"];
    TLG_PowerForceReductionPicking[label="<Gate>\n&"];
    SpeedModeFast -> TLG_HighSpeedPicking[label=""];
    EnvironmentUnoccupiedLong -> TLG_HighSpeedPicking[label=""];
    ModesConfigured -> TLG_HighSpeedPicking[label=""];
    TLG_HighSpeedPicking -> HighSpeedPicking[label=""];
    SpeedModeReduced -> Gate1[label=""];
    ModesConfigured -> Gate1[label=""];
    Gate1 -> TLG_MediumSpeedPicking[label=""];
    EnvironmentUnoccupiedMedium -> TLG_MediumSpeedPicking[label=""];
    TLG_MediumSpeedPicking -> MediumSpeedPicking[label=""];
    SpeedModeReduced -> Gate1[label=""];
    ModesConfigured -> Gate1[label=""];
    PowerAndForceReduction -> Gate0[label=""];
    MeasuredForce -> Gate0[label=""];
    MountedToolSafe -> Gate0[label=""];
    Gate1 -> TLG_PowerForceReductionPicking[label=""];
    Gate0 -> TLG_PowerForceReductionPicking[label=""];
    TLG_PowerForceReductionPicking -> PowerForceReductionPicking[label=""];{rank=same;
    HighSpeedPicking[label="<Guarantee>\n\nHighSpeedPicking\n\nVelocity : meter per second\n2.0..=3.0 (G <= D)\n\nPL\n[PL d] (D <= G)\n\nSIL\n[SIL 2] (D <= G)"];
    MediumSpeedPicking[label="<Guarantee>\n\nMediumSpeedPicking\n\nVelocity : meter per second\n0.0..=2.0 (G <= D)\n\nPL\n[PL d] (D <= G)\n\nSIL\n[SIL 2] (D <= G)"];
    PowerForceReductionPicking[label="<Guarantee>\n\nPowerForceReductionPicking\n\nVelocity : meter per second\n0.0..=1.5 (G <= D)\n\nPL\n[PL d] (D <= G)\n\nSIL\n[SIL 2] (D <= G)"];}{rank=same;}}
```

The robot has demands in terms of an unoccupied environment.
The major quantity that must be guaranteed is the worst-case time window the environment can be guaranteed to be free.
The robot arm's current speed has a major impact on this requirement, as its stopping time is dependent on this.
Obviously, the run-time evidence "Modes properly configured" is required and checked manually by the health and safety engineer.

For each configured speed mode, we find the appropriate time \\(0 .. t\\) by considering the following measures and their effect on the worst-case stopping distance:

* *Robot Arm Speed*: deccelerating to 0 takes longer for higher speeds.
* *Robot Arm Elevation*: the arm might be closer to the edge of the workspace.
* *Robot Arm Load*: higher load means higher inertia and longer time to stop.
* *Electric Response Time*: is spent between the detection of a signal and the stop action.

Using the worst-case human speed, we can turn this into a time value \\(t\\).

Note that the ConSert could be more detailed if the robot has more sensing capabilities:

* If the loads of the arm are known, multiple values for its impact on the stopping time can be provided. Hence, lightly loaded arms can move faster.
* If the elevation is known, multiple values for its impact can be provided.

## Workspace

Providing guarantees about an unoccupied environment is done by the workspace:

```dot process
digraph consert_workspace { rankdir = BT; node [fontsize=16 shape=box fontname="Verdana"];
    CommunicationDelay[label="<Evidence>\n\nCommunication Delay <= 10ms\n\nTime : millisecond\n0.0..=10.0 (D <= G)"];
    Gate0[label="<Gate>\n&"];
    InstallationApproved[label="<Evidence>\n\nInstallation Approved w.r.t. ISO 13855\n\nInstallationApproved"];
    TLG_EnvironmentUnoccupiedLong[label="<Gate>\n&"];
    TLG_EnvironmentUnoccupiedMedium[label="<Gate>\n&"];
    UnoccupiedLong[label="<Demand>\n\nUnoccupied Time <= 2.0s\n\nTime : second\n0.0..=2.0 (D <= G)\n\nPL\n[PL d] (D <= G)\n\nSIL\n[SIL 2] (D <= G)"];
    UnoccupiedMedium[label="<Demand>\n\nUnoccupied Time <= 1.5s\n\nTime : second\n0.0..=1.5 (D <= G)\n\nPL\n[PL d] (D <= G)\n\nSIL\n[SIL 2] (D <= G)"];
    InstallationApproved -> Gate0[label=""];
    CommunicationDelay -> Gate0[label=""];
    UnoccupiedLong -> TLG_EnvironmentUnoccupiedLong[label=""];
    Gate0 -> TLG_EnvironmentUnoccupiedLong[label=""];
    TLG_EnvironmentUnoccupiedLong -> EnvironmentUnoccupiedLong[label=""];
    InstallationApproved -> Gate0[label=""];
    CommunicationDelay -> Gate0[label=""];
    UnoccupiedMedium -> TLG_EnvironmentUnoccupiedMedium[label=""];
    Gate0 -> TLG_EnvironmentUnoccupiedMedium[label=""];
    TLG_EnvironmentUnoccupiedMedium -> EnvironmentUnoccupiedMedium[label=""];{rank=same;
    EnvironmentUnoccupiedLong[label="<Guarantee>\n\nUnoccupied Environment Time <= 1.99s\n\nTime : second\n0.0..=1.99 (D <= G)\n\nPL\n[PL d] (D <= G)\n\nSIL\n[SIL 2] (D <= G)"];
    EnvironmentUnoccupiedMedium[label="<Guarantee>\n\nUnoccupied Environment Time <= 1.49s\n\nTime : second\n0.0..=1.49 (D <= G)\n\nPL\n[PL d] (D <= G)\n\nSIL\n[SIL 2] (D <= G)"];}{rank=same;}}
```

In the workspace, we rely on a) detection services (e.g. sensors) as well as b) static measures (e.g. walls, fences) to guarantee a minimal time that is required for a human to go through the workspace into the robot's operation area.
We account for network communication delays (communicating the guarantees and demands between sensor and robot), reducing the guaranteed value appropriately, as well as other surcharges due to the setup of the workspace.
We further require a manual run-time evidence "Installation approved by HSE".

## Environment Detectors

Eventually, the environment detectors provide us with run-time information about the unoccupied state of the area around the robot:

### Scanner

```dot process
digraph consert_scanner { rankdir = BT; node [fontsize=16 shape=box fontname="Verdana"];
    Gate0[label="<Gate>\n&"];
    InstallationApproved[label="<Evidence>\n\nInstallation Approved w.r.t. ISO 13855\n\nInstallationApproved"];
    MultiEvaluation4[label="<Evidence>\n\nMulti Evaluation = 4 PL d\n\nMultiEvaluation"];
    S1_Unoccupied[label="<Evidence>\n\nZone S1 Unoccupied\n\nS1_Unoccupied"];
    S2_Unoccupied[label="<Evidence>\n\nZone S2 Unoccupied\n\nS2_Unoccupied"];
    SingleEvaluation[label="<Evidence>\n\nMulti Evaluation = 1 PL d\n\nMultiEvaluation"];
    TLG_UnoccupiedLong[label="<Gate>\n&"];
    TLG_UnoccupiedMedium[label="<Gate>\n&"];
    TLG_UnoccupiedShort[label="<Gate>\n&"];
    SingleEvaluation -> Gate0[label=""];
    InstallationApproved -> Gate0[label=""];
    S1_Unoccupied -> Gate0[label=""];
    Gate0 -> TLG_UnoccupiedLong[label=""];
    S2_Unoccupied -> TLG_UnoccupiedLong[label=""];
    TLG_UnoccupiedLong -> UnoccupiedLong[label=""];
    SingleEvaluation -> Gate0[label=""];
    InstallationApproved -> Gate0[label=""];
    S1_Unoccupied -> Gate0[label=""];
    Gate0 -> TLG_UnoccupiedMedium[label=""];
    TLG_UnoccupiedMedium -> UnoccupiedMedium[label=""];
    MultiEvaluation4 -> TLG_UnoccupiedShort[label=""];
    InstallationApproved -> TLG_UnoccupiedShort[label=""];
    S1_Unoccupied -> TLG_UnoccupiedShort[label=""];
    S2_Unoccupied -> TLG_UnoccupiedShort[label=""];
    TLG_UnoccupiedShort -> UnoccupiedShort[label=""];{rank=same;
    UnoccupiedLong[label="<Guarantee>\n\nUnoccupiedLong\n\nTime : second\n0.0..=2.0 (D <= G)\n\nPL\n[PL d] (D <= G)\n\nSIL\n[SIL 2] (D <= G)"];
    UnoccupiedMedium[label="<Guarantee>\n\nUnoccupiedMedium\n\nTime : second\n0.0..=1.5 (D <= G)\n\nPL\n[PL d] (D <= G)\n\nSIL\n[SIL 2] (D <= G)"];
    UnoccupiedShort[label="<Guarantee>\n\nUnoccupiedShort\n\nTime : second\n0.0..=0.5 (D <= G)\n\nPL\n[PL d] (D <= G)\n\nSIL\n[SIL 2] (D <= G)"];}{rank=same;}}
```

Here, the provided time guarantee is impacted by various factors and in particular the operation mode of the sensor.
For the laser scanner, the following considerations are made (in accordance with ISO13855):

\\[t_{LS} = t_0 + \frac{TZ + Z_R + C}{1600mm/s} \\]

* Response Time of the Sensor: \\(t_0 = (t_s + t_i) \cdot n + t_p\\)
  * \\(t_s\\): Scan Cycle Time (often configurable, e.g. 30ms or 40ms).
  * \\(t_i\\): Interference Protection Time (0 to 3ms).
  * \\(n\\): Multi-evaluation (e.g. between 2 and 16).
  * \\(t_p\\): Signal processing time given by data sheet (e.g. 35ms).
* \\(TZ\\): Sensor Tolerance (static value given in handbook)
* \\(Z_R\\): Reflection surcharges (static values given in handbook)
* \\(C\\): Surcharges for undetected reaching modes, depending on the installation height: \\(C = 1200mm - (0.4 \cdot H_D)\\)

Using this \\(t_{LS}\\) and an equivalent \\(S_{LS}\\):

\\[S_{LS} = 1600 mm/s \cdot t_0 + TZ + Z_R + C\\]

Note that this is the minimal case, i.e. if the zone has a size of \\(S_{LS}\\) we can not guarantee more than 0 sec to other systems (all time budget is used for the scanner itself).
If we set the zone to a larger value \\(S\\), we get \\(S_{diff}\\) and respectively \\(t_{diff} = \frac{S_{diff}}{1600 mm/s}\\) as a time budget.
The matching guarantee (for this zone and scanner config) then guarantees \\(0\ \mathrm{..}\ t_{diff}\\).

Note that different safety zones (with increasing size) can guarantee larger values of \\(t\\).
Also the configuration options (e.g. multi-mode evaluation) have an impact.
