___

```SQL
-- ============================================
-- INSERÇÃO DE DADOS
-- ============================================

INSERT INTO usuario (nome, cpf, telefone, email, sexo, endereco, data_nascimento)
VALUES ('João Silva', '111.111.111-11', '(11) 91111-1111', 'joao@email.com', 'M', 'Rua A, 100', '1990-01-15'),
    ('João Silva Santos', '123.456.789-01', '(62) 98765-4321', 'joao.silva@email.com', 'M', 'Rua das Flores, 123 - Setor Sul', '1990-05-15'),
    ('Maria Oliveira Costa', '234.567.890-12', '(62) 98765-1234', 'maria.oliveira@email.com', 'F', 'Av. Goiás, 456 - Centro', '1985-08-20'),
    ('Carlos Mendes Lima', '345.678.901-23', '(62) 98765-5678', 'carlos.mendes@email.com', 'M', 'Rua T-63, 789 - Setor Bueno', '1992-11-30'),
    ('Ana Paula Ferreira', '456.789.012-34', '(62) 98765-9012', 'ana.ferreira@email.com', 'F', 'Rua 84, 321 - Setor Marista', '1995-03-10'),
    ('Pedro Henrique Alves', '567.890.123-45', '(62) 98765-3456', 'pedro.alves@email.com', 'M', 'Av. T-9, 654 - Setor Oeste', '1988-07-22'),
    ('Juliana Cardoso Rocha', '678.901.234-56', '(62) 98765-7890', 'juliana.rocha@email.com', 'F', 'Rua C-220, 987 - Jardim América', '1993-12-05'),
    ('Ricardo Souza Martins', '789.012.345-67', '(62) 98765-2345', 'ricardo.martins@email.com', 'M', 'Rua 1010, 111 - St. Pedro Ludovico', '1991-09-18'),
    ('Fernanda Lima Santos', '890.123.456-78', '(62) 98765-6789', 'fernanda.santos@email.com', 'F', 'Av. Anhanguera, 222 - Setor Leste Vila Nova', '1994-04-25');


INSERT INTO plano (nome, descricao, valor, tipo, duracao_dias)
VALUES ('Mensal', 'Plano de 1 mês', 150.00, 'mensal', 30),
    ('Básico Mensal', 'Acesso à área de musculação e ergometria', 89.90, 'mensal', 30),
    ('Premium Mensal', 'Musculação + Aulas coletivas ilimitadas', 149.90, 'mensal', 30),
    ('Black Mensal', 'Acesso completo + Personal Trainer 2x/semana', 299.90, 'mensal', 30),
    ('Básico Trimestral', 'Plano básico com 10% de desconto', 242.73, 'trimestral', 90),
    ('Premium Trimestral', 'Plano premium com 10% de desconto', 404.73, 'trimestral', 90),
    ('Anual Premium', 'Plano anual com 20% de desconto - Melhor custo-benefício', 1439.04, 'anual', 365);

INSERT INTO funcionario (usuario_id, cargo, carga_horaria, regime_trabalhista, salario, data_admissao)
VALUES (3, 'Personal Trainer', 40, 'CLT', 3500.00, '2023-01-15'),
	(3, 'Personal Trainer', 44, 'CLT', 3500.00, '2023-01-10', 'ativo'),
    (5, 'Recepcionista', 44, 'CLT', 2200.00, '2023-03-15', 'ativo'),
    (7, 'Personal Trainer', 40, 'PJ', 4200.00, '2023-06-01', 'ativo');

INSERT INTO personal (funcionario_id, cref, especialidades, valor_hora_aula)
VALUES (1, 'CREF-123456', 'Musculação e Funcional', 80.00),
       (1, 'CREF 123456-G/GO', 'Hipertrofia, Emagrecimento, Funcional', 80.00),
       (3, 'CREF 789012-G/GO', 'Musculação, Reabilitação, Idosos', 95.00);

INSERT INTO aluno (usuario_id, matricula, plano_id, personal_id, data_matricula, data_vencimento_plano)
VALUES (1, 'MAT-001', 1, 1, '2024-01-10', '2024-02-10'),
       (2, 'MAT-002', 2, NULL, '2024-01-15', '2024-04-15')
       (1, 'ACA2024001', 3, 1, '2024-01-15', '2024-12-15', 'ativo'),
       (2, 'ACA2024002', 2, NULL, '2024-02-20', '2024-12-20', 'ativo'),
       (4, 'ACA2024003', 1, NULL, '2024-03-10', '2024-12-10', 'ativo'),
       (6, 'ACA2024004', 3, 2, '2024-04-05', '2024-12-05', 'ativo'),
       (8, 'ACA2024005', 2, NULL, '2024-05-12', '2024-12-12', 'inadimplente');

INSERT INTO mensalidade (aluno_id, plano_id, mes_referencia, valor, data_vencimento, status)
VALUES (1, 1, '2024-01-01', 150.00, '2024-02-10', 'pendente'),
       (1, 3, '2024-11-01', 299.90, '2024-11-10', 'pago'),
       (2, 2, '2024-11-01', 149.90, '2024-11-10', 'pago'),
       (3, 1, '2024-11-01', 89.90, '2024-11-10', 'pendente'),
       (4, 3, '2024-11-01', 299.90, '2024-11-10', 'pago'),
       (5, 2, '2024-10-01', 149.90, '2024-10-10', 'atrasado'),
       (5, 2, '2024-11-01', 149.90, '2024-11-10', 'atrasado');

UPDATE mensalidade 
SET data_pagamento = '2024-11-08', 
    valor_pago = valor,
    forma_pagamento = 'pix'
WHERE id IN (1, 2, 4);

-- ============================================
-- ATUALIZAÇÃO DE DADOS
-- ============================================

UPDATE usuario 
SET telefone = '(11) 98888-8888'
WHERE cpf = '111.111.111-11';

UPDATE aluno
SET status = 'inadimplente',
    observacoes = 'Aluno com mensalidades em atraso - Bloqueio automático'
WHERE id IN (
    SELECT DISTINCT aluno_id 
    FROM mensalidade 
    WHERE status = 'atrasado'
);


-- ============================================
-- CONSULTAS COM JOINS
-- ============================================

SELECT 
    a.matricula,
    u.nome AS aluno_nome,
    u.telefone,
    pl.nome AS plano,
    m.mes_referencia,
    m.valor,
    m.data_vencimento,
    m.data_pagamento,
    m.status AS status_pagamento,
    CASE 
        WHEN m.status = 'atrasado' THEN CURRENT_DATE - m.data_vencimento
        ELSE 0
    END AS dias_atraso
FROM aluno a
INNER JOIN usuario u ON a.usuario_id = u.id
INNER JOIN plano pl ON a.plano_id = pl.id
LEFT JOIN mensalidade m ON a.id = m.aluno_id 
    AND m.mes_referencia >= '2024-10-01'
ORDER BY u.nome, m.mes_referencia;

-------------------------------------------------

SELECT 
    a.matricula,
    u.nome AS aluno_nome,
    u.telefone,
    u.email,
    pl.nome AS plano,
    pl.valor AS valor_plano,
    a.data_vencimento_plano,
    up.nome AS personal_trainer,
    p.cref,
    a.status
FROM aluno a
INNER JOIN usuario u ON a.usuario_id = u.id
INNER JOIN plano pl ON a.plano_id = pl.id
INNER JOIN personal p ON a.personal_id = p.id
INNER JOIN funcionario f ON p.funcionario_id = f.id
INNER JOIN usuario up ON f.usuario_id = up.id
WHERE a.status = 'ativo'
ORDER BY u.nome;


-- ============================================
-- CONSULTAS COM AGREGAÇÃO
-- ============================================

SELECT 
    m.status,
    COUNT(*) AS quantidade_mensalidades,
    SUM(m.valor) AS valor_total,
    AVG(m.valor) AS valor_medio,
    MIN(m.valor) AS menor_valor,
    MAX(m.valor) AS maior_valor
FROM mensalidade m
WHERE m.mes_referencia >= '2024-10-01'
GROUP BY m.status
ORDER BY valor_total DESC;

-----------------------------------------------

SELECT 
    pl.nome AS plano,
    pl.tipo,
    COUNT(DISTINCT a.id) AS total_alunos,
    COUNT(m.id) AS total_mensalidades,
    SUM(CASE WHEN m.status = 'pago' THEN m.valor_pago ELSE 0 END) AS receita_recebida,
    SUM(CASE WHEN m.status IN ('pendente', 'atrasado') THEN m.valor ELSE 0 END) AS receita_pendente
FROM plano pl
LEFT JOIN aluno a ON pl.id = a.plano_id
LEFT JOIN mensalidade m ON a.id = m.aluno_id
WHERE pl.ativo = TRUE
GROUP BY pl.id, pl.nome, pl.tipo
ORDER BY total_alunos DESC;

-----------------------------------------------


-- ============================================
-- CONSULTAS COM SUBCONSULTAS
-- ============================================

SELECT 
    a.matricula,
    u.nome,
    pl.nome AS plano,
    m.valor,
    m.status
FROM aluno a
INNER JOIN usuario u ON a.usuario_id = u.id
INNER JOIN plano pl ON a.plano_id = pl.id
INNER JOIN mensalidade m ON a.id = m.aluno_id
WHERE m.valor > (
    SELECT AVG(valor) 
    FROM mensalidade
)
ORDER BY m.valor DESC;

--------------------------------------------

SELECT DISTINCT 
    tipo,
    COUNT(*) OVER (PARTITION BY tipo) AS quantidade_planos_tipo
FROM plano
WHERE ativo = TRUE
ORDER BY tipo;

--------------------------------------------

SELECT DISTINCT 
    cargo,
    COUNT(*) OVER (PARTITION BY cargo) AS quantidade_funcionarios
FROM funcionario
WHERE status = 'ativo'
ORDER BY cargo;


-- ============================================
-- VIEWS
-- ============================================

CREATE VIEW view_alunos_financeiro AS
SELECT 
    a.id AS aluno_id,
    a.matricula,
    u.nome AS aluno_nome,
    u.cpf,
    u.telefone,
    u.email,
    a.status AS status_aluno,
    pl.nome AS plano_nome,
    pl.valor AS valor_plano,
    a.data_matricula,
    a.data_vencimento_plano,
    COUNT(m.id) AS total_mensalidades,
    SUM(CASE WHEN m.status = 'pago' THEN m.valor_pago ELSE 0 END) AS total_pago,
    SUM(CASE WHEN m.status IN ('pendente', 'atrasado') THEN m.valor ELSE 0 END) AS total_pendente,
    ROUND(
        (SUM(CASE WHEN m.status = 'pago' THEN 1 ELSE 0 END)::DECIMAL / 
        NULLIF(COUNT(m.id), 0)) * 100, 
        2
    ) AS percentual_adimplencia
FROM aluno a
INNER JOIN usuario u ON a.usuario_id = u.id
INNER JOIN plano pl ON a.plano_id = pl.id
LEFT JOIN mensalidade m ON a.id = m.aluno_id
WHERE u.ativo = TRUE
GROUP BY a.id, a.matricula, u.nome, u.cpf, u.telefone, u.email, 
         a.status, pl.nome, pl.valor, a.data_matricula, a.data_vencimento_plano;



-- ============================================
-- CÁLCULOS E OPERAÇÕES
-- ============================================

SELECT 
    aluno.matricula,
    usuario.nome,
    plano.valor AS valor_original,
    plano.valor * 0.90 AS valor_com_10_desconto,
    plano.valor * 0.85 AS valor_com_15_desconto
FROM aluno
INNER JOIN usuario ON aluno.usuario_id = usuario.id
INNER JOIN plano ON aluno.plano_id = plano.id;

INSERT INTO mensalidade (
    aluno_id, 
    plano_id, 
    mes_referencia, 
    valor, 
    data_vencimento, 
    status
)
VALUES (
    1,
    1,
    CURRENT_DATE,
    150.00,
    CURRENT_DATE + INTERVAL '30 days',
    'pendente'
);


```