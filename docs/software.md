# Реалізація інформаційного та програмного забезпечення

В рамках проекту розробляється: 

```sql
-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema databaseLab5
-- -----------------------------------------------------
DROP SCHEMA IF EXISTS `databaseLab5` ;

-- -----------------------------------------------------
-- Schema databaseLab5
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `databaseLab5` DEFAULT CHARACTER SET utf8 ;
USE `databaseLab5` ;

-- -----------------------------------------------------
-- Table `databaseLab5`.`User`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`User` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`User` (
  `id` INT NOT NULL,
  `name` VARCHAR(45) NULL,
  `surname` VARCHAR(45) NULL,
  `email` VARCHAR(45) NULL,
  `password` CHAR(8) NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`Admin`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`Admin` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`Admin` (
  `id` INT NOT NULL,
  `name` VARCHAR(45) NULL,
  `surname` VARCHAR(45) NULL,
  `email` VARCHAR(45) NULL,
  `password` CHAR(8) NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`Permission`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`Permission` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`Permission` (
  `name` VARCHAR(45) NULL,
  `description` VARCHAR(255) NULL,
  `id` INT NOT NULL,
  `User_id` INT NOT NULL,
  `Admin_id` INT NOT NULL,
  PRIMARY KEY (`id`, `User_id`, `Admin_id`),
  INDEX `fk_Permission_User_idx` (`User_id` ASC) VISIBLE,
  INDEX `fk_Permission_Admin1_idx` (`Admin_id` ASC) VISIBLE,
  CONSTRAINT `fk_Permission_User`
    FOREIGN KEY (`User_id`)
    REFERENCES `databaseLab5`.`User` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Permission_Admin1`
    FOREIGN KEY (`Admin_id`)
    REFERENCES `databaseLab5`.`Admin` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`UploadedMedia`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`UploadedMedia` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`UploadedMedia` (
  `id` INT NOT NULL,
  `source` INT NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`UploadedMediaSource`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`UploadedMediaSource` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`UploadedMediaSource` (
  `id` INT NOT NULL,
  `url` VARCHAR(255) NULL,
  `UploadedMedia_id` INT NOT NULL,
  PRIMARY KEY (`id`, `UploadedMedia_id`),
  INDEX `fk_UploadedMediaSource_UploadedMedia1_idx` (`UploadedMedia_id` ASC) VISIBLE,
  CONSTRAINT `fk_UploadedMediaSource_UploadedMedia1`
    FOREIGN KEY (`UploadedMedia_id`)
    REFERENCES `databaseLab5`.`UploadedMedia` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`Form`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`Form` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`Form` (
  `id` INT NOT NULL,
  `name` VARCHAR(45) NULL,
  `description` VARCHAR(45) NULL,
  `content` JSON NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`FormResult`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`FormResult` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`FormResult` (
  `id` INT NOT NULL,
  `formid` INT NULL,
  `answer` JSON NULL,
  `Form_id` INT NOT NULL,
  PRIMARY KEY (`id`, `Form_id`),
  INDEX `fk_FormResult_Form1_idx` (`Form_id` ASC) VISIBLE,
  CONSTRAINT `fk_FormResult_Form1`
    FOREIGN KEY (`Form_id`)
    REFERENCES `databaseLab5`.`Form` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`Content`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`Content` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`Content` (
  `id` INT NOT NULL,
  `type` ENUM("answer") NULL,
  `Form_id` INT NOT NULL,
  PRIMARY KEY (`id`, `Form_id`),
  INDEX `fk_Content_Form1_idx` (`Form_id` ASC) VISIBLE,
  CONSTRAINT `fk_Content_Form1`
    FOREIGN KEY (`Form_id`)
    REFERENCES `databaseLab5`.`Form` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`OpenAnswer`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`OpenAnswer` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`OpenAnswer` (
  `id` INT NOT NULL,
  `text` TEXT NULL,
  `Content_id` INT NOT NULL,
  PRIMARY KEY (`id`, `Content_id`),
  INDEX `fk_OpenAnswer_Content1_idx` (`Content_id` ASC) VISIBLE,
  CONSTRAINT `fk_OpenAnswer_Content1`
    FOREIGN KEY (`Content_id`)
    REFERENCES `databaseLab5`.`Content` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`SingleChoise`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`SingleChoise` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`SingleChoise` (
  `id` INT NOT NULL,
  `variant` VARCHAR(45) NULL,
  `Content_id` INT NOT NULL,
  PRIMARY KEY (`id`, `Content_id`),
  INDEX `fk_SingleChoise_Content1_idx` (`Content_id` ASC) VISIBLE,
  CONSTRAINT `fk_SingleChoise_Content1`
    FOREIGN KEY (`Content_id`)
    REFERENCES `databaseLab5`.`Content` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`MultiChoise`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`MultiChoise` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`MultiChoise` (
  `id` INT NOT NULL,
  `variants` JSON NULL,
  `Content_id` INT NOT NULL,
  PRIMARY KEY (`id`, `Content_id`),
  INDEX `fk_MultiChoise_Content1_idx` (`Content_id` ASC) VISIBLE,
  CONSTRAINT `fk_MultiChoise_Content1`
    FOREIGN KEY (`Content_id`)
    REFERENCES `databaseLab5`.`Content` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;

-- -----------------------------------------------------
-- Data for table `databaseLab5`.`User`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`User` (`id`, `name`, `surname`, `email`, `password`) VALUES (1, 'David', 'Krykota', 'a@gmail.com', 'a111');
INSERT INTO `databaseLab5`.`User` (`id`, `name`, `surname`, `email`, `password`) VALUES (2, 'Tymur', 'Naboka', 'b@gmail.com', 'b222');
INSERT INTO `databaseLab5`.`User` (`id`, `name`, `surname`, `email`, `password`) VALUES (3, 'Nika', 'Maslo', 'c@gmail.com', 'c333');

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`Admin`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`Admin` (`id`, `name`, `surname`, `email`, `password`) VALUES (1, 'admin', 'adminovich', 'admin@gmail.com', 'admin111');

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`Permission`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`Permission` (`name`, `description`, `id`, `User_id`, `Admin_id`) VALUES ('edit', 'allows to edit form', 1, 1, 1);
INSERT INTO `databaseLab5`.`Permission` (`name`, `description`, `id`, `User_id`, `Admin_id`) VALUES ('answer', 'allows to chose an answer', 2, 1, 1);
INSERT INTO `databaseLab5`.`Permission` (`name`, `description`, `id`, `User_id`, `Admin_id`) VALUES ('check_results', 'allows to check results', 3, 1, 1);

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`UploadedMedia`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`UploadedMedia` (`id`, `source`) VALUES (1, 123);
INSERT INTO `databaseLab5`.`UploadedMedia` (`id`, `source`) VALUES (2, 456);

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`UploadedMediaSource`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`UploadedMediaSource` (`id`, `url`, `UploadedMedia_id`) VALUES (1, 'media.com', 1);
INSERT INTO `databaseLab5`.`UploadedMediaSource` (`id`, `url`, `UploadedMedia_id`) VALUES (2, 'othermedia.com', 2);

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`Form`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`Form` (`id`, `name`, `description`, `content`) VALUES (1, 'wild animals of ukraine', 'choose the best choices for survival', '{\"1\": \"Deers\", \"2\": \"Bears\", \"3\": \"Birds\"}');
INSERT INTO `databaseLab5`.`Form` (`id`, `name`, `description`, `content`) VALUES (2, 'emotions', 'which emotions you experience the most', '{\"1\": \"Emotion1\",\"2\":\"Emotion2\",\"3\":\"Emotion3\"}');

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`FormResult`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`FormResult` (`id`, `formid`, `answer`, `Form_id`) VALUES (1, 1, '{\"1\":\"a\",\"2\":\"c\",\"3\":\"b\"}', 1);
INSERT INTO `databaseLab5`.`FormResult` (`id`, `formid`, `answer`, `Form_id`) VALUES (2, 2, '{\"1\":\"d\",\"2\":\"a\",\"3\":\"c\",\"4\":\"a\"}', 2);

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`Content`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`Content` (`id`, `type`, `Form_id`) VALUES (1, 'answer', 1);

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`OpenAnswer`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`OpenAnswer` (`id`, `text`, `Content_id`) VALUES (1, 'I love wild animals of Ukraine and wish to protect them', 1);

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`SingleChoise`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`SingleChoise` (`id`, `variant`, `Content_id`) VALUES (1, 'a', 1);
INSERT INTO `databaseLab5`.`SingleChoise` (`id`, `variant`, `Content_id`) VALUES (2, 'sadness', 1);
INSERT INTO `databaseLab5`.`SingleChoise` (`id`, `variant`, `Content_id`) VALUES (3, 'Deer', 1);

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`MultiChoise`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`MultiChoise` (`id`, `variants`, `Content_id`) VALUES (1, '{\"1\":\"a\",\"2\":\"b\",\"3\":\"c\"}', 1);
INSERT INTO `databaseLab5`.`MultiChoise` (`id`, `variants`, `Content_id`) VALUES (2, '{\"1\":\"d\",\"2\":\"b\",\"3\":\"c\"}', 1);

COMMIT;
```
## REST API для управління даними

### Підключення до бази даних MySql

#### config/db.js

```javascript
const { Sequelize } = require('sequelize');
require('dotenv').config();

const sequelize = new Sequelize(
    process.env.DB_NAME,
    process.env.DB_USER,
    process.env.DB_PASSWORD,
    {
        host: process.env.DB_HOST,
        dialect: 'mysql',
        port: process.env.DB_PORT,
    }
);

sequelize.authenticate()
    .then(() => console.log('Database connected...'))
    .catch((err) => console.error('Unable to connect to the database:', err));

module.exports = sequelize;
```
### Налаштування сервера

#### app.js

```javascript
const express = require('express');
const bodyParser = require('body-parser');
const adminRoutes = require('./routes/admin.routes');
require('dotenv').config();
require('./config/db'); // Ensure database connection

const app = express();
const PORT = process.env.PORT || 3000;

app.use(bodyParser.json());
app.use('/api/admins', adminRoutes);

app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
```
### Маршрутизація

#### routes/admin.routes.js

```javascript
const express = require('express');
const {
    getAllAdmins,
    getAdminById,
    createAdmin,
    updateAdmin,
    deleteAdmin,
} = require('../controllers/admin.controller');

const router = express.Router();

router.get('/', getAllAdmins);
router.get('/:id', getAdminById);
router.post('/', createAdmin);
router.put('/:id', updateAdmin);
router.delete('/:id', deleteAdmin);

module.exports = router;
```
### Контролери

#### controllers/admin.controller.js

```javascript
const Admin = require('../models/admin.model');

exports.getAllAdmins = async (req, res) => {
    try {
        const admins = await Admin.findAll();
        res.json(admins);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
};

exports.getAdminById = async (req, res) => {
    try {
        const admin = await Admin.findByPk(req.params.id);
        if (!admin) return res.status(404).json({ message: 'Admin not found' });
        res.json(admin);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
};

exports.createAdmin = async (req, res) => {
    try {
        const newAdmin = await Admin.create(req.body);
        res.status(201).json(newAdmin);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
};

exports.updateAdmin = async (req, res) => {
    try {
        const [updated] = await Admin.update(req.body, { where: { id: req.params.id } });
        if (!updated) return res.status(404).json({ message: 'Admin not found' });
        res.json({ message: 'Admin updated successfully' });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
};

exports.deleteAdmin = async (req, res) => {
    try {
        const deleted = await Admin.destroy({ where: { id: req.params.id } });
        if (!deleted) return res.status(404).json({ message: 'Admin not found' });
        res.json({ message: 'Admin deleted successfully' });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
};

```
### Модель Адміна

#### models/admin.model.js

```javascript
const { DataTypes } = require('sequelize');
const sequelize = require('../config/db');

const Admin = sequelize.define('Admin', {
    id: {
        type: DataTypes.INTEGER,
        primaryKey: true,
        autoIncrement: true,
    },
    name: {
        type: DataTypes.STRING,
        allowNull: false,
    },
    surname: {
        type: DataTypes.STRING,
        allowNull: false,
    },
    email: {
        type: DataTypes.STRING,
        allowNull: false,
        unique: true,
    },
    password: {
        type: DataTypes.STRING(8),
        allowNull: false,
    },
}, {
    tableName: 'Admin',
    timestamps: false,
});

module.exports = Admin;

```
### Коди статусів HTTP

#### utils/statusCodes.js

```javascript
const HTTP_STATUS_CODES = {
  OK: 200,
  CREATED: 201,
  BAD_REQUEST: 400,
  NOT_FOUND: 404,
  INTERNAL_SERVER_ERROR: 500,
};

export default HTTP_STATUS_CODES;
```
