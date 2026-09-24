# CondoMural — Etapa 3: Conteúdo, Taxonomia e Arquitetura da Informação

Vamos continuar o Product Discovery do **CondoMural**.

O GitHub é a fonte canônica de verdade do projeto:

https://github.com/collapsy/condomural

Antes de iniciar qualquer análise:

1. consulte o estado atual do repositório;
2. leia integralmente o `README.md`;
3. leia integralmente `docs/01-product-definition.md`;
4. leia integralmente `docs/02-users-and-permissions.md`;
5. trate as decisões aprovadas nas Etapas 1 e 2 como premissas canônicas;
6. não rediscuta decisões já fechadas sem identificar uma inconsistência concreta;
7. se houver conflito entre este prompt e a documentação aprovada, sinalize-o antes de prosseguir.

## Objetivo

Trabalhar **somente na Etapa 3 — Conteúdo, Taxonomia e Arquitetura da Informação**.

O objetivo desta etapa é definir:

- quais conceitos de conteúdo existem no CondoMural;
- como esses conteúdos são classificados;
- como devem ser organizados;
- como usuários encontram informações recentes e antigas;
- como conteúdos públicos, privados e segmentados convivem;
- como preservar histórico sem poluir a experiência atual;
- como organizar informações permanentes e temporais;
- como mídia e documentos se relacionam com o conteúdo;
- quais informações devem aparecer como seções, assuntos, filtros ou metadados;
- como evitar que a Central se transforme apenas em um feed cronológico;
- como manter essa estrutura configurável por tenant sem overengineering.

Não queremos ainda definir implementação técnica ou desenhar interfaces.

---

# Premissas canônicas das etapas anteriores

Considere obrigatoriamente as seguintes decisões.

## Produto

O CondoMural é uma plataforma web mobile-first destinada a centralizar, organizar e preservar comunicações e informações oficiais de condomínios.

A plataforma é a fonte persistente da informação oficial.

WhatsApp e outros canais podem continuar funcionando como meios complementares de distribuição.

O produto não é:

- rede social;
- fórum;
- chat de moradores;
- ERP condominial completo;
- sistema financeiro;
- plataforma de reservas;
- sistema de garagem;
- marketplace.

---

## Multi-tenant

Palermo é o primeiro tenant, não a arquitetura do produto.

Informações específicas de um condomínio devem pertencer ao tenant correspondente.

Não assumir globalmente:

- nome Central Palermo;
- três torres;
- termo "torre";
- quantidade fixa de unidades;
- mesmos assuntos;
- mesmos serviços;
- mesma identidade visual;
- mesmas regras;
- mesmas estruturas físicas.

Preferir configuração a hardcode quando fizer sentido.

Ao mesmo tempo:

> projetar para evoluir, implementar somente o necessário.

---

## Conteúdo público

Todo conteúdo pertence a exatamente um tenant no modelo atual.

Não existe conteúdo público global compartilhado automaticamente entre múltiplos tenants.

Quando um conteúdo é público, isso significa:

> conteúdo público daquele tenant, acessível sem autenticação dentro da experiência pública daquele condomínio.

---

## Autorização

Conteúdo privado ou segmentado depende sempre de autorização no tenant correspondente.

Usuário autenticado não significa automaticamente membro autorizado.

Uma pessoa pode possuir:

- vínculo com vários tenants;
- várias unidades dentro de um tenant.

Várias pessoas também podem estar vinculadas à mesma unidade.

---

## Administração editorial

Existem dois papéis administrativos no MVP:

### Administrador do Condomínio

Também possui capacidade editorial.

### Gestor de Conteúdo

Possui capacidades editoriais sem poderes administrativos sobre moradores ou administradores.

---

## Conceitos de conteúdo já aprovados

A Etapa 1 estabeleceu que:

### Comunicado

Conteúdo normalmente relacionado a acontecimento ou período.

Exemplos:

- manutenção;
- interrupção;
- evento;
- aviso operacional.

### Orientação permanente

Informação que continua relevante até ser atualizada.

Exemplos:

- descarte de lixo;
- mudanças;
- entregas;
- regras de convivência.

### Documento

Foi identificado como possível tipo de conteúdo, mas seu comportamento ainda precisa ser definido.

---

## Dimensões já separadas

Não confundir:

**tipo de conteúdo**

com

**assunto**

com

**prioridade**

com

**público**.

"Urgente" não é assunto.

---

## Assuntos

Assuntos:

