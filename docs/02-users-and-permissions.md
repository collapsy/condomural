# CondoMural — Etapa 2: Usuários e Permissões

**Status:** concluída e aprovada  
**Data:** 23/09/2026  
**Produto:** CondoMural  
**Etapa anterior:** Etapa 1 — Definição do Produto

## 1. Resumo executivo

A Etapa 2 define o modelo conceitual de usuários, vínculos, papéis, autorização, bootstrap administrativo, auditoria e isolamento entre tenants.

O modelo aprovado separa três conceitos:

1. **identidade da pessoa**;
2. **vínculo da pessoa com um condomínio (tenant)**;
3. **um ou mais vínculos dessa pessoa com unidades daquele condomínio**.

Uma identidade autenticada não recebe acesso privado automaticamente. O acesso depende do tenant correspondente, do estado dos vínculos, das permissões atribuídas naquele tenant e do público do recurso acessado.

Para o MVP existirão dois papéis administrativos:

- **Administrador do Condomínio**;
- **Gestor de Conteúdo**.

O Administrador do Condomínio também pode exercer funções editoriais. O Gestor de Conteúdo existe para permitir delegação editorial sem conceder poderes sobre moradores, vínculos ou administradores.

Não haverá `Tenant Owner` no MVP. Administradores do Condomínio serão autoridades equivalentes dentro do tenant.

## 2. Atores conceituais

### Visitante público

Pessoa sem autenticação.

Pode acessar somente conteúdo classificado como público no contexto de um tenant específico.

### Usuário autenticado

Pessoa cuja identidade foi autenticada, mas que pode não possuir vínculo aprovado com nenhum condomínio.

Pode administrar sua própria conta, acompanhar solicitações e solicitar vínculo, mas não recebe acesso privado por estar autenticada.

### Condômino/morador verificado

Usuário com vínculo ativo e aprovado com determinado condomínio.

Pode acessar conteúdo privado destinado ao público do qual participa, sempre dentro daquele tenant.

### Gestor de Conteúdo

Usuário com responsabilidade editorial em determinado tenant.

Pode criar, editar, publicar, arquivar e administrar conteúdo oficial conforme as permissões editoriais.

### Administrador do Condomínio

Autoridade administrativa do tenant.

Pode administrar usuários, vínculos, papéis administrativos, solicitações, conteúdo e demais capacidades administrativas previstas para o tenant.

O Administrador também possui as capacidades editoriais necessárias para administrar conteúdo oficial.

### Responsável técnico

Responsável pelo produto, código, infraestrutura, deploy, configuração técnica e diagnóstico.

Não é automaticamente autoridade administrativa ou editorial de um condomínio.

### Suporte excepcional

Situação temporária em que o responsável técnico recebe acesso adicional para diagnosticar ou corrigir um problema específico.

Esse acesso deve possuir escopo, motivo e auditoria.

## 3. Identidade, tenant e unidades

A identidade pertence à pessoa, não a um condomínio ou unidade.

Uma mesma identidade poderá possuir vínculos independentes com diferentes tenants.

Exemplo:

```text
Pessoa/Identidade
├─ Vínculo com Condomínio A
│  ├─ Vínculo com Unidade 101
│  └─ Vínculo com Unidade 10
└─ Vínculo com Condomínio B
   └─ Vínculo com Unidade 32
```

Portanto:

- uma pessoa pode possuir vínculo com mais de um condomínio;
- uma pessoa pode possuir vínculo com mais de uma unidade do mesmo condomínio;
- cada vínculo deve possuir ciclo de vida próprio;
- papéis administrativos pertencem ao contexto de um tenant, não globalmente à identidade;
- o MVP não precisa expor todas essas combinações imediatamente, mas o modelo não deve bloqueá-las.

Uma pessoa pode, por exemplo, ser Administrador no Condomínio A e apenas morador no Condomínio B.

## 4. Vínculo com condomínio

O vínculo com condomínio representa que uma pessoa possui uma relação validada e reconhecida com determinado tenant.

Estados conceituais:

- **ativo** — vínculo válido e capaz de conceder acesso;
- **suspenso** — vínculo existente, com acesso temporariamente revogado;
- **encerrado** — relação com o condomínio terminou;
- **invalidado** — vínculo foi criado incorretamente e posteriormente corrigido.

Autenticação não cria vínculo.

Informar uma unidade também não cria vínculo.

O vínculo somente é considerado válido depois da validação administrativa correspondente.

