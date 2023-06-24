# Devoir_3
#Manipulation des taches utilisant un API/MySql/PHP


<h2>Video</h2>
https://github.com/Aboufariss-saad/Devoir_3/assets/96661067/5280aa5c-1cc4-4c73-870a-cf3085acbc69

<h2>Design</h2>
![img](https://github.com/Aboufariss-saad/Devoir_3/assets/96661067/77949953-c493-44f8-bd59-7179fbce18d6)

![img1](https://github.com/Aboufariss-saad/Devoir_3/assets/96661067/9d9a878a-2316-4ea6-92f0-1fb6f6225585)

## Objectif
Nous avons vu comment manipuler les données persistantes en utilisant une base de données locale. Dans ce chapitre, nous allons voir comment manipuler des données persistantes en utilisant une base de données distante.

Nous allons aussi voir comment utiliser le ***multithreading*** pour améliorer les performances de l'application.

## Prérequis 
Avant de commencer, on démarre ***XMPP*** et on lance le serveur ***Apache*** et la base de données ***MySQL***.

![img.png](img.png)

## 1. Création de la base de données
Le script SQL suivant permet de créer la base de données ***calendar*** et la table ***tasks***.

```sql
CREATE DATABASE calendar;
USE calendar;
CREATE TABLE tasks (
    id INT NOT NULL AUTO_INCREMENT,
    task VARCHAR(255) NOT NULL,
    PRIMARY KEY (id)
);
```

Nous pouvons maintenant insérer des données dans la table ***tasks***.

```sql
INSERT INTO tasks (task) VALUES ('Faire les courses');
INSERT INTO tasks (task) VALUES ('Faire la vaisselle');
```
![img_1.png](img_1.png)

## 2. Web services PHP
Nous allons maintenant créer un web service PHP appelé `getalltasks.php` qui permet de récupérer les données de la table ***tasks***.
```php
<?php
  DEFINE('DB_USERNAME', 'root');
  DEFINE('DB_PASSWORD', '');
  DEFINE('DB_HOST', 'localhost');
  DEFINE('DB_DATABASE', 'calendar');

  $mysqli = new mysqli(DB_HOST, DB_USERNAME, DB_PASSWORD, DB_DATABASE);

  if (mysqli_connect_error())
   {
    die('Connect Error ('.mysqli_connect_errno().') '.mysqli_connect_error());
    echo "Connection failed";
   }
  else
   {
    $sql="select * from tasks";
    $result=$mysqli->query($sql);
    $array=array();
      while ($raw=$result->fetch_assoc())
       {
        array_push($array,$raw);
        }
      echo json_encode($array);
    }

  $mysqli->close();
 ?>
```
Nous allons maintenant créer un web service PHP appelé `inserttask.php` qui permet d'ajouter une tâche dans la table ***tasks***.

```php
<?php
  DEFINE('DB_USERNAME', 'root');
  DEFINE('DB_PASSWORD', '');
  DEFINE('DB_HOST', 'localhost');
  DEFINE('DB_DATABASE', 'calendar');

  $mysqli = new mysqli(DB_HOST, DB_USERNAME, DB_PASSWORD, DB_DATABASE);
  if (mysqli_connect_error())
  {
    die('Connect Error ('.mysqli_connect_errno().') '.mysqli_connect_error());
    echo "Connection failed";
  }

    $task=$_POST['task'];
    $sql="INSERT INTO tasks(task) values('$task')";
    $result=$mysqli->query($sql);

      $resultJson["result"]=$result;
      echo json_encode($resultJson);

  $mysqli->close();
 ?>
```

On insère ces deux fichiers dans le répertoire ***htdocs*** du serveur ***Apache***.

![img_2.png](img_2.png)
