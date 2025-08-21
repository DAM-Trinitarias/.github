# .# 👩‍🏫 Organización GitHub · Ciclo DAM (1.º y 2.º)
> Repositorios, tareas y evaluación continua con GitHub Classroom y GitHub Actions

![Logo del centro](./profile/logo.png)

---

## 📌 Enlaces rápidos
- 📚 **Guía del estudiante** → `docs/guia-estudiante.md`
- 🧑‍🏫 **Guía del profesorado** → `docs/guia-profesorado.md`
- 🧪 **Política de tests y autogrado** → `docs/politica-tests.md`
- 🛠️ **Plantillas por asignatura** → `https://github.com/<ORG>/?q=plantilla&type=all`
- 🏫 **GitHub Classroom (tablero de tareas)**:
  | Módulo | Curso | Classroom |
  |-------|------:|-----------|
  | Programación | 1.º DAM | <enlace>
  | Bases de Datos | 1.º DAM | <enlace>
  | Entornos de Desarrollo | 1.º DAM | <enlace>
  | Lenguajes de Marcas | 1.º DAM | <enlace>
  | Desarrollo de Interfaces | 2.º DAM | <enlace>
  | Acceso a Datos | 2.º DAM | <enlace>
  | Programación Multimedia y Dispositivos Móviles | 2.º DAM | <enlace>
  | Programación de Servicios y Procesos | 2.º DAM | <enlace>
  | Sistemas de Gestión Empresarial | 2.º DAM | <enlace>
  | Proyecto DAM | 2.º DAM | <enlace>

> Sustituye `<ORG>` y los enlaces de Classroom por los reales.

---

## 🎯 Misión
Centralizar el desarrollo, la entrega y la evaluación formativa/sumativa del ciclo **DAM** mediante:
- **Flujo automatizado** de corrección (CI) en cada *push*.
- **Feedback inmediato** de errores y resultados de tests.
- **Protección de ficheros de evaluación** para garantizar la integridad.
- **Autonomía del profesorado**: cada docente gestiona su módulo y sus tareas.

---

## 🧩 Estructura de la organización
- **Equipos de profesorado por módulo**  
  `DAM1-Programacion-Profes`, `DAM2-DesarrolloInterfaces-Profes`, … (permiso *Admin* en sus repos).
- **Equipos de alumnado por cohorte**  
  `DAM1-Programacion-2025-Alumnos`, … (permiso *Write* en repos de tarea).
- **Repositorios de plantilla por módulo**  
  `dam1-programacion-plantilla`, `dam2-desarrollo-interfaces-plantilla`, …
- **Repositorios de tareas (creados por Classroom)**  
  `tarea-<modulo>-<apellido1-apellido2>-<usuario>`

---

## 🏗️ Convenciones de nombres
- **Repos plantilla**: `dam<curso>-<modulo>-plantilla`
- **Repos de tarea**: `tarea-<modulo>-<unidad>-<usuario>`
- **Ramas**: trabajo en `main` (sin PR); opcionalmente `feature/<tema>` para trabajo local.
- **Commits**: usar prefijos  
  `feat:`, `fix:`, `test:`, `docs:`, `refactor:`, `chore:`

---

## 🚀 Flujo de trabajo del estudiante (entrega sin PR)
1. **Aceptar la tarea** en GitHub Classroom.
2. **Clonar** el repo que se crea automáticamente.
3. **Desarrollar** en la rama `main`.
4. **Hacer commit y push** con mensajes claros.
5. Revisar **Actions** → estado de CI y **anotaciones** de fallos (motivo del error y línea afectada).
6. Si hay errores, **corregir** y **volver a hacer push**.  
   *No es necesaria intervención del profesor.*

---

## 🔒 Protección de tests y workflows (muy importante)
- Los **archivos de evaluación** (por ejemplo `/test/**`, `tests/**`, `.github/workflows/**`, `.gitmodules`) están **protegidos**:
  - **Reglas de la organización** impiden subir cambios a esas rutas.
  - Además, el **workflow de CI** comprueba cada *push*:
    - Si detecta cambios en `/test` o los workflows, **falla** y muestra una **alerta con el motivo**:
      > “Has modificado archivos de `/test` o de CI. Estos ficheros están bloqueados porque forman parte de la evaluación. Reviértelo y vuelve a hacer *push*.”
- Si los tests **fallan**, el informe de CI muestra **qué prueba**, **qué esperaba** y **qué obtuviste** (feedback inmediato).

---

## 🧪 Evaluación automática (CI)
Cada *push* ej
