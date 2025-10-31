___
## 1. Regras Gerais

RN-ATA-001: Cada pessoa cadastrada como aluno deve possuir CPF único no sistema; CPF duplicado impede novo cadastro. (RF-ATA-001, RNF-ATA-021)

RN-ATA-002: Todo registro crítico (cadastro/alteração/exclusão) deve gerar registro/relatório de auditoria com usuário, timestamp, operação e IP. (RNF-ATA-006)

RN-ATA-003: Todas as operações que manipulam dados sensíveis (saúde, financeiros, senhas) devem armazenar dados criptografados em repouso e em trânsito. (RNF-ATA-004, README)

RN-ATA-004: O banco deve manter versão do esquema e migrações registradas (versão semântica; ex.: v1.0.0). (RNF-ATA-015)

## 2. Regras de Cadastro e Dados Pessoais (Alunos e Funcionários)

RN-ATA-005: Ao cadastrar aluno: validar CPF, formato de e-mail e telefone; exigir nome e data de nascimento. Campos de saúde podem ficar nulos inicialmente, mas devem ser preenchidos antes do primeiro acesso físico se houver restrição médica. (RF-ATA-001, RF-ATA-002, RNF-ATA-021)

RN-ATA-006: Fotos e documentos (RG, laudos) são armazenados em área segura e referenciados por hash; upload gera meta (quem, quando). (RF-ATA-004)

RN-ATA-007: Alterações em dados cadastrais registram versão anterior para histórico; somente perfis autorizados podem alterar CPF. (RF-ATA-003, RNF-ATA-005)

RN-ATA-008: Funcionários têm contrato e cargo; cada funcionário tem perfil de acesso (Admin, Gerente, Recepção, Personal, Instrutor). A alteração de cargo só é efetiva após aprovação de gerente. (RF-ATA-019 a RF-ATA-022, RNF-ATA-005)

## 3. Regras de Planos, Matrículas e Mensalidades

RN-ATA-009: Planos são modelos com duração (mensal/trimestral/semestral/anual), preço base e Personais. (RF-ATA-007, RF-ATA-008)

RN-ATA-010: Matrícula vincula um aluno a um plano com data de início/fim e status (`ativa`, `suspensa`, `cancelada`, `finalizada`). Só pode existir 1 matrícula ativa por aluno. (RF-ATA-009)

RN-ATA-011: Mensalidades são geradas automaticamente conforme calendário do plano; regra padrão: gerar fatura X dias antes do vencimento (configurável por plano). Pagamentos vinculam-se à mensalidade. (RF-ATA-010, RF-ATA-011)

RN-ATA-012: Ao registrar pagamento: armazenar data, valor pago, forma de pagamento, operador e possível desconto. Pagamentos parciais são permitidos somente quando configurado para o plano. (RF-ATA-011, RF-ATA-013)

RN-ATA-013: Mensalidade em atraso: se não paga até N dias após vencimento (configurável — padrão 7 dias), sistema marca como `inadimplente`; após M dias (configurável — padrão 30), suspende acesso automático até regularização. (RF-ATA-012, RF-ATA-029)

RN-ATA-014: Descontos aplicado na mensalidade com validade e motivo; devem ser revogáveis e limitados por regras (ex.: não cumulativo, percentual máximo). (RF-ATA-013)

## 4. Regras de Acesso e Controle de Frequência

RN-ATA-015: Registro de entrada/saída (`acessos`) obrigatório para todos os usuários (alunos e funcionários) que utilizam sistema de catraca/leitor. Cada acesso guarda timestamp, leitura de dispositivo e validação de elegibilidade. (RF-ATA-028)

RN-ATA-016: Validação de acesso: ao tentar entrar, (1) verificar matrícula ativa, (2) mensalidade em dia. Em caso negativo, negar o acesso e registrar motivo. (RF-ATA-029)

RN-ATA-017: A presença em aula pode ser registrada automaticamente via check-in ou manualmente pelo instrutor; check-in conta para relatórios de frequência. (RF-ATA-027, RF-ATA-030)

## 5. Regras de Aulas, Reservas e Limites

RN-ATA-018:  Usuário só pode reservar vaga se sua matrícula e mensalidade estiverem ativas e em dia, salvo exceções definidas (ex.: aulas abertas). (RF-ATA-025, RF-ATA-029)

RN-ATA-019:  Políticas de cancelamento: reserva pode ser cancelada até X horas antes sem penalidade; cancelamentos tardios podem gerar advertência ou bloqueio temporário de reserva (regra configurável). (RF-ATA-025)

## 6. Regras de Treinos, Avaliações e Personal Trainers

RN-ATA-020: Cada aula tem capacidade máxima (`capacidade`), instrutor, sala e horário. Reservas não podem exceder capacidade; reservas confirmadas ocupam vaga. (RF-ATA-023 a RF-ATA-026)

RN-ATA-021:  Fichas de treino são vinculadas ao aluno e ao personal trainer responsável; cada ficha tem validade e versão (histórico de alterações). (RF-ATA-014, RF-ATA-016)

RN-ATA-022:  Cada exercício registra séries, repetições, carga e observações; alterações importantes (mudança de carga > X%) devem ser registradas como nova versão. (RF-ATA-015)

RN-ATA-023:  Avaliações físicas periódicas são registradas com data, operador e medidas (peso, altura, IMC, % gordura, medidas); gera indicadores para acompanhamento. (RF-ATA-018)

RN-ATA-024:  Só profissionais com certificação válida podem assumir função de personal; certificações são validadas e com data de validade. (RF-ATA-022)

## 7. Regras de Equipamentos e Manutenção

RN-ATA-025:  Cada equipamento tem status (`disponível`, `em_manutencao`, `fora_de_uso`) e histórico de manutenções. Ativar `em_manutencao` impede agendamento de uso e deve notificar gestores. (RF-ATA-031 a RF-ATA-033)

RN-ATA-026:  Manutenção preventiva é registrada com periodicidade configurável; sistema gera alertas quando próxima manutenção estiver a Y dias. (RF-ATA-032)

RN-ATA-027:  Custos de manutenção são registrados e associados ao equipamento e fornecedor; relatórios financeiros consideram esses custos. (RF-ATA-032)

## 8. Regras Financeiras 

RN-ATA-028: Lançamentos financeiros (receitas, despesas) vinculados a pagamentos, mensalidades e manutenções; cada lançamento tem classificação contábil para relatórios. (RF-ATA-034)



## 9. Regras de Operação e Manutenibilidade

RN-ATA-029:  Nomes de tabelas e colunas seguem convenção: `snake_case`, singular/plural padronizado (ex.: `alunos`, `mensalidades`), PK `id` do tipo serial/UUID conforme política. (RNF-ATA-016)

RN-ATA-030:  Índices: chaves primárias/estrangeiras indexadas; índices compostos nas consultas frequentes (e.g., `matricula_id + data_acesso`). (RNF-ATA-017)

RN-ATA-031:  Dados históricos (acessos, logs, mensalidades antigas) podem ser particionados por período para performance. (README: particionamento)

