# Guia de estilo Clojure

> Os modelos são importantes. <br/>
> -- Oficial Alex J. Murphy / RoboCop

Este guia de estilo recomenda melhores práticas para que programadores Clojure possam escrever códigos e possam ser mantidos por outros programadores. A style guide that reflects real-world usage gets used, and a
style guide that holds to an ideal that has been rejected by the people it is
supposed to help risks not getting used at all &mdash; não importa quão bom ele seja.

O guia está dividido em várias seções de regras relacionadas. Eu tentei adicionar toda a lógica por trás das regras (Se for omitido, estou assumindo que é bastante óbvio).

Eu não apareci com todas as regras do nada; elas foram feitas principalmente com base na minha extensa carreira como engenheiro de software profissional,
feedback e sugestões de membros da comunidade Clojure, and various highly regarded Clojure programming resources, como
["Clojure Programming"](http://www.clojurebook.com/)
e ["The Joy of Clojure"](http://joyofclojure.com/).

O guia ainda é um trabalho em andamento; está faltando algumas seções, outras estão incompletas, algumas regras faltam exemplos, outras não tem exemplos que as ilustre bem o suficiente. Estas questões seram abordadas em seu devido tempo &mdash; Apenas mantenha-os em mente por enquanto.

Por favor, notem que a comunidade de desenvolvedores do Clojure também mantém uma [lista de padrões de codificação](http://dev.clojure.org/display/community/Library+Coding+Standards) para as bibliotecas.

Você pode gerar uma cópia em PDF ou em HTML deste guia usando o [Pandoc](http://pandoc.org/).

Traduções deste guia estão disponíveis também nas seguintes linguagens:

* [Inglês](https://github.com/bbatsov/clojure-style-guide)
* [Chinês](https://github.com/geekerzp/clojure-style-guide/blob/master/README-zhCN.md)
* [Japonês](https://github.com/totakke/clojure-style-guide/blob/ja/README.md)
* [Coreano](https://github.com/kwakbab/clojure-style-guide/blob/master/README-koKO.md)
* [Russo](https://github.com/Nondv/clojure-style-guide/blob/master/ru/README.md)
* [Espanhol](https://github.com/jeko2000/clojure-style-guide/blob/master/README.md)

## Índice

* [Disposição do código & Organização](#disposição-do-código--organização)
* [Sintaxe](#sintaxe)
* [Nomenclaturas](#nomenclaturas)
* [Collections](#collections)
* [Mutações](#mutações)
* [Strings](#strings)
* [Exceções](#exceções)
* [Macros](#macros)
* [Comentários](#comentários)
    * [Anotações de Comentários](#anotações-de-comentários)
* [Existencial](#existencial)
* [Ferramentas](#ferramentas)
* [Testando](#testando)
* [Documentação](#documentação)
* [Organização das bibliotecas](#library-organization)


## Disposição do código & Organização

> Nearly everybody is convinced that every style but their own is
> ugly and unreadable. Leave out the "but their own" and they're
> probably right... <br/>
> -- Jerry Coffin (on indentation)

* <a name="spaces"></a>
  Use **espaços** na identação. Não use tab.
<sup>[[link](#spaces)]</sup>

* <a name="body-indentation"></a>
Use 2 spaces to indent the bodies of
forms that have body parameters.  This covers all `def` forms, special
forms and macros that introduce local bindings (e.g. `loop`, `let`,
`when-let`) and many macros like `when`, `cond`, `as->`, `cond->`, `case`,
`with-*`, etc.
<sup>[[link](#body-indentation)]</sup>


    ```Clojure
    ;; bom
    (when alguma-coisa
      (outra-coisa))

    (with-out-str
      (println "Oĺa, ")
      (println "mundo!"))

    ;; ruim - quatro espaços
    (when alguma-coisa
        (outra-coisa))

    ;; ruim - um espaço
    (with-out-str
     (println "Oĺá, ")
     (println "mundo!"))
    ```

* <a name="vertically-align-fn-args"></a>
Alinhar verticalmente argumentos da função (macro) distribuída em várias linhas.
<sup>[[link](#vertically-align-fn-args)]</sup>

    ```Clojure
    ;; bom
    (filter even?
            (range 1 10))

    ;; ruim
    (filter even?
      (range 1 10))
    ```

* <a name="one-space-indent"></a>
Use um único espaço para os argumentos da função quando não há argumentos na mesma linha que o nome da função.
<sup>[[link](#one-space-indent)]</sup>

    ```Clojure
    ;; bom
    (filter
     even?
     (range 1 10))

    (or
     ala
     bala
     portokala)

    ;; ruim - dois espaços
    (filter
      even?
      (range 1 10))

    (or
      ala
      bala
      portokala)
    ```

* <a name="vertically-align-let-and-map"></a>
  Alinhar verticalmente ligações `let` e keyword maps.
<sup>[[link](#vertically-align-let-and-map)]</sup>

    ```Clojure
    ;; bom
    (let [coisa1 "alguma coisa"
          coisa2 "outra coisa"]
      {:coisa1 coisa1
       :coisa2 coisa2})

    ;; ruim
    (let [coisa1 "alguma coisa"
      coisa2 "outra coisa"]
      {:coisa1 coisa1
      :coisa2 coisa2})
    ```

* <a name="optional-new-line-after-fn-name"></a>
  Opcionalmente, omita a nova linha entre o nome da função e o vetor de
  argumento para `defn` quando não há docstring.
<sup>[[link](#optional-new-line-after-fn-name)]</sup>

    ```Clojure
    ;; bom
    (defn foo
      [x]
      (bar x))

    ;; bom
    (defn foo [x]
      (bar x))

    ;; ruim
    (defn foo
      [x] (bar x))
    ```

* <a name="multimethod-dispatch-val-placement"></a>
  Coloque a `dispatch-val` de um método múltiplo na mesma linha que o nome da
  função.
<sup>[[link](#multimethod-dispatch-val-placement)]</sup>


    ```Clojure
    ;; bom
    (defmethod foo :bar [x] (baz x))

    (defmethod foo :bar
      [x]
      (baz x))

    ;; ruim
    (defmethod foo
      :bar
      [x]
      (baz x))

    (defmethod foo
      :bar [x]
      (baz x))
    ```

* <a name="oneline-short-fn"></a>
  Opcionalmente, omita a nova linha entre o vetor de argumento e um corpo de
  função curto.
<sup>[[link](#oneline-short-fn)]</sup>

    ```Clojure
    ;; bom
    (defn foo [x]
      (bar x))

    ;; bom for a small function body
    (defn foo [x] (bar x))

    ;; bom for multi-arity functions
    (defn foo
      ([x] (bar x))
      ([x y]
       (if (predicate? x)
         (bar x)
         (baz x))))

    ;; ruim
    (defn foo
      [x] (if (predicate? x)
            (bar x)
            (baz x)))
    ```

* <a name="multiple-arity-indentation"></a>
  Indente cada forma de aridade de uma definição de função alinhada verticalmente
  com seus parâmetros.<sup>[[link](#multiple-arity-indentation)]</sup>

    ```Clojure
    ;; bom
    (defn foo
      "I have two arities."
      ([x]
       (foo x 1))
      ([x y]
       (+ x y)))

    ;; ruim - extra indentation
    (defn foo
      "I have two arities."
      ([x]
        (foo x 1))
      ([x y]
        (+ x y)))
    ```

* <a name="multiple-arity-order"></a> Sort the arities of a function
  from fewest to most arguments. The common case of multi-arity
  functions is that some K arguments fully specifies the function's
  behavior, and that arities N < K partially apply the K arity, and
  arities N > K provide a fold of the K arity over varargs.
  <sup>[[link](#multiple-arity-order)]</sup>

    ```Clojure
    ;; bom - it's easy to scan for the nth arity
    (defn foo
      "I have two arities."
      ([x]
       (foo x 1))
      ([x y]
       (+ x y)))

    ;; okay - the other arities are applications of the two-arity
    (defn foo
      "I have two arities."
      ([x y]
       (+ x y))
      ([x]
       (foo x 1))
      ([x y z & more]
       (reduce foo (foo x (foo y z)) more)))

    ;; ruim - unordered for no apparent reason
    (defn foo
      ([x] 1)
      ([x y z] (foo x (foo y z)))
      ([x y] (+ x y))
      ([w x y z & more] (reduce foo (foo w (foo x (foo y z))) more)))
    ```

* <a name="crlf"></a>
  Use Unix-style line endings. (*BSD/Solaris/Linux/OSX users are
  covered by default, Windows users have to be extra careful.)
<sup>[[link](#crlf)]</sup>

    * If you're using Git you might want to add the following
    configuration setting to protect your project from Windows line
    endings creeping in:

    ```
    bash$ git config --global core.autocrlf true
    ```

* <a name="bracket-spacing"></a>
  If any text precedes an opening bracket(`(`, `{` and
`[`) or follows a closing bracket(`)`, `}` and `]`), separate that
text from that bracket with a space. Conversely, leave no space after
an opening bracket and before following text, or after preceding text
and before a closing bracket.
<sup>[[link](#bracket-spacing)]</sup>

    ```Clojure
    ;; bom
    (foo (bar baz) quux)

    ;; ruim
    (foo(bar baz)quux)
    (foo ( bar baz ) quux)
    ```

> Syntactic sugar causes semicolon cancer. <br/>
> -- Alan Perlis

* <a name="no-commas-for-seq-literals"></a>
  Don't use commas between the elements of sequential collection literals.
<sup>[[link](#no-commas-for-seq-literals)]</sup>

    ```Clojure
    ;; bom
    [1 2 3]
    (1 2 3)

    ;; ruim
    [1, 2, 3]
    (1, 2, 3)
    ```

* <a name="opt-commas-in-map-literals"></a>
  Consider enhancing the readability of map literals via judicious use
of commas and line breaks.
<sup>[[link](#opt-commas-in-map-literals)]</sup>

    ```Clojure
    ;; bom
    {:name "Bruce Wayne" :alter-ego "Batman"}

    ;; bom and arguably a bit more readable
    {:name "Bruce Wayne"
     :alter-ego "Batman"}

    ;; bom and arguably more compact
    {:name "Bruce Wayne", :alter-ego "Batman"}
    ```

* <a name="gather-trailing-parens"></a>
  Place all trailing parentheses on a single line instead of distinct lines.
<sup>[[link](#gather-trailing-parens)]</sup>

    ```Clojure
    ;; bom; single line
    (when something
      (something-else))

    ;; ruim; distinct lines
    (when something
      (something-else)
    )
    ```

* <a name="empty-lines-between-top-level-forms"></a>
  Use empty lines between top-level forms.
<sup>[[link](#empty-lines-between-top-level-forms)]</sup>

    ```Clojure
    ;; bom
    (def x ...)

    (defn foo ...)

    ;; ruim
    (def x ...)
    (defn foo ...)
    ```

    An exception to the rule is the grouping of related `def`s together.

    ```Clojure
    ;; bom
    (def min-rows 10)
    (def max-rows 20)
    (def min-cols 15)
    (def max-cols 30)
    ```

* <a name="no-blank-lines-within-def-forms"></a>
  Do not place blank lines in the middle of a function or
macro definition.  An exception can be made to indicate grouping of
pairwise constructs as found in e.g. `let` and `cond`.
<sup>[[link](#no-blank-lines-within-def-forms)]</sup>

* <a name="80-character-limits"></a>
  Where feasible, avoid making lines longer than 80 characters.
<sup>[[link](#80-character-limits)]</sup>

* <a name="no-trailing-whitespace"></a>
  Avoid trailing whitespace.
<sup>[[link](#no-trailing-whitespace)]</sup>

* <a name="one-file-per-namespace"></a>
  Use one file per namespace.
<sup>[[link](#one-file-per-namespace)]</sup>

* <a name="comprehensive-ns-declaration"></a>
  Start every namespace with a comprehensive `ns` form, comprised of
  `refer`s, `require`s, and `import`s, conventionally in that order.
<sup>[[link](#comprehensive-ns-declaration)]</sup>

    ```Clojure
    (ns examples.ns
      (:refer-clojure :exclude [next replace remove])
      (:require [clojure.string :as s :refer [blank?]]
                [clojure.set :as set]
                [clojure.java.shell :as sh])
      (:import java.util.Date
               java.text.SimpleDateFormat
               [java.util.concurrent Executors
                                     LinkedBlockingQueue]))
    ```

* <a name="prefer-require-over-use"></a>
  In the `ns` form prefer `:require :as` over `:require :refer` over `:require
  :refer :all`.  Prefer `:require` over `:use`; the latter form should be
  considered deprecated for new code.
<sup>[[link](#prefer-require-over-use)]</sup>

    ```Clojure
    ;; bom
    (ns examples.ns
      (:require [clojure.zip :as zip]))

    ;; bom
    (ns examples.ns
      (:require [clojure.zip :refer [lefts rights]]))

    ;; acceptable as warranted
    (ns examples.ns
      (:require [clojure.zip :refer :all]))

    ;; ruim
    (ns examples.ns
      (:use clojure.zip))
    ```

* <a name="no-single-segment-namespaces"></a>
  Avoid single-segment namespaces.
<sup>[[link](#no-single-segment-namespaces)]</sup>

    ```Clojure
    ;; bom
    (ns example.ns)

    ;; ruim
    (ns example)
    ```

* <a name="namespaces-with-5-segments-max"></a>
  Avoid the use of overly long namespaces (i.e., more than 5 segments).
<sup>[[link](#namespaces-with-5-segments-max)]</sup>

* <a name="10-loc-per-fn-limit"></a>
  Avoid functions longer than 10 LOC (lines of code). Ideally, most
  functions will be shorter than 5 LOC.
<sup>[[link](#10-loc-per-fn-limit)]</sup>

* <a name="4-positional-fn-params-limit"></a>
  Avoid parameter lists with more than three or four positional parameters.
<sup>[[link](#4-positional-fn-params-limit)]</sup>

* <a name="forward-references"></a>
  Avoid forward references.  They are occasionally necessary, but such occasions
  are rare in practice.
<sup>[[link](#forward-references)]</sup>

## Sintaxe

* <a name="ns-fns-only-in-repl"></a>
  Evite o uso de funções de manipulação de namespace como `require` e
  `refer`. Elas são desnecessárias fora do ambiente REPL.
<sup>[[link](#ns-fns-only-in-repl)]</sup>

* <a name="declare"></a>
  Use `declare` para habilitar referências futuras quando elas forem
  necessárias.
<sup>[[link](#declare)]</sup>

* <a name="higher-order-fns"></a>
  Prefira funções de alta ordem como `map` no lugar de `loop/recur`.
<sup>[[link](#higher-order-fns)]</sup>

* <a name="pre-post-conditions"></a>
  Prefira funções de pre e pos condições para fazer checagens
  dentro do corpo de uma função.
<sup>[[link](#pre-post-conditions)]</sup>

    ```Clojure
    ;; bom
    (defn foo [x]
      {:pre [(pos? x)]}
      (bar x))

    ;; ruim
    (defn foo [x]
      (if (pos? x)
        (bar x)
        (throw (IllegalArgumentException. "x deve ser um número positivo!")))
    ```

* <a name="dont-def-vars-inside-fns"></a>
  Não defina variáveis dentro de funções.
<sup>[[link](#dont-def-vars-inside-fns)]</sup>

    ```Clojure
    ;; muito ruim
    (defn foo []
      (def x 5)
      ...)
    ```

* <a name="dont-shadow-clojure-core"></a>
  Não sobrescreva nomes `clojure.core` com atribuições locais.
<sup>[[link](#dont-shadow-clojure-core)]</sup>

    ```Clojure
    ;; ruim - você é forçado a usar clojure.core/map com todo namespace
    (defn foo [map]
      ...)
    ```

* <a name="alter-var"></a>
  Use `alter-var-root` no lugar de `def` para alterar o valor de variáveis.
<sup>[[link]](#alter-var)</sup>

    ```Clojure
    ;; bom
    (def variável 1) ; o valor de coisa agora é 1
    ; faz algo com variável
    (alter-var-root #'variável (constantly nil)) ; valor de variável agora é nil

    ;; ruim
    (def variável 1)
    ; faz algo com variável
    (def variável nil)
    ; valor de variável é nil agora
    ```

* <a name="nil-punning"></a>
  Use `seq` como uma condição terminal para verificar quando uma sequência
  é vazia (essa técnica é as vezes chamada de *nil punning*).
<sup>[[link](#nil-punning)]</sup>

    ```Clojure
    ;; bom
    (defn print-seq [s]
      (when (seq s)
        (prn (first s))
        (recur (rest s))))

    ;; ruim
    (defn print-seq [s]
      (when-not (empty? s)
        (prn (first s))
        (recur (rest s))))
    ```

* <a name="to-vector"></a>
  Prefira `vec` no lugar de `into` que precisar converter uma sequância em um vetor.
<sup>[[link](#to-vector)]</sup>

    ```Clojure
    ;; bom
    (vec uma-seq)

    ;; ruim
    (into [] uma-seq)
    ```

* <a name="when-instead-of-single-branch-if"></a>
  Use `when` instead of `(if ... (do ...))`.
<sup>[[link](#when-instead-of-single-branch-if)]</sup>

    ```Clojure
    ;; bom
    (when pred
      (foo)
      (bar))

    ;; ruim
    (if pred
      (do
        (foo)
        (bar)))
    ```

* <a name="if-let"></a>
  Use `if-let` no lugar de `let` + `if`.
<sup>[[link](#if-let)]</sup>

    ```Clojure
    ;; bom
    (if-let [resultado (foo x)]
      (algo-com resultado)
      (oura-coisa))

    ;; ruim
    (let [resultado (foo x)]
      (if resultado
        (algo-com resultado)
        (outra-coisa)))
    ```

* <a name="when-let"></a>
  Use `when-let` no lugar de `let` + `when`.
<sup>[[link](#when-let)]</sup>

    ```Clojure
    ;; bom
    (when-let [resultado (foo x)]
      (faz-algo-com resultado)
      (faz-algo-mais-com resultado))

    ;; ruim
    (let [resultado (foo x)]
      (when resultado
        (faz-algo-com resultado)
        (faz-algo-mais-com resultado)))
    ```

* <a name="if-not"></a>
  Use `if-not` no lugar de `(if (not ...) ...)`.
<sup>[[link](#if-not)]</sup>

    ```Clojure
    ;; bom
    (if-not pred
      (foo))

    ;; ruim
    (if (not pred)
      (foo))
    ```

* <a name="when-not"></a>
  Use `when-not` no lugar de `(when (not ...) ...)`.
<sup>[[link](#when-not)]</sup>

    ```Clojure
    ;; bom
    (when-not pred
      (foo)
      (bar))

    ;; ruim
    (when (not pred)
      (foo)
      (bar))
    ```

* <a name="when-not-instead-of-single-branch-if-not"></a>
  Use `when-not` instead of `(if-not ... (do ...))`.
<sup>[[link](#when-not-instead-of-single-branch-if-not)]</sup>

    ```Clojure
    ;; bom
    (when-not pred
      (foo)
      (bar))

    ;; ruim
    (if-not pred
      (do
        (foo)
        (bar)))
    ```

* <a name="not-equal"></a>
  Use `not=` no lugar de `(not (= ...))`.
<sup>[[link](#not-equal)]</sup>

    ```Clojure
    ;; bom
    (not= foo bar)

    ;; ruim
    (not (= foo bar))
    ```

* <a name="printf"></a>
  Use `printf` no lugar de `(print (format ...))`.
<sup>[[link](#printf)]</sup>

    ```Clojure
    ;; bom
    (printf "Olá, %s!\n" nome)

    ;; ok
    (println (format "Olá, %s!" nome))
    ```

* <a name="multiple-arity-of-gt-and-ls-fns"></a>
  Quando fizer comparações, tenha em mente que as funções Clojure `<`,
  `>`, etc. aceitam um número variável de argumentos.
<sup>[[link](#multiple-arity-of-gt-and-ls-fns)]</sup>

    ```Clojure
    ;; bom
    (< 5 x 10)

    ;; ruim
    (and (> x 5) (< x 10))
    ```

* <a name="single-param-fn-literal"></a>
  Prefira `%` no lugar de `%1` nos argumentos de funções com apenas um parâmetro.
<sup>[[link](#single-param-fn-literal)]</sup>

    ```Clojure
    ;; bom
    #(Math/round %)

    ;; ruim
    #(Math/round %1)
    ```

* <a name="multiple-params-fn-literal"></a>
  Prefira `%1` no lugar de `%` nos argumentos de funções com mais de um parâmetro.
<sup>[[link](#multiple-params-fn-literal)]</sup>

    ```Clojure
    ;; bom
    #(Math/pow %1 %2)

    ;; ruim
    #(Math/pow % %2)
    ```

* <a name="no-useless-anonymous-fns"></a>
  Não envolva funções com funções anônimas sem necessidade.
<sup>[[link](#no-useless-anonymous-fns)]</sup>

    ```Clojure
    ;; bom
    (filter even? (range 1 10))

    ;; ruim
    (filter #(even? %) (range 1 10))
    ```

* <a name="no-multiple-forms-fn-literals"></a>
  Não use literais de uma função se o corpo da função vai consistir em
  mais de uma chamada.
<sup>[[link](#no-multiple-forms-fn-literals)]</sup>

    ```Clojure
    ;; bom
    (fn [x]
      (println x)
      (* x 2))

    ;; ruim (você precisa explicitamente de uma chamada do)
    #(do (println %)
         (* % 2))
    ```

* <a name="complement"></a>
  Prefira o uso de `complement` no lugar de uma função anônima.
<sup>[[link](#complement)]</sup>

    ```Clojure
    ;; bom
    (filter (complement some-pred?) coll)

    ;; ruim
    (filter #(not (some-pred? %)) coll)
    ```

    Essa regra deve obviamente ser ignorada se já existir uma função que
    faz o mesmo que usar o complemento (e.g. `even?` e `odd?`).

* <a name="comp"></a>
  Usar `comp` pode deixar o código mais simples.
<sup>[[link](#comp)]</sup>

    ```Clojure
    ;; Assumindo `(:require [clojure.string :as str])`...

    ;; bom
    (map #(str/capitalize (str/trim %)) ["top " " test "])

    ;; melhor
    (map (comp str/capitalize str/trim) ["top " " test "])
    ```

* <a name="partial"></a>
  Usar `partial` pode deixar o código mais simples.
<sup>[[link](#partial)]</sup>

    ```Clojure
    ;; bom
    (map #(+ 5 %) (range 1 10))

    ;; (dicutivelmente) melhor
    (map (partial + 5) (range 1 10))
    ```

* <a name="threading-macros"></a>
  Prefira o uso de threading macros `->` (thread-first) e `->>`
(thread-last) no lugar de aninhamentos exaustivos.
<sup>[[link](#threading-macros)]</sup>

    ```Clojure
    ;; bom
    (-> [1 2 3]
        reverse
        (conj 4)
        prn)

    ;; não muito bom
    (prn (conj (reverse [1 2 3])
               4))

    ;; bom
    (->> (range 1 10)
         (filter even?)
         (map (partial * 2)))

    ;; não muito bom
    (map (partial * 2)
         (filter even? (range 1 10)))
    ```

* <a name="else-keyword-in-cond"></a>
  Use `:else` como um teste de experessão catch-all em `cond`.
<sup>[[link](#else-keyword-in-cond)]</sup>

    ```Clojure
    ;; bom
    (cond
      (neg? n) "negativo"
      (pos? n) "positivo"
      :else "zero")

    ;; ruim
    (cond
      (neg? n) "negativo"
      (pos? n) "positivo"
      true "zero")
    ```

* <a name="condp"></a>
  Prefira `condp` no lugar de `cond` quando o predicado e expressão não mudarem.
<sup>[[link](#condp)]</sup>

    ```Clojure
    ;; bom
    (cond
      (= x 10) :dez
      (= x 20) :vinte
      (= x 30) :trinta
      :else :nao-sei)

    ;; muito melhor
    (condp = x
      10 :dez
      20 :vinte
      30 :trinta
      :nao-sei)
    ```

* <a name="case"></a>
  Prefira `case` no lugar de `cond` ou `condp` quando expressões de checagens são
  constantes de tempo de compilação.
<sup>[[link](#case)]</sup>

    ```Clojure
    ;; bom
    (cond
      (= x 10) :dez
      (= x 20) :vinte
      (= x 30) :trinta
      :else :nao-sei)

    ;; melhor que anterior
    (condp = x
      10 :dez
      20 :vinte
      30 :trinta
      :nao-sei)

    ;; melhor caso
    (case x
      10 :dez
      20 :vinte
      30 :trinta
      :nao-sei)
    ```

* <a name="shor-forms-in-cond"></a>
  Use formas curtas no `cond` e similares. Se não for possível dê dicar visuais
para as assosiações com comentário ou linhas em branco.
<sup>[[link](#shor-forms-in-cond)]</sup>

    ```Clojure
    ;; bom
    (cond
      (test1) (acao-1)
      (test2) (acao-2)
      :else   (action-padrao))

    ;; ok
    (cond
      ;; test caso 1
      (test1)
      (funcao-com-nome-longo-que-requer-uma-nova-linha
        (sub-forma-complicada
          (-> 'que-abrange multiplas-linhas)))

      ;; test caso 2
      (test2)
      (outra-funcao-de-nome-grande
        (outra-sub-forma
          (-> 'que-abrange multiplas-linhas)))

      :else
      (o-caso-padrao
        (que-tambem-abrange 'multiplas
                            'linhas)))
    ```

* <a name="set-as-predicate"></a>
  Use um `set` como predicado quando apropriado.
<sup>[[link](#set-as-predicate)]</sup>

    ```Clojure
    ;; bom
    (remove #{1} [0 1 2 3 4 5])

    ;; ruim
    (remove #(= % 1) [0 1 2 3 4 5])

    ;; bom
    (count (filter #{\a \e \i \o \u} "mary had a little lamb"))

    ;; ruim
    (count (filter #(or (= % \a)
                        (= % \e)
                        (= % \i)
                        (= % \o)
                        (= % \u))
                   "mary had a little lamb"))
    ```

* <a name="inc-and-dec"></a>
  Use `(inc x)` & `(dec x)` no lugar de `(+ x 1)` e `(- x 1)`.
<sup>[[link](#inc-and-dec)]</sup>

* <a name="pos-and-neg"></a>
  Use `(pos? x)`, `(neg? x)` & `(zero? x)` no lugar de `(> x 0)`,
`(< x 0)` & `(= x 0)`.
<sup>[[link](#pos-and-neg)]</sup>

* <a name="list-star-instead-of-nested-cons"></a>
  Use `list*` no lugar de uma serie de chamadas `cons` aninhadas.
<sup>[[link](#list-star-instead-of-nested-cons)]</sup>

    ```Clojure
    ;; bom
    (list* 1 2 3 [4 5])

    ;; ruim
    (cons 1 (cons 2 (cons 3 [4 5])))
    ```

* <a name="sugared-java-interop"></a>
  Use os formulários de interoperabilidade Java.
<sup>[[link](#sugared-java-interop)]</sup>

    ```Clojure
    ;;; criação do objeto
    ;; bom
    (java.util.ArrayList. 100)

    ;; ruim
    (new java.util.ArrayList 100)

    ;;; chamada de método estático
    ;; bom
    (Math/pow 2 10)

    ;; ruim
    (. Math pow 2 10)

    ;;; chamada de instância de método
    ;; bom
    (.substring "ola" 1 3)

    ;; ruim
    (. "ola" substring 1 3)

    ;;; acesso a campo estático
    ;; bom
    Integer/MAX_VALUE

    ;; ruim
    (. Integer MAX_VALUE)

    ;;; acesso a instância de campo
    ;; bom
    (.umCampo um-objeto)

    ;; ruim
    (. um-objeto umObjeto)
    ```

* <a name="compact-metadata-notation-for-true-flags"></a>
  Use a notação compacta de metadado para metadados de contém apenas
  slots em que as keys são keywords e o valor é booleano `true`.
<sup>[[link](#compact-metadata-notation-for-true-flags)]</sup>

    ```Clojure
    ;; bom
    (def ^:private a 5)

    ;; ruim
    (def ^{:private true} a 5)
    ```

* <a name="private"></a>
  Denote partes privadas do seu código.
<sup>[[link](#private)]</sup>

    ```Clojure
    ;; bom
    (defn- funcao-privada [] ...)

    (def ^:private variavel-privada ...)

    ;; ruim
    (defn funcao-privda [] ...) ; Não é uma função privada

    (defn ^:private funcao-privada [] ...) ; muito verbosa

    (def variavel-privada ...) ; Não é privada
    ```

* <a name="access-private-var"></a>
  Para acessar uma variável privada (e.g. para testes), use a notação `@#'some.ns/var`.
<sup>[[link](#access-private-var)]</sup>

* <a name="attach-metadata-carefully"></a>
  Tenha cuidado ao que você associa seu metadado exatamente.
<sup>[[link](#attach-metadata-carefully)]</sup>

    ```Clojure
    ;; nós associamos o metadado a variável referenciada por `a`
    (def ^:private a {})
    (meta a) ;=> nil
    (meta #'a) ;=> {:private true}

    ;; nós associamos o metadado ao valor vazio do hash-map
    (def a ^:private {})
    (meta a) ;=> {:private true}
    (meta #'a) ;=> nil
    ```

## Nomenclaturas

> As únicas reais dificuldades em programação são invalidação de cache e
> nomear coisas. <br/>
> -- Phil Karlton

* <a name="ns-nomeando-schemas"></a>
  Ao nomear namespaces escolha entre os dois seguintes schemas:
<sup>[[link](#ns-naming-schemas)]</sup>

    * `project.module`
    * `organization.project.module`

* <a name="lisp-case-ns"></a>
  Use `lisp-case` em segmentos de namespace compostos(e.g. `bruce.project-euler`)
<sup>[[link](#lisp-case-ns)]</sup>

* <a name="lisp-case"></a>
  Use `lisp-case` em funções e nomes de variáveis.
<sup>[[link](#lisp-case)]</sup>

    ```Clojure
    ;; bom
    (def uma-var ...)
    (defn uma-fun ...)

    ;; ruim
    (def umaVar ...)
    (defn umafun ...)
    (def uma_fun ...)
    ```

* <a name="CamelCase-for-protocols-records-structs-and-types"></a>
  Use `CamelCase` para protocolos, registros, estruturas e tipos. (Mantenha
   acrônimos como HTTP, RFC, XML em maiúsculo.)
<sup>[[link](#CamelCase-for-protocols-records-structs-and-types)]</sup>

* <a name="pred-with-question-mark"></a>
  Os nomes dos métodos predicados (métodos que retornam um valor booleano)
     devem terminar em um ponto de interrogação
  (e.g., `impar?`).
<sup>[[link](#pred-with-question-mark)]</sup>

    ```Clojure
    ;; bom
    (defn palindrome? ...)

    ;; ruim
    (defn palindrome-p ...) ; estilo Common Lisp
    (defn is-palindrome ...) ; estilo Java
    ```

* <a name="changing-state-fns-with-exclamation-mark"></a>
  Os nomes de funções/macros que não são "safe" em transações STM
  devem terminar com uma ponto de exclamação (e.g. `reset!`).
<sup>[[link](#changing-state-fns-with-exclamation-mark)]</sup>

* <a name="arrow-instead-of-to"></a>
  Use `->` no lugar de `to` ao nomear funções de conversão.
<sup>[[link](#arrow-instead-of-to)]</sup>

    ```Clojure
    ;; bom
    (defn f->c ...)

    ;; não muito bom
    (defn f-to-c ...)
    ```

* <a name="earmuffs-for-dynamic-vars"></a>
  Use `*earmuffs*` para coisas destinatas a rebinding (ou seja, são dinâmicas).
<sup>[[link](#earmuffs-for-dynamic-vars)]</sup>

    ```Clojure
    ;; bom
    (def ^:dynamic *a* 10)

    ;; ruim
    (def ^:dynamic a 10)
    ```

* <a name="dont-flag-constants"></a>
  Não use notações especiais para constantes; tudo é constante
  a não ser que seja especificado do contrário.
<sup>[[link](#dont-flag-constants)]</sup>

* <a name="underscore-for-unused-bindings"></a>
  Use `_` para destructuring e nomes formais para argumentos que terão
  seus valores ignorado pelo código em mãos.
<sup>[[link](#underscore-for-unused-bindings)]</sup>

    ```Clojure
    ;; bom
    (let [[a b _ c] [1 2 3 4]]
      (println a b c))

    (dotimes [_ 3]
      (println "Hello!"))

    ;; ruim
    (let [[a b c d] [1 2 3 4]]
      (println a b d))

    (dotimes [i 3]
      (println "Hello!"))
    ```

* <a name="idiomatic-names"></a>
  Siga o exemplo `clojure.core`'s para nomes idiomáticos como `pred` e `coll`.
<sup>[[link](#idiomatic-names)]</sup>

    * em funções:
        * `f`, `g`, `h` - input da função
        * `n` - input inteiro, normalmente tamanho
        * `index`, `i` - index inteiro
        * `x`, `y` - números
        * `xs` - sequência
        * `m` - map
        * `s` - input string
        * `re` - expressão regular
        * `coll` - uma coleção
        * `pred` - um fechamento de predicado
        * `& more` - input variante
        * `xf` - xform, um transducer
    * em macros:
        * `expr` - uma expressão
        * `body` - o corpo de uma macro
        * `binding` - um vetor binding de uma macro

## Collections

> É melhor ter 100 funções operando em uma estrutura de dados
> do que ter 10 funções operando em 10 estrutura de dados. <br/>
> -- Alan J. Perlis

* <a name="avoid-lists"></a>
  Evite o uso de listas para armazenamento genérico de dados (a menos que uma seja
  exatamente o que você precisa).
<sup>[[link](#avoid-lists)]</sup>

* <a name="keywords-for-hash-keys"></a>
  Prefira o uso de keywords para hash keys.
<sup>[[link](#keywords-for-hash-keys)]</sup>

    ```Clojure
    ;; bom
    {:nome "Bruce" :idade 30}

    ;; ruim
    {"nome" "Bruce" "idade" 30}
    ```

* <a name="literal-col-syntax"></a>
  Prefira o uso da sintaxe da coleção literal quando aplicável. No entanto, ao
  definir conjuntos, apenas use a sintaxe literal quando os valores forem constantes
  de tempo de compilação.
<sup>[[link](#literal-col-syntax)]</sup>

    ```Clojure
    ;; bom
    [1 2 3]
    #{1 2 3}
    (hash-set (func1) (func2)) ; valores determinados em tempo de execução

    ;; ruim
    (vector 1 2 3)
    (hash-set 1 2 3)
    #{(func1) (func2)} ; irá lançar exceção de tempo de execução se (func1) = (func2)
    ```

* <a name="avoid-index-based-coll-access"></a>
  Evite acessar os membros da coleção por índice sempre que possível.
  <sup>[[link](#avoid-index-based-coll-access)]</sup>

* <a name="keywords-as-fn-to-get-map-values"></a>
  Prefira o uso de keywords como funções para recuperar valores de mapas,
  quando aplicável.
<sup>[[link](#keywords-as-fn-to-get-map-values)]</sup>

    ```Clojure
    (def m {:nome "Bruce" :idade 30})

    ;; bom
    (:nome m)

    ;; mais detalhado que o necessário
    (get m :nome)

    ;; ruim - suscetível a NullPointerException
    (m :nome)
    ```

* <a name="colls-as-fns"></a>
  Aproveite o fato de que a maioria das coleções são funções de seus elementos.
<sup>[[link](#colls-as-fns)]</sup>

    ```Clojure
    ;; bom
    (filter #{\a \e \o \i \u} "isso é um teste")

    ;; ruim - muito feio para compartilhar
    ```

* <a name="keywords-as-fns"></a>
  Aproveite o fato de que keywords podem ser usadas como funções de uma coleção.
<sup>[[link](#keywords-as-fns)]</sup>

    ```Clojure
    ((juxt :a :b) {:a "ala" :b "bala"})
    ```

* <a name="avoid-transient-colls"></a>
  Evite o uso de coleções transitórias, exceto em partes críticas de desempenho
   do código.
<sup>[[link](#avoid-transient-colls)]</sup>

* <a name="avoid-java-colls"></a>
  Evite o uso de Java collections.
<sup>[[link](#avoid-java-colls)]</sup>

* <a name="avoid-java-arrays"></a>
  Evite o uso de arrays de of Java arrays, exceto para cenários de
  interoperabilidade e código de desempenho crítico lidando fortemente com tipos
   primitivos.
<sup>[[link](#avoid-java-arrays)]</sup>

## Mutações

### Refs

* <a name="refs-io-macro"></a>
  Considere agrupar todas as chamadas I/O com a macro `io!` para evitar surpresas
  desagradáveis se acidentalmente acabar chamando esse código em uma transação.
<sup>[[link](#refs-io-macro)]</sup>

* <a name="refs-avoid-ref-set"></a>
  Evite o uso de `ref-set` sempre que possível.
<sup>[[link](#refs-avoid-ref-set)]</sup>

    ```Clojure
    (def r (ref 0))

    ;; bom
    (dosync (alter r + 5))

    ;; ruim
    (dosync (ref-set r 5))
    ```

* <a name="refs-small-transactions"></a>
  Tente manter o tamanho das transações (a quantidade de trabalho encapsulado nelas)
  o menor possível.
<sup>[[link](#refs-small-transactions)]</sup>

* <a name="refs-avoid-short-long-transactions-with-same-ref"></a>
  Evite que transações de curta e longa duração interajam com a mesma Ref.
<sup>[[link](#refs-avoid-short-long-transactions-with-same-ref)]</sup>

### Agentes

* <a name="agents-send"></a>
  Use `send` somente para ações que são ligadas à CPU e não bloqueiam em I/O
  ou outras threads.
<sup>[[link](#agents-send)]</sup>

* <a name="agents-send-off"></a>
  Use `send-off` para ações que possam bloquear, suspender ou amarrar a thread.
<sup>[[link](#agents-send-off)]</sup>

### Atoms

* <a name="atoms-no-update-within-transactions"></a>
  Evite atualizações de atom dentro de transações STM.
<sup>[[link](#atoms-no-update-within-transactions)]</sup>

* <a name="atoms-prefer-swap-over-reset"></a>
  Tente usar `swap!` no lugar de `reset!` onde for possível.
<sup>[[link](#atoms-prefer-swap-over-reset)]</sup>

    ```Clojure
    (def a (atom 0))

    ;; bom
    (swap! a + 5)

    ;; not as good
    (reset! a 5)
    ```

## Strings

* <a name="prefer-clojure-string-over-interop"></a>
  Prefira as funções de manipulação de strings do `clojure.string` do que a
  interoperabilidade Java ou escrever sua própria.
<sup>[[link](#prefer-clojure-string-over-interop)]</sup>

    ```Clojure
    ;; bom
    (clojure.string/upper-case "bruce")

    ;; ruim
    (.toUpperCase "bruce")
    ```

## Exceções

* <a name="reuse-existing-exception-types"></a>
  Reutilize os tipos de exceção existentes. O código idiomático Clojure &mdash;
  quando lança uma exceção &mdash; lança uma exceção de um tipo padrão (por exemplo, `java.lang.IllegalArgumentException`, `java.lang.UnsupportedOperationException`, `java.lang.IllegalStateException`, `java.io.IOException`).
<sup>[[link](#reuse-existing-exception-types)]</sup>

* <a name="prefer-with-open-over-finally"></a>
  Prefira `with-open` a `finally`.
<sup>[[link](#prefer-with-open-over-finally)]</sup>

## Macros

* <a name="dont-write-macro-if-fn-will-do"></a>
  Não escreva uma macro se uma função faz o mesmo.
<sup>[[link](#dont-write-macro-if-fn-will-do)]</sup>

* <a name="write-macro-usage-before-writing-the-macro"></a>
   Crie um exemplo de uso de macro primeiro e depois a macro.
<sup>[[link](#write-macro-usage-before-writing-the-macro)]</sup>

* <a name="break-complicated-macros"></a>
  Quebre as macros complicadas em funções menores sempre que possível.
<sup>[[link](#break-complicated-macros)]</sup>

* <a name="macros-as-syntactic-sugar"></a>
  Uma macro geralmente deve fornecer apenas açúcar sintático e o núcleo da macro
  deve ser uma função simples. Isso aumentará a composibilidade.
<sup>[[link](#macros-as-syntactic-sugar)]</sup>

* <a name="syntax-quoted-forms"></a>
  Prefira formulários entre aspas de sintaxe a construir listas manualmente.
<sup>[[link](#syntax-quoted-forms)]</sup>

## Comentários

> Um bom código é sua melhor documentação. Quando estiver prestes a adicionar um
> comentário, pergunte a si mesmo, "Como eu posso melhorar o código de maneira que esse
> comentário não seja necessário?" Melhore o código e então o documente para deixá-lo
> ainda mais claro. <br/>
> -- Steve McConnell

* <a name="self-documenting-code"></a>
  Se esforce para tornar seu código o mais auto-documentável possível.
<sup>[[link](#self-documenting-code)]</sup>

* <a name="four-semicolons-for-heading-comments"></a>
  Escreva comentários de cabeçalho com pelo menos quatro ponto e vírgulas.
<sup>[[link](#four-semicolons-for-heading-comments)]</sup>

* <a name="three-semicolons-for-top-level-comments"></a>
  Escreva comentários top-level com três ponto e vírgulas.
<sup>[[link](#three-semicolons-for-top-level-comments)]</sup>

* <a name="two-semicolons-for-code-fragment"></a>
  Escreva comentários em um fragmento particular de código antes do fragmento
e alinhado com o mesmo, utilizando dois ponto e vírgulas.
<sup>[[link](#two-semicolons-for-code-fragment)]</sup>

* <a name="one-semicolon-for-margin-comments"></a>
  Escreva comentários de margem com um ponto e vírgula.
<sup>[[link](#one-semicolon-for-margin-comments)]</sup>

* <a name="semicolon-space"></a>
  Sempre tenha pelo menos um espaço entre o ponto e vírgula
  e o texto que o segue.
<sup>[[link](#semicolon-space)]</sup>

    ```Clojure
    ;;;; Frob Grovel

    ;;; Esse código tem algumas implicações importantes:
    ;;;   1. Foo.
    ;;;   2. Bar.
    ;;;   3. Baz.

    (defn fnord [zarquon]
      ;; Se zob, então veeblefitz.
      (quux zot
            mumble             ; Zibblefrotz.
            frotz))
    ```

* <a name="english-syntax"></a>
  Comentários maiores que uma palavra começam com letra maiúscula e usam
  pontuação. Sentenças separadas com
  [um espaço](http://en.wikipedia.org/wiki/Sentence_spacing).
<sup>[[link](#english-syntax)]</sup>

* <a name="no-superfluous-comments"></a>
  Evite comentários supérfulos.
<sup>[[link](#no-superfluous-comments)]</sup>

    ```Clojure
    ;; ruim
    (inc counter) ; incrementa contador em um
    ```

* <a name="comment-upkeep"></a>
  Mantenha os comentários existentes atualizados. Um comentário desatualizado é
  pior que nenhum comentário.
<sup>[[link](#comment-upkeep)]</sup>

* <a name="dash-underscore-reader-macro"></a>
  Prefira o uso do macro leitor `#_` no lugar de um comentário normal quando
  precisar comentar um formulário específico.
<sup>[[link](#dash-underscore-reader-macro)]</sup>

    ```Clojure
    ;; bom
    (+ foo #_(bar x) delta)

    ;; ruim
    (+ foo
       ;; (bar x)
       delta)
    ```

> Um bom código é como uma boa piada - não precisa de explicação. <br/>
> -- Russ Olsen

* <a name="refactor-dont-comment"></a>
  Evite escrever comentários para explicar um código ruim. Refatore o código para
  torná-lo auto-explicativo. ("Faça ou não faça. Não há tentativa." --Yoda)
<sup>[[link](#refactor-dont-comment)]</sup>

### Anotações de Comentários

* <a name="annotate-above"></a>
  Anotações devem geralmente ser escritas na linha imediatamente acima da linha
  do código de relevância.
<sup>[[link](#annotate-above)]</sup>

    ```Clojure
    ;; good
    (defn some-fun
      []
      ;; FIXME: Replace baz with the newer bar.
      (baz))

    ;; bad
    ;; FIXME: Replace baz with the newer bar.
    (defn some-fun
      []
      (baz))
    ```

* <a name="annotate-keywords"></a>
  A palavra-chave de anotação é seguida por dois-pontos e um espaço, depois por
  uma nota descrevendo o problema.
<sup>[[link](#annotate-keywords)]</sup>

    ```Clojure
    ;; good
    (defn some-fun
      []
      ;; FIXME: Replace baz with the newer bar.
      (baz))

    ;; bad - no colon after annotation
    (defn some-fun
      []
      ;; FIXME Replace baz with the newer bar.
      (baz))

    ;; bad - no space after colon
    (defn some-fun
      []
      ;; FIXME:Replace baz with the newer bar.
      (baz))
    ```

* <a name="indent-annotations"></a>
  Se várias linhas forem necessárias para descrever o problema, as linhas
  subsequentes deverão ser indentadas tanto quanto a primeira.
<sup>[[link](#indent-annotations)]</sup>

    ```Clojure
    ;; good
    (defn some-fun
      []
      ;; FIXME: This has crashed occasionally since v1.2.3. It may
      ;;        be related to the BarBazUtil upgrade. (xz 13-1-31)
      (baz))

    ;; bad
    (defn some-fun
      []
      ;; FIXME: This has crashed occasionally since v1.2.3. It may
      ;; be related to the BarBazUtil upgrade. (xz 13-1-31)
      (baz))
    ```

* <a name="sing-and-date-annotations"></a>
  Marque a anotação com suas iniciais e uma data para que sua relevância possa
  ser facilmente verificada.
<sup>[[link](#sing-and-date-annotations)]</sup>

    ```Clojure
    (defn some-fun
      []
      ;; FIXME: Isso quebra ocasionalmente desde v1.2.3. Isso pode
      ;;        estar relacionado a atualização do BarBazUtil. (xz 13-1-31)
      (baz))
    ```

* <a name="rare-eol-annotations"></a>
  Em casos que o problema é tão óbvio que qualquer documentação se tornaria redundante,
  anotações podem ser deixadas no final da linha ofensiva sem anotações. Esse uso
  deve ser a exceção e não a regra.
<sup>[[link](#rare-eol-annotations)]</sup>

    ```Clojure
    (defn bar
      []
      (sleep 100)) ; OPTIMIZE
    ```

* <a name="todo"></a>
  Use `TODO` para deixar uma observação de recursos ou funcionalidades ausentes
  que devem ser adicionados posteriormente.
<sup>[[link](#todo)]</sup>

* <a name="fixme"></a>
  Use `FIXME` para deixar uma observação de código quebrado que precisa ser
  corrigido.
<sup>[[link](#fixme)]</sup>

* <a name="optimize"></a>
  Use `OPTIMIZE` para deixar uma observação de código lento ou ineficiente que pode
  causar problemas de desempenho.
<sup>[[link](#optimize)]</sup>

* <a name="hack"></a>
  Use `HACK` para deixar uma observação de to note "code smells" onde práticas de
  codificação questionáveis foram utilizadas e devem ser refatoradas.
<sup>[[link](#hack)]</sup>

* <a name="review"></a>
  Use `REVIEW` para deixar uma observação de qualquer coisa que deva ser analisada
  para confirmar se está funcionando conforme o esperado. Por exemplo: `REVIEW:
  Temos certeza de que é assim que o cliente faz X atualmente?`
<sup>[[link](#review)]</sup>

* <a name="document-annotations"></a>
  Use outras palavras-chave de anotação personalizadas se parecer apropriado, mas
  certifique-se de documentá-las no `README` do seu projeto ou similar.
<sup>[[link](#document-annotations)]</sup>

## Documentação

Docstrings are the primary way to document Clojure code. Many definition forms
(e.g. `def`, `defn`, `defmacro`, `ns`)
support docstrings and usually it's a good idea to make good use of them, regardless
of whether the var in question is something public or private.

If a definition form doesn't support docstrings directly you can still supply them via
the `:doc` metadata attribute.

This section outlines some of the common conventions and best
practices for documenting Clojure code.


* <a name="prefer-docstrings"></a>
  If a form supports docstrings directly prefer them over using `:doc` metadata:
  <sup>[[link](#prefer-docstrings)]</sup>

```clojure
;; good
(defn foo
  "This function doesn't do much."
  []
  ...)

(ns foo.bar.core
  "That's an awesome library.")

;; bad
(defn foo
  ^{:doc "This function doesn't do much."}
  []
  ...)

(ns ^{:doc "That's an awesome library.")
  foo.bar.core)
```

* <a name="docstring-summary"></a>
Let the first line in the doc string be a complete, capitalized
sentence which concisely describes the var in question. This makes it
easy for tooling (Clojure editors and IDEs) to display a short a summary of
the docstring at various places.
<sup>[[link](#docstring-summary)]</sup>

```clojure
;; good
(defn frobnitz
  "This function does a frobnitz.
  It will do gnorwatz to achieve this, but only under certain
  circumstances."
  []
  ...)

;; bad
(defn frobnitz
  "This function does a frobnitz. It will do gnorwatz to
  achieve this, but only under certain circumstances."
  []
  ...)
```

* <a name="document-pos-arguments"></a>
Document all positional arguments, and wrap them them with backticks
(\`) so that editors and IDEs can identify them and potentially provide extra
functionality for them.
<sup>[[link](#document-pos-arguments)]</sup>

```clojure
;; good
(defn watsitz
  "Watsitz takes a `frob` and converts it to a znoot.
  When the `frob` is negative, the znoot becomes angry."
  [frob]
  ...)

;; bad
(defn watsitz
  "Watsitz takes a frob and converts it to a znoot.
  When the frob is negative, the znoot becomes angry."
  [frob]
  ...)
```

* <a name="document-references"></a>
Wrap any var references in the docstring with \` so that tooling
can identify them.
<sup>[[link](#document-references)]</sup>

```clojure
;; good
(defn wombat
  "Acts much like `clojure.core/identity` except when it doesn't.
  Takes `x` as an argument and returns that. If it feels like it."
  [x]
  ...)

;; bad
(defn wombat
  "Acts much like clojure.core/identity except when it doesn't.
  Takes `x` as an argument and returns that. If it feels like it."
  [x]
  ...)
```

* <a name="docstring-grammar"></a> Docstrings should be comprised from
proper English sentences - this means every sentences should start
with an capitalized word and should end with the proper punctuation. Sentences
should be separated with a single space.
<sup>[[link](#docstring-grammar)]</sup>

```clojure
;; good
(def foo
  "All sentences should end with a period (or maybe an exclamation mark).
  And the period should be followed by a space, unless it's the last sentence.")

;; bad
(def foo
  "all sentences should end with a period (or maybe an exclamation mark).
  And the period should be followed by a space, unless it's the last sentence")
```

* <a name="docstring-indentation"></a>
Indent multi-line docstrings by two spaces.
<sup>[[link](#docstring-indentation)]</sup>

```clojure
;; good
(ns my.ns
  "It is actually possible to document a ns.
  It's a nice place to describe the purpose of the namespace and maybe even
  the overall conventions used. Note how _not_ indenting the doc string makes
  it easier for tooling to display it correctly.")

;; bad
(ns my.ns
  "It is actually possible to document a ns.
It's a nice place to describe the purpose of the namespace and maybe even
the overall conventions used. Note how _not_ indenting the doc string makes
it easier for tooling to display it correctly.")
```

* <a name="docstring-leading-trailing-whitespace"></a>
Neither start nor end your doc strings with any whitespace.
<sup>[[link](#docstring-leading-trailing-whitespace)]</sup>

```clojure
;; good
(def foo
  "I'm so awesome."
  42)

;; bad
(def silly
  "    It's just silly to start a doc string with spaces.
  Just as silly as it is to end it with a bunch of them.      "
  42)
```

* <a name="docstring-after-fn-name"></a>
  When adding a docstring – especially to a function using the above form – take
  care to correctly place the docstring after the function name, not after the
  argument vector.  The latter is not invalid syntax and won’t cause an error,
  but includes the string as a form in the function body without attaching it to
  the var as documentation.
<sup>[[link](#docstring-after-fn-name)]</sup>

```Clojure
;; good
(defn foo
  "docstring"
  [x]
  (bar x))

;; bad
(defn foo [x]
  "docstring"
  (bar x))
```

## Existencial

* <a name="be-functional"></a>
  Codifique de maneira funcional, usando mutação somente quando faz sentido.
<sup>[[link](#be-functional)]</sup>

* <a name="be-consistent"></a>
  Seja consistente. Em um mundo ideal, seja consistente com essas diretrizes.
<sup>[[link](#be-consistent)]</sup>

* <a name="common-sense"></a>
  Use o senso comum.
<sup>[[link](#common-sense)]</sup>

## Ferramentas

Existem algumas ferramentas criadas pela comunidade Clojure que podem ajudá-lo
em seu esforço em escrever o código de Clojure idiomático.

* [Slamhound](https://github.com/technomancy/slamhound) é uma ferramenta que
  gerará automaticamente declarações `ns` adequadas a partir do seu código existente.

* [kibit](https://github.com/jonase/kibit) é um analisador de código estático
  para Clojure que usa [core.logic](https://github.com/clojure/core.logic) para
  procurar padrões de código para os quais pode haver uma função ou macro mais idiomático.

## Testando

 * <a name="test-directory-structure"></a>
   Armazene seus testes em um diretório separado, normalmente `test/seuprojeto/` (em
   oposição a `src/seuprojeto/`). Sua ferramenta de compilação é responsável por
   disponibilizá-los nos contextos onde são necessários; a maioria dos modelos
   vai fazer isso automaticamente para você.
   <sup>[[link](#test-directory-structure)]</sup>

 * <a name="test-ns-naming"></a>
  Nomeie seu ns `seuprojeto.algumacoisa-test`, um arquivo que geralmente está em
   `test/seuprojeto/algumacoisa_test.clj` (ou `.cljc`, `cljs`).
   <sup>[[link](#test-ns-naming)]</sup>

 * <a name="test-naming"></a> Ao usar `clojure.test`, defina seus testes
   com `deftest` and nomeie com `algumacoisa-test`. For example:

   ```clojure
   ;; bom
   (deftest algumacoisa-test ...)

   ;; ruim
   (deftest algumacoisa-tests ...)
   (deftest test-algumacoisa ...)
   (deftest algumacoisa ...)
   ```

   <sup>[[link](#test-naming)]</sup>

## Library Organization

 * <a name="lib-coordinates"></a>
   If you are publishing libraries to be used by others, make sure to
   follow the [Central Repository
   guidelines](http://central.sonatype.org/pages/choosing-your-coordinates.html)
   for choosing your `groupId` and `artifactId`. This helps to prevent
   name conflicts and facilitates the widest possible use. A good
   example is [Component](https://github.com/stuartsierra/component).
   <sup>[[link](#lib-coordinates)]</sup>

 * <a name="lib-min-dependencies"></a>
   Avoid unnecessary dependencies. For example, a three-line utility
   function copied into a project is usually better than a dependency
   that drags in hundreds of vars you do not plan to use.
   <sup>[[link](#lib-min-dependencies)]</sup>

 * <a name="lib-core-separate-from-tools"></a>
   Deliver core functionality and integration points in separate
   artifacts.  That way, consumers can consume your library without
   being constrained by your unrelated tooling prefences. For example,
   [Component](https://github.com/stuartsierra/component) provides
   core functionality, and
   [reloaded](https://github.com/stuartsierra/reloaded) provides leiningen
   integration.
   <sup>[[link](#lib-core-separate-from-tools)]</sup>

# Contribuindo

Nada de escrito neste guia é definitivo. É meu desejo trabalhar em conjunto com
todos os interessados no estilo de codificação em Clojure, para que possamos
criar um recurso que seja benéfico para toda a comunidade Clojure.

Sinta-se à vontade para abrir tickets ou enviar pull requests com melhorias.
Agradeço antecipadamente por sua ajuda!


Você também pode apoiar o guia de estilo com contribuições financeiras via [gittip](https://www.gittip.com/bbatsov).

[![Apoie via Gittip](https://rawgithub.com/twolfson/gittip-badge/0.2.0/dist/gittip.png)](https://www.gittip.com/bbatsov)

# Licença

![Creative Commons License](http://i.creativecommons.org/l/by/3.0/88x31.png)
Este trabalho é licenciado sob
[Creative Commons Attribution 3.0 Unported License](http://creativecommons.org/licenses/by/3.0/deed.en_US)

# Espalhe a Palavra

Um guia de estilo dirigido pela comunidade é de pouca utilidade para uma
comunidade que não sabe sobre sua existência. Tweet sobre o guia, compartilhe
com seus amigos e colegas. Todos os comentários, sugestões ou opiniões que nós
recebemos faz o guia um pouco melhor. E queremos ter o melhor guia possível, não é?

Felicidades,<br/>
[Bozhidar](https://twitter.com/bbatsov)
