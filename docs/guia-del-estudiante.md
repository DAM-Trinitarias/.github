# 📘 Guía del Estudiante · Organización GitHub (Ciclo DAM)

> Cómo trabajar tus prácticas con **GitHub Classroom** y recibir **feedback inmediato** con **GitHub Actions**, sin Pull Requests y con **tests protegidos**.

---

## Índice
1. Objetivo
2. Requisitos previos
3. Flujo de trabajo (sin Pull Requests)
4. Normas de evaluación y protección de ficheros
5. Ejecutar pruebas y linters en local
6. Cómo leer el feedback de CI
7. Criterios de evaluación
8. Buenas prácticas
9. Uso responsable de IA
10. Privacidad y soporte
11. FAQ (problemas típicos)
12. Checklists de entrega

---

## 1) Objetivo
- Entregar prácticas con **control de versiones** y **CI** real.
- Recibir **retroalimentación automática** en cada *push* (qué prueba falló y por qué).
- Aprender a **trabajar con tests** sin modificarlos y a **interpretar informes**.
- Contribuir a los resultados de aprendizaje de **Entornos de Desarrollo**, **Programación**, **Desarrollo de Interfaces**, **Sistemas de Gestión Empresarial** y **Programación de Dispositivos Móviles**.

---

## 2) Requisitos previos (primera vez)
1. **Cuenta de GitHub** con tu correo del centro.
2. **Git instalado**. Comprueba en terminal:
   ```bash
   git --version
   ```
3. (Recomendado) **SSH configurado**:
   ```bash
   ssh-keygen -t ed25519 -C "tu@correo"
   ```
   Añade la clave pública en GitHub → *Settings* → *SSH and GPG keys*.
4. **Configura tu identidad**:
   ```bash
   git config --global user.name "Nombre Apellido"
   git config --global user.email "tu@correo"
   ```
5. (Opcional) Solicita el **GitHub Student Developer Pack** si aún no lo tienes.

---

## 3) Flujo de trabajo (sin Pull Requests)
> En esta organización **no usamos PRs**: el *feedback* llega con cada *push* a `main`.

1. **Aceptar la tarea** en GitHub Classroom (se crea tu repo privado).
2. **Clonar** el repo:
   ```bash
   git clone <URL-de-tu-repo>
   cd <carpeta-del-repo>
   ```
3. **Explorar la estructura**:
   ```text
   /src/...                  # Tu código
   /test/...                 # Tests (protegidos)
   .github/workflows/ci.yml  # CI (protegido)
   README.md                 # Enunciado, criterios y rúbrica
   ```
4. **Desarrollar** en `main` con commits pequeños:
   ```bash
   git add .
   git commit -m "feat: implementa calculadora de pedidos"
   git push
   ```
5. **Revisar Actions**: pestaña *Actions* → ejecución más reciente → ver *Jobs* y *Logs*.
6. **Corregir y repetir** hasta que todos los checks estén en verde ✅.

---

## 4) Normas de evaluación y protección de ficheros
- **No modifiques** (ni borres, ni renombres):
  - `/test/**` o `tests/**`
  - `.github/workflows/**`
  - `.gitmodules`
- La organización aplica **reglas de protección** y el CI añade una **alerta pedagógica**:
  - Si tocas esas rutas, el *push* puede **ser bloqueado** o el **CI fallará** con un mensaje claro indicando el motivo.
- Objetivo: garantizar que **todas las entregas** se corrigen con **la misma base de pruebas** y que el feedback es **inmediato** y **justo**.

---

## 5) Ejecutar pruebas y linters en local
> Intenta reproducir en tu equipo lo que ejecutará el CI.

### Java (Maven) — Programación / Acceso a Datos / PSP / SGE
```bash
mvn -q -B -DskipITs -DskipIT -Dskip-it -DskipIntegrationTests test
# Reports JUnit en target/surefire-reports/
```

### Node.js (Jest) — Desarrollo de Interfaces / PMDM (React Native)
```bash
npm ci
npm test -- --ci
# Si está configurado jest-junit: reports/jest-junit.xml
```

