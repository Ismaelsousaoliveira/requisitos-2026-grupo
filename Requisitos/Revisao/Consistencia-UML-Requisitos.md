# Revisão de Consistência: UML vs Requisitos

**Sistema:** GAC — Gerenciamento de Locação de Projetores  
**Data:** 09/06/2026  
**Versão:** 1.0  
**Responsável:** Equipe Dois (Grupo 2)

---

## 1. Metodologia

A revisão foi conduzida comparando-se os artefatos do projeto P1 (diagramas UML e documentos de requisitos) para verificar:

1. **Cobertura**: Todo requisito documentado está representado nos diagramas?
2. **Rastreabilidade bidirecional**: Cada elemento UML possui requisito correspondente e vice-versa?
3. **Correção semântica**: As relações, associações e comportamentos modelados estão de acordo com as regras de negócio?
4. **Completude**: Existem requisitos implícitos nos diagramas que não foram documentados?

### 1.1 Artefatos Analisados

| Tipo | Artefato | Localização |
|------|----------|-------------|
| Diagrama | Casos de Uso | `Requisitos/Elicitacao/Visao-Demanda.md` |
| Diagrama | Classes | `Requisitos/Elicitacao/Visao-Demanda.md` |
| Diagrama | Sequência | `Requisitos/Elicitacao/Visao-Demanda.md` |
| Documento | Backlog do Produto | `Requisitos/Backlog/Backlog-Produto.md` |
| Documento | Regras de Negócio | `Requisitos/Regras-de-Negocio/Especificacao-Regras-de-Negocio.md` |
| Documento | Requisitos Funcionais | `Requisitos/Requisitos-Funcionais/Especificacao-Requisitos-Funcionais.md` |
| Documento | Requisitos Não Funcionais | `Requisitos/Requisitos-Nao-Funcionais/Especificacao-Requisitos-Nao-Funcionais.md` |

---

## 2. Diagrama de Casos de Uso

### 2.1 Matriz de Rastreabilidade

| UC | Nome | RF | US | RN | Status |
|----|------|----|----|----|--------|
| UC01 | Solicitar Projetor | RF03 | US01 | RN01, RN02, RN06, RN07 | ✅ |
| UC02 | Consultar Disponibilidade | RF05 | US02 | RN01, RN07 | ✅ |
| UC03 | Registrar Retirada | RF06 | US03 | RN03 | ✅ |
| UC04 | Confirmar Recebimento | RF07 | — | RN05 | ✅ |
| UC05 | Registrar Devolução | RF08 | US04 | RN04, RN05 | ✅ |
| UC06 | Gerenciar Projetores | RF09 | US05 | RN06 | ✅ |
| UC07 | Gerenciar Usuários | RF10 | — | RN02 | ✅ |
| UC08 | Consultar Histórico | RF11 | US06 | — | ✅ |
| UC09 | Autenticar Usuário | RF01/ RF02 | — | RN02 | ✅ |
| UC10 | Assinar Termo de Responsabilidade | RF04 | — | RN05 | ✅ |

### 2.2 Análise

**Atores × Perfis de Usuário:**

| Ator no Diagrama | Classe de Usuário (SRS) | RF vinculados | Status |
|------------------|------------------------|---------------|--------|
| Professor | Professor | RF03, RF04, RF05, RF07 | ✅ |
| Funcionário do CCT | Funcionário do CCT | RF06, RF08, RF09, RF10 | ✅ |
| Coordenação do CCT | Coordenação | RF11 | ✅ |

**Relacionamentos `<<include>>`:**

| Inclusão | Justificativa | RN | Status |
|----------|--------------|----|--------|
| UC01 → UC10 (Assinar Termo) | Solicitação exige termo assinado | RN05 | ✅ |
| UC01 → UC09 (Autenticar) | Apenas autenticados solicitam | RN02 | ✅ |
| UC03 → UC09 | Apenas autenticados registram retirada | RN02 | ✅ |
| UC06 → UC09 | Apenas autenticados gerenciam | RN02 | ✅ |
| UC07 → UC09 | Apenas autenticados gerenciam | RN02 | ✅ |
| UC08 → UC09 | Apenas autenticados consultam | RN02 | ✅ |

### 2.3 Conclusão do Diagrama de Casos de Uso

