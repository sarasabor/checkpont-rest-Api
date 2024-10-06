# checkpont-rest-Api


Étape 1 : Démarrer un nouveau projet Node.js
Ouvrez votre terminal.
Créez un nouveau répertoire pour votre projet et accédez-y :
bash
mkdir my-api  
cd my-api  
Initialisez un nouveau projet Node.js :
bash
npm init -y  
Étape 2 : Installer Mongoose, Express et dotenv
Installez les packages nécessaires :

bash
npm install express mongoose dotenv  
Étape 3 : Configurer les variables d'environnement
Créez un fichier .env dans le dossier racine du projet et ajoutez-y votre URI MongoDB (remplacez par votre propre URI) :

ini
MONGO_URI='your_mongodb_uri_here'  
Étape 4 : Lancer un serveur avec Express
Créez un fichier server.js et ajoutez-y le code suivant :

javascript
// Importation des modules nécessaires  
const express = require('express'); // Framework web pour Node.js  
const mongoose = require('mongoose'); // ORM pour MongoDB  
require('dotenv').config(); // Charge les variables d'environnement  

// Création de l'application Express  
const app = express();  
app.use(express.json()); // Middleware pour analyser le JSON  

// Connexion à la base de données MongoDB  
mongoose.connect(process.env.MONGO_URI, { useNewUrlParser: true, useUnifiedTopology: true })  
    .then(() => console.log('MongoDB Connected'))  
    .catch(err => console.error('Could not connect to MongoDB:', err));  

// Démarrage du serveur  
const PORT = process.env.PORT || 3000; // Définit le port  
app.listen(PORT, () => {  
    console.log(`Server is running on port ${PORT}`);  
});  

// Importation du modèle User  
const User = require('./models/User');  

// Route GET : Retourner tous les utilisateurs  
app.get('/users', async (req, res) => {  
    try {  
        const users = await User.find(); // Récupère tous les utilisateurs  
        res.json(users); // Renvoie la liste des utilisateurs  
    } catch (err) {  
        res.status(500).send(err.message); // Gère l'erreur  
    }  
});  

// Route POST : Ajouter un nouvel utilisateur  
app.post('/users', async (req, res) => {  
    const newUser = new User(req.body); // Crée un nouvel utilisateur avec les données reçues  
    try {  
        const savedUser = await newUser.save(); // Sauvegarde l'utilisateur dans la base de données  
        res.status(201).json(savedUser); // Renvoie l'utilisateur créé  
    } catch (err) {  
        res.status(400).send(err.message); // Gère l'erreur  
    }  
});  

// Route PUT : Éditer un utilisateur par ID  
app.put('/users/:id', async (req, res) => {  
    try {  
        const updatedUser = await User.findByIdAndUpdate(req.params.id, req.body, { new: true }); // Met à jour l'utilisateur  
        res.json(updatedUser); // Renvoie l'utilisateur mis à jour  
    } catch (err) {  
        res.status(400).send(err.message); // Gère l'erreur  
    }  
});  

// Route DELETE : Supprimer un utilisateur par ID  
app.delete('/users/:id', async (req, res) => {  
    try {  
        const deletedUser = await User.findByIdAndRemove(req.params.id); // Supprime l'utilisateur  
        res.json(deletedUser); // Renvoie l'utilisateur supprimé  
    } catch (err) {  
        res.status(500).send(err.message); // Gère l'erreur  
    }  
});  
Étape 5 : Créer un modèle Mongoose pour les utilisateurs
Créez un dossier models et un fichier User.js à l'intérieur.

bash
mkdir models  
touch models/User.js  
Ajoutez le code suivant dans User.js :

javascript
// Importation de mongoose  
const mongoose = require('mongoose');  

// Définition du schéma de l'utilisateur  
const userSchema = new mongoose.Schema({  
    name: { type: String, required: true }, // Nom de l'utilisateur (obligatoire)  
    email: { type: String, required: true, unique: true }, // Email de l'utilisateur (obligatoire et unique)  
    age: { type: Number, required: false } // Âge de l'utilisateur (optionnel)  
});  

// Exportation du modèle User  
module.exports = mongoose.model('User', userSchema);  
Étape 6 : Structure des dossiers
Votre projet devrait maintenant avoir la structure suivante :

arduino
my-api/  
├── config/  
│   └── .env  
├── models/  
│   └── User.js  
└── server.js  
Étape 7 : Démarrer l'application
Pour démarrer votre application, exécutez la commande suivante dans le terminal :

bash
node server.js  
Test de l'API
Vous pouvez tester votre API en utilisant un outil comme Postman ou curl pour effectuer des requêtes HTTP sur les différentes routes définies dans votre serveur.

Remarque
N'oubliez pas de gérer la sécurité (comme la validation des entrées) et d'ajouter éventuellement des messages d'erreur et des gestionnaires d'erreurs plus robustes pour une application en production.
