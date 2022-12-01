## Avaliação sobre os critérios

Aqui usamos um escala de 1 a 5. 1 significa extrema pobreza no uso das práticas enquanto 5 fala com excelência. 

1. O quão bem o código utiliza os recursos disponíveis nas tecnologias da stack escolhida em cima daquela tecnologia
    1. **Nota**: 1
    1. **Motivo**: O código repetidamente não demonstra uso dos recursos oferecidos por padrão na linguagem. Um exmeplo é a falta de uso do Optional. Também não demonstra o bom uso de recursos providos pelo framework, como a falta de utilização do Controller Advice para centralizar tratamentos comuns a todos os controllers. Pode-se falar também da falta das asserções mais específicas do JUnit para verificar o estado interno dos objetos. 
    1. **Um pouco de analytics**: A [ferramenta Spring Lint](https://github.com/mauricioaniche/springlint) encontrou mais de 102 smells de código MVC. Ela tem o foco de analisar anti patterns em projetos java utilizando Spring. Neste caso a grande maioria dos mal cheiros foram encontrados nos controllers. 
    
1. Nível de refinamento das heurísticas para tomada de decisão sobre detalhes do design do código
    1. **Nota**: 1
    1. **Motivo**: O padrão do código é criar classes quando tem que salvar no banco, controllers para receber requisições e services para concentrar a suposta lógica. Além disso qualquer alteração de estado dos objetos é realizada através do uso direto dos métodos de acesso. A consequência disso, por exemplo, no service, é que eles tendem a ficar cada vez maiores. As classes de domínio também flertam demais com modelos anêmicos. Na prática um modelo anêmico significa falta de coesão o que afeta a testabilidade, por exemplo. 
    
1. O quão revelador de bugs aquele código é.
    1. **Nota**: 1
    1. **Motivo**: A ideia aqui é analisar o interesse do código de produção em revelar os problemas o mais cedo possível. Isso é inexistente. A constante passagem de nulo como argumento, a falta de checagem de pré e pós condições dentro dos métodos, assim como a falta de tratamento de erros para as chamadas remotas indicam isso. 

5. Os testes automatizados de fato utilizam as melhores técnicas de modo a potencializar a revelação de bugs e, consequentemente, aumentar o nível de confiabilidade do código?
    1. **Nota**: 2
    1. **Motivo**: Os testes são superficiais quando falamos das asserções. Verificam um percentual baixíssimo do estado de cada objeto sob verificação. Além disso a cobertura de código não está adequada, visto que não navega pela quantidade mínima de caminhos para uma determinada lógica. 

6. Como estão as práticas de refatoração no código? Ela é feita em algum momento? Nunca é feita?
    1. **Nota**: 1
    1. **Motivo**: A ferramenta [refactoringMiner](https://github.com/tsantalis/RefactoringMiner) tenta identificar diferenças entre commits que sinalizem que alguma prática de refatoração foi feita. Os últimos 100 commits do projeto não demonstraram nenhuma refatoração. Importante reforçar que a análise é baseada em heurísticas e claro que existe a chance da heurística ter dado errado. Por outro lado ela é usada recorrentemente em pesquisas de repositórios e por conta disso consideramos que ela possui um bom índice de confiabilidade. 

8. Quando olhamos para as métricas que servem de proxy para a qualidade do código como LOC, Coesão, Acoplamento entre outras, qual o nível de saúde do código
    1. **Nota**: 1
    1. **Motivo**: Coesão baixa, acoplamento muito alto, número de linhas de código só na crescente 
    1. **Análise complementar**: Rodamos uma ferramenta que análise a evolução do número de linhas de código e qual a tendência para o futuro. O resumo é que a tendência no número de linhas é aumentar indefinidamente. É importante ressaltar que existe uma correlação entre número de linhas e complexidade. Não se deve considerar apenas este fator, pois ele pode gerar incentivo perverso. Ao mesmo tempo é uma bandeira amarela.

9. Considerando diversas combinações de itens derivadas do CDD, o quão fácil está de entender?
    1. **Nota**: 1
    1. **Motivo**: O CDD é uma teoria de design que analisa, dado um determinado contexto, o quão fácil é de entender um código. Selecionamos algumas combinações de itens e em todos o código se mostra de dificil de entendimento.
    1. **Análise complementar**: Para uma combinação de itens de complexidade em específico(acoplamento, branch, lambdas) encontramos 160 classes acima do limimte. Dessas, 47 eram service e 41 eram controllers. Aqui tem dois sinais. O código deve ser dificil de entender e também o simples fato de dividir em camadas seguindo alguma inspiração arquitetural está longe de servir de garantia quando falamos em qualidade. 

10. Qual o nível de preocupação que o código dá sobre o mínimo de preocupação com performance,escalabilidade e resiliência
    1. **Nota**: 1
    1. **Motivo**: O que mais penaliza a performance aqui, como é de praxe, é a execuçao de operações de I/O. Por exemplo, existem chamadas remotas que são feitas dentro de loop, o que demonstra uma falta de cuidado na granularidade do serviço impactando diretamente a performance. Como já falado antes, o código não demonstra preocupação em revelar bugs e isso também é percebido nas chamadas remotas. Não existe nenhuma expectativa que uma chamada HTTP retorne um status de erro ou que demore mais do que o normal para executar. 

10. Olhando para documentação dentro do próprio código + repo em geral, como está a qualidade? 
    1. **Nota**: 2
    1. **Motivo**: Apesar da documentação geral ser pobre, existe alguns comentários inline que tentam explicar pedaços do código. Acreditamos que documentação, quando bem utilizada, acelera a produtividade e onboarding de novas pessoas no time.  

11. Pensando em LGPD e privacidade no geral, o código demonstra preocupação com isso?
    1. **Nota**: 2
    1. Nossa ferramenta encontrou 391 potenciais violações de dados pessoais e 22 potenciais violações de dados sensíveis, somente nos arquivos Java. Se considerar todo o projeto, o número sobe para 1.581 e 203, respectivamente.

13. E pensando em segurança, o que o código fala para a gente?
    1. Não consegui ter algum resultado aqui, pois precisava compilar o SSP pra poder rodar as ferramentas :-(


## Plano de ação para melhorar o nível técnico da equipe

### Priorização dos pontos que precisam ser trabalhados

O plano de ação tenta priorizar o que parece ser a maior dor para que a equipe consiga produzir um código bom o suficiente para sustentar o produto da melhor maneira possível. 

Trabalhamos com um tripé que consideramos essencial para maximizar a chance de elevação de régua de conhecimento e de capacidade de utilização de tal conhecimento nos cenários desejados.

1. Quando for necessário, trabalhar a teoria para que as pessoas saibam o que é necessário fazer. 
2. Exagerar na prática, fazendo de maneira progressiva, para que as pessoas possam exercitar em cenários cada vez mais próximos da realidade. Aqui não consideramos apenas complexidade técnica, mas todos os fatores que são esperados daquela equipe. Nossa inspiração aqui vem do esporte, que atualmente, junto com as profissões de risco, possui o sistema de treino mais efetivo do planeta. 
3. Máximo de feedback sobre a aprendizagem. É muito fácil consumir algo, praticar e já achar que está no caminho certo. A avaliação recorrente sobre o que está sendo aprendido serve de GPS aumentando as chances de irmos pelo caminho correto. 

Agora para os itens que entendemos que necessitam de melhora. 

## Dominar os recursos básicos da Stack de tecnologia. 

A equipe precisa dominar mais os recursos mais básicos da linguagem Java, Spring e Hibernate. O básico bem treinado deve fazer com que o time entregue tudo com muito mais qualidade. **Java**: Aqui estamos falando de uso mais racional das abstrações disponíveis na própria linguagem como as coleções, tipo opcional e interfaces funcionais. **Spring**: Aqui a ideia é usar melhor os mecanismos de validação de entrada de dados, controle de exceptions, integrações com libs que facilitam o trabalho como o Feign. **Hibernate**: Aqui é praticar muito o controle do estado do objeto gerenciado pelo hibernate e ter total clareza do que está lidando. 

### Plano de ação

1. Realizar os treinamentos que temos prontos na nossa plataforma de ensino sobre os aspectos mapeados do Spring e Hibernate. 
1. Como não temos treinamentos prontos sobre os recursos padrões da API do Java, podemos oferecer uma série de workshosp(3 ou mais) que conseguimos construir num prazo bem curto. Gostamos de usar 3 ou mais para que possamos trabalhar claramente uma progressão de dificuldade. 
1. Também entendemos ser interessante uma outra série de workshops(3 ou mais) para praticarmos juntos todas as habilidades em conjunto. Aqui é mais uma oportunidade de avaliarmos o nível da equipe e também de trazer mais componentes como pressão, requisitos mal descritos etc. 

## Melhoria nas práticas de design de código

Em segundo lugar o código apresenta práticas muito pobres de design. Aqui é necessário muita prática em cima da combinação clássica: Alta coesão e baixo aclopamento. Provavelmente a equipe carece de heurísticas mais refinadas para decidir onde os códigos vão viver. Também parece importante exercitar muito a prática de programação defensiva. Um meio de custo baixo e de impacto interessante quando falamos em aumentar a confiabilidade de execução. Para fechar é necessário praticar muita refatoração. Aparentemente essa não é uma prática regular na equipe. 

### Plano de ação

1. Três ou mais sessões de workshop focadas em práticas relativas divisão de responsabilidades, coesão e programação defensiva. 
1. Uma sessão extra de workshop para que possamos aprofundar a ideia de utilização de camadas para postegar decisões, se proteger de mudanças etc. Aqui entendemos que a equipe até sabe o que fazer, mas podemos ter trabalhar mais opções, principalmente utilização de interfaces para inverter dependências. 
1. Três ou mais sessões de refatoração para que seja possível praticar isso em códigos bem diferentes

## Aprofundamento em testes

Ainda sobre aspectos básicos de qualidade. Os testes não cumprem o papel que deveriam. A falta de asserts e de cobertura considerando as branches do código não ajuda nem na confiança para refatorar e testar o código pós correções de bugs e evoluções assim como não ajuda a revelar problemas o mais cedo possível. 

### Plano de ação

1. Realizar os treinamentos na nossa plataforma sobre testes e também do módulo de testes do Spring. 
1. Um workshop com a intenção de reforçar o feedack sobre o aprendizado relativo aos testes. 

## Sobre privacidade

Quando olhamos para privacidade parece que não existe qualquer ideia de como implementar os comportamentos esperados a nível de código. Aqui a ideia é partir do básico para, inicialmente, gerar consciência sobre o assunto. 

### Plano de ação

Aqui não parece fazer sentido já realizar uma imersão mais profunda para melhorar as habilidades práticas das pessoas. Nesse momento parece mais adequado algumas palestras sobre o tema para gerar este sentimento de alerta que isso é importante. Talvez valha a pena um workshop para realizarmos novas inspeções no código. 

## Sobre resiliência

Os pontos de integração do sistema não demonstram qualquer preocupação com resiliência. Não existe uma sistematização para lidar com estouros de tempo, problemas de conexão, retornos como erro etc. Este é um ponto onde o sistema está operando basicamente na sorte. Aqui parece necessário tanto um apuro teórico quanto muita prática para que essa preocupação seja padronizada. 

### Plano de ação

1. Três ou mais workshops focados exclusivamente em construir uma sistematização para trazer resiliência para as aplicações. O conceito de resiliência não é complicado, o problema é ter repertório para as múltiplas situações que podem aparecer. É isso que vamos atacar. 

## Documentação como cidadã de primeiro nível. 

O código carece demais de documentação. Este é um tema controverso e muitos times, até por conta de uma linha de argumentação de referências na comunidade, consideram que este não é um tema prioritário. Nós, por outro lado, entendemos que documentação pode ser um fator de aumento de produtividade muito interessante. A ideia é trabalhar quais tipos de documentação podem ser escritas e exercitar muito isso em cima da base de código que já existe. 

### Plano de ação

Aqui entendemos que pode ser válido uma palestra explorando uma variedade de formas de documentação que quando combinadas tendem a facilitar o entendimento do código para quem já trabalha no produto e para as novas pessoas que vão chegar. 

## Oportunidades extras

Além das ações de treinamento, também entendemos que existem oportunidade de investigação de pesquisa recorrentes dentro do contexto de times de engenharia. Temos um time especializado nisso e podemos realizar um trabalho de três meses trabalhando em cima de hipóteses para validar ou refutar cada uma delas. 

Isso gera ainda mais visão crítica para a própria equipe facilitando futuras interevenções. 



