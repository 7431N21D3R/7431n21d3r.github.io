---
layout: ../../layouts/Post.astro
title: Cap
description: IDOR en la descarga de capturas de red, credenciales FTP en claro y escalada con cap_setuid en Python.
date: 2026-11-24
platform: Hack The Box
os: Linux
difficulty: Easy
tags: [idor, pcap, ftp, capabilities, linux]
---

## Resumen

| Fase | Técnica |
|---|---|
| Acceso inicial | IDOR en la descarga de capturas PCAP, credenciales FTP en claro y SSH |
| Escalada | `cap_setuid` sobre el intérprete de Python |

## Enumeración

Servicios expuestos: **FTP** (vsftpd 3.0.3), **SSH** (OpenSSH 8.2) y **HTTP** (Gunicorn con Flask, un "Security Dashboard").

La aplicación genera capturas de red y las sirve en `/data/<id>` y `/download/<id>`, con identificadores secuenciales.

## Acceso inicial

1. Las rutas `/download/<id>` no comprueban a quién pertenece la captura (**IDOR**): pidiendo el ID 0 se descarga una captura de otro usuario.
2. En la captura aparece una sesión **FTP** completa; como FTP no cifra, usuario y contraseña viajan en claro.
3. Esas credenciales se reutilizan en **SSH**: shell como usuario y `user.txt`.

## Escalada de privilegios

1. `getcap -r / 2>/dev/null` muestra que el binario de Python tiene la *capability* `cap_setuid=ep`.
2. Con ella, Python puede cambiar su UID a 0 sin ser root (`os.setuid(0)`) y lanzar una shell como root.

## Lecciones y mitigaciones

| Problema | Mitigación | ENS |
|---|---|---|
| IDOR en descargas | Comprobar la autorización por objeto, no solo por ruta | op.acc.2 |
| Protocolos en claro (FTP) | Sustituir por SFTP o FTPS | mp.com.2 |
| Reutilización de contraseñas | Credenciales distintas por servicio | op.acc.6 |
| Capabilities peligrosas | Auditar `getcap` periódicamente; nunca `cap_setuid` en intérpretes | op.exp.2 |
| App web ejecutándose como root | Usuario de servicio sin privilegios | op.exp.2 |
