# From Fixed to Unfixed.. Triggered Address Space Layout Re-randomization for Kernel Code Based on Isolation
I-KASLR's source code

Get the 5.10 version of the kernel: https://www.kernel.org/pub/linux/kernel/v5.x/linux-5.10.tar.xz

Unpack linux-5.10.tar.xz

cd to linux repo and apply the patch to kernel:

```
> patch -p1 < ../0001-1.patch
```

## Kconfig Settings to Enable Compile Plugin

### GCC_PLUGINS: Controls whether to compile GCC plugins

GCC_PLUGIN_TRAMPOLINE: This option is only available when `GCC_PLUGINS` is enabled. It can be enabled only if `ARM64_RANDOM_MODULE` is enabled (or another future switch for overall randomization control. It controls whether to compile the trampoline plugin.

ENABLE_TRAMPOLINE_PLUGIN: This is a compilation option defined in scripts/Makefile.gcc-plugins (similar to options like -O1 -O2 -ffunction-sections, but not a Kconfig option. It will have content only if `GCC_PLUGIN_TRAMPOLINE` is enabled, otherwise it is empty. This compilation option includes using the trampoline plugin, skipping certain optimization passes, and enabling -ffunction-sections.

### SAMPLES: Controls whether to compile some sample kernel code.

SAMPLE_RERANDOMIZABLE_LKM: Controls whether to compile the re-randomizable kernel module test code, i.e., test_driver. This kernel module can only be compiled when `ARM64_RANDOM_MODULE` is enabled. Only when `ARM64_RANDOM_MODULE` is enabled can `GCC_PLUGIN_TRAMPOLINE` be enabled, and the `ENABLE_TRAMPOLINE_PLUGIN` compilation option will contain content. The corresponding Makefile for `SAMPLE_RERANDOMIZABLE_LKM` uses `ENABLE_TRAMPOLINE_PLUGIN` to control whether to compile it as a re-randomizable kernel module.