## 5. Vínculo com unidade

Um vínculo com unidade existe dentro do contexto de um vínculo com o condomínio.

Uma pessoa pode possuir mais de um vínculo de unidade ativo dentro do mesmo tenant quando houver situação legítima.

A mesma unidade também pode possuir vínculos com várias pessoas.

Exemplos legítimos incluem:

- mais de um morador da mesma residência;
- responsáveis diferentes vinculados à mesma unidade;
- situações futuras envolvendo proprietário e residente.

Portanto, **unidade não é uma chave exclusiva de pessoa**.

### Criação

O vínculo pode surgir por:

1. solicitação iniciada pelo próprio usuário, seguida de aprovação administrativa;
2. pré-cadastro iniciado pela administração, seguido de convite e ativação pelo usuário.

### Encerramento

Quando um vínculo deixa de ser válido, seu acesso deve ser revogado sem apagar o histórico relevante.

### Troca de unidade

Uma troca não sobrescreve o vínculo anterior.

Fluxo conceitual:

```text
Vínculo Unidade A ativo
→ Vínculo Unidade A encerrado
→ Vínculo Unidade B criado/ativado
```

Isso preserva a linha do tempo e evita que o histórico indique incorretamente que a pessoa sempre pertenceu à unidade atual.

## 6. Prevenção de duplicidades

A prevenção de duplicidade não pode impedir múltiplas pessoas legítimas na mesma unidade.

### Deve ser evitado

Para o MVP, uma mesma identidade não deve possuir simultaneamente solicitações ou vínculos equivalentes duplicados para o mesmo:

```text
identidade + tenant + unidade
```

### Deve ser permitido

Pessoas diferentes podem:

- solicitar vínculo com a mesma unidade;
- possuir vínculo ativo com a mesma unidade.

Uma nova solicitação para uma unidade que já possui moradores vinculados **não deve ser rejeitada automaticamente**.

A existência de outros vínculos ou solicitações para a unidade pode ser apresentada à administração como contexto de validação.

Esse contexto é administrativo e não deve expor dados pessoais de outros moradores ao solicitante.

## 7. Papéis administrativos do MVP

### 7.1 Administrador do Condomínio

Responsável pela governança operacional do tenant.

Pode:

- aprovar e rejeitar solicitações;
- administrar vínculos;
- suspender ou encerrar vínculos;
- iniciar pré-cadastros;
- criar convites;
- criar administradores;
- atribuir ou remover papéis administrativos;
- desativar administradores;
- consultar auditoria permitida;
- criar, editar, publicar e arquivar conteúdo;
- administrar configurações funcionais permitidas ao tenant.

Não pode:

- obter autoridade automática em outro tenant;
- alterar infraestrutura técnica apenas por ser administrador;
- apagar registros de auditoria para remover rastreabilidade;
- conceder a si próprio autoridade global sobre o produto.

### 7.2 Gestor de Conteúdo

Responsável exclusivamente pela operação editorial.

Pode:

- criar conteúdo;
- editar conteúdo;
- publicar;
- arquivar;
- administrar mídia;
- definir assunto, prioridade e público conforme as regras permitidas.

Não pode:

- aprovar moradores;
- administrar vínculos;
- criar administradores;
- alterar papéis administrativos;
- bloquear usuários;
- acessar dados pessoais de moradores sem finalidade editorial legítima.

### 7.3 Relação entre os papéis

Administrador do Condomínio é um papel mais abrangente e **também pode administrar conteúdo**.

Gestor de Conteúdo permite que uma pessoa trabalhe somente com conteúdo sem receber os poderes sensíveis de um Administrador.

### 7.4 Gestor de Moradores

Não será criado no MVP.

Pode ser considerado posteriormente caso o volume de operação justifique delegar gestão de moradores sem conceder poderes completos de administrador.

## 8. Tenant Owner

O MVP não terá `Tenant Owner`.

Administradores do Condomínio possuem autoridade equivalente dentro do tenant.

Regra obrigatória:

> nenhuma operação administrativa normal pode deixar o tenant sem pelo menos um Administrador do Condomínio ativo.

Operacionalmente, manter dois ou mais administradores é desejável, principalmente para reduzir risco de perda de acesso, mas não haverá hierarquia de owner no MVP.

## 9. Matriz funcional de permissões

