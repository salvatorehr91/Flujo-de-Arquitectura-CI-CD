# 🚀 Flujo de Despliegue de Contenedores (Docker/Jenkins + GitHub + OpenShift)

---

## 📌 Descripción del Proyecto

Este proyecto demuestra cómo desplegar una aplicación contenerizada utilizando un flujo automatizado de CI/CD conectado a GitHub.

---

### 1. Repositorio y Control de Código (GitHub)
* Cada microservicio Java se gestiona en un repositorio independiente.

* Se definen ramas principales:

  * `main` → estable, lista para producción.

  * `develop` → integración continua.

  * `feature/*` → nuevas funcionalidades.

* Se configuran GitHub Actions o webhooks para notificar a Jenkins cuando hay un push o pull request.
 
 * `Dockerfile`  para construir la imagen.

 * Archivos de configuración de Jenkins (`Jenkinsfile`) y manifiestos de OpenShift (`deployment.yaml`, `service.yaml`).

---

### 2. Integración Continua (Jenkins)
   
* Jenkins recibe el trigger desde GitHub.

* Jenkins usa el Jenkinsfile para definir las etapas:

 1. Checkout: Clonar el código desde GitHub.

 2. Build: Construir la imagen Docker.
```dockerfile
stage('Build Docker Image') {
  steps {
    script {
      docker.build("app:${env.BUILD_NUMBER}")
    }
  }
}
```

* Pipeline típico:

  1.Checkout del código.

  2.Compilación con Maven/Gradle.

  3.Pruebas unitarias (JUnit, Mockito).

  4.Análisis estático (SonarQube, CodeQL).

  5.Empaquetado en imagen Docker.

  6.Push de la imagen al registro (por ejemplo, Quay.io, Harbor o Red Hat Registry).

👉 Jenkins se encarga de la parte CI: asegurar que el código es válido y generar artefactos listos para desplegar.

---

### 3. Entrega Continua (Harness)
* Harness se conecta al registro de imágenes y a OpenShift.

* Define pipelines de CD con etapas:

  * Deploy en entorno de pruebas → despliegue automático en un namespace de OpenShift.

  * Validación automática → pruebas de integración, smoke tests.

  * Aprobación manual/automática → paso a producción.

  * Deploy en producción → despliegue controlado con estrategias como blue/green o canary.

👉 Harness aporta observabilidad: métricas, logs y rollback automático si falla el despliegue.

---

### 4. Plataforma de Ejecución (OpenShift)
* Cada microservicio se despliega como un DeploymentConfig o Deployment en OpenShift.

* Se gestionan:

  * Pods con contenedores Java.

  * Services para exponer APIs internas.

  * Routes para acceso externo.

  * ConfigMaps/Secrets para configuración segura.

* OpenShift maneja escalado automático (HPA) y balanceo de carga.

---

📊 Esquema Visual del Flujo

<p align="center">
  <img src="images/Flujo-CI-CD con-Jenkins- OpenShift.png" width="800">
</p>

---

#### ✅ Beneficios del Flujo
* Automatización completa desde commit hasta despliegue.

* Calidad asegurada con pruebas y análisis estático.

* Flexibilidad con Harness para elegir estrategias de despliegue.

* Escalabilidad y resiliencia gracias a OpenShift.

---

### 5. Ciclo de Retroalimentación
 * Jenkins reporta estado del pipeline en GitHub (checks).

 * OpenShift expone métricas y logs para validar despliegues.

 * Notificaciones opcionales vía correo/Slack.

---

#### 📊 Esquema Visual del Flujo

| Etapa | Herramienta | Acción |
| --- | --- | --- |
| Código | GitHub | Push/PR activa webhook |
| CI/CD | Jenkins | Build → Test → Push → Deploy |
| Contenedor | Docker | Imagen construida y enviada a registry |
| Orquestación | OpenShift | Despliegue, escalado y monitoreo |

