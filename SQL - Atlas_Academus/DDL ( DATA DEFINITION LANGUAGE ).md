___

```SQL
-- ============================================
-- ATLAS ACADEMUS
-- ============================================

-- Tabela de Usuários (base para alunos e funcionários)
CREATE TABLE usuario (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(200) NOT NULL,
    cpf VARCHAR(14) UNIQUE NOT NULL,
    telefone VARCHAR(20),
    email VARCHAR(200) UNIQUE,
    sexo CHAR(1) CHECK (sexo IN ('M', 'F', 'O')),
    endereco TEXT,
    data_nascimento DATE,
    data_cadastro TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    ativo BOOLEAN DEFAULT TRUE
);

-- Índices para buscas frequentes
CREATE INDEX idx_usuario_cpf ON usuario(cpf);
CREATE INDEX idx_usuario_nome ON usuario(nome);

-- ============================================
-- MÓDULO DE ACESSO E CONTROLE
-- ============================================

CREATE TABLE acesso (
    id SERIAL PRIMARY KEY,
    usuario_id INTEGER NOT NULL,
    data_entrada TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    data_saida TIMESTAMP,
    tipo VARCHAR(50) CHECK (tipo IN ('entrada', 'saida', 'liberado', 'bloqueado')),
    observacao TEXT,
    FOREIGN KEY (usuario_id) REFERENCES usuario(id) ON DELETE CASCADE
);

CREATE INDEX idx_acesso_usuario ON acesso(usuario_id);
CREATE INDEX idx_acesso_data ON acesso(data_entrada);

-- ============================================
-- MÓDULO FINANCEIRO
-- ============================================

CREATE TABLE plano (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    descricao TEXT,
    valor DECIMAL(10,2) NOT NULL,
    tipo VARCHAR(50) CHECK (tipo IN ('mensal', 'trimestral', 'semestral', 'anual')),
    duracao_dias INTEGER NOT NULL,
    ativo BOOLEAN DEFAULT TRUE,
    data_criacao TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE mensalidade (
    id SERIAL PRIMARY KEY,
    aluno_id INTEGER NOT NULL,
    plano_id INTEGER NOT NULL,
    mes_referencia DATE NOT NULL,
    valor DECIMAL(10,2) NOT NULL,
    data_vencimento DATE NOT NULL,
    data_pagamento DATE,
    valor_pago DECIMAL(10,2),
    desconto DECIMAL(10,2) DEFAULT 0,
    juros DECIMAL(10,2) DEFAULT 0,
    status VARCHAR(30) CHECK (status IN ('pendente', 'pago', 'atrasado', 'cancelado')) DEFAULT 'pendente',
    forma_pagamento VARCHAR(50) CHECK (forma_pagamento IN ('dinheiro', 'debito', 'credito', 'pix', 'boleto')),
    observacao TEXT,
    data_criacao TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (aluno_id) REFERENCES aluno(id) ON DELETE CASCADE,
    FOREIGN KEY (plano_id) REFERENCES plano(id) ON DELETE RESTRICT
);

CREATE INDEX idx_mensalidade_aluno ON mensalidade(aluno_id);
CREATE INDEX idx_mensalidade_status ON mensalidade(status);
CREATE INDEX idx_mensalidade_vencimento ON mensalidade(data_vencimento);

-- ============================================
-- MÓDULO DE ALUNOS
-- ============================================

CREATE TABLE aluno (
    id SERIAL PRIMARY KEY,
    usuario_id INTEGER NOT NULL UNIQUE,
    matricula VARCHAR(50) UNIQUE NOT NULL,
    plano_id INTEGER,
    personal_id INTEGER,
    data_matricula DATE NOT NULL DEFAULT CURRENT_DATE,
    data_vencimento_plano DATE,
    status VARCHAR(30) CHECK (status IN ('ativo', 'inativo', 'suspenso', 'inadimplente')) DEFAULT 'ativo',
    observacoes TEXT,
    FOREIGN KEY (usuario_id) REFERENCES usuario(id) ON DELETE CASCADE,
    FOREIGN KEY (plano_id) REFERENCES plano(id) ON DELETE SET NULL
);

CREATE INDEX idx_aluno_matricula ON aluno(matricula);
CREATE INDEX idx_aluno_status ON aluno(status);

-- ============================================
-- MÓDULO DE FUNCIONÁRIOS
-- ============================================

CREATE TABLE funcionario (
    id SERIAL PRIMARY KEY,
    usuario_id INTEGER NOT NULL UNIQUE,
    cargo VARCHAR(100) NOT NULL,
    carga_horaria INTEGER CHECK (carga_horaria > 0),
    regime_trabalhista VARCHAR(50) CHECK (regime_trabalhista IN ('CLT', 'PJ', 'estagio', 'temporario')),
    salario DECIMAL(10,2) NOT NULL,
    data_admissao DATE NOT NULL,
    data_demissao DATE,
    status VARCHAR(30) CHECK (status IN ('ativo', 'inativo', 'ferias', 'afastado')) DEFAULT 'ativo',
    FOREIGN KEY (usuario_id) REFERENCES usuario(id) ON DELETE CASCADE
);

CREATE INDEX idx_funcionario_cargo ON funcionario(cargo);
CREATE INDEX idx_funcionario_status ON funcionario(status);

CREATE TABLE personal (
    id SERIAL PRIMARY KEY,
    funcionario_id INTEGER NOT NULL UNIQUE,
    cref VARCHAR(20) NOT NULL UNIQUE,
    especialidades TEXT,
    valor_hora_aula DECIMAL(10,2),
    FOREIGN KEY (funcionario_id) REFERENCES funcionario(id) ON DELETE CASCADE
);

-- Relacionamento Aluno-Personal (atualizar FK em aluno)
ALTER TABLE aluno ADD FOREIGN KEY (personal_id) REFERENCES personal(id) ON DELETE SET NULL;

CREATE TABLE recepcionista (
    id SERIAL PRIMARY KEY,
    funcionario_id INTEGER NOT NULL UNIQUE,
    turno VARCHAR(20) CHECK (turno IN ('manha', 'tarde', 'noite', 'integral')),
    FOREIGN KEY (funcionario_id) REFERENCES funcionario(id) ON DELETE CASCADE
);

-- ============================================
-- MÓDULO DE TREINOS E EXERCÍCIOS
-- ============================================

CREATE TABLE treino (
    id SERIAL PRIMARY KEY,
    aluno_id INTEGER NOT NULL,
    personal_id INTEGER,
    nome VARCHAR(200) NOT NULL,
    descricao TEXT,
    objetivo TEXT,
    frequencia_semanal INTEGER CHECK (frequencia_semanal BETWEEN 1 AND 7),
    data_criacao DATE NOT NULL DEFAULT CURRENT_DATE,
    data_inicio DATE,
    data_fim DATE,
    ativo BOOLEAN DEFAULT TRUE,
    observacoes TEXT,
    FOREIGN KEY (aluno_id) REFERENCES aluno(id) ON DELETE CASCADE,
    FOREIGN KEY (personal_id) REFERENCES personal(id) ON DELETE SET NULL
);

CREATE INDEX idx_treino_aluno ON treino(aluno_id);
CREATE INDEX idx_treino_personal ON treino(personal_id);

CREATE TABLE exercicio (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(200) NOT NULL,
    grupo_muscular VARCHAR(100),
    descricao TEXT,
    equipamento_necessario VARCHAR(200),
    nivel_dificuldade VARCHAR(30) CHECK (nivel_dificuldade IN ('iniciante', 'intermediario', 'avancado'))
);

CREATE TABLE treino_exercicio (
    id SERIAL PRIMARY KEY,
    treino_id INTEGER NOT NULL,
    exercicio_id INTEGER NOT NULL,
    ordem INTEGER NOT NULL,
    series INTEGER NOT NULL,
    repeticoes VARCHAR(50) NOT NULL,
    carga VARCHAR(50),
    tempo_descanso INTEGER,
    observacoes TEXT,
    FOREIGN KEY (treino_id) REFERENCES treino(id) ON DELETE CASCADE,
    FOREIGN KEY (exercicio_id) REFERENCES exercicio(id) ON DELETE CASCADE,
    UNIQUE(treino_id, ordem)
);

-- ============================================
-- MÓDULO DE AULAS COLETIVAS
-- ============================================

CREATE TABLE aula (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(200) NOT NULL,
    descricao TEXT,
    capacidade_maxima INTEGER NOT NULL CHECK (capacidade_maxima > 0),
    duracao_minutos INTEGER NOT NULL CHECK (duracao_minutos > 0),
    nivel VARCHAR(30) CHECK (nivel IN ('iniciante', 'intermediario', 'avancado', 'todos')),
    ativo BOOLEAN DEFAULT TRUE
);

CREATE TABLE aula_horario (
    id SERIAL PRIMARY KEY,
    aula_id INTEGER NOT NULL,
    personal_id INTEGER,
    dia_semana INTEGER CHECK (dia_semana BETWEEN 0 AND 6),
    horario_inicio TIME NOT NULL,
    horario_fim TIME NOT NULL,
    vagas_disponiveis INTEGER,
    ativo BOOLEAN DEFAULT TRUE,
    FOREIGN KEY (aula_id) REFERENCES aula(id) ON DELETE CASCADE,
    FOREIGN KEY (personal_id) REFERENCES personal(id) ON DELETE SET NULL
);

CREATE TABLE reserva_aula (
    id SERIAL PRIMARY KEY,
    aluno_id INTEGER NOT NULL,
    aula_horario_id INTEGER NOT NULL,
    data_reserva TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    data_aula DATE NOT NULL,
    status VARCHAR(30) CHECK (status IN ('confirmada', 'cancelada', 'realizada', 'falta')) DEFAULT 'confirmada',
    FOREIGN KEY (aluno_id) REFERENCES aluno(id) ON DELETE CASCADE,
    FOREIGN KEY (aula_horario_id) REFERENCES aula_horario(id) ON DELETE CASCADE,
    UNIQUE(aluno_id, aula_horario_id, data_aula)
);

CREATE INDEX idx_reserva_aluno ON reserva_aula(aluno_id);
CREATE INDEX idx_reserva_data ON reserva_aula(data_aula);

-- ============================================
-- MÓDULO DE EQUIPAMENTOS
-- ============================================

CREATE TABLE equipamento (
    id SERIAL PRIMARY KEY,
    codigo VARCHAR(50) UNIQUE NOT NULL,
    nome VARCHAR(200) NOT NULL,
    categoria VARCHAR(100),
    marca VARCHAR(100),
    modelo VARCHAR(100),
    data_compra DATE,
    valor_compra DECIMAL(10,2),
    estado VARCHAR(30) CHECK (estado IN ('novo', 'bom', 'regular', 'ruim', 'quebrado')) DEFAULT 'bom',
    em_manutencao BOOLEAN DEFAULT FALSE,
    ativo BOOLEAN DEFAULT TRUE,
    observacoes TEXT
);

CREATE INDEX idx_equipamento_categoria ON equipamento(categoria);
CREATE INDEX idx_equipamento_estado ON equipamento(estado);

CREATE TABLE manutencao_equipamento (
    id SERIAL PRIMARY KEY,
    equipamento_id INTEGER NOT NULL,
    recepcionista_id INTEGER,
    data_solicitacao TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    data_inicio DATE,
    data_fim DATE,
    tipo VARCHAR(50) CHECK (tipo IN ('preventiva', 'corretiva', 'revisao')),
    descricao TEXT,
    custo DECIMAL(10,2),
    status VARCHAR(30) CHECK (status IN ('solicitada', 'em_andamento', 'concluida', 'cancelada')) DEFAULT 'solicitada',
    FOREIGN KEY (equipamento_id) REFERENCES equipamento(id) ON DELETE CASCADE,
    FOREIGN KEY (recepcionista_id) REFERENCES recepcionista(id) ON DELETE SET NULL
);

-- ============================================
-- MÓDULO DE AVALIAÇÕES FÍSICAS
-- ============================================

CREATE TABLE avaliacao_fisica (
    id SERIAL PRIMARY KEY,
    aluno_id INTEGER NOT NULL,
    personal_id INTEGER,
    data_avaliacao DATE NOT NULL DEFAULT CURRENT_DATE,
    peso DECIMAL(5,2),
    altura DECIMAL(3,2),
    imc DECIMAL(4,2),
    percentual_gordura DECIMAL(5,2),
    massa_magra DECIMAL(5,2),
    observacoes TEXT,
    FOREIGN KEY (aluno_id) REFERENCES aluno(id) ON DELETE CASCADE,
    FOREIGN KEY (personal_id) REFERENCES personal(id) ON DELETE SET NULL
);

CREATE INDEX idx_avaliacao_aluno ON avaliacao_fisica(aluno_id);

-- ============================================
-- VIEWS ÚTEIS
-- ============================================

-- View de alunos com informações completas
CREATE VIEW vw_alunos_completo AS
SELECT 
    a.id,
    a.matricula,
    u.nome,
    u.cpf,
    u.telefone,
    u.email,
    a.status,
    p.nome as plano_nome,
    p.valor as valor_plano,
    a.data_vencimento_plano,
    pu.nome as personal_nome
FROM aluno a
JOIN usuario u ON a.usuario_id = u.id
LEFT JOIN plano p ON a.plano_id = p.id
LEFT JOIN personal per ON a.personal_id = per.id
LEFT JOIN funcionario f ON per.funcionario_id = f.id
LEFT JOIN usuario pu ON f.usuario_id = pu.id
WHERE u.ativo = TRUE;

-- View de mensalidades em atraso
CREATE VIEW vw_mensalidades_atrasadas AS
SELECT 
    m.id,
    a.matricula,
    u.nome as aluno_nome,
    u.telefone,
    m.mes_referencia,
    m.data_vencimento,
    m.valor,
    CURRENT_DATE - m.data_vencimento as dias_atraso
FROM mensalidade m
JOIN aluno a ON m.aluno_id = a.id
JOIN usuario u ON a.usuario_id = u.id
WHERE m.status = 'atrasado'
ORDER BY dias_atraso DESC;

-- View de frequência de alunos
CREATE VIEW vw_frequencia_alunos AS
SELECT 
    a.id as aluno_id,
    u.nome,
    COUNT(ac.id) as total_acessos,
    MAX(ac.data_entrada) as ultimo_acesso
FROM aluno a
JOIN usuario u ON a.usuario_id = u.id
LEFT JOIN acesso ac ON u.id = ac.usuario_id 
    AND ac.tipo = 'entrada'
    AND ac.data_entrada >= CURRENT_DATE - INTERVAL '30 days'
GROUP BY a.id, u.nome
ORDER BY total_acessos DESC;
```