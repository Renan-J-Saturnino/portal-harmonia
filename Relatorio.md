# Relatório de Melhoria Contínua: Aplicando o Ciclo PDCA ao Portal Harmonia

---

### **1. Capa**

| | |
| :--- | :--- |
| **Instituição:** | SENAI DENDEZEIROS |
| **Curso:** | Desenvolvimento de Sistemas |
| **Disciplina:** | Manutenção de Sistemas |
| **Título:** | Relatório de Melhoria Contínua: Aplicando o Ciclo PDCA ao Portal Harmonia |
| **Professor(a):** | Anderson Alves |
| **Alunos:** | Renan J. Saturnino Braga , Átila Silva Barbosa FIlho|
| **Turma:** | G91210 |
| **Turno:** | Matutino |
| **Data:** | 02/11/2025 |

---

### **2. Descrição do Sistema (Portal Harmonia)**

O Portal Harmonia é um sistema web desenvolvido com o objetivo de oferecer uma experiência acessível, segura e harmoniosa aos seus usuários. O sistema implementa os fluxos essenciais de autenticação (login), cadastro de novos usuários e um CRUD (Criar, Ler, Editar, Deletar) completo para o gerenciamento de produtos.

---

### **3. Resumo dos Testes Realizados**

Como parte da avaliação da qualidade do Portal Harmonia, foi executado um conjunto de testes funcionais e não funcionais. O objetivo era registrar os resultados reais e compará-los com os resultados esperados para identificar defeitos e oportunidades de melhoria.

Os testes focaram em três módulos principais:

* **Login:** Verificação de credenciais incorretas, validação de campos e o mecanismo de bloqueio por tentativas excessivas.
* **Cadastro:** Testes de duplicidade de e-mail, validação de senhas fracas e conformidade com requisitos de acessibilidade.
* **CRUD de Produtos:** Testes de criação, edição e remoção de produtos, funcionalidade de busca e, crucialmente, o controle de acesso baseado no perfil do usuário.

---

### **4. Aplicação das Quatro Etapas do PDCA**

O Ciclo PDCA (Planejar, Executar, Verificar, Agir) foi a metodologia utilizada para estruturar a melhoria contínua do portal.

#### **PLANEJAR (Plan)**

Nesta etapa, o objetivo principal foi definido: garantir que as funcionalidades centrais (login, cadastro e CRUD) atendam aos requisitos de qualidade e acessibilidade.

* **Mapeamento:** Os cenários de teste foram mapeados (conforme tabelas da próxima etapa).
* **Riscos Identificados:** Os principais riscos levantados antes da execução foram:
    * Campos de formulário sem validação de erro visível e falta de retorno acessível (`aria-live`).
    * Falhas no controle de acesso, especificamente o perfil "operacional" tendo permissões de exclusão.
    * Aceitação de valores inválidos, como preço negativo.
* **Metas de Melhoria:**
    * Garantir 100% de mensagens de erro acessíveis via leitor de tela.
    * Assegurar o bloqueio correto de funcionalidades por perfil.
    * Implementar corretamente o bloqueio de login após múltiplas tentativas falhas.

#### **EXECUTAR (Do)**

A fase de execução envolveu duas frentes: a execução dos testes planejados e a correção imediata dos defeitos (bugs) encontrados.

* **Execução de Testes:** Os cenários de Login, Cadastro e CRUD de Produtos foram sistematicamente testados.
* **Ações Corretivas Implementadas:**
    * **Acessibilidade:** As mensagens de erro em todos os formulários (login, cadastro, CRUD) foram corrigidas para usar `aria-live="assertive"`, garantindo que leitores de tela anunciem os erros.
    * **Validação (Preço):** A lógica no formulário de produtos foi ajustada para impedir o cadastro de preços negativos (ex: `price < 0`).
    * **Validação (Senha):** A regra de validação de senha no cadastro foi reforçada para exigir 8+ caracteres, 1 letra maiúscula e 1 número, impedindo senhas fracas.
    * **Controle de Acesso:** A lógica de permissão no CRUD foi invertida. O botão "Excluir" agora é renderizado apenas se o perfil do usuário for `administrador`.
    * **Lógica de Bloqueio:** O tempo de bloqueio de login foi corrigido no código (de 10 minutos para 5 minutos) para corresponder à mensagem exibida ao usuário.

#### **VERIFICAR (Check)**

Nesta etapa, os resultados obtidos (antes e depois das correções) foram comparados com os resultados esperados, validando a eficácia das ações da etapa "Executar".

**Tabela Comparativa de Resultados (Check)**

