# CianovaLauncher — Repositorio oficial de Flatpak

> **Repositorio binario de Flatpak** para [CianovaLauncher](https://github.com/CianovaTeam/CianovaLauncher-mcpelauncher)
> (GUI para Minecraft Bedrock Edition en Linux sobre el motor
> [mcpelauncher-manifest](https://github.com/minecraft-linux/mcpelauncher-manifest)). GPL v3.

| Artefacto | Ubicación pública (GitHub Pages) |
|-----------|----------------------------------|
| Repositorio binario (`app/`, deltas, summary firmado) | `https://cianovateam.github.io/flatpak-app/repo_dist` |
| Archivo de repo (añadir remote) | `https://cianovateam.github.io/flatpak-app/CianovaLauncher.flatpakrepo` |
| Metadatos de versión / hotfix | `https://cianovateam.github.io/flatpak-app/version.json` |
| Clave pública GPG (firma del summary) | `public.gpg` — fingerprint `3B72E507 88CA7323 36B40855 E4220487 186BAA61` |

## Instalación (usuarios)

```bash
# Express (un solo comando):
curl -fsSL https://cianovalauncher.org/install.sh | bash

# O añadir el repositorio manualmente:
flatpak remote-add --user --if-not-exists CianovaLauncher \
  https://cianovateam.github.io/flatpak-app/CianovaLauncher.flatpakrepo

# Runtimes necesarios:
flatpak install org.kde.Platform//6.10 io.qt.qtwebengine.BaseApp//6.10

# Instalar el launcher:
flatpak install --user CianovaLauncher org.cianova.Launcher
```

## Estructura

```
flatpak-app/
├── CianovaLauncher.flatpakrepo   ← archivo de repo (Url, GPGKey inline)
├── version.json                  ← { latest_version, prerelease, hotfix? } para update-checker
├── public.gpg                    ← clave GPG que firma repo_dist
├── icon.png
├── .nojekyll                     ← desactiva Jekyll (sirve binarios tal cual)
└── repo_dist/                    ← repositorio binario flatpak (deploy del CI)
```

## Publicación

Los deploys a este repositorio llegan **exclusivamente desde el workflow Release**
de [CianovaLauncher-mcpelauncher](https://github.com/CianovaTeam/CianovaLauncher-mcpelauncher)
(job `release`, paso `Deploy flatpak repo to flatpak-app`), que:
1. Compila el bundle (PyInstaller) + importa la GPG key,
2. Construye y firma `repo_dist` con `flatpak-builder` (runtime KDE 6.10 + qtwebengine BaseApp),
3. Publica `repo_dist/` aquí con `peaceiris/actions-gh-pages` usando el secret `FLATPAK_APP_TOKEN`,
4. Actualiza `version.json` (dual con el espejo legacy `cianovalauncher.org`).

> ⚠️ No editar `repo_dist/` a mano: los commits manuales se sobrescribirán en el próximo release.
> Para ajustar `version.json`/`.flatpakrepo` sí se puede editar directamente en `main`.

## Nota de migración

Transferido desde la rama `gh-pages` del launcher el 2026-10-29 (commit huérfano
`fa751627`, build `a5012c4` del 2026-09-16 — la misma versión en curso en esos momentos).
ADR de referencia: `Docs/adr/0007-repositorio-flatpak-dedicado.md` del repo del launcher.

## Licencias

Binarios del launcher: GPL v3 · Motor mcpelauncher: GPL v3 (proyecto independiente).
