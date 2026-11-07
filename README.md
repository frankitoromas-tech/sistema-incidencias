# Sistema de Incidencias

[![CI Status](https://github.com/frankitoromas-tech/sistema-incidencias/actions/workflows/ci.yml/badge.svg)](https://github.com/frankitoromas-tech/sistema-incidencias/actions/workflows/ci.yml)

Repositorio correspondiente a la **Práctica de Laboratorio N.° 13: Integración Continua con GitHub Actions**, desarrollada para el curso **Herramientas de Desarrollo (100000S66T)** en la **Universidad Tecnológica del Perú (UTP)**.

---

## Información del Proyecto

* **Institución:** Universidad Tecnológica del Perú (UTP)
* **Curso:** Herramientas de Desarrollo (`100000S66T`) — Sección 57524
* **Docente:** Ing. Jeymi Melanie Valdivia Eguiluz
* **Laboratorio:** Práctica N.° 13 — Semana 13, Sesión 25
* **Modalidad:** Parejas (120 min)
* **Integrantes del Equipo:**
  * **Vargas Huamán, Frank Emiliano** (Código UTP: `U23243651`)
  * **More Manrique, Renzo José** (Código UTP: `U22228363`)

---

## Situación Problemática y Propósito

En el marco de la gestión de soporte técnico de una empresa de Tecnologías de la Información y Comunicación (TIC), se desarrolla un sistema para registrar y gestionar incidencias informáticas. Dado que el código experimenta modificaciones frecuentes por parte del equipo, resulta imprescindible contar con un mecanismo automatizado que audite las funciones críticas en cada entrega al repositorio, evitando que errores lógicos o regresiones afecten el funcionamiento del sistema.

El módulo desarrollado en Node.js valida de forma determinista que cada objeto de incidencia cumpla con los requisitos mínimos de negocio antes de ser procesado:
1. **Código identificador:** Texto alfanumérico no vacío (`codigo`).
2. **Descripción del incidente:** Detalle descriptivo no vacío del problema reportado (`descripcion`).
3. **Prioridad estandarizada:** Valor restringido estrictamente al catálogo corporativo: `'alta'`, `'media'` o `'baja'`.

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
└── README.md               # Documentación técnica, integrantes y respuestas del laboratorio
```

---

## Ejecución Local

Para verificar la suite de pruebas unitarias en un entorno local con Node.js (v18 o superior):

```bash
# Ejecutar las pruebas unitarias
npm test
```

Salida esperada de la ejecución:
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

El archivo `.github/workflows/ci.yml` automatiza la verificación continua en la nube:

1. **Disparadores (`on`):** Se activa ante cualquier evento de `push` y en solicitudes `pull_request` a la rama `main`.
2. **Entorno de ejecución:** Contenedor virtual `ubuntu-latest` aprovisionado con Node.js en su versión 22.
3. **Etapas del flujo:**
   * Descarga del repositorio mediante `actions/checkout@v4`.
   * Inicialización del runtime Node.js con `actions/setup-node@v4`.
   * Ejecución de las pruebas unitarias automatizadas con `npm test`.

Historial de ejecuciones disponible en:  
[https://github.com/frankitoromas-tech/sistema-incidencias/actions](https://github.com/frankitoromas-tech/sistema-incidencias/actions)

---

## Procedimiento Realizado (Actividades de Laboratorio)

1. **Estructuración inicial:** Configuración de `package.json` utilizando el test runner nativo de Node.js (`node:test` y `node:assert/strict`), manteniendo el proyecto liviano y sin dependencias externas innecesarias.
2. **Despliegue de CI:** Configuración del pipeline en GitHub Actions y verificación del primer pase exitoso en la nube (Run #1).
3. **Simulación de error controlado (Actividad 4):** Se modificó la lista de prioridades permitidas en `incidencias.js` para aceptar `'urgente'`. Al enviar el commit `c13b442`, el pipeline en GitHub Actions detectó la falla en la prueba `rechaza prioridad desconocida` con `AssertionError: true !== false` y detuvo el proceso de inmediato (Run #2).
4. **Corrección de la falla:** Se restauró el catálogo cerrado de prioridades (`['alta', 'media', 'baja']`) en el commit `d60e246`. Las pruebas se ejecutaron nuevamente de forma automática y alcanzaron un 100% de éxito en 5 segundos (Run #3).

---

## Cuestionario Técnico (Actividad 6)

### 1. ¿Qué evento activa el workflow?
El flujo de trabajo se dispara automáticamente ante los eventos `push` (al enviar nuevos cambios al repositorio) y `pull_request` (al proponer modificaciones mediante solicitudes de fusión). Esto asegura que todo cambio sea auditado antes de consolidarse en la rama principal.

### 2. ¿Qué verifica la prueba automatizada?
La suite `test.js` audita tres reglas de negocio esenciales:
* Que una incidencia con código, descripción y prioridad válida (`'alta'`) sea aceptada correctamente.
* Que se rechacen prioridades no autorizadas por la política de la empresa (como `'urgente'`).
* Que se rechacen incidencias que omitan la descripción o contengan únicamente espacios en blanco.

### 3. ¿Qué diferencia existe entre un commit y una ejecución de Actions?
Un **commit** es un punto de control estático en el historial de Git que registra cambios específicos realizados en los archivos junto con sus metadatos (autor, fecha y mensaje). Por el contrario, una **ejecución de Actions** es un proceso dinámico de cómputo en la nube que toma ese commit, descarga el proyecto en un runner virtual, ejecuta los comandos del flujo (`npm test`) y genera un veredicto de éxito o fallo.

### 4. ¿Qué ventaja ofrece detectar un error antes de integrar más cambios?
Permite identificar fallas en el instante exacto en que se introducen, reduciendo drásticamente el costo y tiempo de depuración. Al bloquear el avance de código defectuoso, se evita que los errores se propaguen hacia la rama principal o afecten a otros colaboradores del equipo.
