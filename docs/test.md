# Тестування працездатності системи
Розробка CRUD-сервісу для управління нашою базою даних

#### Вихідні коди програми
- server.js

```javascript
import express from "express";
import bodyParser from "body-parser";
import contentRoutes from "./routes/contentRoutes.js";
import db from "./config/db.js";

const app = express();
const PORT = 3000;

app.use(bodyParser.json());

app.use("/api/contents", contentRoutes);

app.listen(PORT, () => {
  console.log(`Сервер запущено на http://localhost:${PORT}`);
});
```
- config/db.js

```javascript
import mysql from "mysql2";

const db = mysql.createConnection({
  host: "localhost", 
  user: "root",
  password: "Nekakvcegda_0",
  database: "databaseLab5",
});

db.connect((err) => {
  if (err) {
    console.error("Помилка підключення до бази даних:", err);
    process.exit(1); 
  }
  console.log("Підключення до бази даних встановлено!");
});

export default db;

```

- controllers/contentController.js

```javascript
import db from "../config/db.js";

export const getContents = (req, res) => {
  const query = "SELECT * FROM Content";
  db.query(query, (err, results) => {
    if (err) {
      res.status(500).json({ error: "Помилка отримання даних", details: err });
    } else {
      res.status(200).json(results);
    }
  });
};

export const getContentById = (req, res) => {
  const { id } = req.params;
  const query = "SELECT * FROM Content WHERE id = ?";
  db.query(query, [id], (err, result) => {
    if (err) {
      res.status(500).json({ error: "Помилка отримання даних", details: err });
    } else if (result.length === 0) {
      res.status(404).json({ message: "Запис не знайдено" });
    } else {
      res.status(200).json(result[0]);
    }
  });
};

export const createContent = (req, res) => {
  const { type, form_id } = req.body;
  const query = "INSERT INTO Content (type, Form_id) VALUES (?, ?)";
  db.query(query, [type, form_id], (err, result) => {
    if (err) {
      res.status(500).json({ error: "Помилка створення запису", details: err });
    } else {
      res.status(201).json({
        message: "Запис успішно створено",
        contentId: result.insertId,
      });
    }
  });
};

export const updateContent = (req, res) => {
  const { id } = req.params;
  const { type, form_id } = req.body;
  const query = "UPDATE Content SET type = ?, Form_id = ? WHERE id = ?";
  db.query(query, [type, form_id, id], (err, result) => {
    if (err) {
      res.status(500).json({ error: "Помилка оновлення запису", details: err });
    } else if (result.affectedRows === 0) {
      res.status(404).json({ message: "Запис не знайдено" });
    } else {
      res.status(200).json({ message: "Запис успішно оновлено" });
    }
  });
};

export const deleteContent = (req, res) => {
  const { id } = req.params;
  const query = "DELETE FROM Content WHERE id = ?";
  db.query(query, [id], (err, result) => {
    if (err) {
      res.status(500).json({ error: "Помилка видалення запису", details: err });
    } else if (result.affectedRows === 0) {
      res.status(404).json({ message: "Запис не знайдено" });
    } else {
      res.status(200).json({ message: "Запис успішно видалено" });
    }
  });
};

```

- routes/contentRoutes.js

```javascript
import express from "express";
import {
  getContents,
  getContentById,
  createContent,
  updateContent,
  deleteContent,
} from "../controllers/contentController.js";

const router = express.Router();

router.get("/", getContents); 
router.get("/:id", getContentById);
router.post("/", createContent);
router.put("/:id", updateContent);
router.delete("/:id", deleteContent);

export default router;
```

### Тестування за допомогою Thunder Client

- POST

//![](images/post.png)

- GET ALL

![](images/getAll.png)

- GET BY ID

![](images/get.png)

- PUT

![](images/put.png)

- DELETE

![](images/delete.png)
