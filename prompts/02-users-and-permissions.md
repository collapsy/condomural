# CondoMural — Etapa 2: Usuários e Permissões

Vamos continuar o Product Discovery do **CondoMural**.

O GitHub é a fonte canônica de verdade do projeto:

https://github.com/collapsy/condomural

Antes de iniciar qualquer análise:

1. consulte o estado atual do repositório;
2. leia integralmente o `README.md`;
3. leia integralmente `docs/01-product-definition.md`;
4. trate as decisões aprovadas na Etapa 1 como premissas canônicas;
5. não rediscuta decisões já fechadas sem identificar uma inconsistência concreta;
6. se houver conflito entre este prompt e a documentação aprovada, sinalize-o antes de prosseguir.

## Objetivo

Trabalhar **somente na Etapa 2 — Usuários e Permissões**.

O objetivo desta etapa é definir quem pode acessar o CondoMural, em qual contexto, com quais responsabilidades e com quais limites, mantendo segurança, simplicidade operacional, menor privilégio e preparação para múltiplos condomínios.

Não queremos ainda definir implementação técnica.

## Premissas já aprovadas que impactam esta etapa

Considere obrigatoriamente as decisões registradas na Etapa 1, especialmente:

- existe conteúdo público que não exige autenticação;
- conteúdo pessoal, restrito ou segmentado exige autorização adequada;
- informar uma unidade não concede automaticamente acesso privado;
- o morador pode solicitar seu próprio cadastro;
- a administração também pode iniciar o cadastro de um usuário;
- somente depois da validação apropriada o usuário é considerado vinculado ao condomínio;
- "Administrador" não precisa representar um único papel;
- o responsável técnico não deve automaticamente ser responsável editorial;
- durante o desenvolvimento, o responsável técnico pode operar como superusuário;
- na implantação inicial, o responsável técnico deve realizar o onboarding dos **dois primeiros administradores**;
- após esse onboarding, a administração deve possuir autonomia operacional;
- o mecanismo inicial de bootstrap deve ser seguro e auditável;
- o produto deve respeitar menor privilégio;
- ações administrativas relevantes devem possuir rastreabilidade;
- Palermo é o primeiro tenant, não a arquitetura do produto;
- um condomínio nunca deve conseguir acessar ou modificar dados de outro condomínio sem autorização explícita;
- devemos preparar o modelo para evolução sem construir antecipadamente um SaaS completo.

## Questões que esta etapa precisa resolver

### 1. Atores e identidades

Defina claramente os atores conceituais necessários.

Analise pelo menos:

- visitante público;
- usuário autenticado;
- condômino/morador verificado;
- administração do condomínio;
- responsável técnico.

Questione se outros atores são realmente necessários agora.

Evite criar personas apenas por completude.

### 2. Usuário x vínculo com condomínio

Precisamos separar conceitualmente:

**identidade da pessoa**

de

**vínculo dessa pessoa com um condomínio**.

Analise como isso deve funcionar conceitualmente considerando que, futuramente:

- uma mesma pessoa pode possuir vínculo com mais de uma unidade;
- uma pessoa pode ser proprietária sem residir;
- uma pessoa pode mudar de unidade;
- uma pessoa pode deixar o condomínio;
- uma pessoa pode possuir vínculo com mais de um condomínio.

Não precisamos implementar todas essas possibilidades no MVP, mas as decisões desta etapa não devem impedir sua evolução.

### 3. Vínculo com unidade

Defina conceitualmente:

- quando um usuário pode ser associado a uma unidade;
- quem pode criar esse vínculo;
- quem pode aprová-lo;
- como o vínculo pode ser encerrado;
- o que acontece quando há troca de morador;
- como preservar histórico sem manter acesso indevido.

Não transforme a simples declaração de uma unidade em prova de vínculo.

### 4. Papéis administrativos

Analise se o MVP realmente precisa de mais de um papel administrativo.

Considere possibilidades como, por exemplo:

- administrador principal;
- gestor de comunicação;
- gestor de moradores;
- administrador com poderes limitados.

Esses nomes são apenas exemplos.

Não os adote automaticamente.

Para cada papel sugerido, explique:

- responsabilidade;
- permissões necessárias;
- permissões que não deve possuir;
- por que precisa existir já no MVP.

Se dois papéis puderem ser simplificados sem comprometer segurança ou operação, prefira a solução mais simples.

