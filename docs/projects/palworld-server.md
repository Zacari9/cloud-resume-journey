# Dedicated Palworld Server

**Status:** Active / evolving  
**Environment:** HP EliteDesk home lab, ZimaOS, Docker

## Overview

I deployed and operate a dedicated Palworld server from my home lab for a small community of friends. This project gives me a practical way to build experience with Docker, Linux-based administration, networking, service reliability, and documentation.

## What I Built

- Deployed the dedicated server as a Docker container on ZimaOS
- Configured router port forwarding and verified external player connectivity
- Set up Tailscale and SSH for remote administration
- Added automated backups, update checks, player warnings, and scheduled nightly restarts
- Built a private dashboard container that communicates with the server through a Docker network and REST API
- Established a private Docker network so the dashboard and server containers can communicate internally

## Current Results

The server has run reliably with approximately 6–9 concurrent players without noticeable performance or network issues.

## In Progress

This project is still evolving. Current areas of focus include refining the management dashboard, improving monitoring, continue adding QoL features for server management, and continuing to document operational decisions and troubleshooting.
