## Criterios

1. O quão bem o código utiliza os recursos disponíveis nas tecnologias da stack escolhida em cima daquela tecnologia
1. Nível de refinamento das heurísticas para tomada de decisão sobre detalhes do design do código
1. O quão revelador de bugs aquele código é.
1. Os testes automatizados de fato utilizam as melhores técnicas de modo a potencializar a revelação de bugs e, consequentemente, aumentar o nível de confiabilidade do código.
1. O código demonstra que práticas de refatoração são executadas regularmente. 
1. Nível de saúde quando olhamos para as métricas que servem de proxy para a qualidade do código como LOC, Coesão, Acoplamento entre outras.
1. Nível de facilidade de entendimento
1. Nível de preocupação do código com performance,escalabilidade e resiliência
1. Qualidade da documentação
1. O código demonstra a preocupação necessária com privacidade de dados.
1. Nível de aplicação de práticas de segurança como cidadã de primeiro nível(Security by design)

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

```
StudentIntakeFormManager

Aqui temos um acoplamento lá em cima com outras classes do próprio sistema. Isso fala sobre divisão de responsabilidades e controle de complexidade com o passar do tempo do projeto. Mesmo vendo a classe crescendo, pq ela continuou crescendo desse jeito? E é uma classe muito conectada com uma classe com uma tela, o que gera outra preocupação? Tela complexa vai gerar um backend complexo sempre? 

```

```
StudentIntakeFormManager

Aqui temos um acoplamento lá em cima com outras classes do próprio sistema. Isso fala sobre divisão de responsabilidades e controle de complexidade com o passar do tempo do projeto. Mesmo vendo a classe crescendo, pq ela continuou crescendo desse jeito? E é uma classe muito conectada com uma classe com uma tela, o que gera outra preocupação? Tela complexa vai gerar um backend complexo sempre? 

```

```
StudentIntakeFormManager

		final FormSectionTO confidentialitySection = buildConfidentialitySection();
		if (null != confidentialitySection) {
			formSections.add(confidentialitySection);
		}

```

Mais um exemplo de mal uso do recurso padrão da linguagem. Pq não usou optional?

```
StudentIntakeFormManager

    boolean completed = student.getStudentIntakeCompleteDate()== null ? false : true;

```

Mais um caso que a lógica poderia ter ficado dentro do método. O problema da falta de encapsulamento é que isso deixa o teste mais 
trabalhoso, aumenta as chances de duplicação de código etc. 

```
StudentIntakeFormManager

        student.setDemographics(null);
        student.setEducationGoal(null);
        student.setEducationPlan(null);
        student.getChallenges().clear();
        student.getFundingSources().clear();
        student.getEducationLevels().clear();         	

```

Aqui é mais um caso que fala sobre a falta do bom uso da orientação a objetos. Parece que a única maneira que o código encontra de alterar o estado dos objetos é utilizando os métodos padrões de acesso. Isso prejudica reuso de qualquer lógica, testes etc. 

```
StudentIntakeFormManager

		Person student = securityService.currentUser().getPerson();

		// now refresh Person from Hibernate so lazy-loading works in case the
		// person instance was loaded in a previous request
		student = personService.get(student.getId());
		
		boolean completed = student.getStudentIntakeCompleteDate()== null ? false : true;
			
		formTO.setCompleted(completed);
        if(!completed)
        {
        //blow away old data to be sure we have a tabula rasa
        student.setDemographics(null);
        student.setEducationGoal(null);
        student.setEducationPlan(null);
        student.getChallenges().clear();
        student.getFundingSources().clear();
        student.getEducationLevels().clear()

```

O código acima altera a referência para um objeto que foi criado em outro local da aplicação. Este tipo de prática complica demais a rastreabilidade da alteração no objeto em um fluxo de negócio. E se algum código, em outro lugar, no mesmo fluxo, também alterar?

```
StudentIntakeFormManagerTest

		final FormTO form = formManager.create();

		assertNotNull("Form should not have been null.", form);

		final FormSectionTO section = form
				.getFormSectionById(StudentIntakeFormManager.SECTION_CONFIDENTIALITY_ID);

		assertNotNull("Confidentiality section should not have been null.",
				section);

		assertEquals("Confidentiality section id does not match.",
				StudentIntakeFormManager.SECTION_CONFIDENTIALITY_ID,
				section.getId());
```

A ideia de um teste automatizado é que ele revele problemas o mais cedo possível. O teste acima não segue essa ideia. As asserções não verificam completamente o estado interno do objeto o que diminui em muito a chance de revelar algum bug. Como vai saber se o objeto está 
montado corretamente em função do retorno do método ```create```?

```

StudentIntakeFormManagerTest

		// Assert
		final Set<PersonConfidentialityDisclosureAgreement> agreements = person
				.getConfidentialityDisclosureAgreements();
		assertNotNull("Person agreements should not have been null.",
				agreements);
		assertFalse("Person should have some accepted agreements.",
				agreements.isEmpty());
```

Aqui a gente tem a mesma característica. Quantos agreements deveriam ser retornados? Este teste indica que pode ser 1 ou mais... Dado que existe um completo controle sobre o fluxo, o número esperado deveria ser verificado. 

```

Sobre cobertura

```

Por conta da complexidade do código, percebe-se uma tendência a realizar os testes sempre integrados. Por um lado deixa mais perto da execução real, por outro deixa tudo mais lento por conta da utilização recorrente do banco de dados, setup de dados, limpeza etc. 
Outro detalhe é que por exemplo o teste relativo a classe ```StudentIntakeFormManager``` não verifica o funcionamento do método ```populate```, que é o método, olhando pelas métricas, mais complexo que se tem na classe. Isso também indica a pobreza de cobertura de qualidade. 

```

Sobre testabilidade

```

O código não apresenta nenhuma preocupação em facilitar os testes. Nem do ponto de vista do código de produção e também nem do ponto de vista de infraestrutura de testes em si. 



