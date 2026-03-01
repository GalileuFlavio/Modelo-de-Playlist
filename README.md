# Modelo-de-Playlist
Modelo de Playlist (backend/src/models/Playlist.js)

/**
 * Modelo Mongoose para Playlists
 * Relaciona usuários com coleções de músicas
 */
const mongoose = require('mongoose');

const playlistSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
    trim: true,
    maxlength: 50
  },
  description: {
    type: String,
    maxlength: 200
  },
  // Referência ao usuário criador
  user: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true
  },
  // Array de referências para músicas (relacionamento N:N)
  tracks: [{
    type: mongoose.Schema.Types.ObjectId,
    ref: 'Music'
  }],
  // URL da capa da playlist
  coverUrl: {
    type: String,
    default: 'default-playlist.jpg'
  },
  // Playlist pública ou privada
  isPublic: {
    type: Boolean,
    default: true
  },
  createdAt: {
    type: Date,
    default: Date.now
  },
  updatedAt: {
    type: Date,
    default: Date.now
  }
});

// Atualiza o updatedAt automaticamente antes de salvar
playlistSchema.pre('save', function(next) {
  this.updatedAt = Date.now();
  next();
});

module.exports = mongoose.model('Playlist', playlistSchema);
