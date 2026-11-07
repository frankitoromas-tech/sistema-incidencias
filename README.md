# Sistema de Gestión de Incidencias Informáticas

[![Pruebas de incidencias](https://github.com/frankitoromas-tech/sistema-incidencias/actions/workflows/ci.yml/badge.svg)](https://github.com/frankitoromas-tech/sistema-incidencias/actions/workflows/ci.yml)

> **Práctica de Laboratorio N.° 13: Integración Continua con GitHub Actions**  
> **Curso:** Herramientas de Desarrollo (100000S66T) — Semana 13, Sesión 25  
> **Institución:** Universidad Tecnológica del Perú (UTP)  

---

## 👥 Integrantes del Equipo

* **Vargas Huamán, Frank Emiliano** — Código: `U23243651`
* **More Manrique, Renzo José** — Código: `U22228363`

---

## 🎯 Propósito del Proyecto

El proyecto **`sistema-incidencias`** es un módulo ligero y robusto desarrollado en Node.js que implementa la validación centralizada de requerimientos e incidentes informáticos para una empresa de servicios TIC. Su función principal es asegurar la integridad referencial y de negocio de cada ticket antes de su inserción en la base de datos o su enrutamiento técnico, garantizando:

1. Código de incidencia alfanumérico no vacío.
2. Descripción descriptiva válida del fallo reportado.
3. Clasificación estricta en las tres prioridades normalizadas: `'alta'`, `'media'` o `'baja'`.

---

## 🛠️ Procedimiento de Implementación Realizado

1. **Estructura del Proyecto:** Inicialización del proyecto con `package.json` configurando el runner de pruebas nativo de Node.js (`node --test test.js`).
2. **Desarrollo de la Lógica y Suites de Prueba:** Creación de `incidencias.js` con la función `validarIncidencia(incidencia)` y cobertura de casos borde en `test.js` (`node:test` y `node:assert/strict`).
3. **Automatización de CI con GitHub Actions:** Creación del workflow en `.github/workflows/ci.yml` con disparadores ante `push` y `pull_request`, aprovisionando contenedores `ubuntu-latest` con Node.js 22.
4. **Simulación de Falla Controlada (Actividad 4):** Introducción de una inconsistencia en `incidencias.js` admitiendo `'urgente'` en la lista de prioridades, lo que provocó el fallo automático e inmediato de la prueba `rechaza prioridad desconocida` en GitHub Actions.
5. **Corrección y Paso Exitoso:** Restauración del catálogo cerrado de prioridades (`['alta', 'media', 'baja']`), logrando el 100% de pruebas superadas en el pipeline de CI.

Enlace al historial de ejecuciones de GitHub Actions:  
👉 **[Ver ejecuciones en GitHub Actions](https://github.com/frankitoromas-tech/sistema-incidencias/actions)**

---

## 📝 Preguntas Técnicas de Evaluación (Actividad 6)

### 1. ¿Qué evento activa el workflow?
El flujo de trabajo se dispara de manera automática ante los eventos **`push`** (cada vez que un desarrollador envía nuevos commits a cualquier rama del repositorio) y **`pull_request`** (al abrir o actualizar una solicitud de fusión hacia ramas protegidas), garantizando que ningún cambio entre sin ser previamente validado.

### 2. ¿Qué verifica la prueba automatizada?
La suite en `test.js` evalúa tres reglas de negocio esenciales:
* **Prueba 1:** Acepta correctamente una incidencia con código, descripción y prioridad válida (`'alta'`).
* **Prueba 2:** Rechaza cualquier prioridad no reconocida o fuera del estándar corporativo (como `'urgente'`).
* **Prueba 3:** Rechaza incidencias que contengan una descripción vacía o compuesta únicamente de espacios en blanco.

### 3. ¿Qué diferencia existe entre un commit y una ejecución de Actions?
* **Commit:** Es una instantánea estática e inmutable en el historial de Git que registra cambios específicos en archivos (código, metadatos, autoría y mensaje descriptivo).
* **Ejecución de Actions:** Es un entorno de cómputo temporal (runner virtual en la nube) que reacciona ante eventos del repositorio para compilar código, instalar dependencias, ejecutar pruebas y emitir un veredicto dinámico de éxito (`pass`) o fracaso (`fail`).

### 4. ¿Qué ventaja ofrece detectar un error antes de integrar más cambios?
Aplica el principio de **Shift-Left Testing**: detectar y subsanar fallas en la etapa más temprana del ciclo de vida del software minimiza exponencialmente el costo de corrección, previene la contaminación de la rama `main` y asegura que el equipo trabaje siempre sobre una versión estable y funcional del sistema.
