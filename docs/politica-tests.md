# 🧪 Política de Tests y Evaluación Automática

> Documento oficial de la organización para **cómo se diseñan, protegen y ejecutan los tests** en tareas de GitHub Classroom. Aplica a 1.º y 2.º del **Ciclo DAM**.  
> Objetivo: feedback **inmediato**, **justo** y **reproducible** sin intervención del profesorado (los *checks* corren en cada *push*).

---

## 1) Principios
1. **Integridad**: el alumno **no puede** modificar los ficheros de evaluación (tests, workflows y submódulos de tests).
2. **Transparencia**: los informes de CI indican **qué prueba** falla y **por qué** (esperado vs obtenido).
3. **Reproducibilidad**: los tests deben ser **deterministas** (mismo resultado en local y en CI).
4. **Autonomía**: el feedback llega con cada *push*, sin Pull Requests ni intervención del profesor.
5. **Seguridad**: sin secretos en repos de tarea, y sin dependencias no declaradas.

---

## 2) Rutas protegidas (regla estricta)
Las siguientes rutas **no se pueden editar** en los repos de alumnado:
- `tests/**` o `/test/**`  
- `.github/workflows/**`  
- `.gitmodules` (si se usa submódulo de tests)  

**Consecuencia automática**:  
- El *push* puede ser **rechazado** por las reglas de la organización, **y/o**  
- el **CI fallará** con un mensaje pedagógico indicando el motivo y cómo corregirlo.

Mensaje estándar del CI cuando se detecta edición prohibida:
```
Has modificado archivos de /test, .gitmodules o .github/workflows.
Estos ficheros están bloqueados porque forman parte de la evaluación.
Revertir los cambios en esas rutas y vuelve a hacer push.
```

---

## 3) Estructura mínima en repos de tarea
```text
/src/...                   # Código del alumno
/test/...                  # Tests cerrados (PROTEGIDOS)
/test/resources/...        # Ficheros de datos/fixtures
/.github/workflows/ci.yml  # Pipeline de CI (PROTEGIDO)
README.md                  # Enunciado, rúbrica, instrucciones
.gitmodules                # Solo si se usa submódulo de tests (PROTEGIDO)
```

> El alumno desarrolla en `/src`. Cualquier cambio en `/test`, `.gitmodules` o `/.github/workflows` disparará la protección.

---

## 4) Comprobación de integridad en CI (alerta pedagógica)
Los repos de tarea incluyen un workflow que **falla** si detecta modificaciones prohibidas y **explica el motivo**.

### 4.1. Paso reutilizable (org: `.github/.github/workflows/edu-ci.yml`)
```yaml
jobs:
  block-test-edits:
    name: Bloqueo cambios en /test y CI
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with: { fetch-depth: 2 }
      - name: Detectar cambios prohibidos
        run: |
          set -e
          if [ "$(git rev-list --count HEAD)" -gt 1 ]; then
            CHANGED="$(git diff --name-only HEAD^ HEAD || true)"
            echo "Ficheros cambiados:"
            echo "$CHANGED"
            if echo "$CHANGED" | grep -E '(^tests?/|^test/|^\.gitmodules|^\.github/workflows/)'; then
              echo "::error title=Protección de tests::Has modificado archivos de /test, .gitmodules o workflows.               Estos ficheros están bloqueados porque forman parte de la evaluación.               Revertir los cambios en esas rutas y vuelve a hacer push."
              exit 2
            fi
          fi
```

> Este job se ejecuta **antes** de compilar y testear. Si encuentra cambios prohibidos, **falla** con un error **explicativo**.

### 4.2. Publicación de resultados de tests
Se recomienda publicar los reports para ver **qué prueba** falla y **por qué**:
```yaml
  build-and-test:
    needs: block-test-edits
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      # Ejemplo Java
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '21'
      - run: mvn -q -B -DskipITs -DskipIT -Dskip-it -DskipIntegrationTests test

      # Informe
      - name: Publicar informe de tests
        uses: dorny/test-reporter@v1
        if: always()
        with:
          name: Tests
          path: "**/surefire-reports/*.xml"
          reporter: java-junit
          fail-on-error: true
```

> El informe muestra casos fallidos, mensajes de aserción y, cuando procede, anotaciones línea a línea.

---

## 5) Submódulo de tests (opcional, recomendación avanzada)
Para evitar que el alumnado vea los tests directamente:

1. Repo privado **`dam-tests-<asignatura>`** con las pruebas.  
2. En la plantilla, añadirlo como **submódulo** fijado a un commit:
   ```bash
   git submodule add -b main git@github.com:<ORG>/dam-tests-<asignatura>.git test
   git commit -m "chore: añade submódulo de tests"
   ```