### 5. Administração de administradores

Precisamos definir:

- quem pode criar novos administradores;
- quem pode alterar permissões;
- quem pode desativar administradores;
- se alguém pode remover a própria última autoridade administrativa;
- como evitar que o tenant fique sem administradores;
- como recuperar a administração em caso de perda de acesso;
- se deve existir um conceito de administrador principal/owner do tenant.

Avalie vantagens e riscos de existir um `Tenant Owner` ou papel equivalente.

Não adote esse conceito automaticamente.

### 6. Bootstrap dos dois primeiros administradores

Desenhe conceitualmente o fluxo completo:

Aplicação sem administradores  
→ responsável técnico inicia bootstrap  
→ primeiro administrador é criado/ativado  
→ segundo administrador é criado/ativado  
→ administração assume a gestão  
→ poderes excepcionais do responsável técnico são encerrados ou restringidos.

O mecanismo deve:

- ser seguro;
- ser auditável;
- não depender de alteração manual de banco como operação normal;
- não criar uma backdoor administrativa permanente;
- permitir transferência futura do responsável técnico;
- impedir que o responsável técnico permaneça como publicador oficial simplesmente por manter o sistema.

Diferencie claramente:

**poder técnico sobre a aplicação**

de

**poder administrativo dentro de um tenant**.

### 7. Superusuário durante o desenvolvimento

Precisamos conciliar duas necessidades:

1. durante desenvolvimento e validação, o responsável técnico precisa conseguir testar praticamente todo o sistema;
2. em operação real, esse nível de acesso não deve existir de forma permanente e invisível.

Proponha um ciclo conceitual seguro para:

**desenvolvimento → implantação → onboarding → operação normal → suporte excepcional**

Considere também como acessos excepcionais de suporte deveriam ser registrados.

Não escolha ainda a implementação técnica desse mecanismo.

### 8. Cadastro solicitado pelo morador

Desenhe conceitualmente:

1. pessoa inicia solicitação;
2. fornece dados necessários;
3. informa condomínio e unidade;
4. solicitação fica pendente;
5. administração analisa;
6. aprova ou rejeita;
7. vínculo é criado somente após aprovação;
8. usuário recebe acesso compatível com o vínculo aprovado.

Defina:

- estados necessários da solicitação;
- quem pode analisá-la;
- quem pode visualizar seus dados;
- possibilidade de correção;
- rejeição;
- nova solicitação após rejeição;
- prevenção de duplicidades.

Evite coletar dados sem finalidade clara.

### 9. Cadastro iniciado pela administração

Desenhe também o fluxo em que a administração inicia o cadastro.

Avalie se isso deve funcionar como:

- criação direta;
- convite;
- pré-cadastro seguido de ativação;
- ou outra alternativa.

Compare as alternativas e recomende a mais simples e segura.

Considere que a administração pode possuir apenas alguns dados iniciais do morador.

### 10. Ativação, desativação e saída

Defina conceitualmente o que ocorre quando:

- um morador sai do condomínio;
- muda de apartamento;
- perde autorização;
- um administrador deixa a gestão;
- uma conta precisa ser bloqueada;
- um vínculo foi cadastrado incorretamente.

Prefira desativação/revogação de acesso quando a exclusão destruir histórico ou auditoria relevante.

### 11. Conteúdo público x autenticado x autorizado

Não assuma:

> usuário autenticado = pode visualizar todo conteúdo privado do condomínio.

Precisamos diferenciar:

- público;
- autenticado;
- membro verificado de determinado condomínio;
- público segmentado dentro daquele condomínio;
- administração.

Defina as regras conceituais de acesso sem entrar ainda em arquitetura técnica.

### 12. RBAC e modelo de autorização

Avalie se **RBAC** é adequado ao produto nesta fase ou se precisamos de alguma combinação simples de:

- papel;
- permissão;
- tenant;
- vínculo;
- unidade/estrutura;
- contexto do recurso.

Não escolha frameworks ou bibliotecas.

Queremos apenas definir o modelo conceitual.

A autorização deve considerar sempre o tenant correspondente.

### 13. Multi-tenant

Analise explicitamente os riscos de autorização entre condomínios.

Por exemplo:

- usuário vinculado a dois condomínios;
- administrador de um condomínio tentando acessar outro;
- identificadores válidos pertencentes a outro tenant;
- conteúdo com segmentação incorreta;
- alteração de URL ou parâmetros;
- responsável técnico realizando suporte.

O princípio obrigatório é:

> possuir acesso em um condomínio não concede qualquer permissão automática em outro.

### 14. Auditoria

Defina quais ações precisam necessariamente ser auditáveis.

Considere pelo menos:

- criação de administrador;
- alteração de papel/permissão;
- remoção/desativação de administrador;
- aprovação ou rejeição de cadastro;
- alteração de vínculo com unidade;
- utilização de acesso excepcional do responsável técnico;
- bootstrap inicial;
- alterações relevantes de autorização.

Não precisamos ainda definir tecnologia de logs.

### 15. Menor privilégio

Aplique o princípio de **least privilege**.

Cada ator deve possuir somente os acessos necessários para sua função.

Evite:

- administradores com poder total por conveniência;
- responsável técnico com poderes editoriais permanentes;
- permissões globais quando deveriam pertencer ao tenant;
- acesso a dados pessoais sem finalidade.

## Classificação de escopo

Para cada capacidade identificada, classifique como:

**ESSENCIAL PARA MVP**

**IMPORTANTE, MAS PODE VIR DEPOIS**

**IDEIA FUTURA**

Não coloque algo no MVP apenas porque seria tecnicamente interessante.

## Qualidade e riscos

Como esta etapa trata de autorização, considere segurança e testes como parte da definição.

Identifique os cenários de maior risco que posteriormente precisarão de testes, incluindo quando aplicável:

- privilege escalation;
- acesso cruzado entre tenants;
- administrador sem permissão executando operação restrita;
- morador acessando conteúdo de outra unidade ou grupo;
- usuário desativado mantendo acesso;
- alteração de vínculo sem revogar permissões;
- bootstrap sendo executado novamente indevidamente;
- responsável técnico mantendo poder administrativo após onboarding;
- último administrador sendo removido;
- duplicidade ou fraude em solicitação de cadastro.

Não implemente testes ainda.

## Entregável esperado

Produza uma proposta objetiva contendo:

1. mapa de atores;
2. modelo conceitual de identidade;
3. modelo conceitual de vínculo com condomínio;
4. modelo conceitual de vínculo com unidade;
5. proposta de papéis administrativos;
6. matriz de permissões em nível funcional;
7. fluxo de solicitação de cadastro pelo morador;
8. fluxo de cadastro iniciado pela administração;
9. estados de cadastro/vínculo relevantes;
10. fluxo de ativação;
11. fluxo de rejeição;
12. fluxo de desativação e saída;
13. fluxo de troca de unidade;
14. fluxo de criação e gestão de administradores;
15. bootstrap dos dois primeiros administradores;
16. ciclo do superusuário técnico;
17. regras de acesso público, autenticado e autorizado;
18. proposta conceitual de autorização/RBAC;
19. requisitos de auditoria;
20. implicações multi-tenant;
21. riscos de segurança e autorização;
22. cenários críticos que futuramente precisarão de testes;
23. classificação MVP / depois / futuro;
24. decisões que precisam da minha validação;
25. itens deliberadamente deixados para etapas posteriores.

## Forma de trabalho

Atue como Product Manager, arquiteto de software com foco em segurança e especialista em QA.

Para decisões relevantes:

1. identifique o problema;
2. questione premissas quando houver motivo concreto;
3. apresente alternativas reais;
4. mostre vantagens e desvantagens relevantes;
5. recomende uma direção;
6. destaque claramente o que depende da minha validação;
7. consolide o que puder ser considerado decidido;
8. registre o que deve ficar para depois.

Prefira clareza e decisões objetivas a documentação extensa.

## Restrições desta etapa

NÃO:

- escreva código;
- escolha stack;
- escolha banco de dados;
- escolha provedor de autenticação;
- escolha biblioteca de RBAC;
- detalhe implementação técnica de multi-tenancy;
- avance para arquitetura da informação;
- crie wireframes;
- defina o MVP completo do produto;
- crie backlog;
- crie issues;
- inicie implementação;
- altere o repositório sem minha validação das decisões desta etapa.

Ao final, **pare na Etapa 2**.

Apresente as decisões que precisam da minha validação antes de qualquer consolidação no repositório ou avanço para a Etapa 3.
