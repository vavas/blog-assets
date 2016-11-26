title: NodeSchoolCast 01.02 - process.argv
date: 2014-05-02 05:00:00
tags: 
	- nodejs
	- nodeschoolcast
---

![nodeschoolcast 01.02 hello world](http://i.imgur.com/fL8S6ve.png)

Neste episódio iremos aprender a utilizar um dos objetos globais do NodeJS chamado `process`. O exercício de hoje faz a soma de *n* argumentos passados pela linha de comando na hora da execução do programa e os imprimi.

Veja a resolução do exercício aqui!

<iframe width="560" height="315" src="//www.youtube.com/embed/Whp6s06SD3o" frameborder="0" allowfullscreen></iframe>

E aqui o código do nosso programa chamado `sum.js`.

```js
var enter = process.argv;
var sizeEnter = enter.length;
var i = 2;
var result = 0;

for ( ; i < sizeEnter; i += 1) {
  result += +enter[ i ];
}

console.log( result );
```

Você pode ver **todos** os códigos dos exercícios da nossa série [aqui](https://github.com/ericdouglas/nodeschoolcast).