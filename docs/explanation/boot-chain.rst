.. _boot-chain:

The Jetson boot chain
=====================

Jetson extends the standard arm64 UEFI Secure Boot chain with an NVIDIA-specific multi-stage
early-boot sequence that runs before UEFI hands off to the OS loader.

The stage sequence
------------------

The full chain is:

**BootROM → MB1 (BPMP) → MB2 (CCPLEX) → TF-A / OP-TEE → UEFI → shim → GRUB → Linux kernel**

BootROM, MB1, and MB2 run on the BPMP and CCPLEX micro-controllers before any general-purpose
code executes. They perform SoC initialization, SDRAM training, security configuration, and
chip/board detection.

TF-A (running as BL31) loads OP-TEE as the Trusted OS. OP-TEE includes a TPM Trusted Application
(TA) that the Jetson measured-boot and full-disk encryption flows rely on.

UEFI on Jetson is built from NVIDIA's fork of EDK2 and presents a standard UEFI interface to the
OS-loader pair. From ``shim`` onward the chain matches the standard Ubuntu arm64 UEFI flow.

Firmware and device tree are stored together in the QSPI flash on the System-on-Module (SoM).

Secure Boot key ownership
-------------------------

Unlike many platforms, Secure Boot key ownership on Jetson is entirely user-controlled. PK, KEK,
and db are provisioned by the end user, not by NVIDIA or the board OEM. This means you control
the trust anchor for your deployment.

Further reading
---------------

- `NVIDIA Jetson Orin boot architecture`_
- :doc:`/how-to/build-boot-firmware` — rebuild QSPI firmware
- :doc:`/how-to/flash` — write firmware and OS image to the board
- :doc:`/how-to/secure-boot` — provision PK, KEK, and db

.. _NVIDIA Jetson Orin boot architecture: https://docs.nvidia.com/jetson/archives/r36.4.3/DeveloperGuide/AR/BootArchitecture/JetsonOrinSeriesBootFlow.html
