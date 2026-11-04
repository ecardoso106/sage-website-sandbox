---
sidebar_label: Getting Started
sidebar_position: 2
---

import BrowserOnly from '@docusaurus/BrowserOnly'
import { Button, Chip } from '@mui/material'

# Getting Started

Go from zero to running your first code on a Sandbox node in six steps. Steps 1 to 3 are one-time setup; after that, you only need step 4 to log back in.

{/* Steps 1-3 are based on the Sage Summer Camp 2026 guide:
    https://github.com/waggle-sensor/summer-camp-2026#creating-a-sage-account */}

## 1. Create a Sage account

<BrowserOnly>
  {() => {
    const getUser = require('@site/src/components/getUsername').default;
    return getUser()
      ? <Chip color="success" label={`Done! You are signed in as ${getUser()}`} />
      : (
        <Button href="https://auth.sagecontinuum.org/?callback=https://sagecontinuum.org/docs/sandbox/getting-started" variant="contained">
          <span className="normal-case text-[#fff]">Create an Account</span>
        </Button>
      );
  }}
</BrowserOnly>

1. Go to the [Sage Portal](https://portal.sagecontinuum.org) (or click **Portal** in the top-right corner of [sagecontinuum.org](https://sagecontinuum.org)).
2. Click **Sign In** in the top-right corner, create an account with your school credentials, and fill in the rest of your account details.
3. When your account is created, you land on the Node Status page.

## 2. Request access to the Sandbox

1. In the Portal, open your profile dropdown and select **Request Access**.
2. Choose **Request access to specific nodes or projects**.
3. In step 2 of the form, open the **Projects** dropdown, search for the **UIC Sandbox** project, and select all of its permissions.
4. Fill in section 3, **Project Information**, with a short description of what you plan to do.
5. Submit the request. Approval can take up to 48 hours. Once approved, the Sandbox nodes appear under **My Nodes → My Nodes**.

{/* TODO: confirm the exact project name to search for in the Projects dropdown. */}

:::tip
While you wait, read the [Guidelines](./guidelines) and browse the [Sensors](./sensors) you'll be able to use.
:::

## 3. Set up your SSH credentials

1. In the Portal, open your profile dropdown and click **Access Credentials**.
2. Follow the prompts to generate an SSH key pair on your computer, then upload the **public** key.
3. Follow the steps under **Finish Setup for Node Access**. This adds the Sage settings to your computer's SSH config.

## 4. Log in to a node

Pick any node from the [Nodes](./nodes) page. Then, from a terminal on your computer:

1. Test your connection first. Enter your SSH key passphrase when asked:

   ```bash
   ssh waggle-dev-sshd
   ```

   If it works, you'll see `no command provided` and the connection closes. That's expected.

2. Log in to your node, replacing `H01D` with the node you picked. You'll enter your passphrase twice:

   ```bash
   ssh waggle-dev-node-H01D
   ```

   You should see `We are connecting you to node H01D`.

:::tip[Keep your connection from dropping]

If your session closes with `Broken pipe` after a few idle minutes, add this to the SSH config file on your computer (`~/.ssh/config`):

```
Host *
  TCPKeepAlive yes
  ServerAliveInterval 60
  ServerAliveCountMax 999
```

:::

## 5. Run Hello World

Once you're logged in to a node, create a small Python script:

```bash
cat > hello.py << 'EOF'
import os
import platform
import socket

print(f"Hello from {socket.gethostname()}!")
print(f"Python {platform.python_version()} on {platform.machine()}")
print(f"CPU cores: {os.cpu_count()}")
EOF
```

Run it:

```bash
python3 hello.py
```

You should see a greeting with the node's name, the Python version, `aarch64` (the Thor's Arm processor), and the number of CPU cores.

To see the GPU and how busy it is, run:

```bash
nvidia-smi
```

{/* TODO: test this Hello World and nvidia-smi on a Sandbox node and paste the real output above. */}

## 6. Read from a camera

The Sandbox cameras are on the same network as the nodes, so you can open their video streams directly from your node. See [Sensors](./sensors) for the camera addresses and an example.

## What next?

- See what others have built in [Projects](./projects).
- Learn to package your code as a Sage app with the [Edge Apps tutorials](/docs/tutorials/edge-apps/intro-to-edge-apps).
- Pull data from Sage nodes across the country with the [data API](/docs/tutorials/accessing-data).

---

Stuck? [Get help](./get-help)