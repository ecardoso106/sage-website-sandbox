---
sidebar_label: Nodes
sidebar_position: 3
---

# Nodes

The Sandbox has 10 identical Thor blades. Any one of them will work for learning and testing.

![Inside a Sage Thor blade](../events/img/thor-blade.jpg)

## The Sandbox nodes

Each node has a Sage ID (its VSN). You use the VSN to log in with SSH and to find the node in the [Sage Portal](https://portal.sagecontinuum.org/nodes).

| VSN | Lab name | SSH command | Portal |
| --- | --- | --- | --- |
| H01D | UIC02 | `ssh waggle-dev-node-H01D` | [H01D](https://portal.sagecontinuum.org/nodes/H01D) |
| H01E | UIC03 | `ssh waggle-dev-node-H01E` | [H01E](https://portal.sagecontinuum.org/nodes/H01E) |
| H01F | UIC04 | `ssh waggle-dev-node-H01F` | [H01F](https://portal.sagecontinuum.org/nodes/H01F) |
| H020 | UIC05 | `ssh waggle-dev-node-H020` | [H020](https://portal.sagecontinuum.org/nodes/H020) |
| H021 | UIC06 | `ssh waggle-dev-node-H021` | [H021](https://portal.sagecontinuum.org/nodes/H021) |
| H022 | UIC07 | `ssh waggle-dev-node-H022` | [H022](https://portal.sagecontinuum.org/nodes/H022) |
| H023 | UIC08 | `ssh waggle-dev-node-H023` | [H023](https://portal.sagecontinuum.org/nodes/H023) |
| H039 | | `ssh waggle-dev-node-H039` | [H039](https://portal.sagecontinuum.org/nodes/H039) |
| H041 | | `ssh waggle-dev-node-H041` | [H041](https://portal.sagecontinuum.org/nodes/H041) |
| H043 | | `ssh waggle-dev-node-H043` | [H043](https://portal.sagecontinuum.org/nodes/H043) |

You need an approved account and SSH keys before these commands work. See [Getting Started](./getting-started).

## Hardware

Every Sandbox node is a Sage Grande Testbed **Thor Blade DevKit** node: a 2U rack-mounted blade built around an NVIDIA Jetson Thor.

:::note[Thor Blade at a glance]

- **Computer:** NVIDIA Jetson AGX Thor (T5000 module)
- **CPU:** 14-core Arm Neoverse-V3AE
- **GPU:** 2560-core NVIDIA Blackwell, with 96 Tensor Cores
- **Memory:** 128 GB, shared by the CPU and GPU
- **Storage:** 4 TB NVMe
- **Networking:** a built-in router; the blade runs its own private network for local sensors
- **Power:** about 80 W on average, up to 150 W at peak; a network-controlled power unit lets the Sage team restart it remotely

:::

The DevKit version is meant for "rapid prototyping, model bring-up, and educational AI exploration." Read more about it on the [Next-Gen Nodes page](/labs/next-gen-nodes), or in the [Thor Blade deployment guide (PDF)](../manuals/SGT_Manual_ThorDev_Blade_V1.0.pdf).

## How to pick a node

There is no booking system. For now:

- **Pick any node.** A random pick is fine.
- **Spread out.** If you're in a class or group, have each person start on a different node.
- **Check before you start something big.** After you log in, run `who` to see who else is logged in, and `top` to see how busy the node is. If it's busy, try another one.

:::caution[Your files stay on one node]

Your home directory lives on the node you logged in to. If you switch to another node, your files won't be there. Stick to one node for a project, or copy your files with `scp`.

:::

---

Stuck? [Get help](./get-help)