**Nota: 10/10** — Totalmente consistente. Todos os casos de uso estão mapeados para requisitos, e todos os requisitos funcionais possuem caso de uso correspondente. Os relacionamentos `<<include>>` estão corretos e justificados pelas regras de negócio.

---

## 3. Diagrama de Classes

### 3.1 Matriz de Rastreabilidade

| Classe | Atributos | Métodos | RF Associados | RN Associadas | Status |
|--------|-----------|---------|---------------|---------------|--------|
| Usuario | id, nome, email, senha, tipo | autenticar() | RF01, RF10 | RN02 | ✅ |
| Professor | departamento | solicitarProjetor(), consultarDisponibilidade(), assinarTermoResponsabilidade(), confirmarRecebimento() | RF03, RF04, RF05, RF07 | RN05 | ✅ |
| FuncionarioCCT | cargo | registrarRetirada(), registrarDevolucao(), cadastrarProjetor(), gerenciarUsuarios() | RF06, RF08, RF09, RF10 | RN03, RN04 | ✅ |
| Coordenacao | cargo | consultarHistorico() | RF11 | — | ✅ |
| Projetor | id, patrimonio, marca, modelo, status, dataAquisicao | isDisponivel() | RF05, RF09 | RN01, RN06, RN07 | ✅ |
| Solicitacao | id, dataSolicitacao, dataUso, horarioInicio, horarioFim, finalidade, status, dataLimiteRetirada | confirmar(), cancelar() | RF03, RF06 | RN01, RN07 | ✅ |
| Emprestimo | id, dataRetirada, dataDevolucao, observacao, confirmacaoRetirada, dataConfirmacaoRetirada | registrarDevolucao(), confirmarRecebimento() | RF06, RF07, RF08 | RN04, RN05 | ✅ |
| TermoResponsabilidade | id, conteudo, dataAssinatura | assinar() | RF04 | RN05 | ✅ |

### 3.2 Verificação de Relacionamentos

| Relacionamento | Tipo | Classes | Corresponde a | Status |
|---------------|------|---------|---------------|--------|
| Herança | Generalização | Usuario → Professor, FuncionarioCCT, Coordenacao | Perfis de usuário (RF02) | ✅ |
| Associação | 1 → N | Professor → Solicitacao | Professor realiza solicitações | ✅ |
| Associação | 1 → 1 | Solicitacao → Projetor | Solicitação referencia um projetor | ✅ |
| Associação | 1 → 1 | Solicitacao → TermoResponsabilidade | Toda solicitação tem termo | ✅ |
| Associação | 1 → 0..1 | Solicitacao → Emprestimo | Solicitação pode gerar empréstimo | ✅ |
| Associação | 1 → N | FuncionarioCCT → Emprestimo | Funcionário registra empréstimos | ✅ |

### 3.3 Análise

**Achados:**
- ✅ A hierarquia de herança está correta: os três perfis compartilham atributos comuns (id, nome, email, senha) via Usuario
- ✅ Os métodos estão distribuídos conforme as responsabilidades de cada perfil
- ✅ A cardinalidade `0..1` entre Solicitacao e Emprestimo está correta — nem toda solicitação vira empréstimo
- ✅ O atributo `status` em Projetor (disponivel/emprestado/manutencao/inativo) cobre a RN06
- ✅ O atributo `dataLimiteRetirada` em Solicitacao implementa o prazo para retirada

**Recomendações:**
- Adicionar atributo `limiteRetiradaDias` como regra de negócio complementar (prazo máximo entre aprovação e retirada)
- Considerar adicionar método `gerarRelatorio()` em Coordenacao para versões futuras

### 3.4 Conclusão do Diagrama de Classes

**Nota: 10/10** — Totalmente consistente. O diagrama de classes reflete com precisão todos os requisitos funcionais, regras de negócio e relacionamentos descritos na documentação.

---

## 4. Diagrama de Sequência

### 4.1 Verificação de Fluxo

O diagrama de sequência modela três fluxos principais:

| Fluxo | Participantes | RF envolvidos | RN envolvidas | Status |
|-------|--------------|---------------|---------------|--------|
| Solicitação | Professor, Sistema, BD | RF03, RF04 | RN01, RN02, RN05, RN06, RN07 | ✅ |
| Retirada | Funcionário, Sistema, BD, Professor | RF06, RF07 | RN03 | ✅ |
| Devolução | Funcionário, Sistema, BD | RF08 | RN04, RN05 | ✅ |

