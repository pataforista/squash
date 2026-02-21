# Auditoría de preparación PWA — Squash Solo Pro

## Resultado ejecutivo

**Estado actual: NO lista como PWA de producción.**

La app tiene buena lógica offline en memoria/localStorage y UX sólida para entrenamiento individual, pero **no cumple criterios mínimos de PWA instalable y resiliente offline**.

## Hallazgos clave

### 1) Bloqueantes de PWA (críticos)

- **Falta manifiesto web (`manifest.webmanifest`)** y por tanto no hay `name/short_name/icons/start_url/display` para instalación.
- **No existe registro de Service Worker** ni estrategia de caché para shell/app-data.
- **Sin iconos de app** (`192x192`, `512x512`, maskable) y sin metadatos Apple (`apple-touch-icon`) para instalación móvil.

### 2) Riesgos funcionales

- El selector de notificaciones llama directamente `Notification.requestPermission()` sin guardas de compatibilidad (`"Notification" in window`), lo que puede romper navegadores no compatibles.
- Dependencia de `@import` a Google Fonts puede degradar experiencia offline inicial si no hay caché previa.
- Persistencia únicamente con `localStorage`; útil para prototipo, pero limitada para datasets más grandes y sin versión/esquema de migración.

### 3) Lo que sí está bien

- UI responsive con `viewport-fit=cover` y áreas seguras.
- Persistencia de estado de sesión e historial con `localStorage`.
- Mecanismo de `Wake Lock` para sesiones activas (con verificación de feature).
- Temporizadores, auto-avance y exportación CSV funcionales en cliente.

## Checklist de readiness PWA

- [ ] Manifest válido enlazado desde `<head>`.
- [ ] Service Worker registrado y activo.
- [ ] Estrategia offline (precache shell + runtime cache).
- [ ] Íconos instalables (`192`, `512`, maskable).
- [ ] Pantalla offline/fallback.
- [ ] Soporte de actualización controlada (`skipWaiting`/`clientsClaim` o equivalente).
- [ ] Manejo defensivo de APIs opcionales (`Notification`, `WakeLock`, etc.).
- [ ] Auditoría Lighthouse PWA >= 90 en móvil.

## Recomendación de salida a producción

1. Implementar manifiesto + iconos.
2. Implementar Service Worker (Workbox o vanilla) con caché de HTML/CSS/JS + fuentes.
3. Añadir fallback offline y degradación elegante para APIs no soportadas.
4. Ejecutar pruebas en Android Chrome y Safari iOS (instalación, reanudación de sesión, notificaciones, timers en background).
5. Definir versión de almacenamiento y rutina de migración de estado.

---

Si quieres, en el siguiente paso puedo dejarla **realmente instalable** con:
- `manifest.webmanifest`
- `sw.js`
- registro de SW en la app
- set inicial de iconos
- ajustes de compatibilidad para notificaciones
