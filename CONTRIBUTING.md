# Contributing to Claude TV

Help make **Your TV. Your Rules. No Clutter.** work well for more households. Precise device notes, launcher fixes, translations and documentation improvements are welcome.

Claude TV is an independent derivative of [Gathin's tv-vibehack](https://github.com/Gathin23/tv-vibehack), not affiliated with Anthropic. Preserve original attribution and license notices.

## Work with the existing project

Read the [guide](README.md) and relevant code first. Keep changes focused; documentation and branding changes should not alter runtime behaviour.

- Use **Claude TV** for user-facing project naming.
- Keep `com.gathin.tvhome`, component names, script paths and compatibility identifiers intact.
- Do not change dependencies or SDK versions as part of unrelated work.
- Keep LICENSE, the font license and existing source license notices intact.
- Do not imply official Anthropic affiliation or unverified device support.

## Add a device example

1. Create `examples/<brand>-<model>/debloat.txt`, following an existing list: one package per line, `#` comment groups and inline explanations.
2. Record the model, Android version, firmware, stock launcher package and Live TV package. `tools/tv.py --probe` provides basic device details when you have authorized TV access.
3. Include only packages actually tested disabled. Record broken features, packages re-enabled and personal choices such as casting or streaming apps.
4. Explain how to retain updates, accessibility, input services and other model-specific functions. Do not present a list as universally safe for an entire brand.
5. Add a linked row to [examples/README.md](examples/README.md) and update the main device table if appropriate.

Lists disable packages using `pm disable-user`, never uninstall system packages. Install and verify replacement HOME first. Preserve a baseline and exact list of newly disabled packages so restoration does not enable packages already disabled beforehand.

Installer stock overrides are `STOCK_LAUNCHER` for Bash and `-StockLauncher` for PowerShell. Restore takes the stock package as its second Bash argument or PowerShell `-Stock`. PowerShell may need a separate full HOME component command on some firmware; see [restoration](README.md#debloating-and-restoration).

## Improve the launcher

The launcher uses Java, AndroidX RecyclerView and DynamicAnimation. Keep its modest resource needs and deterministic remote navigation. Avoid advertising feeds, telemetry, video backgrounds and expensive blur effects.

- Put device-specific configuration in [config.xml](launcher/app/src/main/res/values/config.xml).
- Preserve app launching, input switching, focus restoration and Settings access.
- Use Android string resources for localization; default English and German are included.
- Discuss substantial new dependencies or interface changes before implementing them.

The clock screensaver already exists. Build on the current implementation rather than introducing a second one.

## Validate changes

For documentation and label-only edits, review the complete diff, check links/images, parse changed XML and confirm commands against scripts. No TV connection is required.

For local build checks, use JDK 17 and SDK platform 34/build-tools 34.0.0:

```powershell
# Windows, from the project root
Push-Location launcher
./gradlew.bat assembleDebug lint --console=plain
Pop-Location
```

```bash
# macOS / Linux, from the project root
(cd launcher && ./gradlew assembleDebug lint --console=plain)
```

These commands do not contact a TV. The `build-and-install.*` scripts do, and must not be used as build-only checks. Report unavailable JDK, SDK or dependencies instead of claiming a build passed.

For behavioural changes, test only on a TV you are authorized to modify and record model/firmware. Check D-pad edges, scrolling, return focus, HOME selection, inputs and retained features. Remove personal details from screenshots before sharing. Label emulator-only results accurately.

## Improve the prompt and tools

[PROMPT.md](PROMPT.md) should guide assistants to reuse this project and inspect the actual device. Keep model-specific package knowledge in examples. Retain one-command-per-shell-call package operations, authentication, transfer checksums and recovery paths.

Do not promise automatic backups or complete rollback: the scripts implement neither. Document limitations accurately.

## Submit a focused change

- Explain the problem, resulting behaviour and affected files.
- State checks performed and limitations; name a TV only if actually tested.
- Include relevant screenshots for interface changes and before/after behaviour for fixes.
- Exclude `tools/adbkey/`, `backup/`, local SDK paths, signing keys and device identifiers.
- Review the diff, avoid unrelated edits and be considerate in reviews.

Contributions use the project's [MIT License](LICENSE). Outfit retains its separate [SIL Open Font License](launcher/FONT-LICENSE-OFL.txt).
