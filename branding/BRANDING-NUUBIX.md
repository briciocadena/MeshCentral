# Branding Nuubix sobre MeshCentral

Estrategia (igual que con el cliente): **máximo por configuración** (sobrevive updates de
upstream) + **edición de texto VISIBLE** donde "MeshCentral" estaba hardcodeado.
NO se tocan identificadores internos para no romper la app.

## 1. Branding por configuración  → `branding/nuubix-config-sample.json`
Copia ese archivo a `meshcentral-data/config.json` en tu despliegue. Cubre:
- `title` / `title2` → nombre visible "NuubixRemote / Soporte Remoto" en toda la UI.
- `footer`, `loginfooter`, `welcomeText` → textos de marca (en español).
- `nightMode`, `meshMessengerTitle`.
- **Agente** (`agentCustomization`): `displayName=NuubixRemote`, `companyName=Nuubix`,
  `serviceName=NuubixRemoteAgent`, `description`. Esto rebrandea cómo se instala y se ve
  el agente en los equipos (servicio, bandeja) **sin recompilar el binario**.

> Logos: agrega tus imágenes en `meshcentral-data/` y referencia con `titlePicture`,
> `loginPicture`, `welcomePicture` (PNG). No se incluyen aquí porque son tu arte de marca.

## 2. Texto visible rebrandeado en el código (commiteado)
Se reemplazó la palabra suelta `MeshCentral` → `NuubixRemote` SOLO en archivos de UI:
`views/*.handlebars`, `emails/*`, `public/translator.htm`, `translate/translate.json`.

Regex segura usada (palabra suelta, no identificadores):
```bash
perl -i -pe 's/(?<![-\w])MeshCentral(?![\w-])/NuubixRemote/g' <archivos-de-UI>
```

## 3. Lo que se dejó INTACTO a propósito (romperían la app)
- Identificadores de diálogos: `MeshCentralServerUpdate/Config/Errors`.
- Flags de features: `Look-MeshCentral`, `Mode-MeshCentral2`.
- Nombre de paquete npm (`meshcentral`), variables, comentarios, código `.js` del servidor.
- `LICENSE` / atribución (MeshCentral es Apache-2.0; el branding está permitido, la atribución se conserva).

## Repetir tras un update de upstream
1. `git merge` del tag nuevo de MeshCentral (el branding por config no genera conflicto).
2. Re-aplicar la regex del punto 2 sobre los archivos de UI (rápido).
3. Verificar: `curl -sk https://host:4430/ | grep -c MeshCentral` debe dar 0 en la UI.

## Nota de runtime
`meshcentral-data/` está en `.gitignore` (contiene certificados, base de datos y el config
activo con datos sensibles). Nunca se versiona.
