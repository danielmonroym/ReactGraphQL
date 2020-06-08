# ReactGraphQL

# Health Tracking App with React, GraphQL, and TypeORM




**Prerequisitos:** 
[Node.js](https://nodejs.org/).

> [Okta](https://developer.okta.com/) has Authentication and User Management APIs that reduce development time with instant-on, scalable user infrastructure. Okta's intuitive API and expert support make it easy for developers to authenticate, manage, and secure users and roles in any application.


## Getting Started

Para instalar correctamente la aplicación siga estos pasos:

```bash
git clone https://github.com/danielmonroym/ReactGraphQL.git
cd ReactGraphQL
```

para correr la API GraphQL ingrese a la carpeta   `graphql-api` desde la terminal e instale las dependencias


```bash
npm i
```

PAra crear la base de datos MySQL dirigirse a donde está instalado MySQL e ingresar los siguientes comandos:

```bash
mysql -u root -p
create database healthpoints;
use healthpoints;
grant all privileges on *.* to 'health'@'localhost' identified by 'points';
```
Cuando se cree la base de datos, empiece la aplicación con  `npm start`

Para correr el cliente, dirigirse a la carpeta react-apolloGQ y correr:

```bash
npm i
npm start
```


### Configuración Okta
Crear o ingresar a una cuenta Okta ((https://developer.okta.com/signup/) )  y navegar hacia  **Applications** > **Add Application**. Click en **Single-Page App**, click **Next**, y darle un nombre a nuestra aplicación. Click **Done**.

#### Configuración GraphQL con Okta

Abrir `graphql-api/index.ts` y reemplazar `{yourOktadomain}` and `{yourClientId}` en el siguente bloque de codigo. 

```ts
const oktaJwtVerifier = new OktaJwtVerifier({
  clientId: '{yourClientId}',
  issuer: 'https://{yourOktaDomain}/oauth2/default'
});
```

**NOTA:** el valor de `{yourOktaDomain}` debe de ser algo como `dev-123456.okta`. Asegurese de no incluir M `-admin` o dos`.com` en el valor

#### Configuracion Cliente

Para el cliente, ingrese el `issuer` y `client_id` en `react-apolloGQ/src/App.js` igual que en el paso pasado.

```js
class App extends Component {
  render() {
    return (
      <Router>
        <Security issuer='https://{yourOktaDomain}.com/oauth2/default'
                  client_id='{yourClientId}'
                  redirect_uri={window.location.origin + '/implicit/callback'}
                  onAuthRequired={onAuthRequired}>
          <Route path='/' exact={true} component={Home}/>
          <SecureRoute path='/points' component={Points}/>
          <Route path='/login' render={() => <Login baseUrl='https://{yourOktaDomain}.com'/>}/>
          <Route path='/implicit/callback' component={ImplicitCallback}/>
        </Security>
      </Router>
    );
  }
}
```


