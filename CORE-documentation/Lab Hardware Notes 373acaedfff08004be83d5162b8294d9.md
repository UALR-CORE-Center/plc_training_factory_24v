# Lab Hardware Notes

Created by: Atit Kharel

# 🏭 Lab Hardware Notes — Training Factory Industry 4.0 (24V)

> ⚠️ **Safety First:** Always completely unplug the power supply before transporting or performing any work on the system. All fuse lights must be green before powering on.
> 

---

## Official FischerTechnik Documentation:

[Training Factory Industry 4.0 24V Complete Set with PLC S7-1500](https://www.fischertechnik.de/en/products/industry-and-universities/training-models/560840-training-factory-industry-4-0-24v-complete-set-with-plc-s7-1500)

[https://github.com/fischertechnik/plc_training_factory_24v](https://github.com/fischertechnik/plc_training_factory_24v)

## 📦 Hardware Setup

### Unboxing

1. Ensure the large top of the steel flight case is **facing up at all times**
2. **Two people required** to lift the top off together
3. Grab the four latch handles firmly → fold out → twist to unlatch
4. Carefully lift the top to ~2 feet above the base, then carry it to the side
5. Visually inspect for an3y transportation damage

---

## ⚠️ Safety Rules .

- [ ]  Always completely unplug the power supply before transporting or performing any work
- [ ]  Double-check all wires are securely connected to the PLC and I/O modules before powering on
- [ ]  Do not touch the PCB (green circuit boards) during operation
- [ ]  All fuse lights must be **green** — if not, the fuse must be replaced by a lab technician.

---

## 🔧 PLC Reference

### Construction

1. Snap the **PLC connection board** onto a DIN rail (top-hat rail)
2. Insert modules from **left to right** in this order: 

The image below shows the fully assembled PLC connection board with all modules installed. Your final build should look exactly like this.

| **Slot** | **Module** |  |
| --- | --- | --- |
| 1 | CPU 1512SP-1 PN |  |
| 2 | DQ 16x24VDC/0.5A ST |  |
| 3 | DQ 16x24VDC/0.5A ST |  |
| 4 | DQ 16x24VDC/0.5A ST |  |
| 5 | DQ 4x24VDC/2A HS |  |
| 6 | DQ 4x24VDC/2A HS |  |
| 7 | DQ 4x24VDC/2A HS |  |
| 8 | DI 16x24VDC ST |  |
| 9 | DI 16x24VDC ST |  |
| 10 | DI 8x24VDC HS |  |
| 11 | DI 8x24VDC HS |  |
| 12 | AI 2xU ST |  |
| 13 | Server Module |  |

> 
> 
> 
> ![PLC.jpeg](Lab%20Hardware%20Notes/PLC.jpeg)
> 

### Wiring

> 
> 
> - Always put each wire into the terminal with the **same color mark** shows in the image below.
> - Make sure the wire is pushed in all the way (If needed use ELEC SCREWDRIVER flat head).
> - If in case you accidentally put the wrong wire into terminal, be careful while taking out. Press gently the **Tiny switch**  with the help of **ELEC SCREWDRIVER (flat head)** next to  each terminal.
> - It is easier to completely wire the plc modules first, then label them afterwards while checking your work.
> 
> ![PLC Wiring.JPG](Lab%20Hardware%20Notes/638b7230-bdf7-4a79-912a-94da4ca318e9.png)
> 

![image.png](Lab%20Hardware%20Notes/image.png)

## Below attached files are for Reference as per the FisherTechnik Manual.

[PLC Wiring Manual.pdf](Lab%20Hardware%20Notes/PLC_Wiring_Manual.pdf)

[PLC Wiring Manual 2.pdf](Lab%20Hardware%20Notes/PLC_Wiring_Manual_2.pdf)

---

## Quick Reference — Network & Credentials

| Device | IP Address | Service | Username | Password | Port |
| --- | --- | --- | --- | --- | --- |
| Raspberry Pi | `192.168.0.5` | SSH | `pi` | `ft-IOTpi2` | 22 |
| Raspberry Pi | `192.168.0.5` | Node-RED | — | — | 1880 |
| TP-Link Router | `192.168.0.252` | Web UI | `admin` | `TempPass#!` | — |
| TXT Controller | `192.168.0.10` | Web UI | `ft` | `fischertechnik` | — |
| TXT Controller | `192.168.0.10` | SSH | `ft` | `fischertechnik` | 22 |
| TXT Controller | `192.168.0.10` | Node-RED | `ft` | `fischertechnik` | 1880 |
| TXT Controller | `192.168.0.10` | MQTT | — | — | 1883 |
| PLC (S7-1500) | `192.168.0.1` | — | — | — | — |
| TIA Portal | — | App | `UALR-Core` | `TempPass3!` | — |
| Factory HMI |  |  | ualr-core | TempPass#! |  |
|  |  |  |  |  |  |
|  |  |  |  |  |  |

> 💡 Node-RED dashboard URL: `http://192.168.0.5:1880/ui/`
Router also accessible at `tplinkwifi.net` (before setup: `192.168.0.1`)
> 

Factory HMI webpage: [https://www.fischertechnik-cloud.com/en/factory/](https://www.fischertechnik-cloud.com/en/factory/)

## 💻 PLC Programming Guide

### Setup Steps

1. Setup the WiFi router before configuring the PLC: [http://tplinkwifi.net](http://tplinkwifi.net) (see password on router)
2. Change default gateway IP of the router to 192.168.0.252 from 192.168.0.1 (Network>LAN tab) so that 192.168.0.1 can be used by the PLC
3. Change DHCP ip range from 192.168.0.100-199 to 10-199 (in DHCP tab)
4. Connect TXT controller to the router’s wifi
5. Assign static ip for the TXT controller from DHCP Client list to be 192.168.0.10
6. Open project file from GitHub:
[https://github.com/fischertechnik/plc_training_factory_24v](https://github.com/fischertechnik/plc_training_factory_24v)
`PLC_S7_1500 > LearningFactory_4_0_24V_v15_TP_V18.zap18` 

And upgrade the project file to v21 when prompted.
7. Connect laptop by LAN  to PLC and update the PLC’s default IP to be 192.168.0.1 (PLC>Online and Diagnostics>functions>assign ip)
8. Right click on PLC>Change device>CPU 1512SP F-1 PN> 6Es7 512-1SM03-0AB0
    1. Set up the PLC programming password (use: TempPass3!) 
9. Set up a user (Security Settings> Users and Roles)
- Local User
Username: UALR-Core
Password: TempPass3!
10. Assign OPC UA Server Access role to the new user and anonymous user (Assigned Roles tab)
11. Assigned Rights
12. Go to Device Configuration>General (Tab)>OPC UA>Server>Security and then enable “No security” and “RSA encryptions”
13. On this tab, create and assign a certificate.
14. Compile
15. Download to the device
16. See Network Section in this doc for different accessible interfaces
17. Calibrate each components from NodeRed UI ([http://192.168.0.5:1880/#flow](http://192.168.0.5:1880/#flow)) and FischerTechnik Dashboard ([http://192.168.0.5:1880/ui/](http://192.168.0.5:1880/ui/)) 
18. Check Packaging Sequence
19. Check Ordering Sequence

# MQTT Details

[MQTT Notes](https://app.notion.com/p/MQTT-Notes-387acaedfff080a9b847cacf5ec95ee8?pvs=21)

---

### Known Fix — HBW Horizontal Axis Moving in the Wrong Direction

**Symptom:** HBW crane moves toward rack, but actual position reads negative, never reaches the set point, motor stalls at end stop.

**Cause:** Encoder counting direction is inverted — the internal FB receives negative counts when moving toward the rack, so it never confirms the position reached.

**Location:** `PLC > Program Blocks > 3.2. HBW > PRG_HBW_Axis_Horizontal [FB14]`

**Changes required:**

| Line | Original | Fixed |
| --- | --- | --- |
| `indi_Increment` | `"gtyp_HBW".Horizontal_Axis.di_Increment` | `"gtyp_HBW".Horizontal_Axis.di_Increment * -1` |
| `di_Actual_Position` | `#lfb_Horizontal_Axis.outdi_Actual_Position * -1` | `#lfb_Horizontal_Axis.outdi_Actual_Position` |

**Full corrected code block:**

```
IF #lx_Init THEN

    // FB Axis horizontal
    #lfb_Horizontal_Axis(ini_Axis := 6,
                         indi_Increment := "gtyp_HBW".Horizontal_Axis.di_Increment * -1,
                         inx_Ref_Switch := "IX_HBW_RefSwitchHorizontalAxis_I1",
                         inx_Referencing := "gtyp_HBW".Horizontal_Axis.x_Reference,
                         inx_Start_Positioning := "gtyp_HBW".Horizontal_Axis.x_Start_Positioning,
                         indi_Target_Position := "gtyp_HBW".Horizontal_Axis.di_Target_Position,
                         ini_PWM := "gtyp_HBW".Horizontal_Axis.i_PWM,
                         inouttyp_Config := "gtyp_HBW".Horizontal_Axis.Config,
                         inouttyp_Setup := "gtyp_SetupAxis");

    "QX_HBW_M2_HorizontalTowardsRack_Q3"        := #lfb_Horizontal_Axis.outx_Motor_Pos;
    "QX_HBW_M2_HorizontalTowardsConveyorBelt_Q4" := #lfb _Horizontal_Axis.outx_Motor_Neg;
    "QW_HBW_PWM_HorizontalAxis_M2"               := #lfb_Horizontal_Axis.outi_Motor_PWM;
    "gtyp_HBW".Horizontal_Axis.di_Actual_Position := #lfb_Horizontal_Axis.outdi_Actual_Position;
    "gtyp_HBW".Horizontal_Axis.x_Referenced       := #lfb_Horizontal_Axis.outx_Referenced;
    "gtyp_HBW".Horizontal_Axis.x_Position_Reached := #lfb_Horizontal_Axis.outx_Position_Reached;

ELSE
    #lx_Init := TRUE;
END_IF;
```

After editing: **STOP PLC → Compile → Download (Software only) → RUN**

# Known Fix — NFC Reader not working without internet access:

**Problem:** TXT controller NFC commands silently rejected because Nfc_MQTT.py drops any command where the timestamp differs from device clock by more than 60 seconds. Without internet, devices can't reach public NTP servers so clocks drift out of sync.

**Solution:** 

Just connect the wifi router to the internet

OR

Make the Raspberry Pi a local NTP server, use it in PLC and TXT Controller. TXT's sudo access is restricted, so we found that it uses Google NTP and will be redirecting the DNS lookup to the Pi instead.

Step 1 — Install and configure NTP server on Pi:

`ssh [pi@192.168.0.5](mailto:pi@192.168.0.5)
sudo apt-get install ntp -y
sudo nano /etc/ntp.conf`

Add these lines:
`server 127.127.1.0
fudge 127.127.1.0 stratum 10
restrict 192.168.0.0 mask 255.255.255.0 nomodify notrap`

then: 

`sudo systemctl enable ntp
sudo systemctl restart ntp`

Step 2 — Set NTP server on TXT controller via physical UI

On the TXT touchscreen:

Settings → Date & Time
Set NTP server to 192.168.0.5
Save

Step 3 — Set PLC NTP server in TIA Portal

Double-click PLC in the project tree
**Properties → General → Time of day**
Under NTP Mode section: 

Set NTP Server 1: 192.168.0.5

Then, Compile → Download to device

Step 4 — Reboot everything

To verify on TXT:

`timedatectl show-timesync`

*Should show: Server: 192.168.0.5*

### Final Verification

Compare timestamps across all devices — should match within 1-2 seconds:

1. Pi:
date -u
2. TXT Controller: 
date -u
3. TIA Portal: (**Go Online** first)
PLC → Online & Diagnostics → Functions → Set time → read PLC clock

## **Quick Start Instructions**

1. Open the case two feet up and remove to the left or right (two-person lift).
2. Plug in the power supply.
3. On a separate computer, connect to the router TP-Link_8FDD or TP-Link_8FDD_5G. (see back of router for password)
4. Enter [http://192.168.0.252/](http://192.168.0.252/) in the web browser and login to the router (password: TempPass#!)
5. Select “Quick Setup”, “Next”, “Next” (Dynamic IP), and then connect to your internet network with valid credentials (use **personal wireless hotspot** if access point is prohibited from local domain).
6. Place pucks in High-Bay Warehouse (white on top row, red in middle row, blue in bottom row).
7. Ensure the TXT controller python script is running (banner = red - not running, banner = yellow - initializing)
8. Once connected to the hotspot, enter [http://192.168.0.5:1880/ui](http://192.168.0.5:1880/ui) in the web browser to view the dashboard.
9. Select “HMI Main” in the top left.
10. On Siemens SIMATIC ET 200SP, flip the switch from STOP to RUN.
11. Select blue “Fill” button on the view dashboard during 60s after PLC startup. 
12. Check the TXT 4.0 Controller to verify that there are three pucks on each color.
13. Press “order” for any color to start the sequence.

[https://app.notion.com](https://app.notion.com)