| Capacidade | Público | Autenticado sem vínculo | Morador | Gestor de Conteúdo | Administrador | Técnico normal |
|---|---:|---:|---:|---:|---:|---:|
| Ver conteúdo público do tenant | Sim | Sim | Sim | Sim | Sim | Sim |
| Administrar própria conta | Não | Sim | Sim | Sim | Sim | Sim |
| Solicitar vínculo | Não | Sim | Sim* | Sim* | Sim* | Sim* |
| Ver conteúdo privado autorizado | Não | Não | Sim | Sim | Sim | Não |
| Criar/editar/publicar conteúdo | Não | Não | Não | Sim | Sim | Não |
| Alterar público do conteúdo | Não | Não | Não | Sim | Sim | Não |
| Aprovar moradores | Não | Não | Não | Não | Sim | Não |
| Administrar vínculos | Não | Não | Não | Não | Sim | Não |
| Criar administradores | Não | Não | Não | Não | Sim | Não |
| Alterar papéis | Não | Não | Não | Não | Sim | Não |
| Consultar auditoria do tenant | Não | Não | Não | Não | Sim | Não |
| Administrar infraestrutura | Não | Não | Não | Não | Não | Sim |
| Acesso excepcional ao tenant | Não | Não | Não | Não | Não | Somente autorizado |

`*` Uma pessoa já vinculada pode solicitar vínculo adicional quando o produto suportar esse fluxo.

Todas as capacidades privadas e administrativas permanecem limitadas ao tenant correspondente.

## 10. Cadastro solicitado pelo morador

Fluxo:

```text
Pessoa cria/acessa identidade
→ seleciona tenant
→ fornece os dados mínimos necessários
→ informa uma ou mais unidades aplicáveis ao fluxo
→ envia solicitação
→ administração analisa
→ aprova, solicita correção ou rejeita
→ vínculo é criado somente depois da aprovação
```

### Estados da solicitação

- **pendente**;
- **correção necessária**;
- **aprovada**;
- **rejeitada**;
- **cancelada**.

### Análise

Somente Administradores do Condomínio podem analisar a solicitação no MVP.

### Privacidade

Os dados da solicitação podem ser visualizados pelo solicitante e pelos administradores autorizados daquele tenant.

Gestores de Conteúdo não precisam desses dados.

### Rejeição

A rejeição deve possuir justificativa administrativa suficiente para rastreabilidade.

Uma rejeição não bloqueia permanentemente a identidade.

Uma nova solicitação poderá ser realizada posteriormente.

## 11. Cadastro iniciado pela administração

A alternativa aprovada é:

> **pré-cadastro com convite para ativação.**

Fluxo:

```text
Administrador seleciona tenant/unidade
→ informa os dados mínimos disponíveis
→ cria pré-vínculo aprovado
→ convite é enviado
→ pessoa confirma/ativa sua identidade
→ aceita o vínculo
→ vínculo torna-se ativo
```

A administração não cria credenciais em nome da pessoa.

A decisão administrativa que originou o convite já representa a aprovação daquele vínculo, não sendo necessária uma segunda aprovação após uma ativação válida.

Estados conceituais do convite:

- pendente;
- aceito;
- expirado;
- revogado.

## 12. Estados relevantes

### Conta

- ativa;
- bloqueada.

### Solicitação

- pendente;
- correção necessária;
- aprovada;
- rejeitada;
- cancelada.

### Vínculo com tenant

- ativo;
- suspenso;
- encerrado;
- invalidado.

### Vínculo com unidade

- ativo;
- encerrado;
- invalidado.

### Convite

- pendente;
- aceito;
- expirado;
- revogado.

## 13. Desativação, saída e correção

### Saída do condomínio

- encerrar vínculos com unidades;
- encerrar vínculo com tenant se não houver outro vínculo válido;
- revogar permissões derivadas;
- preservar histórico.

### Suspensão temporária

Utilizar suspensão do vínculo quando a perda de autorização não for definitiva.

### Conta comprometida

Bloquear a conta independentemente dos vínculos existentes.

### Administrador deixa a gestão

Remover seu papel administrativo.

Caso continue morador, seu vínculo residencial pode permanecer.

### Vínculo incorreto

Invalidar o vínculo incorreto e registrar o vínculo correto.

Não apagar silenciosamente a ocorrência.

## 14. Gestão de administradores

Somente Administradores do Condomínio podem:

- criar outro Administrador;
- atribuir Gestor de Conteúdo;
- remover papéis administrativos;
- desativar administradores.