- pertencem ao tenant;
- devem ser reutilizáveis;
- não devem ser criados livremente a cada publicação de forma que gerem duplicidades;
- inicialmente utilizarão o termo **Assunto** na interface;
- podem evoluir futuramente para um modelo mais sofisticado de tags se houver necessidade real.

Exemplos possíveis no Palermo:

- manutenção;
- elevadores;
- barulho;
- reciclagem;
- acesso;
- motos;
- lavanderia.

Esses exemplos não são categorias globais obrigatórias do produto.

---

## Estrutura física

O núcleo do produto não deve depender do termo "torre".

Um tenant poderá utilizar:

- torre;
- bloco;
- edifício;
- ala;
- setor;
- ou outra nomenclatura.

---

## Mídia

Conteúdo pode precisar suportar:

- texto;
- imagens;
- vídeos;
- anexos.

A experiência deve continuar adequada para dispositivos móveis.

---

## Histórico

O produto precisa preservar memória institucional.

Alterações relevantes não devem destruir histórico e rastreabilidade.

Ao mesmo tempo, conteúdo antigo não deve poluir a experiência principal do usuário.

---

# Questões que esta etapa precisa resolver

## 1. Modelo conceitual de conteúdo

Defina quais entidades conceituais de conteúdo realmente precisam existir.

Analise pelo menos:

- comunicado;
- orientação permanente;
- documento.

Avalie também se conceitos como:

- informação institucional;
- contato útil;
- serviço;
- link externo;
- regra;
- página informativa;

precisam realmente representar tipos independentes de conteúdo ou se podem ser atendidos por conceitos mais simples.

Evite criar um tipo de conteúdo para cada situação.

Para cada tipo proposto, explique:

- finalidade;
- diferenças em relação aos demais;
- características próprias;
- ciclo de vida;
- por que precisa existir;
- se é essencial para o MVP.

---

# 2. Comunicado x orientação permanente

A diferença já foi aprovada conceitualmente, mas precisa ser aprofundada.

Defina:

### Comunicado

- quando deve ser utilizado;
- relação com data;
- relação com período de validade;
- quando deixa de ser atual;
- como permanece disponível historicamente;
- como tratar alterações posteriores.

### Orientação permanente

- quando deve ser utilizada;
- se representa a versão atual de uma orientação;
- como alterações devem funcionar;
- como preservar versões anteriores;
- como evitar diversas orientações contraditórias sobre o mesmo assunto.

Analise especialmente a diferença entre:

**informação que aconteceu**

e

**informação que atualmente é válida**.

---

# 3. Documento

Defina conceitualmente o que significa "Documento".

Avalie alternativas:

### Documento como conteúdo próprio

Possui:

- título;
- descrição;
- assunto;
- público;
- arquivo;
- histórico.

### Documento como simples anexo

Existe apenas associado a outro conteúdo.

### Modelo híbrido

Documentos relevantes podem existir de forma independente, enquanto outros arquivos funcionam apenas como anexos.

Compare as alternativas e recomende a solução mais simples que preserve encontrabilidade e histórico.

Considere exemplos como:

- regulamento;
- manual;
- ata;
- comunicado formal em PDF;
- formulário;
- documento institucional.

Não transforme o CondoMural prematuramente em um GED completo.

---

# 4. Informações institucionais

A camada pública poderá apresentar informações institucionais do condomínio.

Defina conceitualmente como tratar informações como:

- nome do condomínio;
- endereço;
- apresentação;
- contatos oficiais;
- horários relevantes;
- informações gerais.

Avalie se devem ser:

- conteúdo publicável;
- configuração do tenant;
- páginas institucionais;
- ou combinação desses conceitos.

Evite armazenar como publicação algo que na realidade representa configuração estável do tenant.

---

# 5. Contatos úteis

Precisamos decidir como tratar contatos como:

- administração;
- portaria;
- manutenção;
- prestadores ou serviços relevantes;
- telefones úteis.

Analise se "Contato útil" deve ser:

- entidade própria;
- orientação permanente;
- informação institucional;
- ou outra composição simples.

Considere:

- facilidade de atualização;
- encontrabilidade;
- exposição pública ou privada;
- necessidade de tenant;
- minimização de dados pessoais.

---

# 6. Serviços

A Etapa 1 admite que determinados serviços possam ser apresentados publicamente.

Precisamos definir o que "serviço" significa nesta fase.

Exemplos podem envolver:

- lavanderia;
- recebimento de encomendas;
- mudança;
- descarte;
- acesso;
- serviços externos utilizados pelo condomínio.

