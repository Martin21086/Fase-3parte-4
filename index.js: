const express = require('express');
const bodyParser = require('body-parser');
const connection = require('./db');

const app = express();
app.use(bodyParser.json());

// Ruta para actualizar un ticket
app.put('/api/ticket/:id', (req, res) => {
  const ticketId = req.params.id;
  const { modulo, severidad } = req.body;

  // Actualizar el ticket en la base de datos
  const updateTicketQuery = `
    UPDATE tickets 
    SET modulo = ?, severidad = ?
    WHERE id = ?`;

  connection.query(updateTicketQuery, [modulo, severidad, ticketId], (err, result) => {
    if (err) {
      return res.status(500).send('Error al actualizar el ticket');
    }

    // Registrar el historial de cambios
    const insertHistorialQuery = `
      INSERT INTO historial (ticket_id, modulo, severidad, fecha)
      VALUES (?, ?, ?, NOW())`;

    connection.query(insertHistorialQuery, [ticketId, modulo, severidad], (err, result) => {
      if (err) {
        return res.status(500).send('Error al registrar el historial');
      }

      res.send('Ticket actualizado y historial registrado');
    });
  });
});

// Iniciar el servidor
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Servidor corriendo en el puerto ${PORT}`);
});
