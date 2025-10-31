___
# Atlas Academus - Banco de Dados

## Sobre o Projeto

Documentação do banco de dados para o Sistema de Gestão de Academia (ACA). Este projeto estrutura e organiza todos os dados necessários para operação completa de uma academia, desde cadastro de alunos até relatórios gerenciais.

## 🎯 Objetivo

Fornecer uma estrutura de banco de dados robusta, normalizada e eficiente para gerenciar todas as operações de uma academia, garantindo integridade, segurança e performance.

## 🗄️ SGBD

**PostgreSQL 17+**

## 🗃️ Principais Entidades

| Entidade | Descrição |
|----------|-----------|
| **alunos** | Dados pessoais e de saúde dos alunos |
| **planos** | Tipos de planos oferecidos |
| **matriculas** | Vínculo aluno-plano |
| **mensalidades** | Controle de pagamentos |
| **treinos** | Fichas de treino personalizadas |
| **funcionarios** | Equipe da academia |
| **aulas** | Aulas em grupo |
| **reservas** | Reservas de vagas em aulas |
| **acessos** | Controle de entrada/saída |
| **equipamentos** | Equipamentos e manutenções |

## 📁 Estrutura da Documentação
```
📦 Docs
│
├─ 📋 Regra de negócio
│
├─ 📝 Requisitos do Software
│  ├─ ✅ Requisitos Funcionais
│  └─ ⚙️ Requisitos Não Funcionais
│
└─ 📖 README
```

## 🔒 Segurança

- **Criptografia**: Senhas, dados de saúde, informações financeiras
- **LGPD**: Conformidade com proteção de dados pessoais
- **Controle de Acesso**: Perfis (Admin, Gerente, Recepcionista, Personal, Aluno)
- **Auditoria**: Logs de operações críticas
- **Backup**: Diário automático com retenção de 30 dias

## 📈 Performance

- Índices em chaves primárias e estrangeiras
- Índices compostos para consultas frequentes
- Views materializadas para relatórios
- Particionamento de tabelas de histórico
- Procedures para operações complexas

## 📊 Requisitos Técnicos

- **PostgreSQL**: 12 ou superior
- **Espaço em Disco**: Mínimo 10GB
- **RAM**: Mínimo 8GB recomendado
- **Sistema Operacional**: Linux/Windows/macOS

## 🔄 Backup e Recuperação

- **Backup Completo**: Diário às 02:00
- **Backup Incremental**: A cada 6 horas
- **Retenção**: 30 dias
- **RTO**: 4 horas
- **RPO**: 6 horas

## 🔖 Versão

**v1.0.0** - Outubro/2025

## 📄 Licença

MIT License

---
