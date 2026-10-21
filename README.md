# Panel de citas médicas · Clínica Aurora

Interfaz web del panel de usuario de una plataforma de reservas médicas. Permite iniciar sesión, ver las citas, **cancelarlas** y **reagendarlas**, consumiendo una API REST (simulada con `json-server`).

## Tecnologías
React 18 · Vite · React Router 6 · Axios · Context API + `useReducer` · json-server (API simulada)

## Instalación y ejecución
```bash
npm install
npm start        # levanta la API (puerto 3001) y la app (puerto 5173)
```
Abre http://localhost:5173 e ingresa con **paciente@correo.com / Paciente123**.

También puedes correr por separado: `npm run api` y `npm run dev`. Para otra URL de API, define `VITE_API_URL` en un archivo `.env`.

## Arquitectura (basada en componentes + capa de servicios)
```
src/
├── components/   UI reutilizable: Navbar, Modal, AppointmentRow, RescheduleForm, ProtectedRoute
├── context/      Estado global: AuthContext (sesión) y AppointmentsContext (citas, useReducer)
├── pages/        Pantallas: Login, Appointments
├── services/     api.js: cliente Axios, peticiones y traducción de errores
└── utils/        validators.js: reglas de validación y formato de fechas
```
**Estado:** Context API + `useReducer`. Hay solo dos dominios de estado (sesión y citas), por lo que Redux añadiría código sin aportar valor.

## API (json-server)
| Método | Ruta | Uso |
|---|---|---|
| GET | `/users?email=&password=` | Autenticación simulada |
| GET | `/appointments?userId=` | Citas del paciente |
| GET | `/appointments?doctor=&date=&status=programada` | Verificar horarios ocupados |
| PATCH | `/appointments/:id` | Cancelar (`status`) o reagendar (`date`, `time`) |

## Validaciones y errores
- **Login:** correo con formato válido, contraseña obligatoria (mín. 6 caracteres), error de credenciales del servidor.
- **Reagendar:** fecha posterior a hoy, no domingos, hora dentro de la franja de atención y horario no ocupado por el mismo médico.
- **Red/servidor:** mensajes distintos para sin conexión, timeout, 404 y 5xx; botón «Reintentar» al fallar la carga.
- **Rutas protegidas:** sin sesión, `/citas` redirige a `/login`.


