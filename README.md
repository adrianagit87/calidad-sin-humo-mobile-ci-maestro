# calidad-sin-humo-mobile-ci-maestro

Demo de **Maestro CLI corriendo en CI con GitHub Actions**. Companion oficial de la guía Parte 2 de la serie Mobile Testing en [calidadsinhumo.com](https://calidadsinhumo.com).

## Qué hace este repo

Levanta un emulador Android **headless** en GitHub Actions, instala la APK de Wikipedia (vía `maestro download-samples` — sin commitear ningún binario), corre los flows de Maestro en `.maestro/`, y sube screenshots como artifacts si algún test falla.

Si forkeas este repo, en tu próximo push GitHub Actions corre Maestro contra Wikipedia y te devuelve verde o rojo en ~5 minutos. **Cero setup local.**

## Cómo correrlo local

Pre-requisito: tener Maestro CLI instalado. Si no lo tenés, leé la [Parte 1 de la guía](https://calidadsinhumo.com/guias/tu-primer-test-mobile-en-30-minutos).

```bash
# 1. Bajá el APK de Wikipedia (~30 MB)
maestro download-samples

# 2. Asegurate de tener un emulador corriendo
maestro start-device --platform=android

# 3. Instalá la APK
adb install samples/wikipedia.apk

# 4. Corré los flows
maestro test .maestro/
```

## Cómo corre en CI

Cada push a `main` y cada pull request disparan `.github/workflows/maestro.yml`. El workflow:

1. Spin up emulador Android API 33 headless en runner Ubuntu
2. Instala Maestro CLI
3. Descarga samples (incluye Wikipedia APK)
4. Instala la APK en el emulador
5. Corre todos los flows en `.maestro/`
6. Si algún test falla, sube los screenshots como artifact descargable

## Flows incluidos

| Flow | Qué hace | Cuándo se usa |
| --- | --- | --- |
| `.maestro/01-smoke.yaml` | `launchApp` + screenshot | Smoke test mínimo — valida que toda la infra funciona |

## De acá en adelante

La guía Parte 2 cubre cómo expandir este setup:

- Agregar flows más complejos (búsqueda, scroll, asserts)
- Triggers `on push`, `on pull_request`, `scheduled` (nightly smoke)
- Artifacts cuando un test falla (screenshots + video)
- Cuándo conviene Maestro Cloud (paid) vs runners propios

## Licencia

MIT — fork-friendly.
