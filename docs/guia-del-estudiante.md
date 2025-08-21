# 📘 Guía del Estudiante · Organización GitHub (Ciclo DAM)

> Cómo trabajar tus prácticas con **GitHub Classroom** y recibir **feedback inmediato** con **GitHub Actions**, sin PRs y con **tests protegidos**.

---

## 0) Objetivo y qué vas a conseguir
- Entregar prácticas con **control de versiones** y **CI** real (como en la empresa).
- Recibir **retroalimentación automática** en cada *push* (qué prueba falló y por qué).
- Aprender a **trabajar con tests** sin modificarlos y a **interpretar informes de errores**.
- Contribuir a resultados de aprendizaje de **Entornos de Desarrollo** (VCS/CI-CD), **Programación** (TDD), **Desarrollo de Interfaces** (testing de UI), **SGE** (gestión colaborativa) y **PMDM** (integración y pruebas en móvil).

---

## 1) Requisitos previos (1ª vez)
1. **Cuenta de GitHub** con tu correo del centro.
2. **Git instalado** en tu equipo.  
   - Comprueba: `git --version`
3. (Recomendado) **SSH configurado**  
   - Genera clave: `ssh-keygen -t ed25519 -C "tu@correo"`  
   - Añade la clave pública en GitHub (**Settings → SSH and GPG keys**).
4. **Configura tu identidad en Git**  
   ```bash
   git config --global user.name "Nombre Apellido"
   git config --global user.email "tu@correo"
