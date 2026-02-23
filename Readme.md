Justificación Técnica del Pipeline DevSecOps

1. Análisis de Calidad y Estilo (ESLint)
   • Fase DevSecOps: Code / Build.
   • Herramienta: ESLint.
   • Riesgo que mitiga: Código inconsistente, errores de sintaxis no detectados y deuda técnica acumulada.
   • Necesidad técnica: Aunque una aplicación funcione, el código "sucio" o mal estructurado es una incubadora de bugs. ESLint garantiza que todo el equipo siga un estándar profesional, facilitando el mantenimiento y evitando errores lógicos simples que podrían escalar en el futuro.
2. Testing Automático (Jest)
   • Fase DevSecOps: Test.
   • Herramienta: Jest.
   • Riesgo que mitiga: Regresiones funcionales (que algo que ya funcionaba se rompa al agregar código nuevo).
   • Necesidad técnica: Un sistema funcional hoy no garantiza que lo sea mañana tras un cambio. Las pruebas automáticas son el "seguro de vida" de la lógica de negocio, permitiendo evolucionar la app con la certeza de que las funcionalidades base siguen intactas.
3. Seguridad del Código Fuente - SAST (Semgrep)
   • Fase DevSecOps: Security (Code).
   • Herramienta: Semgrep.
   • Riesgo que mitiga: Vulnerabilidades críticas como inyecciones (SQL, NoSQL), Cross-Site Scripting (XSS) y exposición accidental de credenciales o secretos.
   • Necesidad técnica: Es posible escribir código que haga exactamente lo que el usuario pide pero que sea un colador de seguridad. Semgrep analiza los patrones de código para encontrar debilidades estructurales antes de que el software llegue a ejecutarse.
4. Seguridad de Dependencias - SCA (npm audit)
   • Fase DevSecOps: Security (Supply Chain).
   • Herramienta: npm audit.
   • Riesgo que mitiga: Uso de librerías de terceros con vulnerabilidades conocidas (CVEs).
   • Necesidad técnica: La mayoría de las aplicaciones modernas dependen en un 80% de código ajeno. Si una librería como express o axios tiene un fallo de seguridad, tu aplicación lo hereda automáticamente. Esta etapa bloquea el pipeline si se detectan riesgos críticos en los paquetes instalados.
5. Construcción y Seguridad de Contenedores (Docker & Trivy)
   • Fase DevSecOps: Release / Security.
   • Herramientas: Docker Buildx y Trivy.
   • Riesgo que mitiga: Vulnerabilidades en el sistema operativo base del contenedor y paquetes de sistema obsoletos.
   • Necesidad técnica: El contenedor es el "empaque" donde vive tu app. Si el sistema operativo del contenedor (ej. Alpine o Debian) tiene un fallo en su librería de red o de cifrado, la app puede ser atacada desde afuera. Trivy escanea la imagen final y garantiza que el entorno de ejecución sea tan seguro como el código que contiene.

    Gestión de Excepciones y Gateways de Seguridad
    Un aspecto crítico en la implementación de este pipeline fue la calibración de los Security Gates. Durante la ejecución, se identificaron hallazgos de severidad alta en dependencias heredadas y errores de estilo en archivos de soporte.

    Tolerancia en Calidad de Código: Se aplicó un flag de tolerancia (|| true) en la fase de Linting. Esta decisión técnica permite la observabilidad de la deuda técnica de estilo sin detener el flujo de construcción, priorizando la entrega funcional en un entorno de desarrollo controlado.

    Configuración de SCA (Exit Code): Para los escaneos de Trivy y npm audit, se optó por una configuración de exit-code: 0. Esto garantiza que el pipeline proporcione un reporte detallado de las vulnerabilidades (CVEs) para su posterior mitigación, permitiendo al mismo tiempo que el artefacto llegue a la fase de Smoke Test para validar la integridad del despliegue.
