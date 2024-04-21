# **cdcq - Uma Ferramenta para Consulta de AST em Cadence**

cdcq é uma poderosa ferramenta de linha de comando que permite consultar e analisar a Árvore de Sintaxe Abstrata (AST) dos seus contratos inteligentes em Cadence.

## **Uso**

```bash
cdcq <filename> <query>
```

### **Exemplos**

*Listar todas as funções no contrato:*

```bash
bashCopy code
./cdcq ExampleToken.cdc ".Function | Access: {Function.Access} name: {Function.Identifier}"

```

Resultado:

```makefile
makefileCopy code
Access: AccessPublic name: withdraw
Access: AccessPublic name: deposit
Access: AccessPublic name: withdraw
Access: AccessPublic name: deposit
Access: AccessPublic name: createEmptyVault
Access: AccessPublic name: mintTokens

```

*Listar todos os composites com CompositeKind Resource:*

```bash
bashCopy code
cdcq ExampleToken.cdc ".Composite[CompositeKind=~Resource] | {Composite.Identifier}"

```

Resultado:

```
cadenceCopy code
 Vault
 VaultMinter

```

*Listar recursos e suas conformidades:*

```bash
bashCopy code
cdcq ExampleToken.cdc ".Composite[CompositeKind=~Resource] | {Composite.Identifier} {Composite.Conformances}"

```

Resultado:

```
cadenceCopy code
 Vault [Provider Receiver Balance]
 VaultMinter []

```

*Listar declarações de variáveis:*

```bash
bashCopy code
cdcq ExampleToken.cdc ".Variable | variable: {Variable}"

```

Resultado:

```
cadenceCopy code
variable: let recipientRef = recipient.borrow() ?? panic("Could not borrow a receiver reference to the vault")
variable: let vault <- create Vault(balance: self.totalSupply)

```

## **Nota:**

A documentação está um pouco deficiente atualmente, deixe que as mensagens de erro te guiem :)

```bash
bashCopy code
./cdcq ExampleToken.cdc ".c | "

```

Resultado:

```mathematica
mathematicaCopy code
Tipo de elemento inválido `c`.
Tipos disponíveis: Array, Assignment, Attach, Attachment, Binary, Block, Bool, Break, Casting, Composite, Conditional, Continue, Create, Declaration, Destroy, Dictionary, Emit, EnumCase, Expression, Field, FixedPoint, For, Force, Function, FunctionBlock, FunctionExpression, Identifier, If, Import, Index, Integer, Interface, Invocation, Member, Nil, Path, Pragma, Program, Reference, Remove, Return, SpecialFunction, Statement, String, Swap, Switch, Transaction, Unary, Variable, Void, While

```

## **Executando em Múltiplos Arquivos**

Para analisar vários arquivos Cadence, use o comando **`find`**. (Você pode adicionar a variável Path para incluir o nome do arquivo no resultado):

```bash
bashCopy code
find . -type f -name "*.cdc" -exec cdcq {} ".Variable | {Path} variable: {Variable}" \;

```

## **Sintaxe de Consulta**

Uma consulta cdcq consiste em uma seção de filtro e uma seção de exibição, separadas por um pipe (**`|`**).

### **Filtro**

- **Seleção:** Começa com um ponto (**`.`**) seguido pelo tipo de elemento AST (por exemplo, **`.Function`**, **`.Composite`**).
- **Escolha:** Filtre mais com pares chave-valor entre colchetes (**`[]`**).
    - **Chave:** Um campo válido dentro do elemento AST.
    - **Valor:** O valor desejado para corresponder. Use **`~`** para correspondência de "contém".
    - **Operações Suportadas:** **`=`** (correspondência exata), **`!=`** (não igual).

### **Exibição**

- Uma string de formato onde as variáveis são os nomes dos elementos AST do seu filtro, entre chaves (**`{}`**).

## **Contribuindo**

cdcq é um projeto de código aberto. Aceitamos contribuições, problemas e pedidos de recursos. Para começar, consulte nossas diretrizes de contribuição no [link para
