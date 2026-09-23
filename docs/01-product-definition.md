# CondoMural - Etapa 1: Definição do Produto

**Status:** concluída e aprovada  
**Data:** 23/09/2026  
**Produto:** CondoMural  
**Condomínio piloto:** Cidade Mooca Vila Palermo  
**Experiência do tenant piloto:** Central Palermo

## 1. Resumo executivo

O **CondoMural** é uma plataforma web mobile-first destinada a centralizar, organizar e preservar comunicações e informações oficiais de condomínios residenciais.

O primeiro caso real será o **Cidade Mooca Vila Palermo**, com **732 apartamentos**. A experiência desse primeiro tenant poderá ser apresentada aos moradores como **Central Palermo**, enquanto CondoMural será o nome do produto reutilizável para outros condomínios.

O problema central não é substituir WhatsApp, BZK, Codfy ou outros sistemas. O problema é garantir que informações oficiais estejam disponíveis de forma confiável, organizada, persistente e fácil de encontrar.

Princípio central:

> A Central é a fonte de verdade da informação; outros canais podem funcionar como meios de distribuição.

## 2. Problema principal

Hoje o WhatsApp funciona como principal canal de comunicação do condomínio, mas possui limitações estruturais:

- inclusão manual de moradores;
- cobertura incompleta das unidades;
- perda de informações no histórico;
- dificuldade para localizar comunicados antigos;
- repetição recorrente de orientações;
- ausência de uma referência oficial única e persistente.

O problema de produto é:

> Como garantir que informações oficiais do condomínio estejam disponíveis de forma organizada, confiável, acessível e permanente para moradores e demais públicos autorizados?

## 3. Problemas secundários

### 3.1 Cobertura e identificação de moradores

O produto precisa permitir uma relação estruturada entre pessoa, condomínio, unidade e situação de acesso, sem conceder acesso privado apenas porque alguém informou uma unidade.

### 3.2 Descoberta de informações

O usuário deve conseguir encontrar rapidamente informações recentes e antigas, sem depender de uma rolagem cronológica extensa.

### 3.3 Conteúdo temporal e conteúdo permanente

Comunicados e orientações permanentes possuem naturezas diferentes e não devem ser tratados como o mesmo tipo de conteúdo.

### 3.4 Segmentação

Nem toda informação é relevante para todo o condomínio. Conteúdos poderão ser direcionados conforme o público aplicável, sem assumir uma estrutura física fixa para todos os tenants.

### 3.5 Governança e rastreabilidade

A plataforma precisa tornar claro quem criou, alterou, publicou, arquivou ou administrou informações relevantes.

### 3.6 Experiência mobile

O celular será um canal principal de consumo. A aplicação deve nascer mobile-first, e não ser apenas adaptada posteriormente.

### 3.7 Conteúdo multimídia

Comunicados precisam suportar texto, imagens, vídeos e, quando aplicável, anexos.

### 3.8 Transferência operacional

A operação administrativa não pode depender permanentemente do responsável técnico que criou o produto.

## 4. Usuários e atores conceituais

### Administração

Representa pessoas autorizadas pelo condomínio a operar conteúdo oficial e outras funções administrativas. Não será necessariamente um único papel.

### Condômino

Pessoa vinculada a uma unidade e autorizada a consumir informações privadas destinadas a moradores.

### Visitante público

Pessoa sem autenticação que acessa informações classificadas como públicas.

### Responsável técnico

Pessoa responsável por código, infraestrutura, deploy, configurações e diagnóstico técnico. Não deve ser automaticamente responsável editorial pelo conteúdo do condomínio.

## 5. Proposta de valor

### Para o morador

Encontrar de forma simples e confiável aquilo que o condomínio comunicou oficialmente, sem depender do histórico do WhatsApp.

### Para a administração

Publicar, organizar e manter informações oficiais em um único local, com controle de público, histórico e responsabilidade.

### Para o condomínio

Criar memória institucional da comunicação, reduzindo dependência de canais efêmeros e de pessoas específicas.

### Para o produto futuro

Disponibilizar uma base reutilizável para diferentes condomínios, sem que as particularidades do Palermo sejam tratadas como regras globais.