Questione se realmente precisamos de uma entidade "Serviço" no MVP ou se essas informações podem ser organizadas como orientações permanentes.

Evite transformar cada assunto do condomínio em um módulo da aplicação.

---

# 7. Tipo x assunto x prioridade x público

Formalize claramente as quatro dimensões.

## Tipo

O que o conteúdo é.

## Assunto

Sobre o que ele trata.

## Prioridade

Quanto destaque ou atenção ele exige.

## Público

Quem pode visualizá-lo.

Mostre exemplos combinando as dimensões.

Exemplo conceitual:

```text
Tipo: Comunicado
Assunto: Manutenção
Prioridade: ?
Público: Moradores do tenant
```

ou:

```text
Tipo: Orientação permanente
Assunto: Mudanças
Prioridade: ?
Público: Público do tenant
```

Evite transformar uma dimensão na outra.

---

# 8. Prioridade

Os níveis definitivos de prioridade ainda não foram decididos.

Analise alternativas simples.

Por exemplo:

- normal / importante / urgente;
- padrão / importante / crítica;
- sem prioridade / destaque / urgente;
- outra proposta.

Avalie:

- clareza para moradores;
- risco de tudo virar urgente;
- comportamento editorial;
- acessibilidade;
- necessidade real de três ou mais níveis.

Recomende a menor quantidade necessária.

Defina também conceitualmente:

- o que cada prioridade significa;
- quando pode ser usada;
- se afeta apenas destaque ou também outras regras;
- se prioridade expira;
- se pode ser alterada depois da publicação.

Não defina ainda detalhes visuais.

---

# 9. Assuntos

Defina o modelo conceitual de assuntos.

Precisamos decidir:

- quem pode criar assunto;
- quem pode editar;
- quem pode arquivar;
- se assunto pode ser excluído;
- o que acontece com conteúdos antigos quando um assunto deixa de ser usado;
- como evitar duplicidades como "Elevador", "Elevadores" e "Problema no elevador";
- se um conteúdo terá inicialmente um único assunto ou múltiplos assuntos.

Considere simplicidade para o MVP.

Avalie explicitamente:

### Um assunto por conteúdo

versus

### múltiplos assuntos/tags.

Não adote múltiplas tags apenas por flexibilidade futura.

---

# 10. Ciclo de vida do conteúdo

Defina estados conceituais necessários.

Analise pelo menos:

- rascunho;
- publicado;
- arquivado.

Avalie se realmente precisamos no MVP de:

- agendado;
- expirado;
- removido;
- cancelado.

Para cada estado, explique:

- significado;
- quando entra;
- quando sai;
- quem pode executar a transição;
- impacto para o usuário final;
- impacto no histórico.

Não escolha tecnologia de workflow.

---

# 11. Validade temporal

Alguns conteúdos possuem validade temporal.

Exemplos:

- manutenção amanhã;
- interrupção de água entre 9h e 14h;
- evento sábado;
- aviso válido durante determinada semana.

Defina conceitualmente se conteúdos podem possuir:

- data de início;
- data de término;
- período de relevância;
- expiração.

Diferencie:

**deixar de estar em destaque**

de

**ser apagado**.

Um comunicado antigo pode continuar historicamente disponível sem continuar sendo tratado como atual.

---

# 12. Edição de conteúdo publicado

Defina o comportamento conceitual quando um conteúdo oficial já publicado precisa ser alterado.

Analise situações como:

- correção ortográfica;
- mudança pequena;
- alteração importante;
- mudança de horário;
- mudança de público;
- informação anteriormente errada.

Precisamos equilibrar:

- possibilidade de correção;
- confiança;
- histórico;
- rastreabilidade.

Defina quando uma alteração pode simplesmente atualizar o conteúdo e quando precisa ficar claramente registrada como revisão.

Não defina implementação técnica de versionamento.

---

# 13. Arquivamento

Defina:

- o que significa arquivar;
- diferença entre arquivar e excluir;
- se conteúdo arquivado continua pesquisável;
- se pode ser acessado por link direto;
- se aparece na navegação principal;
- quem pode arquivar;
- se conteúdo pode ser restaurado.

Priorize preservação histórica.

---

# 14. Exclusão

Avalie se conteúdos publicados devem poder ser excluídos permanentemente em operação normal.

Considere:

- erro de publicação;
- informação pessoal publicada por engano;
- conteúdo duplicado;
- obrigação de remoção;
- histórico institucional.

