---
sidebar_label: FAQ
sidebar_position: 7
---

# FAQ

Answers to common questions about access, nodes, and sensors.

## Access

<details>
<summary>Who can use the Sandbox?</summary>

Anyone with a Sage account. Create an account and request Sandbox access by following [Getting Started](./getting-started).

</details>

<details>
<summary>How long does access approval take?</summary>

Up to 48 hours. Once approved, the Sandbox nodes appear in the Portal under **My Nodes → My Nodes**. If it's been longer, [contact us](./get-help).

</details>

<details>
<summary>SSH says "Permission denied". What now?</summary>

Check these in order:

1. Your access request has been approved (the nodes show under **My Nodes** in the Portal).
2. You uploaded your **public** SSH key under **Access Credentials** in the Portal.
3. You completed **Finish Setup for Node Access**, which adds the Sage settings to your SSH config.
4. `ssh waggle-dev-sshd` works. If it doesn't, the problem is your keys or setup, not the node.

See steps 3 and 4 of [Getting Started](./getting-started).

</details>

<details>
<summary>My SSH session closes with "Broken pipe". How do I fix it?</summary>

Idle sessions close after about 10 to 15 minutes. Add the keep-alive settings from [Getting Started, step 4](./getting-started#4-log-in-to-a-node) to your SSH config file.

</details>

## Using nodes

<details>
<summary>Which node should I use?</summary>

Any of them. All 10 Sandbox nodes are identical. Pick one from the [Nodes](./nodes) page, and if it's busy (check with `who` and `top`), try another.

</details>

<details>
<summary>My files are gone after switching nodes. Why?</summary>

Your home directory lives on each node separately, so files you create on one node aren't on the others. Stick to one node for a project, or copy files between nodes with `scp`.

</details>

<details>
<summary>Is the Sandbox the same as Sage field nodes?</summary>

They run the same Sage software, but the Sandbox nodes are in a lab at UIC for learning and testing. Field nodes collect real science data outdoors. In the Sandbox, every node can reach every shared camera; in the field, each camera belongs to one node. See [About](./about#sandbox-vs-field-nodes).

</details>

## Sensors

<details>
<summary>How do I get video from a camera?</summary>

The cameras stream over RTSP on the same network as the nodes, and no login is needed. See [Sensors](./sensors#grab-a-frame) for a command-line and a Python example.

</details>

---

Stuck? [Get help](./get-help)