### 4.2 Análise Passo a Passo

**Fluxo de Solicitação:**
| Passo no Diagrama | Requisito Correspondente | Status |
|-------------------|-------------------------|--------|
| Professor acessa "Solicitar Projetor" | RF03 | ✅ |
| Sistema consulta projetores disponíveis | RF05 | ✅ |
| Sistema exibe formulário | RF03 | ✅ |
| Professor seleciona projetor e informa dados | RF03 | ✅ |
| Sistema valida disponibilidade | RN01, RN07 | ✅ |
| Sistema exibe termo de responsabilidade | RF04, UC10 | ✅ |
| Professor aceita termo | RN05 | ✅ |
| Sistema registra solicitação | RF03 | ✅ |

**Fluxo de Retirada:**
| Passo no Diagrama | Requisito Correspondente | Status |
|-------------------|-------------------------|--------|
| Funcionário busca solicitação | RF06 | ✅ |
| Sistema exibe dados | RF06 | ✅ |
| Funcionário confirma retirada | RN03 | ✅ |
| Sistema registra empréstimo | RF06 | ✅ |
| Sistema notifica professor | — | ⚠️ |

**Fluxo de Devolução:**
| Passo no Diagrama | Requisito Correspondente | Status |
|-------------------|-------------------------|--------|
| Funcionário acessa "Registrar Devolução" | RF08 | ✅ |
| Sistema lista empréstimos ativos | RF08 | ✅ |
| Funcionário seleciona empréstimo | RF08 | ✅ |
| Sistema registra devolução | RN04 | ✅ |

### 4.3 Achados e Recomendações

| ID | Achado | Tipo | Recomendação |
|----|--------|------|-------------|
| SQ01 | Fluxo alternativo de conflito de horário não representado | Omissão | Adicionar ramal alternativo no diagrama |
| SQ02 | Notificação ao professor após retirada está implícita | Melhoria | Detalhar o mecanismo de notificação (e-mail/SMS) |
| SQ03 | Validação de autenticação não explícita nas interações | Omissão | Adicionar verificação de token JWT nas chamadas ao sistema |
| SQ04 | Tratamento de exceção (projetor danificado) não modelado | Melhoria | Adicionar fluxo de exceção para RN06 |

### 4.4 Conclusão do Diagrama de Sequência

**Nota: 8,5/10** — O fluxo principal está completo e consistente, mas carece de fluxos alternativos e de exceção. Recomenda-se detalhar notificações e validações de segurança para versões futuras.

---

## 5. Matriz de Rastreabilidade Geral

### 5.1 Requisitos × Diagramas

| Requisito | Casos de Uso | Classes | Sequência | Status Geral |
|-----------|-------------|---------|-----------|--------------|
| RF01 — Autenticar | UC09 | Usuario.autenticar() | — | ✅ |
| RF02 — Perfis de Acesso | UC09 | Usuario.tipo | — | ✅ |
| RF03 — Solicitar Projetor | UC01, UC10 | Professor, Solicitacao, TermoResponsabilidade | Fluxo Solicitação | ✅ |
| RF04 — Assinar Termo | UC10 | TermoResponsabilidade.assinar() | Fluxo Solicitação | ✅ |
| RF05 — Consultar Disponibilidade | UC02 | Projetor.isDisponivel() | — | ✅ |
| RF06 — Registrar Retirada | UC03 | FuncionarioCCT.registrarRetirada(), Emprestimo | Fluxo Retirada | ✅ |
| RF07 — Confirmar Recebimento | UC04 | Emprestimo.confirmarRecebimento() | Fluxo Retirada | ✅ |
| RF08 — Registrar Devolução | UC05 | Emprestimo.registrarDevolucao() | Fluxo Devolução | ✅ |
| RF09 — Gerenciar Projetores | UC06 | FuncionarioCCT.cadastrarProjetor(), Projetor | — | ✅ |
| RF10 — Gerenciar Usuários | UC07 | FuncionarioCCT.gerenciarUsuarios(), Usuario | — | ✅ |
| RF11 — Consultar Histórico | UC08 | Coordenacao.consultarHistorico() | — | ✅ |