## 6. Objetivos

1. Estabelecer a Central como referência oficial de informação do condomínio.
2. Facilitar o acesso dos moradores a comunicados e orientações.
3. Preservar histórico relevante.
4. Permitir localização de informações antigas.
5. Permitir conteúdo com texto e mídia.
6. Proteger conteúdo privado por autenticação e autorização.
7. Permitir segmentação adequada ao público.
8. Reduzir repetição desnecessária de orientações.
9. Entregar excelente experiência no celular.
10. Permitir operação autônoma pela administração.
11. Tratar qualidade, segurança e testabilidade como requisitos desde o início.
12. Evitar decisões estruturais que prendam o produto ao Palermo.

## 7. Não objetivos iniciais

Não são objetivos do MVP, salvo decisão futura explícita:

- substituir completamente o WhatsApp;
- substituir BZK ou Codfy;
- criar rede social, fórum ou chat entre moradores;
- permitir publicação oficial livre por moradores;
- implementar financeiro condominial completo;
- implementar reservas de áreas como módulo próprio;
- implementar controle de visitantes como módulo próprio;
- implementar sistema de garagem;
- criar cobrança, planos SaaS, marketplace, CRM ou operação comercial;
- construir onboarding automatizado de condomínios antes de haver necessidade real.

## 8. Princípios do produto

### P1 - Fonte de verdade

O CondoMural deve ser a referência persistente da informação oficial. Outros canais podem distribuir essa informação.

### P2 - Mobile-first

Os fluxos principais devem funcionar confortavelmente em telas pequenas.

### P3 - Simplicidade

A experiência deve ser compreensível inclusive para pessoas com pouca familiaridade tecnológica.

### P4 - Informação antes de interação social

O produto organiza comunicação e conhecimento oficial; não é uma rede social.

### P5 - Administração responsável pelo conteúdo

A gestão do condomínio controla conteúdo oficial. O responsável técnico mantém o produto, mas não assume automaticamente função editorial.

### P6 - Segurança e privacidade por padrão

Autenticação, autorização, LGPD e minimização de dados devem orientar as decisões do produto.

### P7 - Histórico preservado

Alterações administrativas relevantes não devem destruir rastreabilidade ou memória institucional.

### P8 - Qualidade é parte da funcionalidade

Uma funcionalidade não é considerada pronta apenas por estar visualmente implementada. Critérios de aceite, regras, permissões e testes fazem parte da entrega.

### P9 - Palermo é configuração

Nome, identidade, estrutura, serviços e classificações específicas do piloto pertencem ao tenant Palermo.

### P10 - Projetar para evoluir, implementar somente o necessário

Preparar o produto para crescer sem construir antecipadamente um SaaS completo.

## 9. Decisões fechadas da Etapa 1

### D1 - Conteúdo público e privado

A plataforma terá uma camada pública. Informações não pessoais e não restritas poderão ser disponibilizadas sem login, incluindo informações institucionais, orientações gerais, contatos úteis, informações de acesso e determinados serviços.

Conteúdos pessoais, restritos ou segmentados exigirão autenticação e autorização adequadas.

### D2 - Duas formas de cadastro

O produto suportará conceitualmente:

1. solicitação de cadastro pelo próprio morador, sujeita a validação administrativa;
2. cadastro iniciado pela administração.

As etapas técnicas exatas serão definidas posteriormente. Informar uma unidade não concederá acesso privado automaticamente.

### D3 - Valor sem cadastro

A Central deve agregar valor mesmo para usuários não autenticados por meio da camada pública.

### D4 - Administração não será obrigatoriamente um único papel

A Etapa 2 poderá decompor o conceito amplo de administrador em personas, papéis ou conjuntos de permissões distintos.

### D5 - Responsável técnico e superusuário temporário

Durante o desenvolvimento, o responsável técnico poderá operar como superusuário para construir, testar, diagnosticar e validar o sistema.

Na primeira implantação real, o responsável técnico deverá realizar o onboarding dos **dois primeiros administradores** do condomínio. Depois disso, a operação administrativa deve ficar com a administração do tenant, enquanto o responsável técnico permanece com atribuições técnicas.