3. En el workflow, clonar submódulos automáticamente:
   ```yaml
   - uses: actions/checkout@v4
     with:
       submodules: true
   ```
4. **Proteger** `.gitmodules` y la ruta `test/` con las reglas de la org (ver §2).
5. Si actualizas los tests, crea una **nueva asignación** o **actualiza la plantilla** fijando el nuevo commit del submódulo.

**Pros**: mayor confidencialidad. **Contras**: más mantenimiento (pin de commit).

---

## 6) Estándares para escribir buenos tests (para profesorado)
- **Determinismo**: sin aleatoriedad *o* semilla fija (`Random(seed)` / `Math.random` con *seeded* en JS mediante librerías).  
- **Reloj controlado**: simula tiempo (e.g., `Clock.fixed` en Java, *fake timers* en Jest).  
- **Nada de red**: *mock* de HTTP/DB/IO. Si hace falta, usar *fixtures* locales en `test/resources`.  
- **Aislamiento**: cada test prepara y limpia su estado (*setup/teardown*).  
- **Rendimiento**: evitar tests > 5–10 s.  
- **Mensajes claros**: aserciones con mensaje que explique el criterio (por qué falla).  
- **Nomenclatura**:
  - **Java/JUnit**: `NombreClaseTest` / `metodo_deberia_hacer_...()`
  - **Jest**: `describe('feature')` + `it('debería ...')`  
- **Patrón AAA**: Arrange → Act → Assert; una idea por test.
- **Cobertura**: fijar umbrales solo si son razonables; evita “forzar” cobertura con tests artificiales.

---

## 7) Criterios de evaluación automáticos
1. **Tests superados** (peso principal).
2. (Opcional) **Linter/estilo** (sin *warnings* críticos).  
3. (Opcional) **Cobertura mínima** (cuando la práctica lo especifique).  
4. (Opcional) **Análisis estático** (CodeQL/Sonar) como métrica informativa.

> La **rúbrica concreta** se publica en el README de cada práctica.

---

## 8) Política ante infracciones (alumnado)
- **Edición de ficheros protegidos** → CI **falla** con explicación y cómo revertir.  
- **Persistencia en la infracción** → el docente puede considerar la entrega **no válida** hasta revertir.  
- **Manipulación intencionada** de la evaluación → actuación según normativa del centro.

**Cómo deshacer cambios involuntarios** en tests (ejemplos):
```bash
# Descartar cambios en un archivo de test
git restore test/RutaDelTest.java

# Revertir el último commit (si tocó tests)
git revert HEAD

# Volver al estado del remoto en la carpeta de tests
git fetch origin
git checkout origin/main -- test/
```

---

## 9) Compatibilidad y versiones de referencia
- **Java**: Temurin 21  
- **Node.js**: 20.x  
- **JUnit**: 5.x · **Jest**: ^29  
- **Acciones de GitHub**: `actions/checkout@v4`, `actions/setup-java@v4`, `actions/setup-node@v4`

> Las plantillas de módulo fijan estas versiones para **alinear** local y CI.

---

## 10) Checklist de calidad (para profesorado)
- [ ] Los tests son **deterministas** (sin red/tiempo real).  
- [ ] Mensajes de aserción **explican el criterio**.  
- [ ] Fixtures en `test/resources` documentados.  
- [ ] Cobertura razonable de casos válidos y de error.  
- [ ] Rutas protegidas configuradas (ruleset de la org).  
- [ ] Workflow incluye **bloqueo de ediciones** y **publicación de reports**.  
- [ ] (Si procede) Submódulo de tests fijado a commit.  

---

## 11) Preguntas frecuentes
**¿Puedo ver los tests?**  
— En la mayoría de asignaturas sí, pero **no puedes modificarlos**. En otras, se usan submódulos privados.

**Mis tests pasan en local y fallan en CI.**  
— Alinea versiones (Java 21/Node 20) y revisa *logs*. Evita dependencias globales.

**¿Puedo añadir mis propios tests?**  
— Sí, en tu carpeta de trabajo (no en `/test`). No alteran la evaluación oficial, pero te ayudan a mejorar.

**¿Qué hago si toqué `/test` sin querer?**  
— Usa los comandos de §8 para **revertir** y vuelve a hacer *push*.

---

## 12) Mantenedores
- Coordinación DAM: `<nombre>`  
- Soporte técnico: `<correo>` · incidencias en `org-support`

---

> Esta política asegura corrección **consistente** y **escalable** en GitHub Classroom. Cualquier mejora será versionada y comunicada en el Overview de la organización.
