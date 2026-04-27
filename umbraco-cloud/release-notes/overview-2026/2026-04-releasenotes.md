# April 2026

## Key Takeaways

* **Always On toggle for Dedicated plans** - A new Always On toggle is available in the Platform Configuration section. It keeps your application loaded at all times so it does not unload after periods of inactivity.
* **Platform Configuration section renamed** - The Auto-Heal Settings section in the Advanced Configuration has been renamed to Platform Configuration and now groups the Proactive Auto-Heal and Always On toggles.

## Always On toggle for Dedicated plans

The Platform Configuration section now includes an **Always On** toggle alongside the Proactive Auto-Heal toggle. Always On keeps your application loaded, so it does not unload after periods of inactivity. Without Always On, an idle application is unloaded to free resources, and the next incoming request has to wait for it to start again.

Changes to either toggle take effect after you select **Save** and confirm the change in the dialog. The environment restarts to apply the new settings.

For more details, see the [Platform Configuration](../../build-and-customize-your-solution/set-up-your-project/project-settings/platform-configuration.md) documentation.

## Platform Configuration section renamed

The section previously called **Auto-Heal Settings** in **Configuration** > **Advanced** has been renamed to **Platform Configuration**. This section now groups the Proactive Auto-Heal and Always On toggles. Each change is applied together through a single **Save** action and a confirmation dialog.

For more details, see the [Platform Configuration](../../build-and-customize-your-solution/set-up-your-project/project-settings/platform-configuration.md) documentation.
