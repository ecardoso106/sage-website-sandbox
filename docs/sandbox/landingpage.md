---
title: UIC Sandbox
sidebar_label: Overview
sidebar_position: 0
---

import BrowserOnly from '@docusaurus/BrowserOnly'
import { Button, Chip } from '@mui/material'
import Card from '@site/src/components/Card'

# UIC Sandbox

A hands-on Sage cluster at UIC for learning, testing and breaking things safely. Anyone with a Sage account can log in to one of 10 NVIDIA Jetson Thor blades, run AI code, and read live data from shared cameras and a weather station.

<BrowserOnly>
  {() => {
    const getUser = require('@site/src/components/getUsername').default;
    return getUser()
      ? <Chip color="success" label={`Signed in as ${getUser()}`} />
      : <Chip variant="outlined" label="Not signed in" component="a" href="/docs/sandbox/getting-started" clickable />;
  }}
</BrowserOnly>

<div className="my-6">
  <Button href="/docs/sandbox/getting-started" variant="contained" size="large">
    <span className="normal-case text-[#fff]">Start here</span>
  </Button>
</div>

## Explore the Sandbox

<div className="grid grid-cols-1 md:grid-cols-3 gap-4">
  {[
    ['about', 'About', 'What the Sandbox is and why you would use it.'],
    ['getting-started', 'Getting Started', 'Create an account, add your SSH key, and run your first code.'],
    ['nodes', 'Nodes', 'The 10 Thor blades and how to pick one.'],
    ['sensors', 'Sensors', 'Cameras and a weather station on the lab network.'],
    ['projects', 'Projects', 'What students have built on Sage blades.'],
    ['guidelines', 'Guidelines', 'How to share the Sandbox fairly.'],
    ['faq', 'FAQ', 'Common questions about access, nodes, and sensors.'],
    ['get-help', 'Get Help', 'Where to ask when you are stuck.'],
  ].map(([slug, title, desc]) => (
    <Card key={slug} to={`/docs/sandbox/${slug}`} className="block p-4">
      <h3 className="mb-1">{title}</h3>
      <p className="mb-0">{desc}</p>
    </Card>
  ))}
</div>

## Coming soon

<Chip label="Coming soon" size="small" /> **Live node status:** see which nodes are free, from the sage-gui dashboard.

---

Stuck? [Get help](./get-help)