Recomende regras que evitem destruição desnecessária de histórico sem impedir remoção quando realmente necessária.

---

# 15. Página inicial / entrada do tenant

Sem criar wireframes, defina quais informações conceitualmente precisam receber maior destaque ao acessar a Central.

Considere:

- comunicados recentes;
- conteúdos importantes;
- orientações permanentes;
- busca;
- informações úteis;
- atalhos relevantes.

Evite transformar a página inicial em:

- mural infinito;
- painel excessivamente complexo;
- menu de dezenas de assuntos.

Defina princípios de priorização, não layout visual.

---

# 16. Navegação principal

Avalie quais áreas conceituais realmente merecem navegação principal.

Exemplos para análise, não decisões:

- Início;
- Comunicados;
- Orientações;
- Documentos;
- Informações úteis;
- Busca.

Questione se:

- assuntos devem virar itens de menu;
- serviços devem virar seções;
- cada tipo precisa de área própria;
- público e privado devem possuir navegações separadas.

Prefira uma arquitetura simples, compreensível e escalável.

Não crie wireframes.

---

# 17. Público x privado na arquitetura da informação

O usuário não deveria precisar compreender a arquitetura interna de autorização para usar a aplicação.

Defina conceitualmente como público e privado devem coexistir.

Considere:

- visitante sem login;
- usuário autenticado sem vínculo;
- morador;
- administrador;
- usuário com acesso a mais de um tenant.

Não devemos criar duas aplicações conceitualmente desconectadas se isso não for necessário.

Ao mesmo tempo, informações privadas nunca podem aparecer indevidamente em:

- busca;
- sugestões;
- listagens;
- contagens;
- metadados;
- previews.

---

# 18. Busca

Encontrar informação antiga é um problema central do produto.

Defina os requisitos conceituais de busca.

Analise quais atributos devem contribuir para descoberta, como:

- título;
- conteúdo textual;
- assunto;
- tipo;
- data;
- documento;
- palavras relacionadas.

Considere:

- resultados recentes e antigos;
- conteúdo arquivado;
- conteúdo permanente;
- filtros;
- ordenação;
- busca mobile.

Regra obrigatória:

> a busca nunca pode revelar existência, título, trecho, quantidade ou metadados de conteúdo que o usuário não esteja autorizado a visualizar.

Não escolha mecanismo ou tecnologia de busca.

---

# 19. Filtros e descoberta

Avalie quais filtros realmente agregam valor.

Possibilidades:

- tipo;
- assunto;
- período;
- prioridade.

Não crie filtros apenas porque os metadados existem.

Considere principalmente a experiência mobile.

Determine quais filtros parecem:

- essenciais;
- úteis posteriormente;
- desnecessários inicialmente.

---

# 20. Conteúdo recente x histórico

Precisamos evitar dois extremos:

1. esconder informação antiga demais;
2. transformar a experiência em um arquivo desorganizado.

Defina como separar conceitualmente:

- atual;
- recente;
- histórico;
- arquivado;
- permanente.

Não use apenas data como critério se isso não fizer sentido para orientações permanentes.

---

# 21. Conteúdo em destaque

Avalie se precisamos de um conceito específico de:

- destaque;
- fixado;
- importante;
- prioridade;

ou se alguns desses conceitos seriam redundantes.

Evite criar várias propriedades diferentes que tentam responder à mesma pergunta:

> "o usuário precisa perceber isso agora?"

Se alguma distinção for necessária, explique claramente.

---

# 22. Mídia

Defina conceitualmente como conteúdo pode incluir:

- imagens;
- vídeos;
- arquivos.

Considere:

- um comunicado com várias imagens;
- vídeo acompanhado de contexto textual;
- comunicado publicado principalmente como imagem;
- imagem criada originalmente para WhatsApp;
- PDF anexado;
- arquivo para download.

Princípio importante:

> o CondoMural não deve depender exclusivamente de uma imagem para transmitir informação essencial quando houver possibilidade razoável de estruturar contexto textual.

Isso melhora:

- acessibilidade;
- busca;
- compreensão;
- preservação da informação.

Não escolha formatos, limites de tamanho, storage ou CDN nesta etapa.

---

# 23. Acessibilidade de conteúdo

Como parte do modelo editorial, identifique requisitos conceituais de acessibilidade.

Considere:

- títulos claros;
- contexto textual para imagens;
- descrição alternativa quando relevante;
- não depender apenas de cor;
- conteúdo compreensível em dispositivos móveis;
- anexos que não sejam a única forma de compreender uma informação essencial quando evitável.

