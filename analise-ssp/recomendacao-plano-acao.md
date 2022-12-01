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