### Estilo / Calidad (según plantilla)
- **Java**: Checkstyle / Spotless  
- **JS/TS/React**: ESLint + Prettier  
- **Kotlin**: ktlint / Detekt  
- **Dockerfiles**: Hadolint

Consulta el `README.md` de tu práctica para los comandos concretos incluidos en la plantilla.

---

## 6) Cómo leer el feedback de CI (paso a paso)
1. Abre **Actions** → ejecución más reciente.
2. En *Summary* verás el estado general (✅/❌).
3. En *Jobs* abre **Build/Test**.
4. Revisa:
   - **Informe de tests** (JUnit/Jest): qué prueba falló y **esperado vs obtenido**.
   - **Anotaciones**: a menudo señalan **archivo/línea** con el fallo.
   - **Mensajes didácticos** del workflow (p. ej., modificación de `/test`).

Corrige tu código y **vuelve a hacer push**.

---

## 7) Criterios de evaluación (resumen)
- **% de tests superados** (peso principal).
- **Estilo/calidad** si aplica (linter/formatter).
- **Cobertura mínima** si la práctica lo indica.
- **Buenas prácticas**: claridad de commits, estructura, documentación breve en README.
- La **rúbrica exacta** está en el README de cada práctica.

---

## 8) Buenas prácticas
- **Commits atómicos** y mensajes con prefijos: `feat:`, `fix:`, `test:`, `docs:`, `refactor:`, `chore:`.
- **Ejecuta localmente** antes de *push*.
- **Lee el error** del test: copia el mensaje exacto, entiende el caso, añade tests propios si te ayuda.
- **No manipules** historial para ocultar fallos: aprende del proceso.

---

## 9) Uso responsable de IA
- Puedes usar asistentes (p. ej., **GitHub Copilot**) **siempre que lo declares** en el README:
  - Qué pediste, qué aceptaste, qué modificaste.
- La organización puede usar **heurísticas/detectores** de uso no declarado (alarma informativa).
- **Plagio** o copia literal sin comprensión → intervención docente según normativa del centro.

---

## 10) Privacidad y soporte
- Repos de tareas: **privados**.
- Acceso: tú y el profesorado del módulo.
- **Dudas del ejercicio**: abre un **Issue** en tu repo incluyendo:
  - Qué intentabas hacer
  - Extracto del **error de CI**
  - Pasos para reproducir
- **Incidencias técnicas** de la organización: usa el repositorio `org-support` (si está habilitado) o el canal indicado por el profesor.

---

## 11) FAQ (problemas típicos)
**No puedo hacer *push* (rechazado por el servidor).**  
— Probablemente tocaste `/test/**`, `.github/workflows/**` o `.gitmodules`. Restaura esos cambios y vuelve a *push*.

**Pasan los tests localmente pero fallan en CI.**  
— Alinea versiones: **Java 21** / **Node 20** salvo indicación distinta. Ejecuta `mvn -q -B test` o `npm ci && npm test -- --ci`.

**No veo la pestaña Actions.**  
— Debes haber hecho **push** a `main`. Verifica que existe `.github/workflows/ci.yml` (archivo protegido; **no lo edites**).

**Error de permisos con Git.**  
— Configura `user.name`/`user.email`. Usa **SSH** o un **token** si empleas HTTPS.

**Timeout o dependencias no instalan.**  
— Limpia e instala desde cero (`rm -rf node_modules && npm ci`) o revisa la versión de JDK/Node.

---

## 12) Checklists de entrega

### Para ti (estudiante)
- [ ] Acepté la tarea en Classroom y cloné mi repo.
- [ ] **No** toqué `/test`, `.github/workflows` ni `.gitmodules`.
- [ ] Ejecuté los **tests en local** y pasan.
- [ ] Hice **push** a `main` y revisé **Actions**.
- [ ] Iteré hasta tener **checks en verde**.
- [ ] Anoté en el README si utilicé IA y cómo.

### Para el ejercicio (mínimos)
- [ ] Código en `/src` organizado.
- [ ] README con notas breves de solución.
- [ ] Sin *warnings* de linter (si aplica).
- [ ] Tests pasados en CI.

---

> ¿Tienes dudas? Abre un **Issue** en tu repo con el error del CI y pasos para reproducir. ¡Te ayudará a recibir ayuda más rápido!