Não detalhe ainda componentes de interface.

---

# 24. Compartilhamento e fonte canônica

WhatsApp continuará como canal de distribuição.

Portanto, avalie o princípio de que conteúdos publicados na Central possam possuir uma referência persistente e compartilhável.

Exemplo conceitual:

Administração publica comunicado na Central  
→ compartilha referência no WhatsApp  
→ morador acessa a fonte oficial.

Defina o comportamento esperado para:

- conteúdo público;
- conteúdo privado;
- usuário sem autenticação acessando referência privada;
- conteúdo arquivado.

Não detalhe URLs ou implementação.

---

# 25. Conteúdo relacionado

Avalie se o MVP precisa relacionar conteúdos.

Exemplo:

Comunicado sobre mudança temporária em determinada regra  
→ orientação permanente correspondente.

Ou:

Novo comunicado  
→ documento oficial relacionado.

Questione se isso precisa existir no MVP ou se busca e assunto já resolvem o problema inicialmente.

Evite construir prematuramente um sistema complexo de relacionamentos.

---

# 26. Informações específicas do Palermo

Use Palermo como primeiro caso real para validar as decisões, mas não transforme suas características em arquitetura global.

Considere exemplos reais como:

- lavanderia;
- entregas;
- mudanças;
- acesso;
- reciclagem;
- barulho;
- elevadores;
- motos;
- guia rebaixada;
- vaga PCD;
- área de carga e descarga;
- contatos úteis.

Questione para cada exemplo:

> isso representa um tipo global de conteúdo, um assunto configurável, uma orientação permanente ou apenas informação específica do tenant?

A resposta deve preservar:

> Palermo é configuração, não arquitetura.

---

# 27. Governança editorial

Defina responsabilidades conceituais de:

### Gestor de Conteúdo

e

### Administrador do Condomínio

em relação a:

- criar conteúdo;
- editar;
- publicar;
- arquivar;
- restaurar;
- alterar prioridade;
- definir público;
- administrar assuntos;
- remover conteúdo quando permitido.

Lembre que o Administrador também possui capacidades editoriais.

Avalie se ambos precisam exatamente das mesmas permissões editoriais ou se alguma operação editorial especialmente sensível deveria ficar limitada ao Administrador.

Prefira simplicidade quando não houver ganho claro de segurança.

---

# 28. Auditoria editorial

A Etapa 2 já definiu auditoria tenant-scoped.

Nesta etapa, detalhe quais eventos editoriais precisam ser auditáveis.

Considere:

- criação;
- publicação;
- edição relevante;
- alteração de público;
- alteração de prioridade;
- arquivamento;
- restauração;
- exclusão excepcional;
- alteração de assunto;
- alteração de documento/anexo.

Não defina tecnologia de logs.

---

# 29. Segurança e privacidade na informação

Identifique riscos específicos da arquitetura de conteúdo.

Considere:

- conteúdo privado aparecendo em busca pública;
- preview revelando título restrito;
- anexo com autorização diferente do conteúdo;
- mídia acessível sem autorização;
- conteúdo segmentado indexado incorretamente;
- documento privado compartilhado externamente;
- conteúdo de outro tenant aparecendo em busca ou navegação;
- alteração de público expondo informação existente;
- cache ou histórico conceitual mantendo exposição indevida.

Não detalhe soluções técnicas ainda.

---

# 30. Qualidade e cenários futuros de teste

Identifique cenários críticos que futuramente precisarão de testes.

Considere pelo menos:

- comunicado público aparece somente no tenant correto;
- conteúdo privado não aparece para visitante;
- conteúdo segmentado não aparece fora do público;
- busca não revela conteúdo sem autorização;
- resultado de busca respeita tenant;
- anexo respeita a mesma autorização do conteúdo;
- conteúdo arquivado não aparece indevidamente como atual;
- orientação permanente atual é claramente identificável;
- versão antiga não substitui silenciosamente orientação válida;
- assunto arquivado não quebra conteúdo antigo;
- alteração de público produz comportamento correto;
- prioridade não altera autorização;
- mídia não contorna autorização;
- conteúdo de um tenant nunca aparece em outro;
- usuário com dois tenants enxerga corretamente os dados de cada contexto.

Não implemente testes nesta etapa.

---

# 31. Classificação de escopo

Para cada capacidade identificada, classifique como:

**ESSENCIAL PARA MVP**

**IMPORTANTE, MAS PODE VIR DEPOIS**

**IDEIA FUTURA**

