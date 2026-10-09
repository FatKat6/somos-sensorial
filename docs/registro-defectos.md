# Registro de defectos

Defectos encontrados durante las pruebas del prototipo, ordenados por la
prioridad con que se corrigieron. Después de cada corrección se repitió
la prueba que había encontrado el defecto, para confirmar que quedó resuelto
(prueba de regresión).

| ID | Detectado por | Severidad | Defecto | Corrección | Cómo se verificó |
|---|---|---|---|---|---|
| DEF-01 | Prueba manual con datos inválidos | Media | Al reagendar, si el campo `bloque_id` traía texto (ej. `abc`), la página respondía **error 500**. | `agenda/views.py`: se revisa que `bloque_id` sea un número antes de usarlo y se muestra "Elige un horario válido". | Repetir el reagendamiento con un horario inválido: ahora muestra "Elige un horario válido". |
| DEF-02 | OWASP ZAP (escaneo 1) | Media | Faltaba la cabecera **Content-Security-Policy** en todas las páginas. | Librería `django-csp` con una política que solo permite archivos del propio sitio. Bootstrap se guarda dentro del proyecto (no CDN). | Escaneo 2 de OWASP ZAP, ya sin la alerta. |
| DEF-03 | OWASP ZAP (escaneo 1) | Baja | La cookie `csrftoken` no tenía la marca **HttpOnly**. | `CSRF_COOKIE_HTTPONLY = True` en `config/settings.py`. | Escaneo 2 de OWASP ZAP y revisión de la cookie con F12. |
| DEF-04 | Revisión de los logs del servidor al reproducir DEF-01 | Baja | Con DEBUG apagado, los errores 500 **no quedaban registrados** en los logs y la página de error era la genérica en inglés. | Configuración `LOGGING` para `django.request` y plantilla `templates/500.html` en español. | Revisión de los logs del servidor y de la página de error. |
| DEF-05 | Revisión cruzada de código | Baja | El mensaje "Demasiados intentos fallidos" solo aparecía con correos registrados, por lo que permitía adivinar qué correos tienen cuenta (enumeración de usuarios). | `usuarios/auditoria.py`: `demasiados_intentos()` cuenta los LOGIN_FALLIDO de los últimos 15 minutos por correo, exista o no la cuenta. Se quitaron los campos de bloqueo del modelo Usuario. | Repetir el caso CP-06 con un correo que no existe. |

Estado: 5 defectos encontrados, 5 corregidos, 0 abiertos.
