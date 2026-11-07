# Sistema de Incidencias

[![CI Status](https://github.com/frankitoromas-tech/sistema-incidencias/actions/workflows/ci.yml/badge.svg)](https://github.com/frankitoromas-tech/sistema-incidencias/actions/workflows/ci.yml)

Repositorio correspondiente a la **Práctica de Laboratorio N.° 13: Integración Continua con GitHub Actions**, desarrollada para el curso **Herramientas de Desarrollo (100000S66T)** en la **Universidad Tecnológica del Perú (UTP)**.

---

## Información del Proyecto

* **Institución:** Universidad Tecnológica del Perú (UTP)
* **Curso:** Herramientas de Desarrollo (`100000S66T`) — Sección 57524
* **Docente:** Ing. Jeymi Melanie Valdivia Eguiluz
* **Apartado en Canvas:** Plan De Trabajo Para Tarea • `vitroglass_system_backend`
* **Integrantes:**
  * Frank Emiliano Vargas Huamán (`U23243651`)
  * Renzo José More Manrique (`U22228363`)

---

## Descripción y Propósito

El proyecto implementa un módulo en Node.js para validar registros de incidencias técnicas en sistemas de soporte de TI. Su objetivo es asegurar que toda incidencia enviada contenga los atributos indispensables antes de ser procesada por el sistema:

* Un código identificador en formato texto no vacío.
* Una descripción clara del problema reportado.
* Una prioridad válida perteneciente al catálogo cerrado: `alta`, `media` o `baja`.

---

## Estructura del Repositorio

```text
sistema-incidencias/
├── .github/
│   └── workflows/
│       └── ci.yml          # Definición del flujo de CI en GitHub Actions
├── incidencias.js          # Lógica de validación de incidencias
├── package.json            # Configuración del proyecto y scripts de ejecución
├── test.js                 # Casos de prueba unitarios con el runner nativo de Node.js
└── README.md               # Documentación técnica y respuestas del laboratorio
```

---

## Ejecución Local

Para reproducir las pruebas en un entorno local con Node.js (v18 o superior):

```bash
# Ejecutar la suite de pruebas unitarias
npm test
```

Salida esperada:
```text
✔ acepta una incidencia válida
✔ rechaza prioridad desconocida
✔ rechaza descripción vacía
ℹ tests 3
ℹ pass 3
ℹ fail 0
```

---

## Flujo de Integración Continua (GitHub Actions)

El archivo `.github/workflows/ci.yml` automatiza la verificación de calidad en la nube:

1. **Disparadores (`on`):** Se ejecuta automáticamente ante eventos `push` a cualquier rama y en solicitudes `pull_request`.
2. **Entorno de ejecución:** Máquina virtual `ubuntu-latest` con Node.js 22.
3. **Pasos del job:**
   * Descarga del código fuente vía `actions/checkout@v4`.
   * Configuración del runtime con `actions/setup-node@v4`.
   * Ejecución de las pruebas unitarias mediante `npm test`.

Historial de ejecuciones disponible en:  
[https://github.com/frankitoromas-tech/sistema-incidencias/actions](https://github.com/frankitoromas-tech/sistema-incidencias/actions)

---

## Procedimiento Realizado (Actividades de Laboratorio)

1. **Configuración inicial:** Creación del proyecto Node.js sin dependencias externas pesadas, aprovechando `node:test` y `node:assert/strict`.
2. **Pipeline de CI:** Configuración del workflow de GitHub Actions y comprobación del primer pase exitoso en la nube (Run #1).
3. **Simulación de error controlado (Actividad 4):** Se modificó la lista de prioridades válidas en `incidencias.js` para admitir `'urgente'`. Al hacer `push` (commit `c13b442`), la prueba `rechaza prioridad desconocida` falló de inmediato con `AssertionError: true !== false`, deteniendo el workflow en GitHub Actions (Run #2).
4. **Corrección de la regresión:** Se restauró el catálogo cerrado `['alta', 'media', 'baja']` en el commit `d60e246`. Las pruebas se ejecutaron nuevamente y finalizaron con éxito en 5 segundos (Run #3).

---

## Cuestionario Técnico (Actividad 6)

### 1. ¿Qué evento activa el workflow?
El flujo se activa mediante los eventos `push` (al enviar nuevos commits al repositorio) y `pull_request` (al proponer cambios hacia ramas del proyecto). Esto garantiza que cualquier cambio en el código sea evaluado antes de considerarse listo para su integración.

### 2. ¿Qué verifica la prueba automatizada?
La suite `test.js` comprueba tres condiciones de negocio:
* Que una incidencia con código, descripción y prioridad válida sea aceptada correctamente.
* Que se rechacen prioridades no estandarizadas (por ejemplo, `urgente`).
* Que se rechacen incidencias cuya descripción esté vacía o compuesta solo de espacios en blanco.

### 3. ¿Qué diferencia existe entre un commit y una ejecución de Actions?
Un **commit** es un registro estático en el historial de Git que almacena los cambios realizados sobre los archivos en un momento determinado. Por otro lado, una **ejecución de Actions** es un proceso dinámico en la nube que toma ese commit, levanta un runner virtual, ejecuta los comandos definidos en el flujo (como `npm test`) y genera un veredicto de éxito o fallo.

### 4. ¿Qué ventaja ofrece detectar un error antes de integrar más cambios?
Permite identificar fallas de manera inmediata en el momento en que se introducen, evitando que código con errores llegue a ramas principales o a producción. Esto reduce drásticamente el tiempo de depuración, ya que el desarrollador conoce exactamente qué commit causó el problema y puede corregirlo antes de que afecte el trabajo de los demás integrantes del equipo.