Não coloque no MVP uma capacidade apenas porque seria tecnicamente interessante.

A classificação desta etapa é preliminar e específica ao domínio de conteúdo.

A definição final do escopo global do MVP ocorrerá posteriormente.

---

# Entregável esperado

Produza uma proposta objetiva contendo:

1. modelo conceitual de conteúdo;
2. tipos de conteúdo recomendados;
3. definição consolidada de comunicado;
4. definição consolidada de orientação permanente;
5. definição de documento;
6. tratamento de informações institucionais;
7. tratamento de contatos úteis;
8. tratamento de serviços;
9. modelo de tipo / assunto / prioridade / público;
10. níveis recomendados de prioridade;
11. modelo de assuntos;
12. regras de criação e manutenção de assuntos;
13. ciclo de vida do conteúdo;
14. regras de validade temporal;
15. regras para edição de conteúdo publicado;
16. regras de histórico e versões;
17. arquivamento;
18. exclusão excepcional;
19. princípios da página inicial;
20. proposta de arquitetura da informação;
21. proposta de navegação principal;
22. relação entre camada pública e privada;
23. modelo conceitual de busca;
24. filtros essenciais;
25. tratamento de conteúdo recente, permanente e histórico;
26. decisão sobre destaque/fixação;
27. modelo conceitual de mídia e anexos;
28. requisitos editoriais de acessibilidade;
29. comportamento conceitual de compartilhamento;
30. decisão sobre conteúdo relacionado;
31. governança editorial;
32. eventos editoriais auditáveis;
33. implicações multi-tenant;
34. riscos de segurança e privacidade;
35. cenários críticos que futuramente precisarão de testes;
36. classificação MVP / depois / futuro;
37. decisões que precisam da minha validação;
38. itens deliberadamente deixados para etapas posteriores.

---

# Forma de trabalho

Atue como:

- Product Manager;
- especialista em arquitetura da informação;
- especialista em UX mobile-first;
- arquiteto de software com foco em segurança;
- especialista em QA.

Para decisões relevantes:

1. identifique o problema;
2. considere as decisões canônicas existentes;
3. questione premissas somente quando houver motivo concreto;
4. apresente alternativas reais;
5. mostre vantagens e desvantagens relevantes;
6. recomende uma direção;
7. prefira a solução mais simples quando não houver benefício claro em maior complexidade;
8. destaque claramente o que depende da minha validação;
9. consolide o que puder ser considerado decidido;
10. registre o que deve ficar para depois.

Prefira decisões claras e justificadas a documentação extensa ou abstrações excessivas.

---

# Princípios obrigatórios

Durante toda a análise preserve:

### Fonte de verdade

A Central mantém a informação oficial persistente.

### Mobile-first

Encontrar e consumir informação precisa funcionar bem em celular.

### Encontrabilidade

Informação relevante deve poder ser localizada posteriormente.

### Informação antes de interação social

Não transformar o produto em rede social.

### Histórico

Informação antiga relevante não deve desaparecer silenciosamente.

### Segurança por padrão

Conteúdo nunca pode vazar entre públicos ou tenants.

### Tenant-scoped

Conteúdo, assuntos, permissões, busca e auditoria pertencem ao tenant correspondente.

### Configuração antes de hardcode

Particularidades do Palermo não devem contaminar o núcleo do produto.

### Simplicidade editorial

A administração não deve precisar entender um CMS complexo para publicar um aviso.

### Projetar para evoluir, implementar somente o necessário

Evitar tanto hardcode quanto abstrações prematuras.

---

# Restrições desta etapa

NÃO:

- escreva código;
- escolha stack;
- escolha banco de dados;
- escolha mecanismo de busca;
- escolha storage;
- escolha CDN;
- escolha formatos ou limites técnicos de upload;
- escolha framework de CMS;
- detalhe APIs;
- detalhe modelo físico de dados;
- detalhe implementação de versionamento;
- detalhe implementação de multi-tenancy;
- crie wireframes;
- desenhe telas;
- escolha design system;
- defina componentes visuais;
- detalhe fluxos completos de UX que pertencem a etapa posterior;
- defina o MVP global completo;
- crie backlog;
- crie issues;
- inicie implementação;
- altere o repositório sem minha validação das decisões desta etapa.

Não transforme esta etapa em especificação técnica.

Ao final, **pare na Etapa 3**.

Apresente claramente as decisões que precisam da minha validação antes de qualquer consolidação no repositório ou avanço para a próxima etapa.
