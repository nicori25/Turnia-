# Turnia

Aplicación móvil para gestionar los turnos de peluquerías y barberías. El cliente reserva su turno desde el celular, el barbero administra su agenda y el servidor valida todas las reglas del negocio. Incluye un asistente con Inteligencia Artificial para pedir turnos en lenguaje natural.

Proyecto Integrador **P.D.I.S.C** (1 de octubre – 13 de noviembre de 2026).

- **Autor:** Ariel Aranda
- **Repositorio:** https://github.com/nicori25/Turnia-

## El problema

Muchos negocios gestionan los turnos por WhatsApp, llamadas o una agenda en papel. Esto genera superposición de horarios, ausencias sin aviso y tiempo perdido en tareas administrativas. Turnia digitaliza ese proceso y permite ofrecer planes con prioridad (Básico y VIP).

## Funcionalidades

**Cliente**
- Registro e inicio de sesión.
- Ver servicios y horarios disponibles.
- Reservar, cancelar y reprogramar turnos.
- Plan Básico o VIP con distinta anticipación de reserva.
- Asistente con IA: pedir turno escribiendo en lenguaje natural.

**Barbero**
- Gestión de servicios (nombre, duración y precio).
- Definición de días y horarios de atención.
- Agenda diaria y semanal.
- Marcar turnos como atendidos o ausentes.
- Informe semanal generado con IA.

## Reglas de negocio

| ID | Regla |
|---|---|
| R01 | Un barbero no puede tener dos turnos en el mismo horario. |
| R02 | No se puede reservar en una fecha u hora pasada. |
| R03 | Solo se puede reservar dentro del horario de atención. |
| R04 | Un turno solo se puede cancelar hasta 2 horas antes. |
| R05 | Cliente Básico: hasta 7 días de anticipación. Cliente VIP: hasta 14 días. |
| R06 | Un servicio con turnos no se elimina, solo se desactiva. |
| R07 | Cada cliente ve solo sus turnos y cada barbero solo su agenda. |
| R08 | Un cliente Básico puede tener como máximo 2 turnos activos. |
| R09 | Un turno ausente queda registrado y no se puede borrar. |

## Tecnologías

| Parte | Tecnología |
|---|---|
| App móvil | React Native |
| Backend y autenticación | Supabase (Auth, PostgreSQL, RPC, Edge Functions) |
| Seguridad de datos | Row Level Security (RLS) |
| Inteligencia Artificial | Modelo de IA consumido desde una Edge Function |
| Control de versiones | Git y GitHub |
| Pruebas | Postman (unitarias) y pruebas integrales front + back |

## Arquitectura

```
App React Native  ──►  Supabase (Auth · PostgreSQL · RPC · Edge Functions)  ──►  Servicio de IA
```

La lógica importante (por ejemplo, la validación de turnos) vive en el servidor, en la función `reservar_turno`. La app no puede saltearse las reglas del negocio.

## Base de datos

Tablas principales: `planes`, `perfiles`, `servicios`, `horarios_atencion` y `turnos`.

- Claves primarias y foráneas en todas las relaciones.
- Restricciones `CHECK` para evitar datos incorrectos (duraciones, precios, estados y roles).
- `UNIQUE (barbero_id, inicio)` para impedir turnos superpuestos.
- RLS activado en todas las tablas.

El script SQL completo está en la carpeta `database/` *(a completar)*.

## Instalación

### Requisitos

- Node.js 18 o superior
- Cuenta y proyecto en [Supabase](https://supabase.com)
- Expo Go en el celular (o un emulador de Android/iOS)

### Pasos

```bash
# 1. Clonar el repositorio
git clone https://github.com/nicori25/Turnia-.git
cd Turnia-

# 2. Instalar dependencias
npm install

# 3. Configurar las variables de entorno (ver más abajo)
cp .env.example .env

# 4. Iniciar la app
npx expo start
```

### Variables de entorno

Crear un archivo `.env` en la raíz del proyecto:

```env
EXPO_PUBLIC_SUPABASE_URL=https://tu-proyecto.supabase.co
EXPO_PUBLIC_SUPABASE_ANON_KEY=tu-clave-anon
```

> La clave del servicio de IA **no** va en la app: se guarda como secreto en las Edge Functions de Supabase. Nunca subas claves secretas al repositorio.

> Si la estructura final del proyecto no usa Expo, ajustá los comandos de instalación a la que corresponda.

## Pruebas

Se realizan pruebas positivas, negativas y de límite. Ejemplos:

| ID | Función | Tipo | Prueba | Resultado esperado |
|---|---|---|---|---|
| CP-01 | Login | Positivo | Email y contraseña correctos | Permite ingresar |
| CP-02 | Login | Negativo | Contraseña incorrecta | Rechaza el acceso |
| CP-04 | Registro | Límite | Contraseña de 7 caracteres (mínimo 8) | Rechaza |
| CP-06 | Reserva | Negativo | Horario ya ocupado | Rechaza el turno |
| CP-08 | Cancelación | Límite | Cancelar exactamente 2 h antes | Permite cancelar |

El registro completo de pruebas está en la documentación del proyecto.

## Flujo de trabajo con Git

- `main`: versión estable.
- `develop`: integración de funcionalidades.
- `feature/nombre-tarea`: una rama por tarea.
- Commits descriptivos, por ejemplo: `feat: agrega pantalla de login`.
- Cada tarea tiene su issue y se integra mediante Pull Request hacia `develop`.

## Uso de Inteligencia Artificial

Se utilizó IA como ayuda durante el desarrollo (consultas, ideas, código y búsqueda de errores). Todo el código generado fue revisado, comprendido y probado. El detalle está en el registro de uso de IA de la documentación.

Además, el sistema incorpora el asistente de reservas con IA descripto arriba.

## Estado del proyecto

- [ ] Empresa, problema y relevamiento
- [ ] Requisitos e historias de usuario
- [ ] Diagramas y diseño de la base de datos
- [ ] Base de datos y servidor (Supabase)
- [ ] Pantallas en React Native
- [ ] Integración, seguridad e IA
- [ ] Pruebas y documentación
- [ ] Entrega final (13 de noviembre)

## Autor

**Ariel Aranda** — Proyecto Integrador P.D.I.S.C
