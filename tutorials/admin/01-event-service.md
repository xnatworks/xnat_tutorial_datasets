# Event Service setup

**Level:** Admin · **Time:** 15 min · **Recommended dataset:** any small
DICOM or BIDS project

## Goal

Enable XNAT Event Service on a training site and identify a safe
trigger / action pair before subscribing real automations.

## Preconditions

- Use a disposable training site, or a site where Event Service testing
  is approved. Do **not** practice on production.
- Site administrator account.
- Container Service installed and a tutorial project ready (e.g.
  `tcia_dicom_intro` loaded via
  [intro/01-load-sample-data](../intro/01-load-sample-data.md)).

## Walkthrough

1. Log in as a site admin.
2. Open **Administer → Event Service**.
3. On **Event Setup**, set Event Service to **enabled**.
4. Open **Event Service History**. Review whether any events have
   already been recorded on this site.
5. If the class will use container actions, confirm Container Service is
   installed, the required image is installed, and the command is
   enabled for the tutorial project.
6. Identify one safe pair, for example:
   - Trigger: `imageSession.archived`
   - Action: notify a curator (email or webhook), **not** a heavy
     container.
7. Subscribe with project scope set to the tutorial project only.

## Expected result

Event Service is enabled, history shows recorded events for the site,
and a single tutorial-scoped subscription exists.

## Verify

Trigger the event by archiving a tutorial session
([intro/03-dicom-import-archive](../intro/03-dicom-import-archive.md))
and confirm a new entry appears in Event Service History.

## If it does not work

- **History is empty after a known-good action**: confirm the
  subscription scope matches the project and that the trigger event
  fires for that operation. Some triggers (e.g. session received vs
  archived) fire at different points.
- **Action not available**: container actions need their wrapper enabled
  at project scope before Event Service can call them.

## Cautions

- Avoid subscriptions that launch heavy containers on every archived
  session unless queueing and resource limits are configured.
- Events are global in scope. A site-wide subscription affects every
  project. Use project scope for tutorials.

## Reference

- [Enabling the XNAT Event Service](https://wiki.xnat.org/documentation/enabling-the-xnat-event-service)
- [Launching Containers from Commands](https://wiki.xnat.org/container-service/launching-containers-from-commands)
