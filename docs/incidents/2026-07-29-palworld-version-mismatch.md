# Incident Report: Palworld Client–Server Version Mismatch

**Date:** July 29, 2026  
**Status:** Resolved

## Summary

A Palworld client update caused a version mismatch between players' game clients and the dedicated server. Players were unable to connect until the server was updated.

## Impact

Players could not join the server during the version mismatch. The issue was reported while I was away from home and at work.

## Detection

The issue was detected through player reports in Discord rather than automated monitoring.

## Response

Using Tailscale for remote access to my server, I restarted the Palworld Docker container. The restart updated the server and restored version compatibility, allowing players to reconnect.

## Root Cause

The dedicated server did not automatically update after the Palworld client update. This created a client–server version mismatch that prevented connections.

## Follow-Up Work

This incident prompted improvements to the server's operational workflow, including:

- Automated backups and update checks
- Player warnings before planned maintenance
- Scheduled nightly restarts
- A private dashboard for server administration
- Continued work on monitoring and service visibility
