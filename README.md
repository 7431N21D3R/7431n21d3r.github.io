# 7H31N51D3R · blog

Blog personal con write-ups de máquinas retiradas, recursos de auditoría y notas de seguridad ofensiva y GRC. Hecho con [Astro](https://astro.build) y publicado en GitHub Pages.

## Desarrollo

```bash
npm install
npm run dev      # http://localhost:4321
npm run build    # genera dist/
```

## Añadir un write-up

Crea `src/pages/posts/<maquina>.md` copiando la cabecera de `cap.md` (título, fecha, SO, dificultad y etiquetas). Aparecerá solo en la portada, en Write-ups y en Etiquetas.

Los datos del perfil, certificaciones, recursos y trayectoria están en `src/data/site.js`.

Cada push a `main` publica la web con GitHub Actions.
