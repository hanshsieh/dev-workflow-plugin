---
name: create-devcontainer
description: Create or update devcontainer config for a project. Use when user asks to create dev container config.
disable-model-invocation: true
---

## Purpose

Create a project dev container configuration that is reproducible and ready for local development.

## Parameters

- `understand` (boolean, default: `true`): Whether to clone the `Understand-Anything` repo

## When To Use

Use this skill when the user asks to:
- create or update `.devcontainer/Dockerfile`
- create or update `.devcontainer/devcontainer.json`
- configure a development container for this project

## Required Outcomes

1. Add or update `.devcontainer/Dockerfile`.
2. Add or update `.devcontainer/devcontainer.json`.
3. Ensure all required packages and repository setup are included.
4. Verify Dockerfile by creating a container.
5. Remove verification container and image after verification.

## Dockerfile Requirements

In addition to project-specific setup, include all of the following:

- When `understand=true`
  - Clone repos below to `~/.cursor/plugins/local`
    - `https://github.com/Lum1104/Understand-Anything.git`
    - `https://github.com/hanshsieh/dev-workflow-plugin.git`
  - `ln -sfn /home/${USERNAME}/.cursor/plugins/local/Understand-Anything/understand-anything-plugin /home/${USERNAME}/.understand-anything-plugin`
- Install:
  - Python `3.9+`
  - Node.js `24+`
  - `pnpm`
  - `gh`
  - `vim`
- Run:
  - `git config --global core.editor vim`
  - `git config --global safe.directory /workspace`

## devcontainer.json Requirements

Configure `postStartCommand` with project-appropriate setup, such as:
- dependency install (`npm install` or `pnpm install` based on repo)
- lightweight checks needed on container start

## Verification Workflow

After writing files:

1. Build the image from `.devcontainer/Dockerfile`.
2. Run a disposable container from that image.
3. Verify required tools/versions inside container
4. Verify logged in user
6. Remove verification container.
7. Remove verification image.
