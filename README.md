# Ronda — Asistente para visitas domiciliarias en Atención Primaria

> **Prototipo en prueba.** Herramienta personal para asistir al profesional de salud durante visitas domiciliarias en Atención Primaria (APS), reduciendo la carga administrativa sin reemplazar el criterio clínico.

Ronda acompaña al profesional **antes, durante y después** de una visita domiciliaria: prepara el resumen del paciente, permite registrar la atención en segundos (por voz o texto), y genera una nota de evolución estructurada lista para copiar al sistema de registro.

---

## El problema

En Atención Primaria, gran parte del tiempo del profesional no se destina al paciente, sino a escribir: evoluciones manuales, registro duplicado, seguimientos, y abundante documentación en cada visita domiciliaria. Ronda busca **devolver ese tiempo** ordenando y documentando en lugar de decidir.

La filosofía es explícita: **la IA nunca reemplaza el criterio clínico.** Organiza, resume, recuerda y propone borradores. El profesional siempre valida, corrige y aprueba antes de guardar.

---

## Funcionalidades

- **Ficha de paciente** con nombres, apellidos, RUT, edad, contacto, dirección y antecedentes.
- **Importación desde foto**: lee el listado de pacientes a visitar desde una foto (cámara o galería) usando visión por IA, con revisión manual obligatoria antes de agregar.
- **Registro rápido durante la visita**: botones de estado (paciente/familiar presente, educación realizada, derivación, etc.) y captura de signos vitales.
- **Alertas de signos vitales**: al ingresar los valores, se marcan automáticamente presión arterial, glicemia, saturación, frecuencia cardíaca y temperatura fuera de rango, como orientación para que el profesional evalúe.
- **Relato por voz o texto**: dictado con reconocimiento de voz del navegador, o escritura manual directa (útil para registrar con discreción frente al paciente).
- **Nota de evolución estructurada**: genera un borrador organizado en secciones (motivo, hallazgos, signos vitales, acciones, pendientes) que el profesional **revisa y edita antes de guardar**.
- **Derivación con selección de profesional**: al derivar, se elige el destino (enfermera, médico, nutricionista, psicólogo/a, asistente social, TENS, dentista, kinesiólogo/a, matrona u otro) y se genera automáticamente el seguimiento pendiente.
- **Resumen listo para SAC**: al cerrar la atención se produce un resumen completo con datos de identificación, listo para copiar y pegar, exportar a PDF o descargar como `.txt`.
- **Historial de visitas** por paciente.

---

## Diseño respecto a privacidad

Este es un **prototipo**, y la privacidad de datos de salud se trató como decisión de diseño consciente, no como una omisión:

- Los datos se guardan **localmente** en el navegador (`localStorage`), sin servidor central ni base de datos compartida. Cada dispositivo mantiene su propia información, aislada.
- Compartir el enlace de la app **no expone** los datos cargados: cada usuario parte con la app vacía en su propio navegador.
- **Una versión productiva o de uso compartido requeriría**: autenticación por usuario, almacenamiento seguro con control de acceso, trazabilidad de auditoría, y cumplimiento de la **Ley 21.719** (protección de datos personales) y la **Ley 20.584** (derechos y deberes del paciente) en Chile.

En resumen: apto para prueba personal; no apto para uso con datos reales de pacientes de terceros hasta resolver el marco de cumplimiento correspondiente.

---

## Stack

- **Frontend**: HTML, CSS y JavaScript vanilla, en un único archivo autocontenido (sin build).
- **Voz**: Web Speech API (reconocimiento de voz nativo del navegador).
- **IA (opcional)**: API de Anthropic (Claude) para lectura de listados desde foto y mejora de notas. La app funciona **completamente offline sin IA**: la nota estructurada se arma localmente.
- **Despliegue**: Vercel (hosting estático + función serverless como proxy de API).

---

## Ejecución

### Opción rápida (solo local, sin IA)

Abre `index.html` en cualquier navegador moderno. Funciona sin conexión: registro por voz/texto, alertas de signos vitales, notas estructuradas y exportación.

### Con funciones de IA (lectura de foto y mejora de notas), en Vercel

1. Estructura del proyecto:
   ```
   ronda/
   ├── index.html
   └── api/
       └── claude.js
   ```
2. En Vercel, define la variable de entorno `ANTHROPIC_API_KEY`.
3. Deploy.

La app detecta automáticamente su entorno: dentro de un contexto con acceso directo usa la API directa; desplegada en Vercel usa el proxy `/api/claude`, de modo que **la API key nunca queda expuesta en el frontend**.

> ⚠️ **Nunca** incluyas tu API key en el código ni la subas al repositorio. Ver `.gitignore`.

---

## Estado del proyecto

Prototipo funcional en fase de prueba en terreno. Próximos pasos considerados: incorporación de pautas de evaluación estructuradas por tipo de visita, y análisis del marco de cumplimiento para un eventual uso institucional.

---

*Proyecto desarrollado como exploración de IA aplicada a un problema real de la Atención Primaria de Salud.*
