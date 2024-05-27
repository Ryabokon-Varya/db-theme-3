# Реалізація інформаційного та програмного забезпечення

В рамках проекту розробляється:
- SQL-скрипт для створення на початкового наповнення бази даних

```sql
SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';
    
DROP SCHEMA IF EXISTS `db-theme-3` ;
   
CREATE SCHEMA IF NOT EXISTS `db-theme-3` DEFAULT CHARACTER SET utf8 ;
USE `db-theme-3` ;
DROP TABLE IF EXISTS `db-theme-3`.`Export`;
 
CREATE TABLE IF NOT EXISTS `db-theme-3`.`Export` (
    `Export.id` INT NOT NULL,
    `Export.isSuccess` BOOLEAN,
    `Export.time` DATETIME NOT NULL,
    `History_History.id` INT NOT NULL,
     PRIMARY KEY (`Export.id`),
     INDEX `fk_History_Export_idx` (`History_History.id` ASC) VISIBLE,
     CONSTRAINT `fk_History_Export`
        FOREIGN KEY (`History_History.id`)
        REFERENCES `db-theme-3`.`History` (`History.id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION)
ENGINE = InnoDB;

DROP TABLE IF EXISTS `db-theme-3`.`History` ;
    
CREATE TABLE IF NOT EXISTS `db-theme-3`.`History` (
      `History.id` INT NOT NULL,
      `History.name` VARCHAR(45) NULL,
      `History.time` DATETIME NULL,
      PRIMARY KEY (`History.id`))
ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS `db-theme-3`.`SupportRequest` (
    `SupportRequest.id` INT NOT NULL PRIMARY KEY,
    `SupportRequest.isAnswered` BOOLEAN,
    `SupportRequest.type` VARCHAR(45) NOT NULL,
    `Client_Client.id` INT NOT NULL,
    `History_History.id` INT NOT NULL,
    INDEX `fk_SupportRequest_Client_idx` (`Client_Client.id` ASC) VISIBLE,
    CONSTRAINT `fk_SupportRequest_Client`
        FOREIGN KEY (`Client_Client.id`)
        REFERENCES `db-theme-3`.`Client` (`id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION,
    INDEX `fk_SupportRequest_History_idx` (`History_History.id` ASC) VISIBLE,
    CONSTRAINT `fk_SupportRequest_History`
        FOREIGN KEY (`History_History.id`)
        REFERENCES `db-theme-3`.`History` (`History.id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `db-theme-3`.`adminAnswer`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `db-theme-3`.`adminAnswer` ;

CREATE TABLE IF NOT EXISTS `db-theme-3`.`adminAnswer` (
  `adminAnswer.id` SERIAL NOT NULL,
  `adminAnswer.text` VARCHAR(250) NOT NULL,
  `adminAnswer.time` DATETIME NOT NULL,
  `supportRequest.id` INT NOT NULL,
  PRIMARY KEY (`adminAnswer.id`),
  INDEX `fk_SupportRequest_Export_idx` (`supportRequest.id` ASC) VISIBLE,
     CONSTRAINT `fk_SupportRequest_Export`
        FOREIGN KEY (`supportRequest.id`)
        REFERENCES `db-theme-3`.`SupportRequest` (`SupportRequest.id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION)
ENGINE = InnoDB;
-- -----------------------------------------------------
-- Table `db-theme-3`.`userMessage`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `db-theme-3`.`userMessage` ;
CREATE TABLE IF NOT EXISTS `db-theme-3`.`userMessage` (
  `userMessage.id` INT NOT NULL,
  `userMessage.text` VARCHAR(250) NOT NULL,
  `userMessage.time` DATETIME NOT NULL,
  `SupportRequest_SupportRequest.id` INT NOT NULL,
  PRIMARY KEY (`userMessage.id`),
  INDEX `fk_userMessage_SupportRequest_idx` (`SupportRequest_SupportRequest.id` ASC) VISIBLE,
  CONSTRAINT `fk_userMessage_SupportRequest`
    FOREIGN KEY (`SupportRequest_SupportRequest.id`)
    REFERENCES `db-theme-3`.`SupportRequest` (`SupportRequest.id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


DROP TABLE IF EXISTS `db-theme-3`.`Task`;

CREATE TABLE IF NOT EXISTS `db-theme-3`.`Task` (
    `Task.id` INT NOT NULL ,
    `Task.name` VARCHAR(255) NOT NULL,
    `Task.deadline` DATETIME NOT NULL,
    `Client_Client.id` INT NOT NULL,
    PRIMARY KEY (`Task.id`),
    INDEX `fk_Task_Client1_idx` (`Client_Client.id` ASC) VISIBLE,
    CONSTRAINT `fk_Task_Client1`
    FOREIGN KEY (`Client_Client.id`)
    REFERENCES `db-theme-3`.`Client` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;

DROP TABLE IF EXISTS `db-theme-3`.`Request` ;
    
CREATE TABLE IF NOT EXISTS `db-theme-3`.`Request` (
      `id` INT NOT NULL,
      `name` VARCHAR(45) NULL,
      `time` TIME NULL,
      `description` VARCHAR(45) NULL,
      `History_History.id` INT NOT NULL,
      `Client.id` INT NOT NULL,
      PRIMARY KEY (`id`),
      INDEX `fk_Request_History1_idx` (`History_History.id` ASC) VISIBLE,
      INDEX `fk_Request_Client1_idx` (`Client.id` ASC) VISIBLE,
      CONSTRAINT `fk_Request_History1`
        FOREIGN KEY (`History_History.id`)
        REFERENCES `db-theme-3`.`History` (`History.id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION,
      CONSTRAINT `fk_Request_Client1`
        FOREIGN KEY (`Client.id`)
        REFERENCES `db-theme-3`.`Client` (`id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION)
ENGINE = InnoDB;


DROP TABLE IF EXISTS `db-theme-3`.`Client` ;
    
CREATE TABLE IF NOT EXISTS `db-theme-3`.`Client` (
      `id` INT NOT NULL,
      `name` VARCHAR(45) NULL,
      `lastname` VARCHAR(45) NULL,
      `mail` VARCHAR(45) NULL,
      `password` VARCHAR(45) NULL,
      `Role.id` INT NOT NULL,
       UNIQUE INDEX `mail_UNIQUE` (`mail` ASC) VISIBLE,
      PRIMARY KEY (`id`),
      INDEX `fk_Client_Role1_idx` (`Role.id` ASC) VISIBLE,
      CONSTRAINT `fk_Client_Role1`
        FOREIGN KEY (`Role.id`)
        REFERENCES `db-theme-3`.`Role` (`id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION)
ENGINE = InnoDB;


    DROP TABLE IF EXISTS `db-theme-3`.`Role` ;
    
    CREATE TABLE IF NOT EXISTS `db-theme-3`.`Role` (
      `id` INT NOT NULL,
      `name` VARCHAR(45) NULL,
      `permission` VARCHAR(45) NULL,
      `description` VARCHAR(45) NULL,
      PRIMARY KEY (`id`))
    ENGINE = InnoDB;

START TRANSACTION;
USE `db-theme-3`;
INSERT INTO `db-theme-3`.`userMessage` (`userMessage.id`, `userMessage.text`, `userMessage.time`, `SupportRequest_SupportRequest.id`) VALUES (1, 'Question 1', '2024-04-19 12:34:56', 1);
INSERT INTO `db-theme-3`.`userMessage` (`userMessage.id`, `userMessage.text`, `userMessage.time`, `SupportRequest_SupportRequest.id`) VALUES (2, 'Question 2', '2024-05-19 21:43:11', 1);
INSERT INTO `db-theme-3`.`userMessage` (`userMessage.id`, `userMessage.text`, `userMessage.time`, `SupportRequest_SupportRequest.id`) VALUES (3, 'Question 3', '2024-05-19 16:05:25', 1);
COMMIT;

START TRANSACTION;
USE `db-theme-3`;
INSERT INTO `db-theme-3`.`Request` (`id`, `name`, `time`, `description`, `History_History.id`, `Client.id`) VALUES (1, 'Swc', '11:30', 'SwERVREVREc', 1, 1);
INSERT INTO `db-theme-3`.`Request` (`id`, `name`, `time`, `description`, `History_History.id`, `Client.id`) VALUES (2, 'Wdcw', '22:00', 'SwFDVFVc', 1, 2);
INSERT INTO `db-theme-3`.`Request` (`id`, `name`, `time`, `description`, `History_History.id`, `Client.id`) VALUES (3, 'Ccec', '13:00', 'SwEVSGMUIc', 1, 3);  
COMMIT;
START TRANSACTION;
USE `db-theme-3`;
INSERT INTO `db-theme-3`.`Client` (`id`, `name`, `lastname`, `mail`, `password`,`Role.id`) VALUES (1, 'Swc', 'frc', 'Swc@gmail.com', '123', 1);
INSERT INTO `db-theme-3`.`Client` (`id`, `name`, `lastname`, `mail`, `password`,`Role.id`) VALUES (2, 'fewc', 'Swc', 'fewc@gmail.com', '321', 2);
INSERT INTO `db-theme-3`.`Client` (`id`, `name`, `lastname`, `mail`, `password`,`Role.id`) VALUES (3, 'Gdwc', 'oirwc','Gdwc@gmail.com', '123456', 3);
COMMIT;

START TRANSACTION;
USE `db-theme-3`;
INSERT INTO `db-theme-3`.`Role` (`id`, `name`, `permission`, `description`) VALUES (1, 'Swc', 'frc', 'bhvruoeijrnwp');
INSERT INTO `db-theme-3`.`Role` (`id`, `name`, `permission`, `description`) VALUES (2, 'fewc', 'Swc', 'njnjevjnvenijveni');
INSERT INTO `db-theme-3`.`Role` (`id`, `name`, `permission`, `description`) VALUES (3, 'Gdwc', 'oirwc', 'fvvkfnjewojcw');
COMMIT;

START TRANSACTION;
USE `db-theme-3`;
INSERT INTO `db-theme-3`.`adminAnswer` (`adminAnswer.id`, `adminAnswer.text`, `adminAnswer.time`, `SupportRequest.id`) VALUES (1, 'Answer1', '2024-03-21', 1);
INSERT INTO `db-theme-3`.`adminAnswer` (`adminAnswer.id`, `adminAnswer.text`, `adminAnswer.time`, `SupportRequest.id`) VALUES (2, 'Answer2', '2024-04-19', 1);
INSERT INTO `db-theme-3`.`adminAnswer` (`adminAnswer.id`, `adminAnswer.text`, `adminAnswer.time`, `SupportRequest.id`) VALUES (3, 'Answer3', '2024-05-10', 1);
COMMIT;

START TRANSACTION;
USE `db-theme-3`;
INSERT INTO `db-theme-3`.`Export` (`Export.id`, `Export.isSuccess`, `Export.time`, `History_History.id`) VALUES (1, 0, '2024-3-26', 1);
INSERT INTO `db-theme-3`.`Export` (`Export.id`, `Export.isSuccess`, `Export.time`, `History_History.id`) VALUES (2, 1, '2024-4-17', 1);
INSERT INTO `db-theme-3`.`Export` (`Export.id`, `Export.isSuccess`, `Export.time`, `History_History.id`) VALUES (3, 0, '2024-4-23', 1);
COMMIT;

START TRANSACTION;
USE `db-theme-3`;
INSERT INTO `db-theme-3`.`History` (`History.id`, `History.name`, `History.time`) VALUES (1, 'History 1', '2024-3-26');     
COMMIT;

START TRANSACTION;
USE `db-theme-3`;
INSERT INTO `db-theme-3`.`Task` (`Task.id`, `Task.name`, `Task.deadline`, `Client_Client.id`) VALUES (1, 'Task1', '2024-3-26', 1);
INSERT INTO `db-theme-3`.`Task` (`Task.id`, `Task.name`, `Task.deadline`, `Client_Client.id`) VALUES (2, 'Task2', '2024-4-26', 2);
INSERT INTO `db-theme-3`.`Task` (`Task.id`, `Task.name`, `Task.deadline`, `Client_Client.id`) VALUES (3, 'Task3', '2024-5-26', 3);
COMMIT;

START TRANSACTION;
USE `db-theme-3`;
INSERT INTO `db-theme-3`.`SupportRequest` (`SupportRequest.id`, `SupportRequest.isAnswered`, `SupportRequest.type`, `Client_Client.id`, `History_History.id`) VALUES (1, 1, 'abc', 1, 1);
INSERT INTO `db-theme-3`.`SupportRequest` (`SupportRequest.id`, `SupportRequest.isAnswered`, `SupportRequest.type`, `Client_Client.id`, `History_History.id`) VALUES (2, 1, 'bcd', 1, 1);
INSERT INTO `db-theme-3`.`SupportRequest` (`SupportRequest.id`, `SupportRequest.isAnswered`, `SupportRequest.type`, `Client_Client.id`, `History_History.id`) VALUES (3, 1, 'cde', 1, 1);
COMMIT;
```

- RESTfull сервіс для управління даними

- app.py:
```python
from flask import Flask

app = Flask(__name__)

from task_controller import *
```

- task_model.py:
```python
import mysql.connector


class Tasks:
    def __init__(self):
        try:
            self.connection = mysql.connector.connect(host='localhost', user='root', password='root12345',
                                                      database='db_theme_3')
            self.cursor = self.connection.cursor(dictionary=True)
            print("Successful connection to database")
        except mysql.connector.Error as err:
            print("Failed to connect to database:", err)

    def get_all_tasks(self):
        try:
            self.cursor.execute("select * from task")
            tasks = self.cursor.fetchall()

            if self.cursor.rowcount == 0:
                return {"message": "No tasks", "error": "Not Found", "status_code": 404}

            return tasks
        except mysql.connector.Error as err:
            return {'message': 'Failed to get all tasks', 'error': str(err), 'status_code': 500}

    def get_task_by_id(self, task_id):
        try:
            task_id = int(task_id)
            self.cursor.execute("select * from task where `Task.id` = %s", (task_id,))
            task = self.cursor.fetchone()

            if self.cursor.rowcount == 0:
                return {"message": f"No task with id {task_id}", "error": "Not Found", "status_code": 404}

            return task
        except mysql.connector.Error as err:
            return {'message': 'Failed to get task', 'error': str(err), 'status_code': 500}
        except ValueError:
            return {"message": "Invalid task id", "error": "Bad Request", "status_code": 400}

    def add_task(self, info):
        try:
            self.cursor.execute(f"insert into task (`Task.id`, `Task.name`, `Task.deadline`, `Client_Client.id`)"
                                f"values {tuple([i for i in info.values()])}")
            self.connection.commit()

            if self.cursor.rowcount > 0:
                return {"message": "Task added to database", "status_code": 200}
            else:
                return {"message": "Task was not added to database", "error": "Not Acceptable", "status_code": 406}
        except mysql.connector.Error as err:
            self.connection.rollback()
            return {'message': 'Failed to add task', 'error': str(err), 'status_code': 500}

    def delete_task(self, task_id):
        try:
            task_id = int(task_id)
            rows_deleted = 0
            self.cursor.execute("delete from task where `Task.id` = %s", (task_id,))
            rows_deleted += self.cursor.rowcount
            self.connection.commit()
            if rows_deleted > 0:
                return {"message": f"Task {task_id} deleted from database", "status_code": 204}
            else:
                return {"message": f"Task {task_id} was not deleted from database",
                        "error": "Not Found", "status_code": 404}
        except mysql.connector.Error as err:
            self.connection.rollback()
            return {'message': 'Failed to delete task', 'error': str(err), 'status_code': 500}
        except ValueError:
            return {"message": "Invalid task id", "error": "Bad Request", "status_code": 400}

    def update_task(self, task_id, info):
        try:
            task_id = int(task_id)
            updated_rows = 0
            for i in info.items():
                self.cursor.execute(f"update task set `{i[0]}` = '{i[1]}' where `Task.id` = {task_id}")
                updated_rows += 1
            self.connection.commit()

            if updated_rows > 0:
                return {"message": f"Task {task_id} updated in database", "status_code": 200}
            else:
                return {"message": f"Task {task_id} was not updated in database",
                        "error": "Not Acceptable", "status_code": 406}
        except mysql.connector.Error as err:
            self.connection.rollback()
            return {'message': 'Failed to update task', 'error': str(err), 'status_code': 500}
        except ValueError:
            return {"message": "Invalid task id", "error": "Bad Request", "status_code": 400}
```

- task_controller.py:
```python
from flask import request, jsonify
from task_model import Tasks
from app import app

tasks = Tasks()

@app.route("/tasks", methods=['GET'])
def get_all_tasks():
    return jsonify(tasks.get_all_tasks())


@app.route("/task/<task_id>", methods=['GET'])
def get_task_by_id(task_id):
    return jsonify(tasks.get_task_by_id(task_id))


@app.route("/tasks/add", methods=['POST'])
def add_task():
    url_params = request.args.to_dict()
    return jsonify(tasks.add_task(url_params))


@app.route("/tasks/delete/<task_id>", methods=['DELETE'])
def delete_task(task_id):
    return jsonify(tasks.delete_task(task_id))


@app.route("/tasks/update/<task_id>", methods=['PUT'])
def update_task(task_id):
    url_params = request.args.to_dict()
    return jsonify(tasks.update_task(task_id, url_params))

```
