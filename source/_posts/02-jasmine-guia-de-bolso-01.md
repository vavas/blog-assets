title: Jasmine Guia de Bolso - Parte 1
date: 2014-02-17 18:34:00
tags: jasmine 
---

![Jasmine](http://i.imgur.com/flSPyah.jpg)

Neste post vamos fazer uma síntese da documentação do framework de testes para código JavaScript [Jasmine](http://eoop.github.io/jasmine-br-docs/), listando seus métodos embutidos e citando sucintamente o que cada um faz.

Como somos desenvolvedores e ler código é a nossa praia, vamos a diversão!

> **Talk is cheap. Show me the code**!  
> - Linus Torvalds

```js
// Sintaxe padrão de um conjunto de testes
describe("Um conjunto", function () {
    it("contém uma especificação com uma expectativa", function () {
        expect(true).toBe(true);
    });
});

// Outro exemplo de como utilizar o conjunto de testes
describe("Um conjunto é apenas uma função", function () {
    var a;

    it("e isto é uma especificação", function () {
        a = true;

        expect(a).toBe(true);
    });
});

// Entendendo os Comparadores
describe("O comparador 'toBe' compara com ===", function () {

    it("e tem um caso positivo", function () {
        expect(true).toBe(true);
    });

    it("e também um caso negativo", function () {
        expect(false).not.toBe(true);
    });
});

// Mais Comparadores... 
// Lembrando que estes devem estar dentro de it("mensagem", function () {...});

// O comparador 'toBe' compara com ===
expect(a).toBe(b);

// O comparador '.not.toBe' compara com !==
expect(a).not.toBe(null); 

// O comparador 'toEqual' trabalha com simples literais e variáveis
var a = 12;
expect(a).toEqual(12);

// E também pode trabalhar com objetos
var foo = {
    a: 12,
    b: 34
};

var bar = {
    a: 12,
    b: 34
};
expect(foo).toEqual(bar);

// O comparador 'toMatch' é para expressões regulares
var message = "foo bar baz";

expect(message).toMatch(/bar/);
expect(message).toMatch("bar");
expect(message).not.toMatch(/quux/);

// O comparador 'toBeDefined' compara com 'undefined'
expect(a.foo).toBeDefined();
expect(a.bar).not.toBeDefined();

// O comparador 'toBeUndefined' compara com 'undefined'
var a = {
    foo: "foo"
};

expect(a.foo).not.toBeUndefined();
expect(a.bar).toBeUndefined();

// O comparador 'toBeNull' compara com null
var a = null;
var foo = "foo";

expect(null).toBeNull();
expect(a).toBeNull();
expect(foo).not.toBeNull();

// O comparador 'toBeTruthy' é para testar o operador booleano
var a, foo = "foo";

expect(foo).toBeTruthy();
expect(a).not.ToBeTruthy();

// O comparador 'toBeFalsy' é para testar o operador booleano
var a, foo = "foo";

expect(a).toBeFalsy();
expect(foo).not.toBeFalsy();

// O comparador 'toContain' é para encontrar um item em um Array
var a = ["foo", "bar", "baz"];

expect(a).toContain("bar");
expect(a).not.toContain("quux");

// O comparador 'toBeLessThan' é para comparações matemáticas
var pi = 3.1415926,
    e  = 2.78;

expect(e).toBeLessThan(pi);
expect(pi).not.toBeLessThan(e);

// O comparador 'toBeGreaterThan' é para comparações matemáticas
 var pi = 3.1415926,
    e  = 2.78;

expect(pi).toBeGreaterThan(e);
expect(e).not.toBeGreaterThan(pi);

// O comparador 'toBeCloseTo' é para comparações matemáticas precisas
var pi = 3.1415926,
    e  = 2.78;

expected(pi).not.toBeCloseTo(e, 2);
expected(pi).toBeCloseTo(e, 0);

// O comparador 'toThrow' é para testar se uma função lançou uma exceção
var foo = function () {
    return 1 + 2;
};
var bar = function () {
    a + 1;
};

expect(foo).not.toThrow();
expect(bar).toThrow();

/*
A função describe é para o agrupamento de especificações relacionadas. 
O parâmetro string é para nomear a coleção de especificações, e vai ser 
concatenado com estas para fazer o nome completo da especificação.

Para ajudar a manter seu conjunto de testes nos padrões DRY 
(Don't Repeat Yourself - Não se repita), o Jasmine fornece as 
funções globais beforeEach e afterEach.

A função beforeEach é chamada uma vez antes de cada especificação quando 
describe é rodado, e a função afterEach é chamada depois de cada especulação. 
A variável em teste é definida no escopo de nível superior - 
o bloco describe - e o código de inicialização é movido para dentro da 
função beforeEach. A função afterEach reinicia a variável antes de continuar.
*/

describe("Uma especificação (com configuração e subdivisão)", function () {
    var foo;

    beforeEach(function () {
        foo = 0;
        foo += 1;
    });

    afterEach(function () {
        foo = 0;
    });

    it("é somente uma função, então pode conter qualquer código", function () {
        expect(foo).toEqual(1);
    });

    it("pode ter mais de uma expectativa", function () {
        expect(too).toEqual(1);
        expect(true).toEqual(true);
    });
});

/* 
Também é possível fazer o aninhamento de blocos describe. 
Para mais, veja a documentação.

É possível desabilitar conjuntos e especificações, renomeando-os, respectivamente, 
para xdescribe e xit
*/

xdescribe("Uma especificação", function () {
    var foo;

    it("é apenas uma função, então pode conter qualquer código", function () {
        expect(foo).toEqual(1);
    });
});

/*
Qualquer especificação declarada com xit é marcada como pendente. 

Especificações pendentes não rodam, mas seus nomes vão aparecer 
nos resultados como pending.

Se você chamar a função pending em qualquer lugar do corpo da 
especificação, não importa as expectativas, a especificação 
vai ser marcada como pendente.
*/

describe("Especulações pendentes", function () {

    xit("podem ser declaradas 'xit'", function () {
        expect(true).toBe(false);
    });

    it("pode ser declarado com 'it' mas sem uma função");

    it("pode ser declarado chamando 'pending' no corpo da especulação", function () {
        expect(true).toBe(false);
        pending();
    });
});
```

E assim encerramos a primeira parte desta síntese da documentação do Jasmine!