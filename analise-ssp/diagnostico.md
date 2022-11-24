## Criterios

1. O quão bem o código utiliza as apis disponíveis na linguagem padrão
2. O quão bem o código utiliza os recursos disponíveis nas tecnologias da stack escolhida em cima daquela tecnologia
3. Quais são as heurísticas demonstradas pelo código para a construção de novas abstrações(independente do motivo da abstração aqui)
4. O quão revelador de bugs aquele código tenta ser
5. Os testes automatizados de fato utilizam as melhores técnicas de modo a potencializar a revelação de bugs e, consequentemente, aumentar o nível de confiabilidade do código?
6. Como estão as práticas de refatoração no código? Ela é feita em algum momento? Nunca é feita?
7. Quando olhamos para as métricas que servem de proxy para a qualidade do código como LOC, Coesão, Acoplamento entre outras, o que elas nos dizem?
8. Considerando diversas combinações de métricas derivadas do CDD, o quão fácil está de entender?
9. Quais são os sinais que o código dá sobre o mínimo de preocupação com performance e escalabilidade(talvez nem seja necessário dado o contexto do software, como é o caso da Handora)
10. Olhando para documentação dentro do próprio código + repo em geral, como está a qualidade?
11. Pensando em LGPD e privacidade no geral, o código demonstra preocupação com isso?
12. E pensando em segurança, o que o código fala para a gente?

## Possíveis evidências

```
		SubjectAndBody message = service.createOutput(planOutputDataTO);
		if(message != null)
			return message.getBody();

		return null;
```

Não utiliza aqui o recurso de Optional.

```
PlanOutputTO planOutputDataTO = new PlanOutputTO();
```

Não utiliza o construtor com argumentos numa situação de obrigatoriedade

```
	private PlanTO stripAdvisorNotes(PlanTO planTO) {
		planTO.setContactNotes(null);
		for ( PlanCourseTO courseTO : planTO.getPlanCourses() ) {
			courseTO.setContactNotes(null);
		}
		for ( TermNoteTO termNoteTO : planTO.getTermNotes() ) {
			termNoteTO.setContactNotes(null);
		}
		return planTO;
	}
```

Lógica sobre um objeto completamente fora da classe em si. 

```
AbstractBaseController
```

Caso clássico de herança que não fala com a semântica de uso do recurso. Essa classe poderia ser substituida por um Controller Advice
do Spring MVC.

```
				final String result = restTemplate.postForObject(
						"Something goes here"
								+ "/createEarlyAlert", params, String.class);

				if (Boolean.parseBoolean(result.trim())) {
```

Mal uso de padrão de integração via HTTP. Poderia ter utilizado o status para representar o sucesso da requisição. Em vez disso, utilizou um retorno booleano para indicar o sucesso. 

```
Linha 160 - EarlyAlertManager

selfHelpGuideResponse.setEarlyAlertSent(true);
```

Altera o estado de um objeto que não foi retornado por outro lugar. Isso dificulta o tracking do estado do objeto e pode facilitar alteração de estado duplicado. 

```
Linha 96 - EarlyAlertManager 

				final String result = restTemplate.postForObject(
						"Something goes here"
								+ "/createEarlyAlert", params, String.class);
```

Aqui tem uma chamada remota dentro de um loop. Tende a penalizar a performance da execução. Outro ponto interessante é que não realiza nenhum tratamento de erro relativo a eventuais problemas na chamada remota, o que diminui a resiliência da aplicação. 