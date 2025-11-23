# selfieGlam
import express from 'express';
// bodyParser ya no es necesario importarlo por separado
import { setRoutes } from './routes/makeupRoutes.js'; // Asegúrate de la extensión .js si usas ES Modules

const app = express();
const PORT = process.env.PORT || 3000;

// Usar los métodos nativos de Express para manejar el cuerpo de las peticiones
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

setRoutes(app);

// Middleware para manejar rutas no encontradas (404)
app.use((req, res, next) => {
  res.status(404).json({ error: 'Ruta no encontrada' });
});

// Middleware para manejo de errores generales
// NOTA: Asegúrate de que este middleware de error sea el último definido
app.use((err, req, res, next) => {
  console.error(err.stack); // Registra el error en la consola del servidor
  // Envía una respuesta de error 500 al cliente
  res.status(500).json({ error: 'Error interno del servidor', message: err.message });
});

app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);