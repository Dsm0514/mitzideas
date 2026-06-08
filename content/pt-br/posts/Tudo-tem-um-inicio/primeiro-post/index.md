---
title: "Tudo tem um início"
slug: "tudo-tem-um-inicio"
date: 2026-05-14T10:00:00-03:00
draft: false
categories: ["Início"]
series: []
description: "O início do MitzIdeas."
translationKey: "primeiro-post"
---

Olá! Meu nome é Davi Schmitz, tenho 21 anos (intencionalmente publiquei este primeiro post no dia do meu aniversário), sou um desenvolvedor Junior que atualmente trabalha com o framework Django e curso Ciências da Computação.

<small>Achei necessário me introduzir aqui porque irei atualizar a página "Sobre" ao longo do tempo, então essa é uma introdução que provavelmente ficará desatualizada em breve.</small>

---

**MitzIdeas** é meu blog — meu espaço para registrar ideias, estudos e experiências.

Este blog é um registro da minha vida: carreira, estudos, hobbies, livros que leio e tudo mais que me parecer interessante compartilhar.

O objetivo principal é que, por meio desse projeto, eu consiga consolidar o conhecimento que obtenho através dos estudos, visto que aprendemos muito ao tentar ensinar ou explicar para outra pessoa. Eu também quero desenvolver minha escrita, algo que há tempos desejo fazer. Além disso, como não vejo esse projeto como sendo de curto prazo, o que eu escrever aqui deve servir como um histórico para o meu eu do futuro lembrar e entender toda a trajetória percorrida. Meus erros, acertos, ideias... tudo ficará salvo.

Ainda sobre a minha escrita, eu sei que ainda escrevo muito mal. Frequentemente eu mudo de assunto e acabo voltando para a minha ideia de antes. Essa característica já é quase uma assinatura minha. Provavelmente irei utilizar IAs para que ajudem a corrigir meus erros gramaticais e sugiram melhorias no meu texto, entre outras coisas, com o objetivo de ser um escritor melhor. Mas uma coisa eu garanto que *NUNCA* irá acontecer: eu nunca irei permitir que as IAs batam o martelo de como meu texto final ficará. Acima de tudo, quero depositar a minha identidade em cada post. A Internet já está cheia de textos rasos gerados com IA, algo que eu realmente não aguento mais ver por aí.


---

Esta é a primeira publicação do meu blog e eu não poderia perder a oportunidade de contar um pouco sobre como ele foi criado. Se você é brasileiro(a) e da área de tecnologia, provavelmente reconheceu o visual deste blog de algum lugar. Eu acompanho o Fabio Akita há um tempo porque o conteúdo dele é essencial para quem quer aprender de verdade sobre programação, e utilizei basicamente as mesmas ferramentas que ele para criar o meu próprio blog. Para falar a verdade, eu não fazia ideia de como usar tais ferramentas, e ainda estou aprendendo, mas como estamos em uma época em que já conseguimos produzir muita coisa legal com o auxílio de LLMs, o Claude AI foi de grande ajuda para deixar este blog com a aparência que eu desejava.

Conhecendo a mim mesmo, é bem provável que eu realize ainda mais alterações no futuro, então decidi guardar aqui um screenshot de como foi a primeira versão da página inicial do blog.


{{< figure src="MitzIdeasHome.png" title="A primeira versão da página inicial do MitzIdeas (Maio de 2026)." alt="Captura de tela da página inicial do blog" >}}

Contudo, o que mais interessa aqui é o que está por trás da estrutura, o que possibilita que criemos posts da forma que quisermos, de forma rápida, mantendo tudo organizado. Para isso, vou tentar dar uma pequena introdução sobre RSS, Hextra e Hugo, que é o que está por trás disso tudo. Também irei explicar sobre a tradução ou internacionalização do blog, mais especificamente, como eu traduzo a estrutura do blog e o conteúdo das publicações.

### Entendendo RSS

RSS é a sigla para *Really Simple Syndication*. De forma simplificada, é um arquivo XML que lista suas postagens recentes, permitindo que leitores de feed como o Feedly ou NetNewsWire saibam que você postou algo sem precisarem visitar seu site.

### Hugo

O Hugo é um gerador de sites estáticos (SSG - Static Site Generator) escrito em Go. Cada parte deste blog é um arquivo Markdown (.md) que o Hugo processa através dos templates HTML que eu tenho em uma pasta chamada *layouts* e "cospe" um site estático (HTML/CSS/JS puro) na pasta *public*. É extremamente rápido (gera centenas de páginas em milissegundos) e seguro, pois não tem banco de dados rodando em tempo real no servidor. Em razão de tudo ser estático, eu consigo hospedá-lo em plataformas de forma gratuita, que era justamente o que eu buscava: poder compartilhar minhas ideias sem ter que pagar hospedagem.

### Hextra

O Hextra é o tema ou framework de design que eu uso por cima do Hugo. Ele facilita muitas coisas, como a criação da barra de buscas de publicações, a configuração de *Dark Mode* e *Light Mode* e a organização das seções. Eu o customizei para ter um visual mais "clean" e minimalista, já que a intenção era que o conteúdo recebesse quase toda a atenção do leitor; eu sempre tive problemas com sites ou interfaces que mostram muita informação na tela. 

### Internacionalização e tradução

Você deve ter percebido que o site e os posts são traduzidos para outros idiomas. Para isso, utilizei o *Multilingual Mode* do Hugo. Lembra que comentei que cada parte do blog é um arquivo Markdown? Eu salvo cada um deles com as informações de idioma na extensão, como *.en.md* para o inglês. Exemplos: *primeiro-post.en.md* para inglês ou *primeiro-post.fr.md* para francês. O Hugo cria árvores de diretórios separadas para cada idioma no build final (/en/, /es/, etc.). Além disso, uso arquivos na pasta *i18n/* ou configurações no *hugo.toml* para traduzir botões como "Voltar" ou "Próximo".

---
Montar toda essa estrutura do zero já foi um grande desafio, então vou finalizar este primeiro post por aqui para mantê-lo mais simples. Conforme eu for ganhando experiência e fluência com essas ferramentas, os próximos textos serão cada vez mais elaborados.
