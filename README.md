# PrintNC — Machine Configuration Repository

This repository contains machine configurations and documentation for one or more PrintNC-based CNC machines using:

* LinuxCNC
* Mesa Ethernet motion controllers (e.g. 7i96S)
* Huanyang VFD (or similar spindle drives)
* Structured Bill of Materials (BOM)
* Electrical and commissioning documentation

The repository is designed to support **multiple LinuxCNC configurations** within a single version-controlled project.

---

## Repository Structure

```
printnc-machine-config/
├── bom/                         # Bill(s) of Materials (CSV)
├── electronics/
│   └── vfd/                     # VFD parameter configurations
├── linuxcnc/
│   └── configs/
│       ├── machine-A/
│       │   ├── machine-A.ini
│       │   ├── *.hal
│       │   └── tool.tbl
│       ├── machine-B/
│       │   ├── machine-B.ini
│       │   └── ...
│       └── ...
├── resources/
│   └── docs/                    # Reference documentation & datasheets
└── README.md
```

Each subdirectory under:

```
linuxcnc/configs/
```

represents one complete LinuxCNC configuration.

A configuration directory must contain at minimum:

* One `.ini` file (entry point)
* Associated `.hal` files
* Tool table (if used)
* Any additional machine-specific resources

All configuration paths should be relative to allow relocation without modification.

---

## Configuration Installation

LinuxCNC automatically scans subdirectories inside:

```
~/linuxcnc/configs
```

To install this repository, clone it directly into that directory:

```bash
cd ~/linuxcnc/configs
git clone https://github.com/itsjute/printnc-machine-config.git
```

Resulting structure:

```
~/linuxcnc/configs/printnc-machine-config/
```

LinuxCNC will automatically detect all `.ini` files located anywhere within this repository.

No symlink or installation script is required.

---

## Selecting a Configuration

1. Open:

   ```
   LinuxCNC → Configuration Selector
   ```

2. Browse to:

   ```
   printnc-machine-config/linuxcnc/configs/<desired-machine>/<file>.ini
   ```

3. Select the appropriate `.ini` file.

4. Optionally click:

   ```
   Create Desktop Shortcut
   ```

This creates a launcher specific to the selected machine configuration.

---

## LinuxCNC Configuration Philosophy

Each configuration directory is self-contained and should include:

* Mesa hardware abstraction layer configuration
* Axis scaling and kinematics
* Homing configuration
* Tool table
* Spindle control integration
* Any custom HAL logic

Configurations should not rely on absolute paths outside their directory.

---

## PnCconf Project Files

Some machine directories may contain a `*.pncconf` file.

These files are **PnCconf wizard project files** and serve as optional hardware configuration metadata. They are included to:

* Preserve the Mesa hardware and pin-mapping snapshot
* Allow regeneration of a base configuration using the PnCconf wizard
* Improve long-term reproducibility

### Important

The authoritative machine configuration is the **hand-maintained `.ini` and `.hal` files**.

If PnCconf is used to regenerate configuration files:

* Carefully review the resulting changes
* Compare diffs using Git
* Revert any unwanted modifications

PnCconf may overwrite generated sections of `.ini` and `.hal` files. Regeneration should therefore be performed deliberately and reviewed before committing.

The `.pncconf` file is included for convenience and traceability, not as the primary configuration source.

---

## VFD Configuration

Documented VFD parameter sets are located in:

```
electronics/vfd/
```

These files define the programmed parameters for spindle drives used by the machines.

---

## Bill of Materials

Machine BOMs are located in:

```
bom/
```

Multiple BOM files may exist if supporting multiple machine variants.

---

## Updating the Repository

To update the configuration(s):

```bash
cd ~/linuxcnc/configs/printnc-machine-config
git pull
```

All configurations inside the repository will be updated together.

---

## Design Goals

This repository structure allows:

* Multiple machine configurations in a single project
* Version-controlled LinuxCNC configurations
* Separation of configuration and documentation
* Clean LinuxCNC integration without system modifications
* Reproducible deployment
* Straightforward updates via Git

---

## Notes

* Each configuration is machine-specific.
* Always verify parameters before operating hardware.
* Ensure Mesa firmware and network configuration match the respective HAL configuration.