Proteções obrigatórias:

- não remover o último Administrador do Condomínio;
- não rebaixar o último Administrador do Condomínio;
- não desativar a própria última autoridade administrativa;
- nunca administrar papéis pertencentes a outro tenant.

A remoção da própria função é permitida quando outro Administrador do Condomínio permanecer ativo.

## 15. Bootstrap dos dois primeiros administradores

Fluxo:

```text
Tenant sem administradores
→ responsável técnico inicia bootstrap excepcional
→ primeiro Administrador é ativado
→ segundo Administrador é ativado
→ administração assume a gestão
→ bootstrap é encerrado
→ poderes administrativos excepcionais do responsável técnico terminam
```

Os dois primeiros administradores possuem o papel completo de Administrador do Condomínio.

O mecanismo deve:

- ser explícito;
- ser tenant-scoped;
- ser auditável;
- não depender de alteração manual de banco como operação normal;
- não ser reutilizável como fluxo administrativo cotidiano;
- não criar backdoor permanente;
- permitir transferência futura da responsabilidade técnica;
- separar poder técnico de autoridade administrativa.

## 16. Ciclo do responsável técnico

### Desenvolvimento

Pode existir capacidade ampla em ambientes de desenvolvimento e validação.

### Implantação

O responsável técnico possui os poderes técnicos necessários para preparar a aplicação e o tenant.

### Onboarding

Recebe capacidade excepcional para executar o bootstrap dos dois primeiros administradores.

### Operação normal

Mantém responsabilidade por:

- código;
- infraestrutura;
- deploy;
- configuração técnica;
- monitoramento;
- diagnóstico.

Não mantém automaticamente:

- função de Administrador;
- acesso a conteúdo privado;
- capacidade editorial;
- acesso a dados pessoais de moradores.

### Suporte excepcional

Quando houver necessidade real:

```text
incidente/solicitação
→ motivo definido
→ tenant e escopo definidos
→ acesso excepcional concedido
→ intervenção realizada
→ acesso encerrado
→ evento auditado
```

## 17. Conteúdo público, autenticado e autorizado

### Regra central de tenant

Todo conteúdo pertence a um tenant.

Não existe, no modelo atual, conteúdo público global compartilhado por vários tenants.

Quando um conteúdo é classificado como **público**, isso significa:

> conteúdo público **daquele tenant**, acessível sem autenticação dentro da experiência pública correspondente a esse condomínio.

Exemplo:

- uma orientação pública do Palermo pertence ao Palermo;
- outro condomínio não herda, compartilha ou administra esse conteúdo automaticamente.

### Público

Qualquer visitante pode visualizar o conteúdo público daquele tenant.

### Autenticado

Usuário possui identidade autenticada, mas isso não concede acesso privado.

### Membro verificado

Possui vínculo ativo com o tenant e pode acessar conteúdo destinado ao seu público autorizado.

### Público segmentado dentro do tenant

Além do vínculo ativo, o usuário precisa satisfazer a segmentação do recurso, quando houver.

Exemplos futuros:

- unidade;
- estrutura;
- grupo autorizado.

### Administração

Exige papel administrativo válido naquele tenant.

## 18. Modelo conceitual de autorização

A autorização utilizará conceitualmente:

> **RBAC tenant-scoped + regras contextuais.**

Uma decisão pode considerar:

- identidade;
- estado da conta;
- tenant do recurso;
- vínculo com o tenant;
- papel/permissão naquele tenant;
- vínculo com uma ou mais unidades;
- público configurado no recurso;
- contexto da operação.

RBAC puro não é suficiente porque um papel não possui significado seguro sem o tenant correspondente.

Exemplo:

`Administrador` do Condomínio A não recebe qualquer permissão administrativa no Condomínio B.

## 19. Isolamento multi-tenant

Princípio obrigatório:

> possuir acesso em um condomínio não concede qualquer acesso automático a outro.

Isso se aplica a:

- moradores;
- administradores;
- gestores de conteúdo;
- auditoria;
- conteúdo público;
- vínculos;
- solicitações;
- unidades;
- suporte técnico excepcional.

Identificadores válidos de outro tenant não devem conceder acesso apenas porque foram informados em URL, parâmetro ou requisição.

O contexto do tenant precisa participar de toda autorização privada ou administrativa.

## 20. Auditoria

Devem ser auditáveis, no mínimo:

- bootstrap iniciado e concluído;
- criação de administrador;
- alteração de papel;
- desativação ou remoção de administrador;
- aprovação de solicitação;
- rejeição de solicitação;
- criação de vínculo;
- suspensão ou encerramento de vínculo;
- troca de unidade;
- bloqueio de conta;
- uso de acesso excepcional técnico;
- alterações relevantes de autorização;
- alteração relevante do público de conteúdo publicado.

Cada evento deve permitir identificar conceitualmente:

- quem executou;
- qual ação ocorreu;
- em qual tenant;
- sobre qual entidade;
- quando;
- estado anterior e posterior quando relevante.

### Escopo de visualização

A auditoria também é tenant-scoped.

Um usuário com acesso administrativo a apenas um tenant só pode visualizar os registros de auditoria daquele tenant.

Possuir papel de Administrador em um condomínio não concede visibilidade sobre a auditoria de outro.

Caso uma pessoa administre vários tenants, cada acesso continua limitado aos tenants para os quais possui autorização correspondente.

O responsável técnico também não recebe uma visão administrativa irrestrita de auditoria de todos os tenants apenas por manter a aplicação. Qualquer acesso excepcional deve seguir as regras de suporte e auditoria.

## 21. Principais riscos de segurança

- escalada de privilégio;
- acesso cruzado entre tenants;
- alteração de identificador/URL para acessar recurso de outro tenant;
- revogação incompleta de acesso;
- troca de unidade mantendo permissões antigas;
- Gestor de Conteúdo executando operação administrativa;
- bootstrap reutilizável indevidamente;
- backdoor técnica permanente;
- tenant ficar sem administrador;
- solicitação fraudulenta de vínculo;
- duplicidade da mesma identidade para o mesmo vínculo;
- segmentação incorreta de conteúdo;
- exposição indevida de auditoria de outro tenant.

## 22. Cenários críticos para testes futuros

Deverão ser priorizados posteriormente:

1. usuário autenticado sem vínculo não acessa conteúdo privado;
2. morador não acessa tenant diferente;
3. Administrador não administra tenant diferente;
4. Gestor de Conteúdo não administra usuários ou vínculos;
5. Administrador consegue exercer funções editoriais;
6. pessoa pode possuir mais de uma unidade legítima;
7. várias pessoas podem possuir vínculo com a mesma unidade;
8. mesma identidade não cria vínculo equivalente duplicado;
9. solicitações de pessoas diferentes para a mesma unidade não são rejeitadas automaticamente;
10. vínculo encerrado perde autorização;
11. troca de unidade remove autorização da unidade anterior;
12. último Administrador não pode ser removido;
13. bootstrap não pode ser repetido indevidamente;
14. responsável técnico perde poderes administrativos excepcionais após onboarding;
15. suporte excepcional fica limitado ao tenant e escopo autorizados;
16. solicitação rejeitada não cria vínculo;
17. solicitação pendente não concede acesso;
18. convite expirado ou revogado não ativa vínculo;
19. alteração de identificador não contorna isolamento de tenant;
20. conteúdo público de um tenant não se torna conteúdo global;
21. Administrador acessa somente auditoria dos tenants que administra;
22. bloqueio de conta prevalece sobre vínculos e papéis existentes.

## 23. Classificação de escopo

### ESSENCIAL PARA MVP

- identidade separada de vínculos;
- vínculo tenant-scoped;
- suporte conceitual a múltiplos vínculos de unidade;
- múltiplas pessoas na mesma unidade;
- solicitação de cadastro pelo usuário;
- aprovação/rejeição administrativa;
- pré-cadastro com convite;
- Administrador do Condomínio;
- Gestor de Conteúdo;
- Administrador também capaz de operar conteúdo;
- gestão de administradores;
- proteção contra tenant sem administrador;
- bootstrap dos dois primeiros administradores;
- encerramento de vínculos;
- troca de unidade com preservação histórica;
- conteúdo público tenant-scoped;
- conteúdo privado e segmentado;
- RBAC tenant-scoped + contexto;
- isolamento multi-tenant;
- auditoria tenant-scoped;
- suporte técnico excepcional auditado;
- definição dos cenários críticos de QA.

### IMPORTANTE, MAS PODE VIR DEPOIS

- Gestor de Moradores específico;
- diferenciação funcional entre proprietário e residente;
- permissões administrativas mais granulares;
- recuperação administrativa sofisticada;
- visualização avançada de auditoria;
- categorias adicionais de vínculo com unidade.

