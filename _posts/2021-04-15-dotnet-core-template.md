---
layout: post
title: Dotnet core template! 🧾
category: posts
tags: [dotnet, core, dev, study, template]
image: /assets/img/posts/dotnet.webp
accent_image: 
  background: url('/assets/img/posts/dotnet-small.jpg') center 
  overlay: true
description: >
  Como criar um template de dotnet core para ser utilizado via linha de comando ou visual studio.
invert_sidebar: false
---

Padronizar o código em um time ou até diversos times pode ser um desafio, uma das facilidades que o dotnet oferece é a criação de templates, com eles é possível gerar uma estrutura sugerida, facilitando o inicio de um novo projeto. 

Antigamente era necessário utilizar uma nomenclatura especifica para o namespace ou variáveis que precisavam ser substituidas na hora de gerar o template, isso dificultava todo o processo de testar, construir e depois gerar o template, em novas versões isso já não é mais necessário, simplificando bastante a geração dos templates.

Para começar vamos precisar de alguns comandos, basicamente vamos criar uma pasta para o projeto e o arquivo da solution ```.sln```.

```bash
mkdir DotnetTemplateTest
cd DotnetTemplateTest
dotnet new sln -n DotnetTemplateTest
```

Logo em seguida iremos criar a pasta ```Content```, nela criaremos o projeto inicial e as configurações do template, nesse caso usarei uma ```Console Application``` mas você pode alterar para como deseja iniciar seu projeto de template.

```bash
mkdir Content
cd Content
dotnet new console -o DotnetTemplateTest
cd ..
dotnet sln add Content/DotnetTemplateTest
```

Até agora temos uma estrutura de pastas assim:

```
└───DotnetTemplateTest
    │   DotnetTemplateTest.sln
    │   Content/
    │
    └───DotnetTemplateTest
          DotnetTemplateTest.csproj
          Program.cs
```

Agora já podemos executar o projeto para testar se está tudo funcionando até aqui com o comando:

```
dotnet run --project Content/DotnetTemplateTest
```

E vamos fazer uma simples alteração para demonstrar como funciona o replace ao gerar um novo template, nesse caso irei substituir o ```Hello, World!``` para ```Hello, ```**```DotnetTemplateTest```**```!```. Note que coloquei o mesmo nome que chamei o projeto e a solution propositalmente para ser substituido!

Tudo pronto, nosso projeto está funcionando, agora chegou a hora de gerar o template, para isso vamos criar o arquivo ```template.json``` no seguinte caminho ```Content/.template.config``` com o seguinte conteudo:

```json
{
  "$schema": "http://json.schemastore.org/template",
  "author": "Luigi Henrick",                              // Seu nome aqui
  "classifications": [ "Common", "Console" ],             // Tags para encontrar template
  "identity": "LuigiHenrick.DotnetTemplateTest.CSharp",   // Identificador para o template
  "name": "Dotnet Template Test",                         // Nome como será exibido na lista
  "shortName": "templatetest",                            // Alias para usar via linha de comando
  "sourceName": "DotnetTemplateTest"                      // Nome do projeto (Replace será por esse nome)
}
```

Com o arquivo salvo, já podemos instalar e testar o template com os seguintes comandos:

```
dotnet new --install Content/DotnetTemplateTest/
dotnet new templatetest -o NewProject
dotnet run --project NewProject/NewProject.csproj
```

Com tudo certo ja podemos remover o projeto criado para validar o template ```rm -r NewProject``` e feito, agora já está pronto o template para ser utilizado.