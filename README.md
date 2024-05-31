# API Web - Tâches longues

Une API REST accepte une tâche de longue durée via la méthode POST de son URL.

```json
HTTP POST: http://localhost:9000/tasks
{
    "name": "test-task"
}
```

L'API renvoie un coprs de réponse vide mais l'en-tête de la réponse contient un paramètre Location. Ce paramètre est l'URL d'un topic Kafka. A chaque reque un nouveau topic est créé par programme.

Dans l'exemple de réponse suivant, eaa98908-0115-4cd7-8884-0b2155f7811e est un identifiant généré dynamiquement qui sera le nom d'un nouveau topic.
Le client peut interroger le topic directement ou bien appeler l'URL de l'API fournie qui récupérera l'état de la tâche.

```json
HTTP 201
Location: http://localhost:9000/tasks/eaa98908-0115-4cd7-8884-0b2155f7811e/progress
```

L'API interroge le topic et renvoie l'état actuel de la tâche de longue durée :

```json
HTTP GET: http://localhost:9000/tasks/eaa98908-0115-4cd7-8884-0b2155f7811e/progress
{
  "taskId": "eaa98908-0115-4cd7-8884-0b2155f7811e",
  "taskName": "test-task",
  "percentageComplete": 100.0,
  "status": "FINISHED",
}
```

En général Lorsqu'une tâche se termine, le résultat est communiqué dans la même réponse, ou bien dans une autre URL qui renverra le résultat de l'exécution de la tâche.

```json
HTTP GET: http://localhost:9000/tasks/eaa98908-0115-4cd7-8884-0b2155f7811e/progress

{
  "taskId": "eaa98908-0115-4cd7-8884-0b2155f7811e",
  "taskName": "test-task",
  "percentageComplete": 100.0,
  "status": "FINISHED",
  "resultUrl": "http://localhost:9000/tasks/eaa98908-0115-4cd7-8884-0b2155f7811e/result"
}
```

```json
HTTP GET: http://localhost:9000/tasks/eaa98908-0115-4cd7-8884-0b2155f7811e/result

{
 "taskId": "eaa98908-0115-4cd7-8884-0b2155f7811e",
  "taskName": "test-task",
  "status": "FINISHED",
 "sys-log-location":"/log/....",
 "err-log-location":"/log/...."
}
```
