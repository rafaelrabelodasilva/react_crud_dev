# O projeto ✨

Em andamento 🔨

### Passo 1: Criar o projeto e instalar o Material UI

Para criarmos o projeto iremos utilizar o [Vite](https://vitejs.dev/), que é um bundler extremamente rápido e simples de utilizar. Para instalar o Vite, execute o seguinte comando:

```sh
npm create vite@latest react-crud-admin -- --template react-ts
```

Depois do projeto criado, entre na pasta do projeto e instale o [Material UI](https://mui.com/material-ui/getting-started/installation/):

```sh
cd react-crud-admin
npm install @mui/material @emotion/react @emotion/styled @fontsource/roboto @mui/icons-material @mui/x-data-grid @mui/x-date-pickers date-fns axios
```

Criar o arquivo [`src/theme.ts`](./src/theme.ts):

```ts
import { ptBR as MaterialLocale } from "@mui/material/locale"
import { createTheme } from "@mui/material/styles"
import { ptBR as DataGridLocale } from "@mui/x-data-grid"

export const theme = createTheme(
  {
    palette: {
      mode: "dark",
    },
  },
  DataGridLocale,
  MaterialLocale
)
```

E ajustar o arquivo [`src/main.tsx`](./src/main.tsx):

```tsx
import { Box, Container } from "@mui/material"
import CssBaseline from "@mui/material/CssBaseline"
import { ThemeProvider } from "@mui/material/styles"
import { LocalizationProvider } from "@mui/x-date-pickers"
import { AdapterDateFns } from "@mui/x-date-pickers/AdapterDateFns"
import ptBR from "date-fns/locale/pt-BR"
import React from "react"
import ReactDOM from "react-dom/client"

import { theme } from "./theme.ts"

import App from "./App.tsx"

import "@fontsource/roboto/300.css"
import "@fontsource/roboto/400.css"
import "@fontsource/roboto/500.css"
import "@fontsource/roboto/700.css"

import "./index.css"

ReactDOM.createRoot(document.getElementById("root") as HTMLElement).render(
  <React.StrictMode>
    <ThemeProvider theme={theme}>
      <LocalizationProvider dateAdapter={AdapterDateFns} adapterLocale={ptBR}>
        <CssBaseline />
        <Container maxWidth="lg">
          <Box sx={{ my: 4 }}>
            <App />
          </Box>
        </Container>
      </LocalizationProvider>
    </ThemeProvider>
  </React.StrictMode>
)
```

Nisso já não precisaremos mais dos arquivos e podemos excluí-los:

- `src/App.css`
- `src/index.css`
- `src/assets/logo.svg`
- `src/public/vite.svg`

Ajustar o `title` e remover `favicon` do [`index.html`](./index.html):

```html
<!DOCTYPE html>
<html lang="pt-BR">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>React CRUD // Admin Panel</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.tsx"></script>
  </body>
</html>
```

E para que possamos testar se o Material UI está funcionando, vamos adicionar um componente de botão no arquivo [`src/App.tsx`](./src/App.tsx):

```tsx
import { Button } from "@mui/material"

export default function App() {
  return (
    <div>
      <Button variant="contained">Hello World</Button>
    </div>
  )
}
```

E pronto! Já temos algo minimamente funcional:

![Hello World](./docs/hello-world.png)

## Passo 2: Estrutura de base do CRUD

Agora que já fizemos as instalações iniciais, chegou o momento de criar a estrutura de base do nosso CRUD.

Sendo assim, inicie instalando o [React Router](https://reactrouter.com/):

```sh
npm install react-router-dom
```

Crie o arquivo [`src/routes.tsx`](./src/routes.tsx):

```tsx
import { Route, Routes } from "react-router-dom"

import UserCreate from "./pages/Users/Create"
import UserEdit from "./pages/Users/Edit"
import UserList from "./pages/Users/List"

export function AppRoutes() {
  return (
    <Routes>
      <Route path="/users">
        <Route path="/users" element={<UserList />} />
        <Route path="/users/new" element={<UserCreate />} />
        <Route path="/users/:id" element={<UserEdit />} />
      </Route>
    </Routes>
  )
}
```

E ajuste o arquivo [`src/App.tsx`](./src/App.tsx):

```tsx
import { BrowserRouter } from "react-router-dom"

import { AppRoutes } from "./routes"

export default function App() {
  return (
    <BrowserRouter>
      <AppRoutes />
    </BrowserRouter>
  )
}
```

E por fim, vamos criar a pasta [`src/pages/Users`](./src/pages/Users/) e dentro dela vamos criar nas próximas sessões uma estrutura organizada de componentes, sendo eles:

- `pages/Users/List.tsx`: Componente que irá listar os usuários cadastrados.
- `pages/Users/Create.tsx`: Componente que irá criar um novo usuário.
- `pages/Users/Edit.tsx`: Componente que irá editar um usuário existente.
- `pages/Users/components/Form.tsx`: Componente que irá conter o formulário de cadastro e edição de usuários.
- `pages/Users/components/Grid.tsx`: Componente que irá conter a tabela de listagem de usuários.
- `pages/Users/schemas/UserSchema.ts`: Schema de validação do formulário de cadastro e edição de usuários.
- `pages/Users/types/User.ts`: Tipagem do usuário.

Por hora, para cada componente, vamos criar apenas a estrutura mínima para que o React não deixe de funcionar, por exemplo:

```tsx
export default function List() {
  return <>List</>
}
```

E a tipagem de usuário:

```ts
export type User = {
  id: string
  fullName: string
  document: string
  birthDate: Date
  email: string
  emailVerified: boolean
  mobile: string
  zipCode: string
  addressName: string
  number: string
  complement: string
  neighborhood: string
  city: string
  state: string
}
```
