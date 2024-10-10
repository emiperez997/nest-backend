# Backend Nest.js - Anotaciones

## Detalles

Este proyecto es meramente de práctica. La idea es crear un backend de API REST con Nest.js y PostgreSQL. El objetivo es que el backend sea lo más simple posible, sin necesidad de configurar un servidor web ni de un framework de front-end. A medida que lo vaya creando, voy a ir agregando anotaciones sobre conceptos de Backend. La idea es profundizar en Backend utilizando este Framework.

Para este proyecto voy a seguir la guía de BluuWeb, que es una guía para crear un backend de API REST con Nest.js y PostgreSQL.

# Tecnologías

- Nest.js
- MySQL
- TypeScript
- Docker
- Git
- IDX

# Requisitos previos

## Herramientas

- [Node.js](https://nodejs.org/en)
- [Nvm](https://github.com/nvm-sh/nvm) (Opcional)
- [Visual Studio Code](https://code.visualstudio.com/Download)
- [Docker](https://docs.docker.com/get-docker/)

## Tecnologías

- Lógica de Programación
- JavaScript
- TypeScript
- SQL Básico
- Git
- Docker básico

> [!TIP]
> En caso de que no quieras instalar Visual Studio Code, podes usar alternativamente IDX de Google el cual vamos a configurar también en este tutorial

# Secciones

- [Introducción](https://bluuweb.dev/nestjs/crud-mysql.html)
- [Autenticación](https://bluuweb.dev/nestjs/jwt-auth.html)
- [Autorización](https://bluuweb.dev/nestjs/guard.html)
- [Deploy](https://bluuweb.dev/nestjs/deploy.html)
- [Documentación](https://bluuweb.dev/nestjs/openapi.html)
- (Bonus) [Frontend con Next.js](https://bluuweb.dev/nestjs/frontend-next.html)

# Preparando el entorno (IDX)

Para trabajar dentro de IDX de Google, necesitamos loguearnos con nuestra cuenta. Dentro del `home` de IDX, vamos a crear un espacio de trabajo (`workspace`) en blanco (`See all templates/Misc/Blank Workspace`).

IDX nos ofrece personalización de nuestro espacio de trabajo. Podemos modificarlo configurando el archivo `dev.nix`. Además, en este repositorio voy a dejar mi configuración del editor en el archivo `settings.json`.

**Nuestro archivo dev.nix**:

```nix
{ pkgs, ... } : {
  channel = "stable-24.05"; # Servidor a utilizar

  # Lista de paquetes a instalar
  packages = [
    pkgs.nodejs_20
  ];

  # Variables de entorno
  env = {}

  # Habilitar docker para levantar nuestra base de datos
  services.docker.enable = true;

  # Configuración especial de idx
  idx = {
    # Extensiones que utilizaremos
    # También están disponibles para VS Code
    extensions = [
      "vscodevim.vim"
      "BeardedBear.beardedicons"
      "esbenp.prettier-vscode"
      "Llam4u.nerdtree"
      "Prisma.Prisma"
      "teabyii.ayu"
      "humao.rest-client"
      "Supermaven.supermaven"
    ];
  };

  # Las previews son para levantar tu proyecto y obtener una vista previa
  previews = {
    enable = false;
  }


  # Configuración del espacio de trabajo
  workspace = {
    # Corre cuando se crea el espacio de trabajo
    onCreate = {
      # Por defecto, se crea esta configuración
      default.openFiles = [ ".idx/dev.nix" "README.md" ];
    };

    # Corre cuando inicia el espacio de trabajo
    onStart = {
      # Aqui podemos definir comandos com por ejemplo:
      # npm-install = "npm install";
      # git-pull = "git pull";
    };
  };
}
```

Una vez configurado nuestro entorno de IDX, vamos a proceder a instalar los paquetes que necesitamos.

> [!NOTE]
> En caso de que quieras seguir esta guía con VS Code, podés saltearte los pasos anteriores

```bash
# Instalar pnpm
npm i -g pnpm

# Instalar el CLI de nestjs
npm i -g @nestjs/cli
```

# Creación del proyecto

## Usando el _cli_ de _nest_

Una vez instalado todo y preprando nuestro entorno de trabajo, vamos a crear nuestro proyecto con el `cli` de `nest.js`

```bash
> nest new backend

? Which package manager would you ❤️  to use? pnpm
```

## Levantando nuestra base de datos

Para este curso vamos a utilizar _Docker_ para levantar la base de datos

> [!NOTE]
> Docker es un manejador de contenedores. Algo muy similar a un _virtual machine_ para ejecutar aplicaciones.

- Creamos nuestro archivo `docker-compose.yml`

```yml
services:
  mysql:
    image: mysql:8.0
    container_name: mysql_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: db_crud
      MYSQL_USER: user_crud
      MYSQL_PASSWORD: root
    volumes:
      - ./mysql:/var/lib/mysql
    ports:
      - "3307:3306
```

- Ejecutamos el comando para levantar la base de datos

```bash
docker compose up -d
```

- Esto va a crear una carpeta llamada `mysql` que va a contener nuestra base de datos. Para evitar errores, vamos a agregarla en el `.gitignore`