| Teste executado | Resultado Esperado | Resultado Real (Antes da Correção) | Resultado (Após Correção/DO) |
| :--- | :--- | :--- | :--- |
| **Login:** Bloqueio após 5 tentativas | Mensagem "Tente novamente em 5 min." e bloqueio efetivo de 5 min. | Falha: A mensagem dizia 5 min, mas a lógica do código (DEFECT) bloqueava por 10 min. | Corrigido: A lógica de bloqueio foi ajustada para 5 minutos, alinhando-se à mensagem. |
| **Cadastro:** Senha fraca ("abc123") | Impedir envio e exibir dica de senha forte. | Falha: Senha fraca aceita. A validação (DEFECT) era insuficiente. | Corrigido: A validação de senha foi implementada corretamente. |
| **Cadastro:** E-mail duplicado | Impedir envio e exibir erro "E-mail ou usuário já em uso." | Falha: O sistema permitia cadastro (DEFECT), pois só validava *username* duplicado. | Corrigido: A verificação foi expandida para incluir o campo e-mail. |
| **CRUD:** Produto com preço -5,00 | Exibir erro "Preço deve ser zero ou positivo" e bloquear envio. | Falha: O sistema aceitava preços negativos (ex: -0.99) devido a uma lógica de validação falha (DEFECT). | Corrigido: A validação foi corrigida para `price < 0`. |
| **CRUD:** Acesso (Perfil Operacional) | Perfil 'operacional' não deve ter permissão para excluir produtos. | Falha: Lógica de permissão invertida (DEFECT). O perfil 'operacional' podia excluir. | Corrigido: Apenas o perfil 'administrador' agora visualiza e acessa a função de exclusão. |
| **Acessibilidade:** Erro de formulário | Leitor de tela deve anunciar a mensagem de erro (ex: "Usuário ou senha incorretos"). | Falha: Mensagens de erro com `aria-live="off"`. O leitor de tela não anunciava o erro. | Corrigido: Atributo `aria-live="assertive"` aplicado em todas as mensagens de status e erro. |

#### **AGIR (Act)**

Com base na verificação, esta etapa padroniza as melhorias e define o planejamento para o próximo ciclo.

* **Padronização:** As correções (validação de senha, `aria-live`, controle de acesso) foram documentadas e devem servir como padrão para novos formulários e módulos do sistema.
* **Lições Aprendidas:**
    * Acessibilidade não é opcional: Feedbacks dinâmicos (erros, toasts) devem ser acessíveis desde o início (`aria-live`).
    * Testes de permissão são críticos: Uma lógica de permissão invertida é um defeito grave de segurança.
    * Coesão entre mensagem e lógica: A mensagem de bloqueio (5 min) deve refletir a ação real do sistema.
* **Planejamento (Novo Ciclo PDCA - v2.0):**
    * **Plan:** Focar em testes de performance (medir o tempo de resposta dos loaders) e testes de regressão automatizados.
    * **Do:** Implementar uma suíte de testes (ex: Cypress ou Selenium) para automatizar os cenários de Login e CRUD.
    * **Check:** Executar a suíte automatizada a cada nova alteração no código.
    * **Act:** Refinar a automação e adicionar novos testes (ex: responsividade).

---

### **5. Conclusão e Sugestões de Melhoria Contínua**

A aplicação do ciclo PDCA no Portal Harmonia foi fundamental para elevar a qualidade do sistema. O processo permitiu não apenas identificar defeitos críticos que haviam passado despercebidos (como a falha grave no controle de acesso e a inconsistência no bloqueio de login), mas também corrigi-los de forma estruturada.

O sistema agora está mais robusto, seguro e significativamente mais acessível, atendendo melhor ao seu objetivo de ser uma plataforma "harmoniosa".

**Sugestões para Melhoria Contínua:**

1.  **Testes Automatizados:** Como definido na etapa "Agir", a criação de uma suíte de testes de regressão automatizada é a prioridade máxima para garantir que futuras alterações não reintroduzam os defeitos corrigidos.
2.  **Busca Dinâmica (Debounce):** A busca de produtos no CRUD (que agora usa `input` para ser mais responsiva) deve implementar um *debounce* para melhorar a performance e a usabilidade, disparando a busca automaticamente apenas após o usuário parar de digitar por um breve período.
3.  **Testes de Usabilidade (Lembrar-me):** A funcionalidade "Lembrar-me", que foi corrigida na etapa "Do" (para usar `localStorage` vs `sessionStorage`), deve ter um cenário de teste formal no próximo ciclo para validar seu comportamento em diferentes navegadores e sessões.
4.  **Integração Contínua (CI):** Integrar a suíte de testes automatizados a um pipeline de CI/CD (como GitHub Actions ou Jenkins) para validar cada *commit* antes da implantação.