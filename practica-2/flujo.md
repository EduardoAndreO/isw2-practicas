# Práctica 2 — GitFlow y trunk-based

1. GitFlow organiza el trabajo alrededor de ramas permanentes como `main` y `develop`.
2. En GitFlow, cada funcionalidad se construye en una rama propia y luego se integra en `develop`.
3. Antes de publicar una versión, una rama `release` permite estabilizarla y corregir detalles.
4. Las ramas `hotfix` sirven para corregir rápidamente problemas urgentes de producción.
5. Este modelo hace explícitas las etapas de desarrollo, preparación de versión y mantenimiento.
6. A cambio, mantener varias ramas largas puede complicar las integraciones y retrasar la entrega.
7. Trunk-based development integra los cambios con frecuencia en una rama principal compartida.
8. Sus ramas de trabajo suelen ser pequeñas y de vida corta para reducir conflictos al integrarlas.
9. Las pruebas automatizadas y la integración continua ayudan a detectar problemas pronto.
10. Las feature flags permiten integrar una función sin activarla para todos los usuarios.
11. Para una aplicación web elegiría trunk-based, porque facilita publicar mejoras pequeñas y frecuentes.
12. La usaría junto con revisiones y pruebas automáticas para mantener estable `main` y evitar regresiones.