Esse mecanismo deverá ser seguro, auditável e não criar dependência permanente do responsável técnico para publicações ou operações administrativas cotidianas.

### D6 - Comunicados e orientações permanentes são conceitos distintos

**Comunicado:** conteúdo normalmente associado a um acontecimento ou período, como manutenção, interrupção ou evento.

**Orientação permanente:** informação que continua relevante até ser atualizada, como descarte de lixo, mudanças, entregas ou regras de convivência.

### D7 - Prioridade não é assunto

"Urgente" não será categoria de assunto.

O produto separará conceitualmente:

- tipo de conteúdo;
- assunto;
- prioridade;
- público.

### D8 - Estrutura física não será presa ao termo "torre"

O Palermo poderá utilizar "Torre" na interface, mas o núcleo do produto deverá aceitar outras estruturas e nomenclaturas, como bloco, edifício, ala ou setor.

## 10. Classificação conceitual do conteúdo

Cada conteúdo poderá ser descrito por dimensões diferentes.

### Tipo

O que o conteúdo é. Exemplos: comunicado, orientação permanente e documento.

### Assunto

Sobre o que o conteúdo trata. Exemplos: manutenção, elevadores, barulho, reciclagem, acesso, motos e lavanderia.

Na interface, o termo preferencial será inicialmente **Assunto**. O modelo deverá permitir evolução futura para múltiplas tags caso exista necessidade real.

Os assuntos serão configuráveis por condomínio e reutilizáveis, evitando criação livre de variações duplicadas a cada publicação.

### Prioridade

Indica o nível de atenção que o conteúdo exige. Os níveis definitivos serão definidos posteriormente.

### Público

Indica quem está autorizado a visualizar ou para quem aquele conteúdo possui relevância.

## 11. Particularidades do condomínio piloto

O Cidade Mooca Vila Palermo possui **732 apartamentos**.

A estrutura detalhada das unidades deverá ser baseada nos dados reais do condomínio e não inferida matematicamente a partir de aproximações de andares ou unidades por andar.

O condomínio não possui garagem para moradores. Existe apenas uma vaga PCD, que não é vaga de carga e descarga, não deve ser tratada como estacionamento comum e não deve ser apresentada como vaga disponível.

Existe problema recorrente de motos paradas irregularmente em frente ao condomínio, inclusive próximo à guia rebaixada. A Prefeitura está implantando área de carga e descarga em frente ao edifício.

Por isso, o produto não deve criar uma área genérica chamada "Garagem" para o Palermo. Quando necessário, deve tratar conceitos como acesso, área externa, motos, parada irregular, guia rebaixada, vaga PCD e carga e descarga.

## 12. Multi-tenant: impacto no produto

O Palermo é o primeiro tenant, não a arquitetura do produto.

O núcleo não deve assumir globalmente:

- existência de apenas um condomínio;
- exatamente três torres;
- uso obrigatório do termo "torre";
- mesma quantidade de unidades;
- mesmos espaços e regras;
- BZK ou Codfy;
- mesmos assuntos;
- mesmas cores, nome ou identidade visual.

Informações específicas devem pertencer ao tenant correspondente.

Ao mesmo tempo, essa premissa não autoriza overengineering. Não construiremos agora billing, CRM, marketplace, onboarding comercial automatizado ou administração massiva de tenants.

## 13. Hipóteses principais

1. Moradores usarão uma plataforma web quando precisarem consultar informações oficiais.
2. A administração manterá o conteúdo suficientemente atualizado para que a Central preserve credibilidade.
3. WhatsApp poderá continuar como canal de distribuição, apontando para a fonte persistente.
4. Separar comunicados, orientações e documentos aumentará a encontrabilidade.
5. Parte do conteúdo pode ser pública e parte privada.
6. A aprovação administrativa de moradores é viável para o piloto, mesmo que precise evoluir em escala.
7. O celular será o principal dispositivo de consulta para parcela significativa dos usuários.

## 14. Principais riscos

### R1 - Falta de adesão da administração

