# Git e Gitflow

Esse é o manual de procedimentos que envolvem git e gitflow aqui na Wooza. Vamos detalhar todo o nosso fluxo, fiquem à vontade em sugerir melhorias em nosso fluxo. Em breve colocaremos mais detalhes que vão envolver micro-services, etc.

Aqui usamos o Azure DevOps como ferramente de Git e CI/CD.

## Pequena Introdução

Não sei o quanto vocês manjam de terminal, ssh e afins, mas fica a dica: tentem usar o máximo possível disso. É chato, digita mais, o visual é mais fácil? Sim, porém, o mercado pede muito disso em algumas empresas fodásticas.

Teve uma entrevista que participei que o cara me perguntou se eu usava Mac e mexia com Terminal, falei que sim e odiava ferramentas visuais do Git, IDEs, etc. Recebi um puta elogio por isso e eles levam muito em consideração.

## Vamos falar de Gitflow

Segue uma imagem abaixo pra exemplificar:

![Gitflow](../images/gitflow.png)

Temos a branch master, que terá um espelho, que será uma tag. Pra quem não entende sobre tags no Git eu recomendo ver o link: [[https://git-scm.com/book/pt-br/v1/Git-Essencial-Tagging]].

Ela nada mais é que uma versão fechada e que não pode ser alterada, portanto, produção é o espelho da última tag.

### Branches principais

Temos três branches principais:

* develop
* homolog
* master

**NUNCA** commite algo diretamente nessas branches, crie sempre uma branch a partir delas para fazer alguma modificação. Por menor que seja, **SEMPRE** crie uma branch.

Nosso fluxo perfeito de trabalho é criando branches a partir da **develop**. Essa branch é a que terá no ambiente de desenvolvimento, assim como a **homolog** é o espelho do que está no ambiente de homologação.

### Branches filhas

Temos o seguinte padrão para as branches filhas:

* bugfix/
* hotfix/
* feature/
* improvement/

Uma _feature/_ é sempre criada a partir da **develop**, a não ser que seja um componente que publicaremos no NPM, que no caso criamos as features a partir da **master**, não possuímos nem **develop** e nem **homolog** em componentes gerais para a empresa.

Uma branch _bugfix/_ pode ser criada a partir da **master** ou **homolog**, pois é uma branch emergencial. Caso isso seja feito em uma dessas branches, NÃO PODE ESQUECER de descer as modificações para as branches inferiores, por exemplo, se um _bugfix_ foi feito na **master**, você precisa descer a atualização para a **homolog** e **develop** **COM REBASE**, caso seja na **homolog**, depois precisa descer para a **develop** **COM REBASE**.

Uma branch _hotfix/_ chega a ser similar a **bugfix**, porém, ela é para correções rápidas que não são bugs, por exemplo, um acerto de texto, pontuação, cores, espaçamentos, coisas que são urgentes, mas não causam bugs.

Geralmente usamos a feature, que pelo nome já deu pra ver o que é. A bugfix, hotfix e improvement é para melhorar ou corrigir alguma feature que criamos. Depois que finalizarmos a feature, vamos criar o Pull Request.

### Nomes de branches

Já vimos nossos prefixos acima, porém, eles sozinhos não resolvem nada, três branches precisam de um sufixo, que será o seu nome _identificável_.

Pode ser qualquer nome, mas eles sempre devem ser com letras minúsculas. Caso seja um nome composto, será separado por traços _-_.

Vamos supor que seja uma solução para um problema no cartão de crédito:

* **bugfix/retorno-api-cartao**

Lembrando sempre de dar nomes que você tenha uma ideia do que será feito. **NUNCA** vamos usar o ID da Issue do Azure DevOps para nomear as branches. Assim como usar um **bugfix/resolvendo-bugs**, que não mostra o que está sendo realmente feito.

## Mensagens de commit

**NUNCA** faça commits com a mesma mensagem. Seja o mais específico possível com suas mensagens de commit. Se o desenvolvedor souber o que foi feito no PR somente com as mensagens de commit, ela foi feita de forma correta.

Seguimos o padrão do Commitizen, que tem abaixo a seguinte lista de prefixos de commit:

![Adicionando um commit](../images/add-commit.png)

[Veja mais detalhes sobre como usar o commitizen](https://github.com/commitizen/cz-cli).

Siga o seguinte padrão de mensagem pra commit que você pode fazer no Gitbash, Cmder ou ConEmu:

```
git commit -m "feat: Head de tudo que foi feito

#ID_DA_TAREFA_QUE_VEM_NO_AZURE_DEVOPS
* descricao de algo que foi feito
* mas outra coisa que foi feita
* mais outra coisa feita"
```

Esse **ID_DA_TAREFA_QUE_VEM_NO_AZURE_DEVOPS** é aquele ID da tarefa que vocês pegam no Azure DevOps e geralmente tem 5 números. Fazendo isso você vincula aquela tarefa ao commit e ao PR, com isso, ao mergear o PR, sua tarefa será movida para //Resolved// automaticamente. Essa opção é opcional, mas caso queira colocar, faça isso **SOMENTE EM UM COMMIT**.

Claro, não são todos os commits que precisam de uma mensagem grande dessa forma, nesses casos eu uso da seguinte forma:

`git commit -m "refactor: Tarefa realizada com uma descricao completa"`

Depois eu pego essa mensagem e colo numa seção chamada **## Outros** dentro da descrição do PR.

Não coloque acentuação e caracteres especiais nas mensagens de commit para evitar bugs malucos nos terminais.

### Commitlint

Um cara que usamos também é o [commitlint](https://github.com/conventional-changelog/commitlint). Com ele nós verificamos se sua mensagem de commit está dentro dos nossos padrões. Caso não seja, seu commit não será realizado.
