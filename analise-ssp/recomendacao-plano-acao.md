## Avaliação sobre os critérios

Aqui usamos um escala de 1 a 5. 1 significa extrema pobreza no uso das práticas enquanto 5 fala com excelência. 

1. O quão bem o código utiliza os recursos disponíveis nas tecnologias da stack escolhida em cima daquela tecnologia
    1. **Nota**: 1
    1. **Motivo**: O código repetidamente não demonstra uso dos recursos oferecidos por padrão na linguagem. Um exmeplo é a falta de uso do Optional. Também não demonstra o bom uso de recursos providos pelo framework, como a falta de utilização do Controller Advice para centralizar tratamentos comuns a todos os controllers. Pode-se falar também da falta das asserções mais específicas do JUnit para verificar o estado interno dos objetos. 
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
7. Quando olhamos para as métricas que servem de proxy para a qualidade do código como LOC, Coesão, Acoplamento entre outras, qual o nível de saúde do código
    1. **Nota**: 1
    1. **Motivo**: Coesão baixa, acoplamento muito alto, número de linhas de código só na crescente 
8. Considerando diversas combinações de itens derivadas do CDD, o quão fácil está de entender?
    1. **Nota**: 1
    1. **Motivo**: O CDD é uma teoria de design que analisa, dado um determinado contexto, o quão fácil é de entender um código. Selecionamos algumas combinações de itens e em todos o código se mostra de dificil de entendimento.
9. Qual o nível de preocupação que o código dá sobre o mínimo de preocupação com performance,escalabilidade e resiliência
    1. **Nota**: 1
    1. **Motivo**: O que mais penaliza a performance aqui, como é de praxe, é a execuçao de operações de I/O. Por exemplo, existem chamadas remotas que são feitas dentro de loop, o que demonstra uma falta de cuidado na granularidade do serviço impactando diretamente a performance. Como já falado antes, o código não demonstra preocupação em revelar bugs e isso também é percebido nas chamadas remotas. Não existe nenhuma expectativa que uma chamada HTTP retorne um status de erro ou que demore mais do que o normal para executar. 

10. Olhando para documentação dentro do próprio código + repo em geral, como está a qualidade? 
    1. **Nota**: 2
    1. **Motivo**: Apesar da documentação geral ser pobre, existe alguns comentários inline que tentam explicar pedaços do código. Acreditamos que documentação, quando bem utilizada, acelera a produtividade e onboarding de novas pessoas no time.  
11. Pensando em LGPD e privacidade no geral, o código demonstra preocupação com isso?
12. E pensando em segurança, o que o código fala para a gente?