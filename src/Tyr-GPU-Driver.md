# Tyr GPU Driver

## What is Tyr?

Tyr is a new Rust-based DRM driver for CSF-based Arm Mali GPUs. It is a port of
Panthor — a driver written in C for the same hardware — and written as a joint
effort between Collabora, Arm and Google engineers.

Tyr aims to eventually implement the same userspace API offered by Panthor for
compatibility reasons, so that it can be used as a drop-in replacement in our
Vulkan driver, called
[PanVK](https://gitlab.freedesktop.org/mesa/mesa/-/tree/main/src/panfrost/vulkan?ref_type=heads).
In any case, we foresee Panthor being used — and of course supported — for a
relatively long time, as it is a mature driver with a large adoption in the
ecosystem. It will probably take a couple of years for Tyr to fully pick up.

## Where is Tyr developed?

The development of Tyr takes place both upstream, through our [latest
submission](https://lore.kernel.org/rust-for-linux/20250627-tyr-v1-1-cb5f4c6ced46@collabora.com/)
— and downstream, on the `tyr-next` branch at the [Panfrost Gitlab
repository](https://gitlab.freedesktop.org/panfrost/linux).

This split is unfortunately necessary as we do not have the required
infrastructure in upstream yet, although our plan is to eventually migrate to
an upstream-only development model once this changes.

We go into more details about why we chose to develop Tyr this way on our
series of [blog
posts](https://www.collabora.com/news-and-blog/news-and-events/introducing-tyr-a-new-rust-drm-driver.html)
at [Collabora's blog](https://www.collabora.com/news-and-blog/). Anyone willing
to get acquainted with Mali's open source stack should refer to that, as we
will be covering the whole infrastructure from a simple Vulkan application to
the actual GPU hardware in Mali's CSF architecture. We will also cover the
various components needed to write a driver, as well as the status of the
abstractions needed to interact with them from Rust code.

As it currently stands, our downstream branch can be used to test the
abstractions that are still being developed. It makes sure that we can write a
functional driver with the abstractions that are currently being proposed.

## What is the current status of the driver?

The current upstream submission can power up the GPU and probe the device on an
RK3588 system-on-chip. This lets us read a few sections of ROM in the GPU,
which in turn lets us provide this information to userspace by means of a
`DRM_IOCTL_PANTHOR_DEV_QUERY` call.

This is all that can be done for now in upstream code, at least until the Micro
Controller Unit can be made to work.

Our downstream branch (`tyr-next`) can submit small parcels of work to the GPU,
and we will soon be able to submit more elaborate workflows. We hope to see
[VkCube](https://github.com/KhronosGroup/Vulkan-Tools) running on Tyr soon.

In any case, there is no power management and little error recovery. We will be
working on that in the coming months.

## Can I try it out?

Anyone with a RK3588 SoC can test Tyr, but the driver is not capable of
replacing Panthor yet. A good candidate device is Radxa's
[ROCK 5B](https://radxa.com/products/rock5/5b/) Single Board Computer.

A good starting point is to run our [IGT
tests](https://gitlab.freedesktop.org/dwlsalmeida/igt-gpu-tools/-/tree/panthor?ref_type=heads).
While only a subset of the tests pass on the upstream code for the reasons
highlighted above, they should all pass if run on `tyr-next`.

Note that Mali GPUs are found in a vast array of devices, and that we will
support more hardware as we progress in the implementation.

## Contributing

Tyr is open-source software, and as such, anyone interested in its development
can check our [issue
board](https://gitlab.freedesktop.org/panfrost/linux/-/issues/?label_name%5B%5D=tyr).
We will be posting good starting tasks at a future point.

To work on any given task, assign it to yourself and follow up with a merge
request against `tyr-next`. Please also write the IGT tests needed to ensure that
your code works.

Happy hacking!