### IDEIA FUTURA

- Tenant Owner caso surja necessidade concreta;
- hierarquia administrativa complexa;
- papéis totalmente customizáveis por tenant;
- onboarding comercial automatizado de tenants;
- administração centralizada de múltiplos tenants;
- funcionalidades comerciais de SaaS.

## 24. Decisões aprovadas

### D2.1 — Separação entre identidade, tenant e unidade

Aprovado.

Identidade, vínculo com condomínio e vínculos com unidades são conceitos separados.

Uma pessoa pode possuir múltiplos vínculos de unidade dentro de um tenant.

### D2.2 — Papéis administrativos do MVP

Aprovado.

Existirão:

- Administrador do Condomínio;
- Gestor de Conteúdo.

Administrador também pode operar conteúdo.

### D2.3 — Sem Tenant Owner no MVP

Aprovado.

Administradores são autoridades equivalentes dentro do tenant.

### D2.4 — Dois administradores completos no bootstrap

Aprovado.

Os dois primeiros usuários administrativos recebem o papel completo de Administrador do Condomínio.

### D2.5 — Pré-cadastro com convite

Aprovado.

Cadastro iniciado pela administração utilizará pré-cadastro seguido de convite e ativação.

### D2.6 — Estados da solicitação

Aprovado.

- pendente;
- correção necessária;
- aprovada;
- rejeitada;
- cancelada.

### D2.7 — Troca de unidade preserva histórico

Aprovado.

O vínculo anterior é encerrado e um novo vínculo é criado.

### D2.8 — Responsável técnico sem acesso administrativo permanente

Aprovado.

Poderes excepcionais são encerrados após bootstrap; suporte posterior é explícito, limitado e auditado.

### D2.9 — RBAC tenant-scoped + contexto

Aprovado.

Nenhuma permissão privada ou administrativa é considerada fora do contexto do tenant correspondente.

### D2.10 — Tenant nunca pode ficar sem Administrador

Aprovado.

Operações que deixariam zero Administradores ativos devem ser impedidas.

## 25. Decisões complementares consolidadas

Também ficam estabelecidas nesta etapa:

1. a mesma pessoa pode possuir várias unidades no mesmo tenant;
2. várias pessoas podem possuir vínculo com a mesma unidade;
3. duplicidade se refere à repetição equivalente da mesma identidade, não à ocupação compartilhada da unidade;
4. Administrador do Condomínio também possui capacidade editorial;
5. todo conteúdo, inclusive público, pertence a exatamente um tenant no modelo atual;
6. não existe conteúdo público global compartilhado por N tenants;
7. registros de auditoria e sua visualização são tenant-scoped;
8. nenhuma função administrativa em um tenant concede autoridade automática em outro.

## 26. Itens deliberadamente deixados para etapas posteriores

Não foram definidos:

- provedor ou método técnico de autenticação;
- verificação técnica de celular ou e-mail;
- banco de dados;
- modelo físico de dados;
- IDs e chaves;
- implementação técnica do RBAC;
- middleware ou biblioteca de autorização;
- tecnologia de auditoria;
- política detalhada de retenção;
- arquitetura técnica de multi-tenancy;
- UX de seleção de tenant;
- interfaces administrativas;
- arquitetura da informação;
- wireframes;
- stack;
- infraestrutura;
- APIs;
- backlog;
- issues;
- implementação;
- estratégia detalhada de testes automatizados.

## 27. Encerramento da Etapa 2

A Etapa 2 está concluída.

Definição consolidada:

> Uma pessoa possui uma identidade independente. Essa identidade pode possuir vínculos com diferentes tenants e, dentro de cada tenant, um ou mais vínculos com unidades. Várias pessoas também podem compartilhar vínculo com uma mesma unidade. O acesso é determinado sempre pelo tenant, estado dos vínculos, papel/permissão e público do recurso.

Para administração:

> Administradores governam o tenant e também podem administrar conteúdo. Gestores de Conteúdo possuem somente responsabilidade editorial. O responsável técnico governa o produto e a infraestrutura, não o condomínio.

Para isolamento:

> conteúdo público, permissões administrativas e auditoria continuam pertencendo ao tenant correspondente; nada se torna global apenas por estar público ou por um usuário possuir acesso em outro condomínio.

A próxima etapa poderá tratar do próximo bloco do Product Discovery sem alterar estas decisões, salvo inconsistência concreta identificada posteriormente.