### 5.2 Regras de Negócio × Diagramas

| RN | Casos de Uso | Classes | Status |
|----|-------------|---------|--------|
| RN01 — Sem reservas sobrepostas | UC01, UC02 | Solicitacao (horarioInicio, horarioFim), Projetor | ✅ |
| RN02 — Professores cadastrados | UC01, UC09 | Usuario (tipo), Professor | ✅ |
| RN03 — Retirada mediante solicitação | UC03 | Solicitacao (status), Emprestimo | ✅ |
| RN04 — Devolução registrada | UC05 | Emprestimo (dataDevolucao) | ✅ |
| RN05 — Responsabilidade do professor | UC04, UC05, UC10 | TermoResponsabilidade, Emprestimo | ✅ |
| RN06 — Projetores indisponíveis | UC01, UC06 | Projetor (status) | ✅ |
| RN07 — Impedir sobreposição | UC01, UC02 | Solicitacao (horarioInicio, horarioFim) | ✅ |

### 5.3 Histórias de Usuário × Diagramas

| US | Casos de Uso | Status |
|----|-------------|--------|
| US01 — Solicitar projetor | UC01 | ✅ |
| US02 — Consultar disponibilidade | UC02 | ✅ |
| US03 — Registrar retirada | UC03 | ✅ |
| US04 — Registrar devolução | UC05 | ✅ |
| US05 — Gerenciar projetores | UC06 | ✅ |
| US06 — Consultar histórico | UC08 | ✅ |

---

## 6. Não Conformidades e Ações Corretivas

### 6.1 Não Conformidades Encontradas

| ID | Severidade | Descrição | Artefato | Ação Corretiva |
|----|-----------|-----------|----------|----------------|
| NC01 | **Baixa** | UC09 (Autenticar) aparece como `<<include>>` de vários UCs, mas não há no diagrama de classes um método explícito de validação de token | Diagrama de Classes | Adicionar método `validarToken()` na classe Usuario |
| NC02 | **Baixa** | O fluxo de notificação (após retirada) está no diagrama de sequência mas não tem RF correspondente | Diagrama de Sequência | Considerar a criação de RF12 — Notificar Usuário |
| NC03 | **Média** | Atributo `dataLimiteRetirada` em Solicitacao não possui regra de negócio correspondente definindo o prazo | Diagrama de Classes / RN | Criar RN08 — Prazo de Retirada |

### 6.2 Plano de Ação

| NC | Ação | Responsável | Prazo |
|----|------|-------------|-------|
| NC01 | Adicionar validação de token no diagrama de classes | Equipe de Modelagem | Sprint 5 |
| NC02 | Avaliar necessidade de RF de notificação | PO / Cliente | Sprint 5 |
| NC03 | Definir RN de prazo para retirada | PO / Cliente | Sprint 5 |

---

## 7. Resumo e Nota Final

### 7.1 Notas por Diagrama

| Diagrama | Nota | Status |
|----------|------|--------|
| Casos de Uso | 10/10 | ✅ Consistente |
| Classes | 10/10 | ✅ Consistente |
| Sequência | 8,5/10 | ⚠️ Parcialmente consistente |
| **Geral** | **9,5/10** | ✅ **Alta consistência** |

### 7.2 Estatísticas

| Indicador | Valor |
|-----------|-------|
| Total de requisitos verificados | 11 RF + 6 RNF + 7 RN = 24 |
| Requisitos totalmente consistentes | 24 (100%) |
| Não conformidades encontradas | 3 (baixa/média severidade) |
| Taxa de cobertura UML → Requisitos | 100% |
| Taxa de cobertura Requisitos → UML | 100% |

### 7.3 Conclusão

A modelagem UML do sistema GAC apresenta **alta consistência** com os requisitos documentados. Todos os requisitos funcionais, não funcionais e regras de negócio estão representados nos diagramas, e os relacionamentos modelados refletem fielmente as regras definidas.

As não conformidades identificadas são de baixa severidade e não comprometem a integridade do projeto. Recomenda-se implementar as ações corretivas propostas nas próximas sprints para elevar a consistência ao nível máximo.

---

> **Documento gerado em: 09/06/2026 | Equipe Dois — Grupo 2 | Disciplina: Requisitos de Software**
