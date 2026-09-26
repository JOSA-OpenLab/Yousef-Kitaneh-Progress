### Weekly Journal: 9/20/2026 - 9/26/2026

* **Environment Setup:** Installed Zephyr RTOS, the Renode framework, and all required toolchains and dependencies.
* **Build & Simulation:** Compiled the Zephyr OpenTitan `hello_world` application. Created a custom Renode platform and initialization script to execute the resulting ELF binary.
* **Driver Development:** Began developing the Zephyr GPIO driver for the OpenTitan platform. Currently executing the hardware description phase to define the hardware components and interaction interfaces (DeviceTree mapping).
* **Upstream Contributions:** Identified two bugs in the Renode UI. Successfully reproduced and documented one, opening the first-ever issue on the `renode-ui` repository ([Issue #1](https://github.com/antmicro/renode-ui/issues/1)).