Sem conteúdo atualizado e confiável, a Central perde valor rapidamente.

### R2 - Falta de adesão dos moradores

Se o conteúdo completo continuar exclusivamente no WhatsApp, não haverá incentivo para usar a Central.

### R3 - MVP excessivamente grande

Há muitas capacidades possíveis. A Etapa 5 deverá ser rigorosa na definição de escopo.

### R4 - Confundir multi-tenant com SaaS completo

Precisamos evitar tanto hardcode do Palermo quanto abstrações comerciais prematuras.

### R5 - Administração de usuários virar gargalo

Dois fluxos de cadastro ajudam, mas a operação de aprovação precisará ser observada no piloto.

### R6 - Falha de autorização ou segmentação

Exposição de conteúdo ao público errado é risco de alta severidade e exigirá testes específicos.

### R7 - Mídia prejudicar a experiência mobile

Imagens e vídeos podem impactar desempenho, dados móveis e armazenamento.

### R8 - Excesso de seções e taxonomia

Transformar cada assunto em menu ou permitir tags totalmente livres recriaria a desorganização que o produto pretende resolver.

## 15. Critérios de sucesso do MVP

O MVP será considerado bem-sucedido se demonstrar que:

### Administração

- publica comunicados sem suporte técnico;
- publica texto e mídia;
- controla adequadamente o público;
- encontra e administra conteúdo antigo.

### Moradores e usuários

- acessam confortavelmente pelo celular;
- encontram comunicados recentes;
- encontram informações antigas;
- distinguem informação oficial;
- acessam somente conteúdo autorizado.

### Produto

- preserva histórico consistente;
- aplica autenticação e autorização corretamente;
- não apresenta regressão conhecida em fluxos críticos;
- funciona adequadamente em mobile;
- possui testes relevantes nos fluxos de maior risco;
- não depende do usuário pessoal do responsável técnico;
- mantém configurações do Palermo fora do núcleo global do produto.

## 16. Qualidade como requisito

A estratégia de desenvolvimento futura deverá priorizar testes para regras de negócio, segurança, permissões, isolamento entre tenants, cadastro e aprovação, bootstrap administrativo, publicação e segmentação, upload e visualização de mídia, responsividade e regressão de fluxos críticos.

Não será perseguida cobertura de 100% apenas por métrica. Qualidade será avaliada conforme risco e criticidade.

## 17. Naming e identidade

### Nome do produto

**CondoMural**

O nome referencia um mural digital oficial, organizado e persistente, alinhado ao problema central de comunicação e descoberta de informações.

### Nome do tenant piloto

**Central Palermo**

A marca do tenant pode refletir a identidade local do condomínio sem contaminar o nome ou as configurações globais do produto.

### Relação conceitual

- Produto: CondoMural
- Tenant: Cidade Mooca Vila Palermo
- Experiência/branding do tenant: Central Palermo

## 18. Itens deliberadamente deixados para as próximas etapas

Não foram definidos nesta etapa:

- papéis concretos e matriz de permissões;
- RBAC;
- poderes específicos de cada persona administrativa;
- funcionamento técnico do superusuário;
- bootstrap técnico detalhado;
- política detalhada de auditoria;
- associação entre usuário, tenant e unidade;
- regras de múltiplos vínculos;
- autenticação;
- arquitetura da informação;
- fluxos completos;
- escopo final do MVP;
- wireframes e design system;
- modelo de dados definitivo;
- estratégia técnica de multi-tenancy;
- armazenamento de mídia;
- stack e infraestrutura;
- backlog e Kanban;
- implementação.

## 19. Encerramento da Etapa 1

A Etapa 1 está concluída.

Definição consolidada:

> CondoMural é uma plataforma web mobile-first para centralizar, organizar e preservar comunicações e informações oficiais de condomínios, permitindo que a administração mantenha conteúdo confiável e que moradores e outros usuários encontrem facilmente informações públicas ou privadas conforme sua autorização.

O primeiro caso real será o Cidade Mooca Vila Palermo, com 732 apartamentos, utilizando a experiência Central Palermo.

A próxima fase é a **Etapa 2 - Usuários e Permissões**.
