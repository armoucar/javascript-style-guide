[![Gitter](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/airbnb/javascript?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

# Airbnb JavaScript Style Guide() {

*Uma boa abordagem para JavaScript*


## <a name='table-of-contents'>Índice</a>

  1. [Tipos](#types)
  1. [Objetos](#objects)
  1. [Arrays](#arrays)
  1. [Strings](#strings)
  1. [Funções](#functions)
  1. [Propriedades](#properties)
  1. [Variáveis](#variables)
  1. [Hoisting](#hoisting)
  1. [Expressões Condicionais & Comparações](#comparison-operators--equality)
  1. [Blocos](#blocks)
  1. [Comentários](#comments)
  1. [Espaços em branco](#whitespace)
  1. [Vírgulas](#commas)
  1. [Ponto e vírgulas](#semicolons)
  1. [Casting & Coerção de Tipos](#type-casting--coercion)
  1. [Convenções de nomenclatura](#naming-conventions)
  1. [Métodos Acessores](#accessors)
  1. [Construtores](#constructors)
  1. [Eventos](#events)
  1. [Módulos](#modules)
  1. [jQuery](#jquery)
  1. [Compatibilidade ECMAScript 5](#ecmascript-5-compatibility)
  1. [Testes](#testing)
  1. [Performance](#performance)
  1. [Recursos](#resources)
  1. [Empresas utilizando](#in-the-wild)
  1. [Traduções](#translation)
  1. [The JavaScript Style Guide Guide](#the-javascript-style-guide-guide)
  1. [Converse conosco sobre Javascript](#chat-with-us-about-javascript)
  1. [Contribuidores](#contributors)
  1. [Licença](#license)

## <a name='types'>Tipos</a>

  - **Primitivos**: Quando você acessa um tipo primitivo você lida diretamente com seu valor.

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1;
    var bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **Complexos**: Quando você acessa um tipo complexo você lida com a referência para seu valor.

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2];
    var bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ voltar ao topo](#table-of-contents)**

## <a name='objects'>Objetos</a>

  - Use a sintaxe literal para criação de objetos.

    ```javascript
    // ruim
    var item = new Object();

    // bom
    var item = {};
    ```

  - Não use [palavras reservadas](http://es5.github.io/#x7.6.1) como chaves. Não irão funcionar no IE8. [Leia mais](https://github.com/airbnb/javascript/issues/61).

    ```javascript
    // ruim
    var superman = {
      default: { clark: 'kent' },
      private: true
    };

    // bom
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - Use sinônimos legíveis no lugar de palavras reservadas.

    ```javascript
    // ruim
    var superman = {
      class: 'alien'
    };

    // ruim
    var superman = {
      klass: 'alien'
    };

    // bom
    var superman = {
      type: 'alien'
    };
    ```

**[⬆ voltar ao topo](#table-of-contents)**

## <a name='arrays'>Arrays</a>

  - Use a sintaxe literal para a criação de Arrays.

    ```javascript
    // ruim
    var items = new Array();

    // bom
    var items = [];
    ```

  - Use Array#push ao inves de atribuir um item diretamente ao array.

    ```javascript
    var someStack = [];


    // ruim
    someStack[someStack.length] = 'abracadabra';

    // bom
    someStack.push('abracadabra');
    ```

  - Quando precisar copiar um Array utilize Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length;
    var itemsCopy = [];
    var i;

    // ruim
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // bom
    itemsCopy = items.slice();
    ```

  - Para converter um objeto similar a um array para array, utilize Array#slice.

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='strings'>Strings</a>

  - Use aspas simples `''` para strings

    ```javascript
    // ruim
    var name = "Bob Parr";

    // bom
    var name = 'Bob Parr';

    // ruim
    var fullName = "Bob " + this.lastName;

    // bom
    var fullName = 'Bob ' + this.lastName;
    ```

  - Strings maiores que 80 caracteres devem ser escritas em múltiplas linhas e usar concatenação.
  - Nota: Se muito usado, strings longas com concatenação podem impactar na performance. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40)

    ```javascript
    // ruim
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // ruim
    var errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // bom
    var errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  - Quando for construir uma string programaticamente, use Array#join ao invés de concatenação de strings. Principalmente para o IE: [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var items;
    var messages;
    var length;
    var i;

    messages = [{
      state: 'success',
      message: 'This one worked.'
    }, {
      state: 'success',
      message: 'This one worked as well.'
    }, {
      state: 'error',
      message: 'This one did not work.'
    }];

    length = messages.length;

    // ruim
    function inbox(messages) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // bom
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        items[i] = '<li>' + messages[i].message + '</li>';
      }

      return '<ul>' + items.join('') + '</ul>';
    }
    ```

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='functions'>Funções</a>

  - Declarando Funções:

    ```javascript
    // definindo uma função anônima
    var anonymous = function() {
      return true;
    };

    // definindo uma função nomeada
    var named = function named() {
      return true;
    };

    // função imediatamente invocada (IIFE)
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - Nunca declare uma função em um escopo que não seja de uma função (if, while, etc). Ao invés, atribua a função para uma variavel. Os Browsers irão deixar você fazer isso, mas a interpretação disso não é legal. Fazendo isso você pode ter más notícias a qualquer momento.
  - **Nota:** A ECMA-262 define um `bloco` como uma lista de instruções. A declaração de uma função não é uma instrução. [Leia em ECMA-262's](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // ruim
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // bom
    if (currentUser) {
      var test = function test() {
        console.log('Yup.');
      };
    }
    ```

  - Nunca nomeie um parâmetro como `arguments`. Isso sobrescrevá o objeto `arguments` que é passado para cada função.

    ```javascript
    // ruim
    function nope(name, options, arguments) {
      // ...outras implementações...
    }

    // bom
    function yup(name, options, args) {
      // ...outras implementações...
    }
    ```

**[⬆ voltar ao topo](#table-of-contents)**



## <a name='properties'>Properties</a>

  - Use ponto `.` para acessar propriedades.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // ruim
    var isJedi = luke['jedi'];

    // bom
    var isJedi = luke.jedi;
    ```

  - Use colchetes `[]` para acessar propriedades através de uma variável.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='variables'>Variáveis</a>

  - Sempre use `var` para declarar variáveis. Não fazer isso irá resultar em variáveis globais. Devemos evitar poluir o namespace global. O Capitão Planeta já nos alertou disso.

    ```javascript
    // ruim
    superPower = new SuperPower();

    // bom
    var superPower = new SuperPower();
    ```

  - Use somente uma declaração `var` para múltiplas variáveis e declares cada variável em uma nova linha.

    ```javascript
    // ruim
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';

    // bom
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';
    ```

  - Declare as variáveis que você não vai estipular valor por último. É útil no futuro, quando você precisar atribuir valor para ela dependendo do valor da variável já declarada.

    ```javascript
    // ruim
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // ruim
    var i, items = getItems(),
        dragonball,
        goSportsTeam = true,
        len;

    // bom
    var items = getItems(),
        goSportsTeam = true,
        dragonball,
        length,
        i;
    ```

  - Defina variáveis no topo do escopo onde ela se encontra. Isso ajuda a evitar problemas com declaração de variáveis e hoisting.

    ```javascript
    // ruim
    function() {
      test();
      console.log('fazendo qualquer coisa..');

      //..outras implementações..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // bom
    function() {
      var name = getName();

      test();
      console.log('fazendo alguma coisa..');

      //...outras implmementações...

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // ruim
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // bom
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();

      return true;
    }
    ```

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='hoisting'>Hoisting</a>

  - Declarações de váriaveis durante todo o escopo da função são elevadas ao topo função com valor atribuído `undefined`. Esse comportamento é chamado de `hoisting`.

    ```javascript
    // sabemos que isso não irá funcionar (assumindo que
    // não exista uma variável global chamada `notDefined`)
    function example() {
      console.log(notDefined); // => lança uma ReferenceError
    }

    // Declarar uma variável depois de ter referenciado
    // a mesma irá funcionar pelo comportamento do `hoist`
    // Nota: a atribuição do valor `true` não é afetada por `hoisting`.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // O interpretador fez `hoisting` para a declaração
    // da variável. Isso significa que nosso exemplo pode
    // ser reescrito como:
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - Funções anônimas fazem `hoist` para o nome da sua variável, não para a corpo da função.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - Funções nomeadas fazem `hoist` para o nome da variável, não para o nome ou corpo da função.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };


      // O mesmo acontece quando o nome da função
      // é o mesmo da variável.
      function example() {
        console.log(named); // => undefined

        named(); // => TypeError named is not a function

        var named = function named() {
          console.log('named');
        };
      }
    }
    ```

  - Declarações de funções nomeadas fazem `hoist` do nome da função e do seu corpo.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - Para mais informações veja [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) por [Ben Cherry](http://www.adequatelygood.com/)

**[⬆ voltar ao topo](#table-of-contents)**



## <a name='comparison-operators--equality'>Expressões Condicionais & Comparações</a>

  - Use `===` e `!==` ao invés de `==` e `!=`.
  - Expressões condicionais são interpretadas usando coerção de tipos e seguem as seguintes regras:

    + **Objeto** equivale a **true**
    + **Undefined** equivale a **false**
    + **Null** equivale a **false**
    + **Booleans** equivalem a **o valor do boolean**
    + **Numbers** equivalem a **false** se **+0, -0, or NaN**, se não **true**
    + **Strings** equivalem a **false** se são vazias `''`, se não **true**

    ```javascript
    if ([0]) {
      // true
      // Um array é um objeto, objetos equivalem a `true`.
    }
    ```

 - Use atalhos.

    ```javascript
    // ruim
    if (name !== '') {
      // ...outras implementações...
    }

    // bom
    if (name) {
      // ...outras implementações...
    }

    // ruim
    if (collection.length > 0) {
      // ...outras implementações...
    }

    // bom
    if (collection.length) {
      // ...outras implementações...
    }
    ```

  - Para mais informações veja [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) por Angus Croll

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='blocks'>Blocos</a>

  - Use chaves para todos os blocos com mais de uma linha.

    ```javascript
    // ruim
    if (test)
      return false;

    // bom
    if (test) return false;

    // bom
    if (test) {
      return false;
    }

    // ruim
    function() { return false; }

    // bom
    function() {
      return false;
    }
    ```

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='comments'>Comentários</a>

  - Use `/** ... */` para comentários com mais de uma linha. Inclua uma descrição e especifique tipos e valores para todos os parametros e retornos.

    ```javascript
    // ruim
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param <String> tag
    // @return <Element> element
    function make(tag) {

      // ...outra implementação...

      return element;
    }

    // bom
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param <String> tag
     * @return <Element> element
     */
    function make(tag) {

      // ...outras implementações...

      return element;
    }
    ```

  - Use `//` para comentários de uma linha. Coloque comentários de uma linha acima da expressão. Deixe uma linha em branco antes de cada comentário.

    ```javascript
    // ruim
    var active = true;  // is current tab

    // bom
    // is current tab
    var active = true;

    // ruim
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }

    // bom
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

  - Use prefixos `FIXME` or `TODO` nos seus comentários. Isso vai ajudar outros desenvolvedores a entenderem rapidamente se você está indicando um código que precisa ser revisado ou está sugerindo uma solução para o problema e como deve ser implementado. Estes são comentários diferentes dos convencionais, porque eles são acionáveis. As ações são `FIXME -- utilizado para comentários de apresentação` ou `TODO -- necessário a implementação`.

  - Use `// FIXME:` para marcar problemas

    ```javascript
    function Calculator() {

      // FIXME: não utilizar global aqui
      total = 0;

      return this;
    }
    ```

  - Use `// TODO:` para marcar soluções para um problema

    ```javascript
    function Calculator() {

      // TODO: total deve ser configurado por um parâmetro das opções
      this.total = 0;

      return this;
    }
  ```

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='whitespace'>Espaços em branco</a>

  - Use tabs com 2 espaços

    ```javascript
    // ruim
    function() {
    ∙∙∙∙var name;
    }

    // ruim
    function() {
    ∙var name;
    }

    // bom
    function() {
    ∙∙var name;
    }
    ```

  - Coloque um espaço antes da chave que abre o escopo da função.

    ```javascript
    // ruim
    function test(){
      console.log('test');
    }

    // bom
    function test() {
      console.log('test');
    }

    // ruim
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // bom
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```

  - Place 1 space before the opening parenthesis in control statements (`if`, `while` etc.). Place no space before the argument list in function calls and declarations.

    ```javascript
    // ruim
    if(isJedi) {
      fight ();
    }

    // bom
    if (isJedi) {
      fight();
    }

    // ruim
    function fight () {
      console.log ('Swooosh!');
    }

    // bom
    function fight() {
      console.log('Swooosh!');
    }
    ```

  - Colcar espaço entre operadores.

    ```javascript
    // ruim
    var x=y+5;

    // bom
    var x = y + 5;
    ```

  - Coloque uma linha em branco no final do arquivo.

   ```javascript
    // ruim
    (function(global) {
      // ...outras implementações...
    })(this);
    ```

    ```javascript
    // ruim
    (function(global) {
      // ...outras implementações...
    })(this);↵
    ↵
    ```

    ```javascript
    // bom
    (function(global) {
      // ...outras implementações...
    })(this);↵
    ```

  - Use identação quando encadear vários métodos. Use um ponto à esquerda, o que
     enfatiza que a linha é uma chamada de método, não uma nova declaração.

    ```javascript
      // ruim
      $('#items').find('.selected').highlight().end().find('.open').updateCount();

      // ruim
      $('#items').
        find('.selected').
          highlight().
          end().
        find('.open').
          updateCount();

      // bom
      $('#items')
        .find('.selected')
          .highlight()
          .end()
        .find('.open')
          .updateCount();

      // ruim
      var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
          .attr('width', (radius + margin) * 2).append('svg:g')
          .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
          .call(tron.led);

      // bom
      var leds = stage.selectAll('.led')
          .data(data)
        .enter().append('svg:svg')
          .classed('led', true)
          .attr('width', (radius + margin) * 2)
        .append('svg:g')
          .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
          .call(tron.led);
      ```

  - Deixar uma linha em branco depois de blocos e antes da próxima declaração

    ```javascript
    // ruim
    if (foo) {
      return bar;
    }
    return baz;

    // bom
    if (foo) {
      return bar;
    }

    return baz;

    // ruim
    var obj = {
      foo: function() {
      },
      bar: function() {
      }
    };
    return obj;

    // bom
    var obj = {
      foo: function() {
      },

      bar: function() {
      }
    };

    return obj;
    ```

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='commas'>Vírgulas</a>

  - Leading commas: **Nope.**

    ```javascript
    // ruim
    var story = [
        once
      , upon
      , aTime
    ];

    // bom
    var story = [
      once,
      upon,
      aTime
    ];

    // ruim
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // bom
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

  - Additional trailing comma: **Nope.** This can cause problems with IE6/7 and IE9 if it's in quirksmode. Also, in some implementations of ES3 would add length to an array if it had an additional trailing comma. This was clarified in ES5 ([source](http://es5.github.io/#D)):

  > Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

    ```javascript
    // ruim
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // bom
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='semicolons'>Ponto e vírgula</a>

  - **Yup.**

    ```javascript
    // ruim
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // bom
    (function() {
      var name = 'Skywalker';
      return name;
    })();

    // bom (guards against the function becoming an argument when two files with IIFEs are concatenated)
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    [Leia mais](http://stackoverflow.com/a/7365214/1712802).

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='type-casting--coercion'>Casting & Coerção de tipos</a>

  - Faça coerção de tipos no inicio da expressão.
  - Strings:

    ```javascript
    //  => this.reviewScore = 9;

    // ruim
    var totalScore = this.reviewScore + '';

    // bom
    var totalScore = '' + this.reviewScore;

    // ruim
    var totalScore = '' + this.reviewScore + ' total score';

    // bom
    var totalScore = this.reviewScore + ' total score';
    ```

  - Use `parseInt` para Numbers e sempre informe a base de conversão.
 
    ```javascript
    var inputValue = '4';

    // ruim
    var val = new Number(inputValue);

    // ruim
    var val = +inputValue;

    // ruim
    var val = inputValue >> 0;

    // ruim
    var val = parseInt(inputValue);

    // bom
    var val = Number(inputValue);

    // bom
    var val = parseInt(inputValue, 10);
    ```
  - Se por alguma razão você está fazendo algo muito underground e o `parseInt` é o gargalo, se usar deslocamento de bits (`Bitshift`) por [questões de performance](http://jsperf.com/coercion-vs-casting/3), deixe um comentário explicando por que você está fazendo isso.

    ```javascript
    // bom
    /**
     * parseInt é a causa do meu código estar lendo.
     * Bitshifting a String para força-lo como um
     * Number faz isso muito mais rápido.
     */
    var val = inputValue >> 0;
    ```

  - **Nota:** Cuidado com operações de bitshift. Numbers são representados por [valores 64-bit](http://es5.github.io/#x4.3.19), mas operações Bitshift sempre retornarão valores inteiros de 32-bit ([fonte](http://es5.github.io/#x11.7)). Bitshift pode levar a um comportamento inesperado para valores inteiros maiores que 32 bits. [Discussão](https://github.com/airbnb/javascript/issues/109). O mairo valor Integer signed 32-bit é 2.147.483.647:

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - Booleans:

    ```javascript
    var age = 0;

    // ruim
    var hasAge = new Boolean(age);

    // bom
    var hasAge = Boolean(age);

    // bom
    var hasAge = !!age;
    ```

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='naming-conventions'>Convenções de nomenclatura</a>

  - Não use apenas um caracter, seja descritivo.

    ```javascript
    // ruim
    function q() {
      // ...outras implementações...
    }

    // bom
    function query() {
      // ...outras implementações...
    }
    ```

  - Use camelCase quando for nomear objetos, funções e instâncias.

    ```javascript
    // ruim
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c() {}
    var u = new user({
      name: 'Bob Parr'
    });

    // bom
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - Use PascalCase quando for nomear construtores ou classes.

    ```javascript
    // ruim
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope'
    });

    // bom
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  - Use um underscore `_` como primeiro caracter em propriedades privadas.

    ```javascript
    // ruim
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // bom
    this._firstName = 'Panda';
    ```

  - Quando for guardar referência para `this` use `_this`.

    ```javascript
    // ruim
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }

    // ruim
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // bom
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }
    ```

  - Nomeie suas funções. Ajuda bastante quando for analisar pilhas de erro.

    ```javascript
    // ruim
    var log = function(msg) {
      console.log(msg);
    };

    // bom
    var log = function log(msg) {
      console.log(msg);
    };
    ```

  - **Nota:** IE8 ou inferior mostra alguns problemas com funções nomeadas. Veja [http://kangax.github.io/nfe/](http://kangax.github.io/nfe/) para mais informações.

  - Se seu arquivos exporta apenas uma classes, o nome do arquivo deve conter exatamento o nome da classe.
    ```javascript
    // conteúdo do arquivo
    class CheckBox {
      // ...
    }
    module.exports = CheckBox;

    // em outro arquivo
    // ruim
    var CheckBox = require('./checkBox');

    // ruim
    var CheckBox = require('./check_box');

    // bom
    var CheckBox = require('./CheckBox');
    ```

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='accessors'>Métodos Acessores</a>

  - Métodos acessores de propriedades não são obrigatórios.
  - Se você vai criar métodos acessores utilize getVal() e setVal('hello')

    ```javascript
    // ruim
    dragon.age();

    // bom
    dragon.getAge();

    // ruim
    dragon.age(25);

    // bom
    dragon.setAge(25);
    ```

  - Se a propriedade é um boolean, use isVal() ou hasVal()

    ```javascript
    // ruim
    if (!dragon.age()) {
      return false;
    }

    // bom
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - Tudo bem se você criar os métodos get() e set(), mas seja consistente.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function(key) {
      return this[key];
    };
    ```

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='constructors'>Construtores</a>

  - Atribua métodos ao objeto `prototype` ao invés de sobrescrever o prototype com um novo objeto.Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // ruim
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // bom
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - Métodos podem retornar `this` para encadear novas chamadas.

    ```javascript
    // ruim
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20) // => undefined

    // bom
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - Tudo bem em escrever um toString() customizado. Apenas garanta que ele sempre irá funcionar e que não altera nenhum estado.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='events'>Eventos</a>

  - When attaching data payloads to events (whether DOM events or something more proprietary like Backbone events), pass a hash instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event. For example, instead of:

    ```js
    // ruim
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    prefira:

    ```js
    // bom
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

**[⬆ voltar ao topo](#table-of-contents)**

## <a name='modules'>Módulos</a>

  - Um módulo deve começar com `!`. Isso garante que não haverá erros em produção caso os scripts sejam concatenados e um módulo não termine com ponto e vírgula. [Explicação](https://github.com/airbnb/javascript/issues/44#issuecomment-13063933)
  - Nomeie o arquivo em formato camelCase, coloque em uma pasta com o mesmo nome e procure o nome da função que é exportada.
  - Adicione um método noConflict() que exporta o módulo antigo e retorna o módulo que foi criado com o mesmo nome.
  - Sempre declare `'use strict';` no topo do módulo.

    ```javascript
    // fancyInput/fancyInput.js

    !function(global) {
      'use strict';

      var previousFancyInput = global.FancyInput;

      function FancyInput(options) {
        this.options = options || {};
      }

      FancyInput.noConflict = function noConflict() {
        global.FancyInput = previousFancyInput;
        return FancyInput;
      };

      global.FancyInput = FancyInput;
    }(this);
    ```

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='jquery'>jQuery</a>

  - Nomeie objetos jQuery com o prefixo `$`.

    ```javascript
    // ruim
    var sidebar = $('.sidebar');

    // bom
    var $sidebar = $('.sidebar');
    ```

  - Guarde as consultas jQuery para reuso.

    ```javascript
    // ruim
    function setSidebar() {
      $('.sidebar').hide();

      // ...outras implementações...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // bom
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...outras implementações...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - Para pesquisas no DOM use o modo Cascata `$('.sidebar ul')` ou pai > filho `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - Use `find` em objetos jQuery que estão armazenados em variáveis.

    ```javascript
    // ruim
    $('ul', '.sidebar').hide();

    // ruim
    $('.sidebar').find('ul').hide();

    // bom
    $('.sidebar ul').hide();

    // bom
    $('.sidebar > ul').hide();

    // bom
    $sidebar.find('ul').hide();
    ```

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='ecmascript-5-compatibility'>Compatibilidade ECMAScript 5</a>

  - Consulte [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](http://kangax.github.com/es5-compat-table/)

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='testing'>Testes</a>

  - **Yup.**

    ```javascript
    function() {
      return true;
    }
    ```

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='performance'>Performance</a>

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...

**[⬆ voltar ao topo](#table-of-contents)**


## <a name='resources'>Conteúdo</a>


**Leia isso**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**Ferramentas**

  - Code Style Linters
    + [JSHint](http://www.jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/jshintrc)
    + [JSCS](https://github.com/jscs-dev/node-jscs) - [Airbnb Style Preset](https://github.com/jscs-dev/node-jscs/blob/master/presets/airbnb.json)

**Outros guias de estilo**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)
  - [JavaScript Standard Style](https://github.com/feross/standard)

**Outros estilos**

  - [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) - Ross Allen
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
  - [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**Outras Leituras**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
  - [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
  - [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban
  - [Frontend Guidelines](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock

**Livros**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/) - Julien Bouquillon
  - [Third Party JavaScript](http://manning.com/vinegar/) - Ben Vinegar and Anton Kovalyov
  - [Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript](http://amzn.com/0321812182) - David Herman
  - [Eloquent JavaScript](http://eloquentjavascript.net) - Marijn Haverbeke
  - [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS) - Kyle Simpson

**Blogs**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)

**Podcasts**

  - [JavaScript Jabber](http://devchat.tv/js-jabber/)

**[⬆ voltar ao topo](#table-of-contents)**

## <a name='in-the-wild'>Empresas utilizando</a>

  Essa é a lista de organizações que estão utilizando esse guia de estilo. Mande-nos um pull request ou abra um apontamento para adicionarmos você na lista.


  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Adult Swim**: [adult-swim/javascript](https://github.com/adult-swim/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **American Insitutes for Research**: [AIRAST/javascript](https://github.com/AIRAST/javascript)
  - **Apartmint**: [apartmint/javascript](https://github.com/apartmint/javascript)
  - **Avalara**: [avalara/javascript](https://github.com/avalara/javascript)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **DailyMotion**: [dailymotion/javascript](https://github.com/dailymotion/javascript)
  - **Digitpaint** [digitpaint/javascript](https://github.com/digitpaint/javascript)
  - **Evernote**: [evernote/javascript-style-guide](https://github.com/evernote/javascript-style-guide)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Flexberry**: [Flexberry/javascript-style-guide](https://github.com/Flexberry/javascript-style-guide)
  - **Gawker Media**: [gawkermedia/javascript](https://github.com/gawkermedia/javascript)
  - **GeneralElectric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript)
  - **InfoJobs**: [InfoJobs/JavaScript-Style-Guide](https://github.com/InfoJobs/JavaScript-Style-Guide)
  - **Intent Media**: [intentmedia/javascript](https://github.com/intentmedia/javascript)
  - **Jam3**: [Jam3/Javascript-Code-Conventions](https://github.com/Jam3/Javascript-Code-Conventions)
  - **JSSolutions**: [JSSolutions/javascript](https://github.com/JSSolutions/javascript)
  - **Kinetica Solutions**: [kinetica/javascript](https://github.com/kinetica/javascript)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **Money Advice Service**: [moneyadviceservice/javascript](https://github.com/moneyadviceservice/javascript)
  - **Muber**: [muber/javascript](https://github.com/muber/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
  - **Nimbl3**: [nimbl3/javascript](https://github.com/nimbl3/javascript)
  - **Nordic Venture Family**: [CodeDistillery/javascript](https://github.com/CodeDistillery/javascript)
  - **Orion Health**: [orionhealth/javascript](https://github.com/orionhealth/javascript)
  - **Peerby**: [Peerby/javascript](https://github.com/Peerby/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **reddit**: [reddit/styleguide/javascript](https://github.com/reddit/styleguide/tree/master/javascript)
  - **REI**: [reidev/js-style-guide](https://github.com/reidev/js-style-guide)
  - **Ripple**: [ripple/javascript-style-guide](https://github.com/ripple/javascript-style-guide)
  - **SeekingAlpha**: [seekingalpha/javascript-style-guide](https://github.com/seekingalpha/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **StudentSphere**: [studentsphere/javascript](https://github.com/studentsphere/javascript)
  - **Target**: [target/javascript](https://github.com/target/javascript)
  - **TheLadders**: [TheLadders/javascript](https://github.com/TheLadders/javascript)
  - **T4R Technology**: [T4R-Technology/javascript](https://github.com/T4R-Technology/javascript)
  - **Userify**: [userify/javascript](https://github.com/userify/javascript)
  - **VoxFeed**: [VoxFeed/javascript-style-guide](https://github.com/VoxFeed/javascript-style-guide)
  - **Weggo**: [Weggo/javascript](https://github.com/Weggo/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

## <a name='translation'>Traduções</a>

  Este style guide está disponível em outros idiomas:

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - ![bg](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bulgaria.png) **Bulgarian**: [borislavvv/javascript](https://github.com/borislavvv/javascript)
  - ![ca](https://raw.githubusercontent.com/fpmweb/javascript-style-guide/master/img/catala.png) **Catalan**: [fpmweb/javascript-style-guide](https://github.com/fpmweb/javascript-style-guide)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese(Traditional)**: [jigsawye/javascript](https://github.com/jigsawye/javascript)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese(Simplified)**: [adamlu/javascript-style-guide](https://github.com/adamlu/javascript-style-guide)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [nmussy/javascript-style-guide](https://github.com/nmussy/javascript-style-guide)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [sinkswim/javascript-style-guide](https://github.com/sinkswim/javascript-style-guide)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/javacript-style-guide](https://github.com/mitsuruog/javacript-style-guide)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [tipjs/javascript-style-guide](https://github.com/tipjs/javascript-style-guide)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [mjurczyk/javascript](https://github.com/mjurczyk/javascript)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [uprock/javascript](https://github.com/uprock/javascript)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **Thai**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide)

## <a name='guide-guide'>The JavaScript Style Guide Guide</a>

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## Chat With Us About JavaScript

  - Find us on [gitter](https://gitter.im/airbnb/javascript).

## <a name='contributors'>Contribuidores</a>

  - [Ver Contribuidores](https://github.com/airbnb/javascript/graphs/contributors)


## <a name='license'>Licença</a>

(The MIT License)

Copyright (c) 2014 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ voltar ao topo](#table-of-contents)**

# };

