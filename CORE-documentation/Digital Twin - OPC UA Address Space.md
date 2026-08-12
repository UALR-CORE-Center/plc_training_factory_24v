### OPC UA Address Space to Digital Twin MQTT Mapping

**Purpose:** Track which PLC OPC UA addresses to be used for digital twin values sync.

**PLC OPC UA server:** `opc.tcp://192.168.0.1:4840`

**Address pattern:** `ns=3;s="gtyp_<STATION>"."<Axis/Field>"."<Variable>"`

---

| Station | Field / Axis | OPC UA Node Address | Datatype | MQTT Topic | JSON Key |
| --- | --- | --- | --- | --- | --- |
| HBW | Horizontal Axis | `gtyp_HBW.Horizontal_Axis.di_Actual_Position` | Int32 | `fl/i/dt/hbw/pos` | `hz_axis_actpos` |
| HBW | Horizontal Axis | `gtyp_HBW.Horizontal_Axis.di_Target_Position` | Int32 | `fl/i/dt/hbw/pos` | `hz_axis_targetpos` |
| HBW | Horizontal Axis | `gtyp_HBW.Horizontal_Axis.x_Position_Reached` | Boolean | `fl/i/dt/hbw/pos` | `hz_axis_posreached` |
| HBW | Vertical Axis | `gtyp_HBW.Vertical_Axis.di_Actual_Position` | Int32 | `fl/i/dt/hbw/pos` | `vert_axis_actpos` |
| HBW | Vertical Axis | `gtyp_HBW.Vertical_Axis.di_Target_Position` | Int32 | `fl/i/dt/hbw/pos` | `vert_axis_targetpos` |
| HBW | Vertical Axis | `gtyp_HBW.Vertical_Axis.x_Position_Reached` | Boolean | `fl/i/dt/hbw/pos` | `vert_axis_posreached` |
| HBW | Lever Front switch press | `IX_HBW_SwitchCantileverFront_I5` | Boolean | `fl/i/dt/hbw/pos` | `lever_front_switch` |
| HBW | Lever Back switch press | `IX_HBW_SwitchCantileverBack_I6` | Boolean | `fl/i/dt/hbw/pos` | `lever_back_switch` |
| VGR | Horizontal Axis | `gtyp_VGR.horizontal_Axis.di_Actual_Position` | Int32 | `fl/i/dt/vgr/pos` | `hz_axis_actpos` |
| VGR | Vertical Axis | `gtyp_VGR.vertical_Axis.di_Actual_Position` | Int32 | `fl/i/dt/vgr/pos` | `vert_axis_actpos` |
| VGR | Rotate Axis | `gtyp_VGR.rotate_Axis.di_Actual_Position` | Int32 | `fl/i/dt/vgr/pos` | `rot_axis_actpos` |
| VGR | Horizontal Axis | `gtyp_VGR.horizontal_Axis.di_Target_Position` | Int32 | `fl/i/dt/vgr/pos` | `hz_axis_targetpos` |
| VGR | Vertical Axis | `gtyp_VGR.vertical_Axis.di_Target_Position` | Int32 | `fl/i/dt/vgr/pos` | `vert_axis_targetpos` |
| VGR | Rotate Axis | `gtyp_VGR.rotate_Axis.di_Target_Position` | Int32 | `fl/i/dt/vgr/pos` | `rot_axis_targetpos` |
| VGR | Horizontal Axis | `gtyp_VGR.horizontal_Axis.x_Position_Reached` | Boolean | `fl/i/dt/vgr/pos` | `hz_axis_posreached` |
| VGR | Vertical Axis | `gtyp_VGR.vertical_Axis.x_Position_Reached` | Boolean | `fl/i/dt/vgr/pos` | `vert_axis_posreached` |
| VGR | Rotate Axis | `gtyp_VGR.rotate_Axis.x_Position_Reached` | Boolean | `fl/i/dt/vgr/pos` | `rot_axis_posreached` |
| SSC | Horizontal Axis | `gtyp_SSC.Horizontal_Axis.di_Actual_Position` | Int32 | `fl/i/dt/ssc/pos` | `hz_axis_actpos` |
| SSC | Horizontal Axis | `gtyp_SSC.Horizontal_Axis.di_Target_Position` | Int32 | `fl/i/dt/ssc/pos` | `hz_axis_targetpos` |
| SSC | Horizontal Axis | `gtyp_SSC.Horizontal_Axis.x_Position_Reached` | Boolean | `fl/i/dt/ssc/pos` | `hz_axis_posreached` |
| SSC | Vertical Axis | `gtyp_SSC.Vertical_Axis.di_Actual_Position` | Int32 | `fl/i/dt/ssc/pos` | `vert_axis_actpos` |
| SSC | Vertical Axis | `gtyp_SSC.Vertical_Axis.di_Target_Position` | Int32 | `fl/i/dt/ssc/pos` | `vert_axis_targetpos` |
| SSC | Vertical Axis | `gtyp_SSC.Vertical_Axis.x_Position_Reached` | Boolean | `fl/i/dt/ssc/pos` | `vert_axis_posreached` |
| MPO | Oven Door (Open/Closed) | `QX_MPO_ValveOvenDoor_Q13` | Boolean | `fl/i/dt/mpo/pos` | `oven_door_opened` |
| MPO | Saw Activated | `QX_MPO_M3_Saw_Q4` | Boolean | `fl/i/dt/mpo/pos` | `saw_active` |
| MPO | Conveyor Belt Moving | `QX_MPO_M2_ConveyorBeltForward_Q3` | Boolean | `fl/i/dt/mpo/pos` | `convetor_belt` |
| MPO | Oven Light on/off | `QX_MPO_LightOven_Q9` | Boolean | `fl/i/dt/mpo/pos` | `oven_light` |
| MPO | Oven feeder inside | `IX_MPO_RefSwitchOvenFeederInside_I6` | Boolean | `fl/i/dt/mpo/pos` | `feeder_is_inside` |
| MPO | Oven feeder outside | `IX_MPO_RefSwitchOvenFeederOutside_I7` | Boolean | `fl/i/dt/mpo/pos` | `feeder_is_outside` |
| MPO | Turntable at vacuum position | `IX_MPO_RefSwitchTurnTable_PosVac_I1` | Boolean | `fl/i/dt/mpo/pos` | `turntable_pos_vac` |
| MPO | Turntable at saw position | `IX_MPO_RefSwitchTurnTable_PosSaw_I4` | Boolean | `fl/i/dt/mpo/pos` | `turntable_pos_saw` |
| MPO | Turntable at belt position | `IX_MPO_RefSwitchTurnTable_PosBelt_I2` | Boolean | `fl/i/dt/mpo/pos` | `turntable_pos_belt` |
| MPO | Vacuum at turntable position | `IX_MPO_RefSwitchVac_PosTurnTable_I5` | Boolean | `fl/i/dt/mpo/pos` | `vac_pos_turntable` |
| MPO | Vacuum at oven position | `IX_MPO_RefSwitchVac_PosOven_I8` | Boolean | `fl/i/dt/mpo/pos` | `vac_pos_oven` |
| SLD | Valve first (White) | `QX_SLD_ValveFirstEjectorWhite_Q3` | Boolean | `fl/i/dt/sld/pos` | `valve_first` |
| SLD | Valve second (Red) | `QX_SLD_ValveSecondEjectorRed_Q4` | Boolean | `fl/i/dt/sld/pos` | `valve_second` |
| SLD | Valve third (Blue) | `QX_SLD_ValveThirdEjectorBlue_Q5` | Boolean | `fl/i/dt/sld/pos` | `valve_third` |
| SLD | Conveyor Belt Active | `QX_SLD_M1_ConveyorBelt_Q1` | Boolean | `fl/i/dt/sld/pos` | `conveyor_belt_active` |
