# bug-free-journey
# Comparación de la API REST de GitHub y GraphQL API

Obtén información sobre las API de GitHub para ampliar y personalizar la experiencia de GitHub.

## Acerca de las API de GitHub

GitHub proporciona dos API: una API REST y una API GraphQL. Puedes interactuar con ambas API mediante GitHub CLI, curl, las bibliotecas oficiales de Octokit y bibliotecas de terceros. En ocasiones, podría admitirse una característica en una de las API, pero no en la otra.

Debes usar la API que se adapte mejor a tus necesidades y que te resulte más cómoda de usar. No es necesario usar exclusivamente una de las dos API. Los identificadores de nodo permiten cambiar entre la API REST y la API GraphQL. Para más información, consulta [Utilizar las IDs de nodo globales](/es/graphql/guides/using-global-node-ids).

En este artículo se describen las ventajas de cada API. Para más información sobre GraphQL API, consulta [Acerca de la API de GraphQL](/es/graphql/overview/about-the-graphql-api). Para obtener más información sobre la API REST, consulta [Acerca de la API de REST](/es/rest/about-the-rest-api/about-the-rest-api).

## Selección de la API GraphQL

La API GraphQL devuelve exactamente los datos que se solicitan. GraphQL también devuelve los datos de una estructura previamente conocida en función de la solicitud. En cambio, la API REST devuelve más datos de los que se solicitan, y lo hace en una estructura determinada previamente. También puedes conseguir el equivalente de realizar varias solicitudes de API REST en una única solicitud de GraphQL. La capacidad de realizar menos solicitudes y capturar menos datos hace que GraphQL sea una opción atractiva para los desarrolladores de aplicaciones móviles.

Por ejemplo, para obtener el inicio de sesión de GitHub de diez de tus seguidores y el inicio de sesión de diez seguidores de cada uno de tus seguidores, puedes enviar una única solicitud como la siguiente:

```graphql
{
  viewer {
    followers(first: 10) {
      nodes {
        login
        followers(first: 10) {
          nodes {
            login
          }
        }
      }
    }
  }
}
```

La respuesta será un objeto JSON que sigue la estructura de la solicitud.

Por el contrario, para obtener esta misma información mediante la API REST, primero tendrías que realizar una solicitud a `GET /user/followers`. La API devolvería el inicio de sesión de cada seguidor, junto con otros datos sobre los seguidores que no necesitas. Después, para cada seguidor, tendrías que realizar una solicitud a `GET /users/{username}/followers`. En total, tendrías que realizar 11 solicitudes para obtener la misma información que podrías obtener mediante una única solicitud de GraphQL, y recibirías datos en exceso.

## Selección de la API REST

Dado que las API REST llevan existiendo más tiempo que las API GraphQL, algunos desarrolladores se sienten más cómodos al usar API REST. Dado que las API REST usan verbos y conceptos HTTP estándar, muchos desarrolladores ya están familiarizados con los conceptos básicos para usar la API REST.

Por ejemplo, para crear una incidencia en el repositorio `octocat/Spoon-Knife`, tendrías que enviar una solicitud a `POST /repos/octocat/Spoon-Knife/issues` con un cuerpo de solicitud JSON:

```json
{
  "title": "Bug with feature X",
  "body": "If you do A, then B happens"
}
```

Por el contrario, para crear una incidencia mediante la API GraphQL, tendrías que obtener el identificador de nodo del repositorio `octocat/Spoon-Knife` y, luego, enviar una solicitud como la siguiente:

```graphql
mutation {
  createIssue(
    input: {
      repositoryId: "MDEwOlJlcG9zaXRvcnkxMzAwMTky"
      title: "Bug with feature X"
      body: "If you do A, then B happens"}
  ) {
    issue {
      number
      url
    }
  }
}
```

como viaja el dinero por transferencia 