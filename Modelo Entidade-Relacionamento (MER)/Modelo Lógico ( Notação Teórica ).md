___

- **Superclasse Base:** `usuario`
    
    - Armazena dados comuns a todas as pessoas (nome, cpf, telefone).
        
- **Subclasses de `usuario`:**
    
    - `aluno` (Um aluno **"é um"** usuário)
        
    - `funcionarios` (Um funcionário **"é um"** usuário)
        
- **Subclasses de `funcionarios`:**
    
    - O padrão se repete: `funcionarios` é uma superclasse para:
        
    - `personal` (Um personal **"é um"** funcionário)
        
    - `recepcionista` (Um recepcionista **"é um"** funcionário)
        

---

## 📋 Notação Relacional Textual (Análise de Entidades e Relações)

### 1. Entidades de Pessoas (O Padrão de Herança)

- **usuario** (`PK id`)
    
    - A entidade-pai (superclasse) para todas as pessoas.
        
- **funcionarios** (`PK id`, `FK usuario_id` -> `usuario(id)`)
    
    - Relação **1-para-1** com `usuario`.
        
    - Generalização de todos os tipos de funcionários.
        
- **aluno** (`PK id`, `FK usuario_id` -> `usuario(id)`)
    
    - Relação **1-para-1** com `usuario`.
        
- **personal** (`PK id`, `FK funcionarios_id` -> `funcionarios(id)`)
    
    - Relação **1-para-1** com `funcionarios`.
        
- **recepcionista** (`PK id`, `FK funcionarios_id` -> `funcionarios(id)`)
    
    - Relação **1-para-1** com `funcionarios`.
        

### 2. Entidades de Gestão (Academia)

- **plano** (`PK id`, `FK recepcionista_id` (UNIQUE) -> `recepcionista(id)`, `FK aluno_id` (UNIQUE) -> `aluno(id)`)
    
    - _Esta tabela possui um design incomum (veja observações abaixo)._
        
- **acesso** (`PK id`)
    
    - Registra os eventos de entrada e saída. É referenciada por `aluno` (Relação **N-para-1**).
        
- **aulas** (`PK id`)
    
    - Catálogo de aulas disponíveis.
        
- **treino** (`PK id`)
    
    - Catálogo de treinos.
        
- **reserva** (`PK id`)
    
    - Registros de reservas (provavelmente para aulas ou equipamentos).
        
- **equipamento** (`PK id`)
    
    - Catálogo de equipamentos da academia.
        

### 3. Entidades de Pagamento e Matrícula

- **matricula** (`PK id`, `FK aluno_id` -> `aluno(id)`)
    
    - Registra um evento de pagamento/matrícula de um aluno.
        
    - Relação **N-para-1** (Um `aluno` pode ter **N** `matriculas`).
        
- **mensalidade** (`PK id`)
    
    - _Esta tabela é referenciada por `aluno` de forma 1-para-1 (UNIQUE), o que é atípico (veja observações)._
        

### 4. Tabelas Associativas (Relações Muitos-para-Muitos)

Estas tabelas resolvem relações N:M:

- **rel_aluno_treino** (`PK (aluno_id, treino_id)`)
    
    - Resolve **Aluno (N) <-> (M) Treino**.
        
- **rel_aluno_reserva** (`PK (aluno_id, reserva_id)`)
    
    - Resolve **Aluno (N) <-> (M) Reserva**.
        
- **rel_treino_personal** (`PK (treino_id, personal_id)`)
    
    - Resolve **Treino (N) <-> (M) Personal**.
        
- **rel_personal_aulas** (`PK (personal_id, aulas_id)`)
    
    - Resolve **Personal (N) <-> (M) Aulas**.
        
- **rel_aluno_equipamento** (`PK (aluno_id, equipamento_id)`)
    
    - Resolve **Aluno (N) <-> (M) Equipamento**.