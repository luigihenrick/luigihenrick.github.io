---
layout: post
title: Cloudflared - Exponha seus serviços publicamente! 🌐
category: posts
lang: pt
tags: [dev, host, local, lab]
image: /assets/img/posts/cloudflared.jpg
accent_image: 
  background: url('/assets/img/posts/cloudflare-logo.png') center/10%
  overlay: true
description: >
  Serviço gratuito que permite que você compartilhe seus serviços com qualquer pessoa.
invert_sidebar: false
---

Após começar a ter seus apps self hosted e explorar mais desse universo, em algum momento irá se deparar com o problema de ter tudo isso somente disponível em sua rede local, talvez queira compartilhar com algum amigo um novo app que adicionou, ou como no exemplo do cofre de senha, no post do Bitwarden, que torna o app muito mais simples de utilizar, e é isso que irei abordar hoje.

Tudo que irá precisar para tornar seu app acessivel de qualquer lugar é um domínio, haviam algumas alternativas para registro de domínio gratuito porém acredito que nenhuma está disponível no momento, então recomendo domínios no porkbun ou namecheap, que costumam ter promoções e diversos domínios disponíveis.

O cloudflare tunnel funciona semelhante ao ngrok, para quem já teve com contato com ele, a grande vantagem do serviço é que ele cria um tunnel entre sua rede local e a internet, até mesmo adicionando protocolo de encriptação (HTTPS) entre a cloudflare e o cliente conectado, isso sem precisar abrir nenhuma porta do roteador, configurar firewall, ou lidar com outras configurações complexas de segurança.

Para iniciar, se já leu outros posts deve saber onde isso começa, sim, docker! O próprio site da cloudflare disponibiliza o commando para criação do tunnel já com seu token associado a ele se seguir este caminho dentro do dashboard do site:

<p>
<img src="https://raw.githubusercontent.com/luigihenrick/luigihenrick.github.io/refs/heads/master/assets/img/posts/cloudflare-home.png">
<img src="https://raw.githubusercontent.com/luigihenrick/luigihenrick.github.io/refs/heads/master/assets/img/posts/cloudflare-zero-trust.png">
</p>

At the last step you should get the token to create your docker container
```
docker run cloudflare/cloudflared:latest tunnel --no-autoupdate run --token eyJhIjoiYjE....
```

Após criar o seu tunnel deve ser possível ver que ele está online e saudável, como na imagem a seguir:
<p>
<img src="https://raw.githubusercontent.com/luigihenrick/luigihenrick.github.io/refs/heads/master/assets/img/posts/cloudflare-healthy.png">
</p>

O próximo passo é criar uma regra de encaminhamento entre a internet e sua rede local, para isso siga este caminho:

3 pontos > Configure > Public Hostname

<p>
<img src="https://raw.githubusercontent.com/luigihenrick/luigihenrick.github.io/refs/heads/master/assets/img/posts/cloudflare-forward.png">
</p>

E para configurar seu primeiro endpoint você deve preencher as seguintes informações:

<p>
<img src="https://raw.githubusercontent.com/luigihenrick/luigihenrick.github.io/refs/heads/master/assets/img/posts/cloudflare-public-hostname.png">
</p>


Após feitas as configurações já deve estar disponível o serviço acessando o link que configurei na sessão de domínio, uma observação somente é que não consegui utilizar o tunnel para expor um serviço UDP, somente HTTP e TCP, caso conheça alguma alternativa por favor deixe nos comentários e, até a próxima! 🎇